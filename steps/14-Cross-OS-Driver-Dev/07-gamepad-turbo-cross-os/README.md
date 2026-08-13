# 14-07 · Gamepad turbo — attach to an existing USB controller, all 3 OSes

**Week:** W36–38 · **Track:** G · **Prev:** [`../06-usb-device-driver-app`](../06-usb-device-driver-app/README.md) · **Next:** [`../08-capstone-cross-os-driver`](../08-capstone-cross-os-driver/README.md)

## Objective
Write code that *attaches to an existing device* — a normal USB gamepad (Xbox-style HID controller) — and adds a feature it doesn't have: **turbo** (auto-fire: holding a button repeats it at N Hz). Two layers per OS: **interception** (get the button presses out of the device) + **injection** (feed transformed input to a virtual controller games still see). Plus the OS-agnostic turbo state machine. Userspace-first per OS; kernel filter variant as the stretch (that's the "real driver" half).

## Tasks
- [ ] **HID primer**: report descriptor → how an Xbox controller encodes buttons (bitmask bytes), `hid-generic` binding — how a driver claims a device via VID/PID (`usb_device_id` / HID `id_table` / Windows INF / macOS matching dictionary)
- [ ] **Turbo core** (`code/turbo_core`, no OS deps): per-button config (target button, rate Hz), edge-detect on press, timer-synthesized presses, debounce, hold-to-release pass-through. Unit-test standalone with a fake button stream (assert exact output timing)
- [ ] **Linux** (userspace first): `hidraw` read + `uinput` virtual gamepad (evdev) — turbo in the middle; kernel stretch: custom HID driver (clone `hid-generic` bound to your controller, rewrite reports in `raw_event`)
- [ ] **Windows** (userspace first): Raw Input / HID API read + **ViGEmBus** virtual Xbox 360 controller; kernel stretch: KMDF HID **filter driver** on the HID class (attach to the controller, transform input reports before HID client sees them)
- [ ] **macOS** (userspace only — third-party kernel drivers are dead on Apple Silicon): `IOHIDManager` receive + **`IOHIDUserDevice`** virtual controller; DriverKit `HIDVirtualDevice` dext as read-only stretch
- [ ] **Ship**: turbo live on your daily-driver OS — any button selectable, rate measurable (input-viewer shows steady N Hz auto-fire), games see a normal controller (no detectable remapper)
- [ ] Port the turbo core + injection to the other 2 OSes (userspace routes)
- [ ] Writeup in `notes/`: per-OS comparison — kernel filter vs userspace interception; why userspace won on macOS; Windows driver-signing requirement; what a defender sees (new virtual HID device, unsigned driver) — 2-line detection note feeds Track M

## Resources
- Linux: kernel `Documentation/hid/`, hidraw + uinput docs, `drivers/hid/hid-generic.c`
- Windows: MS HID filter-driver docs + WDK samples, ViGEmBus source
- Apple: IOHIDManager / IOHIDUserDevice / DriverKit HIDVirtualDevice docs
- Xbox 360 report descriptor reference (free60 wiki) for the button-bitmap map

## Exit Criteria
- [ ] Turbo runs on 1 OS end-to-end, measured auto-fire rate, configurable button + Hz — `labs/`
- [ ] Turbo core unit-tested standalone, ports on all 3 OSes (userspace routes) — `code/`
- [ ] Kernel variant on 1 OS (Linux HID driver or KMDF HID filter) in a VM — `labs/`
- [ ] Explain in ≤5 lines how a HID driver binds to a controller (VID/PID match → probe → report parsing) — `notes/`
- [ ] Writeup comparing kernel-filter vs userspace interception per OS — `notes/`

## Links
- [uinput (kernel docs)](https://www.kernel.org/doc/html/latest/input/uinput.html)
- [hidraw (kernel docs)](https://www.kernel.org/doc/html/latest/hid/hidraw.html)
- [ViGEmBus](https://github.com/nefarius/ViGEmBus)
- [Windows HID filter drivers (MS)](https://learn.microsoft.com/en-us/windows-hardware/drivers/hid/)
- [IOHIDUserDevice (Apple)](https://developer.apple.com/documentation/iokit/iohiduserdevice)
- [HIDVirtualDevice (DriverKit)](https://developer.apple.com/documentation/driverkit/hidvirtualdevice)
- [Xbox 360 gamepad HID (free60 wiki)](https://free60.org/wiki/Gamepad)
