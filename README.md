# Auto-install OpenBSD on QEMU

See [blog post](https://www.skreutz.com/posts/autoinstall-openbsd-on-qemu/).

## Errata

Starting with OpenBSD 7.0, autoinstall(8) no longer respects the TFTP server
name option. Instead, it uses the next-server DHCP option. Thus, the installer
tries to fetch the response file from the QEMU host at
http://10.0.2.2/install.conf.

To fix that, you can serve the mirror on port 80 instead of 8080 as follows:

    $ python3 -m http.server --directory mirror --bind 127.0.0.1 8080
    $ sudo socat tcp-listen:80,reuseaddr,fork tcp:localhost:8080

Of course, binding to port 80 requires root privileges. I think it should be
possible to avoid that using a corresponding guest forwarding from
tcp:10.0.2.2:80 to tcp:10.0.2.2:8080 but I failed to make that work.
