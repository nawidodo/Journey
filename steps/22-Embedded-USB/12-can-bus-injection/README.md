# 22-12 · DIY CAN bus injector — sniff and speak the car's network (stretch)

**Week:** W36+ stretch · **Track:** N · **Prev:** [`../11-diy-radio-gadgets`](../11-diy-radio-gadgets/README.md)

## Objective
The vehicle network: unauthenticated broadcast bus. ESP32 + CAN transceiver (MCP2515 or SN65HVD230) → sniff frames, decode IDs, inject. **Own car, parked, own tooling only — never on a vehicle you don't own, never while driving.**

## Tasks
- [ ] CAN basics: arbitration, IDs, bit timing, OBD-II pinout; ESP32 + transceiver wiring; `can-utils` on the bench first (loopback)
- [ ] Sniff own car (parked, ignition off→on): log frames, correlate IDs to actions (door lock/unlock, cluster, window) — a CAN mapping session
- [ ] Injection: replay door-lock frame (own car), then a harmless-but-spoofed read (e.g., vehicle-speed display) — CAN has no sender authentication, that's the lesson
- [ ] UDS/OBD basics: read a PID over diagnostic session; where UDS adds auth (security access seed/key) and where it doesn't
- [ ] Writeup: ID map + the "why CAN trusts everyone" security analysis — pairs Track M (vehicle forensics read artifact) — `notes/`

## Resources
- CAN bus specs (Bosch); OBD-II pinout; MCP2515/SN65HVD230 datasheets; your 22-11 radio notes (same sniff→replay shape, different bus)

## Exit Criteria
- [ ] Bench loopback → own-car sniff → one verified injected frame — `labs/`
- [ ] ID map + security writeup — `notes/`

## Links
- [can-utils](https://github.com/linux-can/can-utils)
- [CAN bus introduction](https://www.kvaser.com/lesson/can-protocol-tutorial/)
