# aquascape-studio ROADMAP

> **Language rule (owner, 2026-11-17): everything on GitHub — issues, PRs, comments, commits, docs, code — and the product itself (app UI, plant library) is English-only.** Only the end-of-run chat summary / push notification to the owner stays in Chinese. See `CLAUDE.md`.

> Maintainer: orchestrator (also product manager). Updated at the end of every iteration. Last updated: 2026-11-20.

## Core differentiator
"Watch the tank you designed grow" — driven by a real growth simulation engine, not a pre-rendered animation.

## Priorities and status

| # | Direction | Status | Notes |
|---|------|------|------|
| 1 | Growth time-lapse: 0–90 day timeline, interspecies competition, low-light zones; TS engine runs in a Web Worker | 🟢 v1 live | 11-12: sim-core TS engine + /studio/sim timeline; visuals approved, flag turned on. 11-19: tank conditions (light Low/Medium/High, CO2 on/off) are part of the scape doc and drive the sim (editor 1.3.0, compose 1.1.0). 11-20: High-light bug fixed (sim-core 0.2.0, contract 1.2.0: light above `light.max` floors at 0.75; Dutch 60-P High+CO2 1700 g vs Medium 1535 g). Open: CO2 axis below `co2.min` is lethal ([sim#11](https://github.com/aquascape-studio/aquascape-sim/issues/11)) |
| 2 | 3D editor and renderer rework: drag-and-drop rocks / driftwood / plants, Iwagumi / Nature / Dutch templates, composition guides | 🟡 In progress | 11-12 r2: `<FrontalTank>` frontal stylized tank shipped. 11-14: FrontalTank 1.1.0 (role-based rocks, buried rocks, sloped substrate bed, rule-of-thirds / golden-ratio guides, layout cache) + 4 style templates (Iwagumi sanzon / triangle / concave / Dutch streets); the `styleTemplates` flag is **off by default, waiting for Xiaopu to confirm**. 11-15: drag editor v1 (FrontalTank 1.2.0 selectable / draggable, editor document model + undo/redo + `?scape=` share link, simulation re-runs on release); the `dragEditor` flag is **off by default**. 11-16: three driftwood styles (branchy / root / log, FrontalTank 1.3.0) + editor 1.1.0 (add rocks / driftwood, −/+ resize, change driftwood style, hardscape-only edits don't re-run the simulation) + fixed the 11-15 layout issues, still behind the `dragEditor` flag. 11-17 r2: epiphytes tied onto hardscape (FrontalTank 1.4.0 `hardscapeSurfaceY` / `hardscapeAnchors`, moss cushions, rhizome plants on the surface, root `leanSide`; editor 1.2.1 `attach` + "Tie on" / "Untie" / "Lean"; templates 1.1.0 without regulated species, root driftwood up front), behind flags. 11-18: ⌘K one-sentence scape (compose 1.0.0, rule-based, in-browser: tank presets, style, light / CO2, substitutions, one undo step; flag `commandPalette` off) + render 1.4.1 (real-extent glass clamp, resolveLeanSide, seat-height surface reference) + editor 1.2.2. 11-19: Conditions row (Light / CO2) in the editor toolbar, ⌘K sentence sets conditions, phone layout of the bars and ⌘K sheet fixed; all behind flags, single approval request [studio#23](https://github.com/aquascape-studio/aquascape-studio/issues/23). 11-20: "Nature · Low tech" template (templates 1.2.0) + "low tech" / "no CO2" picks it (compose 1.2.0) + CO2 pill phone fix, behind flags. Next: flags on once approved |
| 3 | AI features: photo recognition of aquatic plants + improvement suggestions; automatic layout by tank size and style | 🟡 Groundwork | 11-18: rule-based compose (`ScapeIntent` → `ScapeDoc`) is the seam an LLM front-end will fill; no AI model yet |
| 4 | Expand the plant library to 150+ species | 🟡 In progress | 11-12: 36 → 78; 11-13: → 111; 11-14: → 142; 11-15: → 157 species ✅ target reached. 11-16: schema 0.7.0 adds `regulated[]` (7 regulated species); 25 species' leaf dimensions replaced with sourced values (botany 0.7.1). 11-15 schema alignment: zod is the single source of truth, JSON Schema is generated automatically, Python validates with jsonschema, checked in CI. Many numeric values are still estimates awaiting sources. 11-17 r2: epiphyte/moss audit blocked by the session network policy (botany#16 open) |
| 5 | Reduce infrastructure cost (remove NAT, trim Fargate) | 🟢 dev done | [infra#1](https://github.com/aquascape-studio/aquascape-infra/pull/1) merged (11-12, authorized by Xiaopu): dev has no NAT, no ALB, uses Spot, scheduled to scale to 0, about $5/month; prod untouched. dev only runs daily 01:30–04:00 PT |

## Visual style (chosen by Xiaopu on 2026-11-11)

- **Page style: Apple-website feel**. White background, generous whitespace, large bold headlines (tight letter-spacing), the product (the tank) is the absolute star, smooth scroll animations, frosted-glass nav and control bars, pill-shaped buttons. Overall precise, premium, restrained.
- **The tank itself: attractive but stylized, not photorealistic**. Like a high-quality game or Apple's own 3D illustrations: clean, soft, refined.
  - Leaves are rounded and have thickness, with soft light transmission via subsurface scattering; leaves tilt upward about 35°, and seen from the front they must not be thin lines like the side edge.
  - Palette: fresh greens (dark-to-light gradients), coral orange as the accent for red plants; rocks are smooth, sculptural shapes, no rough realistic textures.
  - Soft studio lighting, pure white translucent background.
  - **Frontal view only** (facing the tank straight on, like an aquascaping contest photo); no side view and no oblique 45° view.
  - Leaves must be dense and layered, never sparse, never like matchsticks.
- **Rejected, do not use again**: dark ink-green theme (ink-green), paper-white whitespace style (A), light water-blue clear style (B), graphite-gray instrument style (C), photorealism, side and oblique views. The ink-green-design-system skill in the plugin and app-agent's ink-green settings are deprecated.
- **Visual reference**: the "v3 · Ultra-white glass" page on the Design canvas (https://claude.ai/artifact/37R49gWPpKrP32C6ppm4Sa).
  - Design tokens extracted from it: background `#FFFFFF`, body text `#1D1D1F`, secondary text `#6E6E73`, card `#F5F5F7` (28px radius), primary `#0A6F78` (hover `#064C52`), healthy green `#1D7A3E`, warning coral `#B2441F`; fonts Inter (Noto Sans SC dropped 2026-11-17, app is English-only); H1 `clamp(44px,6.6vw,88px)` 700 letter-spacing -0.04em; frosted nav 52px `rgba(255,255,255,.78)` + `blur(20px) saturate(180%)`; pill control bar + 44px round play button + segmented jumps 10/35/60/90 days.
- **Launch rule**: visual changes to the home page and editor go behind a feature flag, off by default on deploy; ask Xiaopu to confirm from screenshots, and turn on in the next round once approved.

## Xiaopu's standing authorization (2026-11-12)
Xiaopu said: "Merge whatever should be merged, no need to wait for my approval, I'll look at the results in a week." (translated) So from 11-12 until they return (about 11-19):
- **Visual changes no longer wait for confirmation**: once done, self-tested, and the screenshots are in the iteration log, turn on the feature flag and ship directly; always keep a "revert flag PR" rollback path.
- **Pending PRs may be merged** (CI green, contract-compliant), including infra PRs already in effect on dev.
- **Still not done**: manually creating resources in the console/CLI, rotating or leaking secrets, exceeding budget, deleting repos, force-pushing the default branch. PRs that add AWS resources still state the cost clearly; deleting or replacing prod resources still requires the reason to be written in the log first, and should wait for their return if possible.
- Keep writing the iteration log + screenshots every day, so they can review everything in one pass on return.

## Interaction principles
The home page is the tank; canvas-first editor with a growth curve overlaid on the bottom timeline; ⌘K command palette for one-sentence scape generation; functional motion that respects prefers-reduced-motion; contrast 4.5:1, touch targets ≥44px.

## Feature flags

| Flag | Environment variable | Default | Status |
|------|----------|------|------|
| `growthTimelapse` | `NEXT_PUBLIC_FF_GROWTH_TIMELAPSE` | on (prod) | Visuals approved by Xiaopu on 2026-11-12, turned on in aquascape-web#3 |
| `stylizedTank` | `NEXT_PUBLIC_FF_STYLIZED_TANK` | on (prod) | Turned on without confirmation per Xiaopu, [web#6](https://github.com/aquascape-studio/aquascape-web/pull/6) (11-12); rollback = revert web#6 |
| `styleTemplates` | `NEXT_PUBLIC_FF_STYLE_TEMPLATES` | off | [web#9](https://github.com/aquascape-studio/aquascape-web/pull/9) (11-14). Turn on after Xiaopu has seen the screenshots |
| `dragEditor` | `NEXT_PUBLIC_FF_DRAG_EDITOR` | off | [web#12](https://github.com/aquascape-studio/aquascape-web/pull/12) (11-15), [web#15](https://github.com/aquascape-studio/aquascape-web/pull/15) (11-16 driftwood / resize / layout fixes), [web#22](https://github.com/aquascape-studio/aquascape-web/pull/22) (11-17 r2 epiphytes "Tie on"). Requires stylizedTank; recommended to confirm together with styleTemplates and turn both on together |
| `commandPalette` | `NEXT_PUBLIC_FF_COMMAND_PALETTE` | off | [web#25](https://github.com/aquascape-studio/aquascape-web/pull/25) (11-18 ⌘K one-sentence scape), [web#28](https://github.com/aquascape-studio/aquascape-web/pull/28) (11-19 conditions + phone sheet), [web#32](https://github.com/aquascape-studio/aquascape-web/pull/32) (11-20 low-tech template + CO2 pill fix). Requires dragEditor + stylizedTank. Approval for all three editor flags requested in [studio#23](https://github.com/aquascape-studio/aquascape-studio/issues/23) |

## Contracts (orchestrator-only)
- `aquascape-sim/ts/src/contract.ts` — `@aquascape/sim-core` v1.2.0 (1.2.0: LIGHT_UPPER_FLOOR 0.75 above light.max, LIGHT_RESPONSE_INVARIANTS; SimInput / Frame / SimResult / Worker protocol; + parDailyMean, substrate grid, cancelled, event and envelope rules)
- `aquascape-render/src/contract.ts` — FrontalTank v1.4.1 (1.4.1: hardscapeExtent, real-extent clampHardscapeXZ, resolveLeanSide, surface y from the piece seat height, buried column → null; 1.4: hardscapeSurfaceY, hardscapeAnchors, attached epiphytes, leanSide; 1.1: rock roles, buriedFrac, guides, focalPoint, bed; 1.2: editable / selected / onSelect / onMove / footprintContains; 1.3: woodStyle, per-piece geometry cache, clampHardscapeXZ)
- `aquascape-web/lib/templates/contract.ts` — style templates v1.2.0 (1.2: required nature-lowtech, Low light / no CO2 species rules; 1.1: no regulated species, root driftwood, epiphyte groups, attach; 1.0.2: English-only names; ScapeTemplate / instantiate; + iwagumi composition, count scaled by tank area)
- `aquascape-web/lib/compose/contract.ts` — one-sentence scape v1.2.0 (1.2: stated no-CO2 picks nature-lowtech; 1.1: composed docs carry conditions, filters follow resolved conditions, mismatch note; ScapeIntent / ComposeResult, tank presets, substitution rules, ⌘K UI)
- `aquascape-web/lib/editor/contract.ts` — drag editor v1.3.0 (1.3.0: ScapeDoc.conditions, setConditions, effectiveConditions; 1.2.2: frozen lean side, canAttach in migration, 12 anchors per host; 1.2: attach map, attachPlant / detachPlant / setLeanSide; 1.2.1: on-host = has a surface; ScapeDoc / EditOp / reducer / encodeScape; 1.1: resize / addHardscape / setWoodStyle / previewBase / needsResim)
- `aquascape-botany/js/schema.ts` (zod, single source of truth, schema 0.8.0) → generates `schema/plant.schema.json`; includes optional `visual` block, optional `regulated[]`

## Current visual decisions (11-12 r2)
- 3D tank: instanced rounded leaves with thickness + shader back-light faking translucency (no transmission, for performance); pure white studio + soft light; frontal camera only; smooth pebbles; sloped substrate bed; red plants use botany `tipColorHex`, falling back to coral orange #E8735A when absent.

## Current visual decisions (11-14)
- Rocks: shaped by Iwagumi role (main stone tallest and leaning slightly forward, secondary/companion stones progressively lower and wider, scatter stones flat), smooth sculptural look, buried in the substrate bed.
- Stem plants: opposite / whorled / alternate leaf arrangement, leaves tilted up about 35°, tips closing into a bud, with side branches (removes the "fishbone" look).
- Guides: 1px #1D1D1F at 35% opacity + focal circle, overlaid in screen space only.
- Template bar: frosted pill bar + segmented control, primary #0A6F78.
- Fonts: Inter (bundled via @fontsource), so the build needs no network. (The system Chinese font stack was dropped on 2026-11-17 when the app became English-only.)

## Current visual decisions (11-15)
- Selection: a soft arc of primary #0A6F78 (60%) on the substrate bed, drawn only as the front half (in the frontal view the back half is hidden by the plant carpet); lifts slightly by 0.4 cm while dragging; no gizmos such as arrows or axes.
- Edit toolbar: frosted pill (undo / redo / delete / copy share link), below it a row of "Select" rock chips and an "Add" plant chips row, touch targets ≥44px.

## Current visual decisions (11-16)
- Driftwood: rounded, tapering tubes with soft transitions at forks and round-tipped twig ends; no bark texture and no hard creases; warm brown gradient #7A5A43 (base) → #A88466 (twig tips), low gloss; buried only 5%. Three styles: branchy (upward-fanning crown), root (30–45° leaning trunk + root plate), log (low and lying horizontal, serving as a platform for epiphytes).
- Resize: −/+ pill buttons on the toolbar + keyboard, no 3D handles. Driftwood style uses the segmented control "Branchy / Root / Log" .
- Regulated species: shown only as "Regulated in some regions"  in the add chip's hint and screen-reader text, with no prominent warning color.

## Current visual decisions (11-17 r2)
- Epiphytes on hardscape: moss is a soft, dense cushion of tiny leaves that follows the branch it sits on; Anubias / Bucephalandra / Java fern sit on a short rhizome lying on the surface, leaves fanning up and towards the viewer (~35°). Nothing passes through the wood.
- Root driftwood leans towards the tank centre by default (30–45°); twig tips are never thinner than 0.35 cm (no matchsticks).
- Editor: "Tie on" chip row (mosses first, 8 visible + "More"), "Untie" for a tied plant, segmented "Lean: Auto / Left / Right" for root wood; same frosted pill styling as the other rows.

## Current visual decisions (11-18)
- ⌘K dialog: centred frosted card (rgba(255,255,255,.78) + blur 20px), 28 px radius, large input with a small teal sparkle, understood phrases as teal-tinted pills, one grey summary line, primary pill "Build scape ↵". Full-width bottom sheet on phones. After Enter, a frosted notes line above the timeline with "Undo" (neutral pill) and "Try another" (primary pill) for 6 s. Toolbar pill "Ctrl K / ⌘K Describe a scape".

## Current visual decisions (11-19)
- Conditions row: frosted pill row under the edit toolbar — label "Conditions", segmented "Light: Low / Medium / High", a primary teal "CO2" toggle pill with a dot when on. ControlBar shows the effective conditions in grey ("Low light · No CO2").
- Phones (< 640 px): Guides is its own centred row under the template bar; the ⌘K sheet is opaque white so nothing behind it shows through; the compose summary wraps.

## Current visual decisions (11-20)
- "Nature · Low tech" template: one centred root driftwood dressed with moss / Java fern, Anubias nana and Bucephalandra on two small stones, crypt bands either side, Vallisneria / Hygrophila back wall, Marsilea carpet. Same stylized frontal look; dense and layered at day 45.
- Phones: the Conditions row steps out to the page's 16 px gutter with tighter segments so the CO2 pill fits at 360 px.

## Deployment verification (11-17 r2)
The deploy job now verifies itself ([web#21](https://github.com/aquascape-studio/aquascape-web/pull/21)): ECS stable, running the new image, and `/`, `/studio/sim`, `/plants` return 200 on https://dev.efferves.live (skipped when dev is scaled to 0 outside its daily window). The orchestrator's sandbox cannot reach efferves.live directly.

## Notes on executing the standing authorization (11-16)
The ROADMAP's "visual changes no longer wait for confirmation" conflicts with the daily scheduled-task prompt (visual changes wait for confirmation). Since 11-14 the practice has been: **keep the flag off and wait for Xiaopu to reply "OK to enable" in the issue**. The scheduled-task prompt takes precedence until Xiaopu says otherwise.
