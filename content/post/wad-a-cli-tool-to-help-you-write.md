+++
title = "WAD a CLI tool to help you write"
date = "2017-02-21T16:02:30+01:00"
tags = ["hackweek", "golang", "writting"]
author = "Mauro Morales"
description = "A rubyist learning Go"
draft = true
+++

As part of the Cloud Foundry team in SUSE I spend most of my time writing Ruby
code like on the OpenStack [CPI] and [Validator]. However many of the sources
like the BOSH [agent] and the [new CLI] are writing in Go so I decided to write
my own application in order to get better acquainted with language. Here is a
summary of my experience.

## The project

Words a Day is a CLI tool to get me into the writing habit. Every year I say I
want to write more but in the end I just do it when I feel like it doing it.
The idea for this tool came from the suggestion that you have to commit to
writing a certain amount of words every day if you want the habit to kick in.

_Note: If you are interested in using WAD and not on my experience writing a
CLI app in Go then check out [WAD]_

## Installing Go

Installing Go on openSUSE Tumbleweed was very as easy as running the following
command:

```
# zypper install go
```

Compared to Ruby it is pretty much the same experience. I don't really like
using the system Ruby but instead use [rbenv] and [ruby-build] which allows me
to have multiple versions installed at the same time. Weather I need to have
multiple versions of Go installed remains to be seen but after a quick google
search I was able to find [Go Version Manager] so I assume it might be the
case.

After installing Go there is an extra step you need to do because Go requires
you to organize your code with a very specific structure. It's not important
where you have your main folder but underneath it you must have a `bin`, `pkg`
and a `src` directory.

I decided to use my home folder because `bin` is already there and I also
happen to like to keep my code under a `src` folder so all I was missing was
the `pkg` directory which got created after running my first go command. You
can achieve this by setting `GOPATH=$HOME`.

## Editor

I use Vim as my main editor. There is an amazing plugin called [vim-go] which
brings syntax highlight and a lot of goodies. When writing Ruby code I a
similar plugin called [vim-ruby] but it doesn't come close to having the same
functionality. This of course is not the plugin's fault but a by product of the
impressive tooling behind Go.

Even though I'm not a fan of Go's syntax, I really enjoyed not having to worry
about formatting my code because vim-go runs `go fmt` every time I save.
Specially if you work on projects with different style guides.

TK more reasons

## Writting Tests

Ok all set. Let's write some ~~code~~ tests! From my little previous experience
with Go I knew there is a library called [Ginkgo] that let's you write tests in
a very similar way to [RSpec]. I decided to use this in order to have a lower
entry barrier since I'm already used to this way of writing tests. To install
Ginkgo I had to run the following commands:

```
$ go get github.com/onsi/ginkgo/ginkgo
$ go get github.com/onsi/gomega
```

This feels very similar to installing a gem in Ruby:

```
$ gem install rspec
```

With the difference that is built in the main `go` command instead of being a
third party package/gem. However I was not very excited to see that when
listing packages with `go list ...` it doesn't display the packages' versions
like `gem list` does for installed gems. Probably not related but I also found
that many Go packages don't even have a version but rely only on a certain
commit in their master branch.


Enough of that, let's see our first test.

```Go
var _ = Describe("FileManager", func() {
	Describe("Track", func() {
		Context("when in this situation", func() {
			It("does this", func() {
				Expect(something).To(Equal(somethingElse))
			})
		})
	})
})
```

Yes I know that all those parenthesis and curly braces make it quite ugly to
read but you get keep the test structure which to me is the main reason why
you'd like to use something like RSpec or Ginkgo. The same test would look
something like this in Ruby:

```Ruby
describe FileManager do
  describe '#files' do
    context 'when there is 1 file' do
      subject.track('filename')

      it 'returns an array with all the files' do
        expect(subject.files).to eq(['filename'])
      end
    end
  end
end
```

When writing RSpec tests I start by describing the class, in this example
`FileManager`, Go doesn't have classes so I decided to write the name of the
struct in this package which I expect to hold the same data.

As a next step I describe a class method `#files` which I expect will return an
array with the file names when called. Similarly in Go I describe the name of
the exported function that returns a slice with the file names when called.

The main surprise here was the fact that you are not encouraged to mock or stub
like in the Ruby world. In Go you are expected to do dependency injection and
fakes. WAD is very simple so this hasn't been much of an issue yet but
hopefully when I add more tests I'll get to figure this out better.

## Writing Implementation

On the implementation side things felt quite different mainly because Go
doesn't have classes. I was able to get around this by thinking of structs as
my classes and functions with a receiver as their methods.

```Ruby
class FileManager
  def track
    ...
  end
end
```

```Go
package filemanager

type Filemanager struct {
  ...
}

func (f *FileManager) Track() {
  ...
}
```

Now I still find Ruby's way of doing it much more elegant but removing classes
all together in order to remove inheritance and rely only on compmosition is
quite a bold move. I guess I need to write more Go code in order reap the
benefits of this feature.

Another tricky thing for me was the native support for constructors but in the
end you can create your own with a function.

```Ruby
class FileManager
  def initializer
    ...
  end
end

file_manager = FileManager.new
```

```Go
package filemanager

type FileManager struct {
  ...
}

func NewFileManager() FileManager {
  ...
}

fileManager := NewFileManager()
```

## Debugging

## Dependancy management?

## Conclusion

[CPI]: https://github.com/cloudfoundry-incubator/bosh-openstack-cpi-release
[Validator]: https://github.com/cloudfoundry-incubator/cf-openstack-validator
[agent]: https://github.com/cloudfoundry/bosh-agent
[new CLI]: https://github.com/cloudfoundry/bosh-cli
[WAD]: https://github.com/mauromorales/wad
[rbenv]: https://github.com/rbenv/rbenv
[ruby-build]: https://github.com/rbenv/ruby-build
[Go Version Manager]: https://github.com/moovweb/gvm
[vim-go]: https://github.com/fatih/vim-go
[vim-ruby]: https://github.com/vim-ruby/vim-ruby
[Ginkgo]: https://onsi.github.io/ginkgo/
[RSpec]: http://rspec.info/documentation/
