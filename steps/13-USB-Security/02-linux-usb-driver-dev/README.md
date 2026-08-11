# 13-02 · Linux USB Driver Development

**Week:** W25–26 · **Track:** F · **Prev:** [`../01-usb-protocol-stack-basics`](../01-usb-protocol-stack-basics/README.md) · **Next:** [`../03-usb-attack-surface`](../03-usb-attack-surface/README.md)

## Objective
Write a driver for a simple USB device — descriptor tables and URBs stop being theory.

## Tasks
- [ ] Linux device model: bus/device/driver, match tables (`usb_device_id`), probe/release
- [ ] Write a module driver (start from `usb-skeleton.c`) for a real device or a gadget you emulate
- [ ] URBs: allocation, submission, completion; read/write file ops
- [ ] Debug with dmesg + gdb in your Dojo VM; module load/unload
- [ ] Bonus: get a USB stick enumerating under your driver

## Resources
- *Linux Device Drivers* 3rd ed. ch.18 (USB) — free online (LDD3)
- Kernel docs `Documentation/usb/`; `drivers/usb/usb-skeleton.c` (perfect starter)
- Your Kernel-Exploit-Dojo toolchain (QEMU VM, gdb)

## Exit Criteria
- [ ] Module source + probe→URB→data demo log — `code/`
- [ ] Diagram of probe/bind/URB flow — `notes/`

## Links
- [Linux USB driver docs](https://docs.kernel.org/driver-api/usb/index.html)
- [LDD3 ch. USB](https://lwn.net/Kernel/LDD3/)
