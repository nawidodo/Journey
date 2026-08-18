# 22-13 · Own RTOS scheduler — tasks, semaphores, ISR, context switch (stretch)

**Week:** W28–36 stretch · **Track:** N · **Prev:** [`../12-can-bus-injection`](../12-can-bus-injection/README.md)

## Objective
Every microcontroller runs an RTOS (FreeRTOS/Zephyr/QNX in cars, planes, pacemakers). Build a mini-RTOS on QEMU ARM (or your ESP32): task control blocks, round-robin + priority scheduling, semaphores, and the heart of the software-hardware interface — the context switch (which registers does the CPU save? who triggers it: SysTick ISR + PendSV). Security tie-in: RTOS bugs are memory-safety bugs in safety-critical code; the scheduler is the attack surface.

## Tasks
- [ ] Core: TCBs, task creation, idle task; round-robin; the context switch in assembly (stack save/restore, PSP vs MSP — pairs 24-01 kernel + 02-11 RISC-V)
- [ ] Scheduling: priority + preemption; SysTick-driven; PendSV for deferred switching (the "switch outside ISR" pattern real RTOSes use)
- [ ] Sync: semaphores (block/wake lists), mutex + priority inversion problem (and basic priority inheritance)
- [ ] Portability: run same code on QEMU ARM and your ESP32 (the porting layer — hardware interface practice)
- [ ] Security lab: buffer overflow in a task corrupting a TCB → scheduled chaos; add stack canaries + MPU region; re-attack blocked (pairs 03-xx, 22-09 firmware) — `labs/`

## Resources
- FreeRTOS source (peer); ARM Cortex-M programming manuals (the registers are the spec); your 24-01 + 22-09 notes

## Exit Criteria
- [ ] Tasks + semaphores + preemption on QEMU ARM and ESP32 — `labs/`
- [ ] TCB-corruption attack → hardened, writeup — `labs/` + `notes/`

## Links
- [FreeRTOS](https://www.freertos.org/)
- [Cortex-M3 manual](https://developer.arm.com/documentation/dui0552/latest/)
