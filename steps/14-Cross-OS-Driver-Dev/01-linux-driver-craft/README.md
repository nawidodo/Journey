# 14-01 · Linux Driver Craft

**Week:** W28–29 · **Track:** G · **Prev:** — · **Next:** [`../02-hardware-facing-dma-irq-mmio`](../02-hardware-facing-dma-irq-mmio/README.md)

## Objective
Turn Phase 4's hello-module into a real device: char driver with ioctl, wait queues, proper copy_to_user.

## Tasks
- [ ] miscdevice/chardev registration; read/write/ioctl handlers; `copy_to_user`/`copy_from_user` fault handling
- [ ] Wait queues + blocking I/O; `file_operations` lifecycle (open/release/llseek)
- [ ] debugfs/procfs hooks; kref refcounting, error paths
- [ ] Build + test with a userspace C harness in your Dojo VM (KASAN/UBSAN on)
- [ ] Plant one bug (leak/UAF), find it with KASAN — bridge to Phase 5 skills

## Resources
- LDD3; kernel docs (`device-api-*`, ioctl); KASAN docs
- Your Dojo VM

## Exit Criteria
- [ ] Char driver + userspace harness — `code/`
- [ ] "`file_operations` lifecycle" notes — `notes/`