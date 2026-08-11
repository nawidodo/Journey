# 19-03 · Linux Hooking

**Week:** W23–25 · **Track:** K · **Prev:** [`../02-windows-userland-hooking`](../02-windows-userland-hooking/README.md) · **Next:** [`../04-macos-hooking`](../04-macos-hooking/README.md)

## Objective
Userland + kernel-assisted hooking on Linux.

## Tasks
- [ ] Own `LD_PRELOAD` shim: your `.so`, dlsym-based forwarding
- [ ] GOT rebinder from scratch: walk ELF relocations (`DT_JUMPLOTS`, `DT_SYMTAB`), rebind at runtime
- [ ] ptrace single-step hook from scratch
- [ ] uprobes/kprobes + eBPF: the kernel layer — you write the BPF programs (the runtime is a kernel framework, not re-implemented)
- [ ] Detect: compare resolved addresses vs expected; `/proc/self/maps` analysis

## Resources
- LD_PRELOAD/dynamic-linking docs; eBPF docs; Phrack; your Dojo VM

## Exit Criteria
- [ ] LD_PRELOAD + GOT hook demos — `code/`

## Links
- [LD_PRELOAD tutorial (man7)](https://man7.org/linux/man-pages/man8/ld.so.8.html)
- [PLT/GOT explanation (blog)](https://www.technovelty.org/linux/plt-and-got-the-key-to-code-sharing-and-dynamic-libraries.html)
