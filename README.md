# Auto-install OpenBSD on QEMU

This is a fork of
[`autoinstall-openbsd-on-qemu`](https://git.skreutz.com/autoinstall-openbsd-on-qemu.git/)
by Stefan Kreutz, who described his approach in a [blog
post](https://www.skreutz.com/posts/autoinstall-openbsd-on-qemu/).

In net-booted [`autoinstall(8)`](https://man.openbsd.org/autoinstall.8) mode,
OpenBSD expects `install.conf` on an HTTP server on port `80` on the host
system. Unfortunately, QEMU does not allow redirection with `guestwd` in this
case. Therefore, this fork introduces an `LD_PRELOAD` patch taht allows
requests to forwarded to an unprivileged port.

## Changes

* Make the OpenBSD version configurable via `OBSD_VERSION`.
* Start a Python HTTP server on an available unprivileged port and
* redirect guest requests for port `80` to this unprivileged port.
* Add scripts to start the VM and ssh into it.

## Usage

Run `autoinstall-openbsd-on-qemu`. This will automatically install OpenBSD and
create wrapper scripts for QEMU and ssh. After installation you can start
the QEMU VM with
```
./run-openbsd [arg, ...]
```
and ssh into the VM via
```
./ssh-openbsd [arg, ...]
```

## License

The script is published under the ISC license.
