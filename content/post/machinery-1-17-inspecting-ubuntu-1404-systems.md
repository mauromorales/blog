---
title: Machinery 1.17 - Inspecting Ubuntu 14.04 Systems
date: 2016-01-25
published: true
---

The main feature of this minor release is the ability to inspect Ubuntu 14.04
and Docker Containers. No special configuration is required you just need to
run the `inspect` or `inspect-container` command pointed at the Ubuntu host.

In order to add this functionality we had to teach Machinery a few new tricks
to handle the upstart init system and the dpkg package manager.

## Upstart

Machinery can already find SysVinit and systemd services in a system. Even
though Upstart is pretty much on it's way out it was the third big player in
the init system league and being able to parse it's services is a great
addition for Machinery's code base.

In the case of Ubuntu 14.04 we were able to find the complete list of services
by merging the SysV and Upstart services with the following commands:

```
# upstart services
/sbin/initctl show-config -e
# sysv services
/usr/sbin/service --status-all
```

The main takeaway for me on this topic was that there is no generic way to
determine the default init system on a Linux distribution and that in some
cases like in Ubuntu 14.04 and RHEL 6 they can be a mix of two.

## Dpkg

Ubuntu is also the first distribution that Machinery can inspect that uses the
dpkg package manager and we had to expand the _packages_, _repositories_, _patterns_,
_config-files_, _changed-managed-files_ and _unmanaged-files scopes_.

For the _packages_ scope we get all the information from running `dpkg -l`. In
order to differentiate rpm from deb packages we added the _package\_system_
attribute which can be "rpm" or "deb" respectively

When working on the _repositories_ scope we were originally going to do the same
but in the end differentiating by the tool used to install those packages
proved to be a better approach. There are enough differences between zypper,
yum and apt so we added the attribute _repository\_system_ which can have a
value of "zypp", "yum" and "apt" accordingly.

In the _patterns_ scope we introduced a dependency to the "tasksel" command. When
present in a system and if there are any tasks installed they get listed using
the `tasksel --list-tasks` command.

The _config-files_ and _changed-managed-files_ scopes depend deeply on the
information provided by the package. In order determine what has changed we
rely on the checksums provided by the package maintainer. Unfortunately many
packages don't provide the checksums for every one of their files. This means
that if a configuration or managed file that lacks it's checksum was changed
after being installed Machinery won't be able to detect this change.

## Updating

If you've installed Machinery as a package using zypper all you need to do is
update it. (Builds on openSUSE Leap and SLE take longer to be available)

```
sudo zypper up machinery
```

If you've installed Machinery as a gem

```
gem update machinery-tool
```

Finally update all your system descriptions by running

```
 machinery upgrade-format --all
```

Learn more about your systems with Machinery!
