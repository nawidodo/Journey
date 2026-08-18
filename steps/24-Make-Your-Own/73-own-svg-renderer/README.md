# 24-73 · Own SVG renderer — path parsing, curves → your framebuffer (stretch)

**Week:** W46+ opt (stretch) · **Track:** P · **Prev:** [`../72-own-spreadsheet`](../72-own-spreadsheet/README.md) · **Next:** [`../74-own-ci-runner`](../74-own-ci-runner/README.md) · **Pairs:** 10-05, 24-49, 24-55

## Objective
Vector graphics is where rendering meets parsing: SVG is text describing shapes. Build a renderer: XML/XML-lite parse (the tree), path grammar (`M/L/C/Q/Z` — cubic/quadratic Béziers, same curves as your 24-49 fonts), transforms (matrix math — pairs 10-05), fills + strokes, and rasterization to your framebuffer (scanline fill — your 10-05/24-49 skills). The security tie: SVG is a browser-attack surface (XSS-in-SVG, entity bombs, path complexity — pairs 24-27 clean-failure discipline); your renderer is the malformed-SVG crash lab.

## Tasks
- [ ] Parse: SVG XML-lite, groups/defs, viewBox; malformed → clean errors
- [ ] Paths: full command grammar, relative/absolute, Bézier flattening (24-49 reuse), arc flags (the nasty bit)
- [ ] Render: transforms, fills (even-odd/nonzero), strokes, gradients-lite; rasterize to PPM/framebuffer
- [ ] Robustness lab: entity-expansion bomb, huge coordinates, pathological path nesting — clean failure, no OOM (pairs 24-27/35) — `labs/`
- [ ] Self-check: a real SVG (drawn by hand or Inkscape export) renders visually correct vs your OS viewer (the oracle)

## Resources
- SVG 1.1/2 spec path chapter (the manual); resvg/librsvg source (peer); your 10-05/24-49/24-55 code

## Exit Criteria
- [ ] SVG→bitmap: paths/fills/transforms correct — `labs/` + `code/`
- [ ] Malformed-SVG clean-failure matrix — `labs/` + `notes/`

## Links
- [SVG spec](https://www.w3.org/TR/SVG2/)
- [resvg](https://github.com/RazrFalcon/resvg)