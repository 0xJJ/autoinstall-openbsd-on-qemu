# Auto-install OpenBSD on QEMU

This is a fork of
[`autoinstall-openbsd-on-qemu`](https://git.skreutz.com/autoinstall-openbsd-on-qemu.git/)
by Stefan Kreutz, who described his approach in a [blog
post](https://www.skreutz.com/posts/autoinstall-openbsd-on-qemu/).

OpenBSD in net-booted [`autoinstall(8)`](https://man.openbsd.org/autoinstall.8)
mode expects `install.conf` on an http server on port `80` on the host system.
Unfortunately QEMU does not allow redirection with `guestwd` in this case.
Therefore, this fork introduces a `LD_PRELOAD` patch to allow forward request
to port `8080`.

## Changes

* Make the OpenBSD version configurable via `OBSD_VERSION`.
* Redirect port `80` to `8080` and run a Python http server on port `8080` as a
  subprocess.

## Potential future changes

Port 8080 is hardcoded. A cleaner a approach would be to bind a random,
available, unprivileged port and pass it to the `LD_PRELOAD` patch via an
environment variable. But this seems a bit overkill.
