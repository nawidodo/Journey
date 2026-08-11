# 03-03 · Linux Kernel — Write a Module

**Week:** W16–17 · **Track:** A · **Prev:** [`../02-read-kernel-mm-sched-net`](../02-read-kernel-mm-sched-net/README.md) · **Next:** [`../04-read-drivers-e1000-virtio-net`](../04-read-drivers-e1000-virtio-net/README.md)

## Objective
First kernel code: loadable module touching `/dev`, proc/sysfs, and module params.

## Tasks
- [ ] Build environment (VM or chroot; Linux headers)
- [ ] `hello` module — `insmod`/`rmmod`, `dmesg`
- [ ] Char device in `/dev` with read/write handlers
- [ ] `/proc` or sysfs entry
- [ ] Module parameters + a `Makefile` (Kbuild)

## Resources
- LKD ch.2 (Building and Running Modules)
- Linux kernel docs (`Documentation/kbuild/modules.rst`)
- tldp.org LKMPG

## Exit Criteria
- [ ] Module loads, logs, and device works — `labs/`

## Links
- [Linux kernel module programming guide](https://sysprog21.github.io/lkmpg/)
- [LDD3 (free online)](https://lwn.net/Kernel/LDD3/)
