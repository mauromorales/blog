---
title: Getting started with Continuous Delivery
date: 2015-04-02
tags: continuous delivery, middleman, git flow, semaphore, ansible, rake
published: true
featured: 2
---

More and more companies are requiring developers to understand Continuous
Integration and Continuous Delivery but starting to implement it in your
projects can be a bit overwhelming. Start with a simple website and soon enough
you will feel more confident to do with more complex projects.

READMORE

## The right mindset

> TDD/BDD, CI/CD, XP, Agile, Scrum .... Ahhhhh, leave me alone I just want to
> code!

Yes, all this methodologies can be a bit complicated at first, but simply
because you are not used to them. Like a muscle you need to train them and the
more you do so, the sooner you won't feel like doing them is a total waste of
time.

Once you have made up your mind that CD is for you, your team or your project
then you will need to define a process and follow it. Don't make it easy to
break the process and before you know it you and your team will feel like fish
in the water.

## Automate a simple website deployment

There are many ways you can solve this problem. I will use a certain stack. If
you don't have experience with any of the tools, try to implement it with one
you do have experience with.

| Stack                    | Tool/Service | Alternatives        |
|--------------------------|--------------|---------------------|
| VPS                      | DigitalOcean | Linode or Vagrant   |
| Configuration Management | Ansible      | Chef or Puppet      |
| Static site generator    | Middleman    | Jekyll or pure HTML |
| CI/CD Server             | Semaphore    | Codeship or Jenkins |

The first thing is to create a new droplet in DO (you could also do this with
Ansible but we won't at this tutorial). Make sure there is a **deploy** user and
to set up ssh keys for it (again something we could do with Ansible but we'll
leave that for another post) Setup your your domain to point to the new server's
IP address, I will use 'example.com'.

### Ansible

Create a folder for your playbook and inside of it start with a file called
`ansible.cfg`. There we will override the default configuration by pointing to
a new inventory inside your playbook's folder and specify the deploy user.

```shell
[defaults]
hostfile=inventory
remote_user=deploy
```

Now in our `inventory` file we specify a group called web and include our
domain.

```shell
[web]
example.com
```

Our tasks will be defined in `simple-webserver.yml`

```yml
---
- name: Simple Web Server
  hosts: example.com
  sudo: True
  tasks:
    - name: Install nginx
      apt: pkg=nginx state=installed update_cache=true
      notify: start nginx
    - name: remove default nginx site
      file: path=/etc/nginx/sites-enabled/default state=absent
    - name: Assures project root dir exists
      file: >
        path=/srv/www/example.com
        state=directory
        owner=deploy
        group=www-data
    - name: copy nginx config file
      template: >
        src=templates/nginx.conf.j2
        dest=/etc/nginx/sites-available/example.com
      notify: restart nginx
    - name: enable configuration
      file: >
        dest=/etc/nginx/sites-enabled/example.com
        src=/etc/nginx/sites-available/example.com
        state=link
      notify: restart nginx
  handlers:
    - name: start nginx
      service: name=nginx state=started
    - name: restart nginx
      service: name=nginx state=restarted
```

In it we make reference to a template called `templates/nginx.conf.j2` where we will
specify a simple virtual host.

```shell
server {
        listen *:80;

        root /srv/www/example.com;
        index index.html index.htm;

        server_name example.com;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

I'll show you in another post how to do this same setup but with multiple
virtual hosts in case you run multiple sites.

Run it by calling:

```shell
ansible-playbook simple-webserver.yml
```


### Middleman

Middleman has a very simple way to deploy over rsync. Just make sure you have
the following gem in your `Gemfile`

```ruby
gem 'middleman-deploy'
```

And then add something like this to your `config.rb`

```ruby
activate :deploy do |deploy|
  deploy.method = :rsync
  deploy.host   = 'example.com'
  deploy.path   = '/srv/www/example.com'
  deploy.user  = 'deploy'
end
```

Before you can deploy you need to remember to build your site. This is prone to
errors so instead we will add a rake task in our `Rakefile` to do this for us.

```ruby
desc 'Build site'
task :build do
  `middleman build`
end

desc 'Deploy site'
task :deploy do
  `middleman deploy`
end

desc 'Build and deploy site'
task :build_deploy => [:build, :deploy] do
end
```

### Git Flow

Technically you don't really need [git flow][1] for this process but I do believe
having a proper [branching model][2] is key to a successful CD environment.
Depending on your team's process you might want to use something else but if you
don't have anything defined please take a look at git flow, it might be just
what you need.

For this tutorial I will oversimplify the process and just use the develop,
master and release branches by following these three steps:

  1. Commit all the desired changes into the develop branch
  2. Create a release and add the release's information
  3. Merge the release into master

Let's go through the steps in the command line. We start by adding the new
features and committing them.

```shell
git add Rakefile
git commit -m 'Add rake task for easier deployment'
```

Now we create a release.

```shell
git flow release start '1.0.0'
```

This would be a good time to test everything out. Bump the version number of
your software (in my case 1.0.0), update the change log and do any last minute
fixes.

Commit the changes and let's wrap up this step by finishing our release.

```shell
git flow release finish '1.0.0'
```

Try to write something significant for your message tag so you can easily refer
to a version later on by it's description.

```shell
git tag -n
```

Hold your horses and don't push your changes just yet.

[1]: https://github.com/nvie/gitflow
[2]: http://news.bbc.co.uk/go/rss/-/2/hi/default.stm

### Semaphore

Add a new project from Github or Bitbucket.

For the build you might want to have something along the lines of:

```ruby
bundle install --path vendor/bundle
bundle exec rake spec
```

Now go into the projects settings inside the _Deployment_ tab and add a server.

Because we are using a generic option Sempahore will need access to our server.
Generate an SSH key and paste the private in Semaphore and the public in your
server.

For the deploy commands you need to have something like this:

```shell
ssh-keyscan -H -p 22 example.com >> ~/.ssh/known_hosts
bundle exec rake build_deploy
```

### Push your changes

Push your changes in the master branch and voilà, Semaphore will build and
deploy your site.

Once you get into the habit of doing this with your website you will feel more
confident of doing it with something like a Rails application.

If you have any questions please leave them below, I'll respond to every single
one of them.
