# 14-02 · Hardware-Facing Drivers: MMIO, DMA, IRQ

**Week:** W29–31 · **Track:** G · **Prev:** [`../01-linux-driver-craft`](../01-linux-driver-craft/README.md) · **Next:** [`../03-windows-kmdf-wdm`](../03-windows-kmdf-wdm/README.md)

## Objective
Write the driver layer that touches hardware — the part no userspace can reach.

## Tasks
- [ ] MMIO: `ioremap`, `readl`/`writel`; platform/PCI probe via device tree or ACPI
- [ ] IRQs: `request_irq`, threaded IRQs, bottom halves
- [ ] DMA: `dma_alloc_coherent`, streaming DMA mapping
- [ ] Drive a virtual device (virtio or a QEMU-emulated PCI device) — no hardware required
- [ ] Fault-inject: wrong MMIO offset, bad DMA — see how drivers break (the surface Phase 5 exploits)

## Resources
- LDD3 ch.9–15; virtio spec; Linux `Documentation/PCI/`, `Documentation/DMA-API.txt`
- QEMU device emulation + your Dojo VM

## Exit Criteria
- [ ] Platform driver with working DMA path — `code/`
- [ ] "MMIO → DMA → IRQ" flow diagram — `notes/`

## Links
- [Linux DMA-API docs](https://docs.kernel.org/core-api/dma-api.html)
- [Linux IRQ docs](https://docs.kernel.org/core-api/genericirq.html)
