# 02-06 · macOS App — SwiftUI Shell

**Week:** W12–14 · **Track:** B · **Prev:** [`../05-nes-cart-mappers-rom-loader`](../05-nes-cart-mappers-rom-loader/README.md) · **Next:** [`../07-metal-mtkview-renderer`](../07-metal-mtkview-renderer/README.md)

## Objective
Wrap the portable C NES core in a Swift + SwiftUI macOS app.

## Tasks
- [ ] SwiftUI app target; menu bar + toolbar
- [ ] Bridge the C core (module map / header, `@_cdecl` callbacks)
- [ ] Open ROM via file picker / drag & drop
- [ ] Keyboard → controller input mapping
- [ ] Run emulation loop off main thread; log frame count

## Resources
- Hacking with Swift (SwiftUI fundamentals)
- Apple SwiftUI documentation
- (Bridge: Swift/C interop docs)

## Exit Criteria
- [ ] App loads a ROM and logs frames — `code/`

## Links
- [SwiftUI docs](https://developer.apple.com/documentation/swiftui)
- [SwiftUI by Example](https://www.hackingwithswift.com/quick-start/swiftui)
