---
description: Log into or run a command on a machine using SSH.
keywords: machine, ssh, subcommand
title: docker-machine ssh
---

Log into or run a command on a machine using SSH.

To login, just run `docker-machine ssh machinename`:

```console
$ docker-machine ssh dev

                        ##        .
                  ## ## ##       ==
               ## ## ## ##      ===
           /""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
           \______ o          __/
             \    \        __/
              \____\______/
 _                 _   ____     _            _
| |__   ___   ___ | |_|___ \ __| | ___   ___| | _____ _ __
| '_ \ / _ \ / _ \| __| __) / _` |/ _ \ / __| |/ / _ \ '__|
| |_) | (_) | (_) | |_ / __/ (_| | (_) | (__|   <  __/ |
|_.__/ \___/ \___/ \__|_____\__,_|\___/ \___|_|\_\___|_|
Boot2Docker version 1.4.0, build master : 69cf398 - Fri Dec 12 01:39:42 UTC 2014
docker@boot2docker:~$ ls /
Users/   dev/     home/    lib/     mnt/     proc/    run/     sys/     usr/
bin/     etc/     init     linuxrc  opt/     root/    sbin/    tmp      var/
```


You can also specify commands to run remotely by appending them directly to the
`docker-machine ssh` command, much like the regular `ssh` program works:

```console
$ docker-machine ssh dev free

total         used         free       shared      buffers
Mem:       1023556       183136       840420            0        30920
-/+ buffers:             152216       871340
Swap:      1212036            0      1212036
```

Commands with flags work as well:

```console
$ docker-machine ssh dev df -h

Filesystem                Size      Used Available Use% Mounted on
rootfs                  899.6M     85.9M    813.7M  10% /
tmpfs                   899.6M     85.9M    813.7M  10% /
tmpfs                   499.8M         0    499.8M   0% /dev/shm
/dev/sda1                18.2G     58.2M     17.2G   0% /mnt/sda1
cgroup                  499.8M         0    499.8M   0% /sys/fs/cgroup
/dev/sda1                18.2G     58.2M     17.2G   0%
/mnt/sda1/var/lib/docker/aufs
```

If you are using the "external" SSH type as detailed in the next section, you
can include additional arguments to pass through to the `ssh` binary in the
generated command (unless they conflict with any of the default arguments for
the command generated by Docker Machine).  For instance, the following command
forwards port 8080 from the `default` machine to `localhost` on your host
computer:

```console
$ docker-machine ssh default -L 8080:localhost:8080
```

## Different types of SSH

When Docker Machine is invoked, it checks to see if you have the venerable
`ssh` binary around locally and attempts to use that for the SSH commands it
needs to run, whether they are a part of an operation such as creation or have
been requested by the user directly. If it does not find an external `ssh`
binary locally, it defaults to using a native Go implementation from
[crypto/ssh](https://godoc.org/golang.org/x/crypto/ssh). This is useful in
situations where you may not have access to traditional UNIX tools, such as if
you are using Docker Machine on Windows without having msysgit installed
alongside of it.

In most situations, you do not need to worry about this implementation detail
and Docker Machine acts sensibly out of the box. However, if you
deliberately want to use the Go native version, you can do so with a global
command line flag / environment variable like so:

```console
$ docker-machine --native-ssh ssh dev
```

There are some variations in behavior between the two methods, so report
any issues or inconsistencies if you come across them.