# Auto-install OpenBSD on QEMU

This repository hosts a POSIX shell script to auto-install OpenBSD/amd64 6.6 to
copy-on-write disk image using QEMU. The script is intended to run on Linux. If
you already have a running OpenBSD installation, you should consider to use
OpenBSD's own hypervisor [vmm(4)](https://man.openbsd.org/vmm) instead of QEMU
as described in the [OpenBSD FAQ](https://www.openbsd.org/faq/faq16.html) and
in this [blog post](https://eradman.com/posts/autoinstall-openbsd.html).

The script will:

* Download and verify the official installation image and file sets.
* Create and serve a TFTP boot environment.
* Create an [autoinstall(8)](https://man.openbsd.org/autoinstall) configuration file including your public ssh key.
* Create and boot a copy-on-write disk image.

## Prerequisites

The script depends on the following tools:

* [QEMU](https://www.qemu.org/)
* [curl](https://curl.haxx.se/)
* Portable [signify](https://github.com/aperezdc/signify)
* [rsync](https://rsync.samba.org/)
* Portable [OpenSSH](https://www.openssh.com/portable.html)
* [socat](http://www.dest-unreach.org/socat/)

The following command installs these dependencies on Arch Linux:

  sudo pacman -S qemu curl signify rsync openssh socat

## Usage

Execute the following command to auto-install OpenBSD/amd64 6.6 to a new disk
image `disk.qcow2` in the current directory.

    ./run

When prompted, run the following command to serve `./mirror/` at
http://127.0.0.1:8080/:

    python -m http.server --directory ./mirror --bind 127.0.0.1 8080

You can override the following environment variable defaults if necessary:

* `DISK_FILE=disk.qcow2`
* `DISK_SIZE=160G`
* `CPU_COUNT=6`
* `MEMORY_SIZE=4G`

For example:

    CPU_COUNT=1 ./run

## Virtual network

The script creates a virtual network, `10.0.2.0/24`, with the following
addresses:

* Host at `10.0.2.2`
* Nameserver at `10.0.2.3`
* Guest at `10.0.2.15`

The script also redirects host host port `2222` to guest port `22` (ssh) and
host port `80` (actually `10.0.2.1` port `80`) to host port `8080`.

## Secure shell

Pass the following options to ssh or scp to connect to the guest machine:

    ssh \
      -o "StrictHostKeyChecking no" \
      -o "UserKnownHostsFile /dev/null" \
      -o "Port 2222" \
      puffy@127.0.0.1

For example, the following command forwards port `3000` on the host to port
`80` on the guest:

    ssh \
      -o "StrictHostKeyChecking no" \
      -o "UserKnownHostsFile /dev/null" \
      -o "Port 2222" \
      -N \
      -L 127.0.0.1:3000:127.0.0.1:80 \
      puffy@127.0.0.1

Press `C-a x` to stop the guest machine.
Press `C-a h` to show other options.
