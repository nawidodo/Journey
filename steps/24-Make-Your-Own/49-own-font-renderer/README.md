# 24-49 · Own font renderer — TrueType rasterization, the hidden format (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../48-own-orchestrator`](../48-own-orchestrator/README.md) · **Next:** [`../50-own-logic-analyzer`](../50-own-logic-analyzer/README.md)

## Objective
Every pixel of text you read is a format you haven't parsed: TrueType/OpenType — sfnt tables, glyph outlines (quadratic Béziers), glyph assembly + hinting (the interpreter!), bitmap rasterization. Build a renderer: parse a .ttf, rasterize glyphs to a bitmap, render your own text. The security tie: font parsers have been exploited for 20 years (the original "code in a font" attacks, Chrome/Android font CVEs) — you'll see why parsing hostile fonts is dangerous.

## Tasks
- [ ] sfnt: table directory, head/hhea/maxp/cmap (character maps), glyf/loca (outlines); parse a real system font
- [ ] Outline→bitmap: quadratic Bézier flattening, scanline fill, anti-aliasing; hinting stretch (the bytecode interpreter — pairs 02-17 wasm, 24-47 JS: a third interpreter!)
- [ ] Layout: cmap → glyph IDs, advance widths, kerning; render a string to a bitmap/PPM
- [ ] Robustness lab: malformed fonts (bad table offsets, huge coordinates, hinting bombs — pairs 24-27/35 clean-failure discipline) — `labs/`
- [ ] Self-check: render "Journey" at multiple sizes, compare visually with your OS renderer (the oracle)

## Resources
- Apple's TrueType reference (the manual); FreeType source (peer); your 24-27/24-35 notes

## Exit Criteria
- [ ] .ttf glyphs rasterized, string rendered — `labs/`
- [ ] Malformed-font clean-failure matrix — `labs/` + `notes/`

## Links
- [TrueType reference (Apple)](https://developer.apple.com/fonts/TrueType-Reference-Manual/)
- [FreeType](https://freetype.org/)
