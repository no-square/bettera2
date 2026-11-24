# BA2 Roadmap

Updated: 2026-11-24

Current milestone: **v0.0.5-beta.1 local test build**. A local portable package has been built; no release archive or publication was created by this rebuild.

## Status guide

- **Included in the local test build:** source and packaged components have been checked; this is not full gameplay certification.
- **In progress:** implemented work still needs controlled in-game acceptance or tuning.
- **Later evaluation:** a possible follow-up, not a committed feature for this release.
- **Removed / out of scope:** not included in the current implementation plan.

## Included in the local v0.0.5-beta.1 test build

### Ground obstacle recovery

- [x] Track progress toward the commanded waypoint; back-and-forth jitter does not reset the stall timer.
- [x] Remember failed directions locally for a bounded time and try successive detour legs.
- [x] Bound recovery attempts and preserve the selected target while trying a usable approach.
- [x] Rotate collision surveys across actor pages and streaming levels; retain recent observations between passes.
- [x] Use transformed supported box-collision footprints with capsule clearance and height filtering.
- [x] Retain approximate fallback for unsupported geometry and shallow-water handling.

**Status:** included in the test package. This is bounded local recovery, not a complete navigation mesh or a guarantee that every obstacle is passable.

### Hybrid zonal F9 / MOBFARM

- [x] Keep one logical F9 session through gathering and FP-recovery farming.
- [x] At low FP, finish the current gathering attempt, land and confirm Ground before starting MOBFARM in the shared circle.
- [x] Keep MOBFARM active until **Resume at FP**, not **FP required to take off**.
- [x] Carry F9's latched recovery request into the hybrid controller when the exact low-FP airborne sample was missed.
- [x] Enter hybrid recovery when grounded below the takeoff requirement instead of leaving ordinary F9 idle.
- [x] Finish existing combat at Resume FP; do not start new ordinary targets during that handoff.
- [x] Return to farming if FP drops below Resume while finishing combat.
- [x] Release combat input before gathering resumes; inactive F9 standby/batch handling must not disturb the farming owner's input.
- [x] Support repeated recovery cycles without restarting F9.
- [x] Preserve shared-circle/material filters, stop behavior and cancellation on unsafe context.

**Observed constraint:** skill use pauses FP regeneration. Recovery can therefore be slow and uneven; no fixed recovery rate or efficiency gain is promised. The controller waits for observed Resume FP rather than assuming that combat restores FP at the idle rate.

**Validation:** 66 native hybrid checks, three settings checks and the settings-view check pass. Three repeated cycles are covered offline. The rebuilt package contains the corrected native module; all 581 runtime component hashes were verified. These results do not replace live acceptance.

### Build Advisor removal

- [x] Remove the workspace, recommendations, comparison modules, dedicated reference catalogs/importers and optional Dwell lookup.
- [x] Keep normal Character readings, including observed equipment, skills and progression data.
- [x] Verify that removed frontend modules and the compiled Dwell backend are absent from the test package.

**Reason:** the available build references do not provide enough verified Global-specific evidence. Korean builds can use different items/cards/Arcana; combining several databases can introduce conflicting recommendations. Some API integrations also require registered applications and credentials.

## In progress — live acceptance and tuning

### Priority 1: repeat the complete hybrid cycle

- [ ] Test F9 -> low FP -> confirmed landing -> MOBFARM -> Resume FP -> finish combat -> F9 for several uninterrupted cycles.
- [ ] Confirm minimum takeoff FP never ends the recovery-farming phase early.
- [ ] Verify skill-interrupted regeneration, no-mob recovery, new attackers and FP drops during combat completion.
- [ ] Verify batch pauses and material filters inside the shared circle.
- [ ] Check explicit stop, manual input, focus/menu loss, death, unknown FP/player data and world/character changes.
- [ ] Confirm input is never sent concurrently by gathering and MOBFARM.

### Priority 2: finish ground-navigation acceptance

- [ ] Reproduce walls, corners, narrow passages, steps and height changes with F9 and MOBFARM.
- [ ] Verify waypoint progress, target retention, bounded detours and eventual target deferral.
- [ ] Check shallow water, transformed meshes, doorways, overhead objects and unsupported-geometry fallbacks.
- [ ] Measure survey coverage latency and rebuild cost in populated/streaming areas.
- [ ] Tune from recorded failures; passing offline tests does not establish universal pathfinding reliability.

### Priority 3: prepare distribution after acceptance

- [ ] Finish controlled gameplay testing and record remaining known limitations.
- [ ] Refresh the English changelog and usage guidance to match tested behavior.
- [ ] Preserve local settings/history/routes and validate update installation.
- [ ] Create the release archive and publish only after a separate explicit request.
- [ ] Exclude local profiles, captures, logs and personal settings from distributed assets.

## Later evaluation — not committed to this release

- Verified passage networks combined with local final-approach recovery.
- Additional collision primitive/component coverage and measured terrain/step constraints.
- Usable client navigation-mesh data, if availability and correctness can be established.

These are alternatives to evaluate after the current controller has been tested, not already implemented features or guaranteed engine navigation calls.

## Removed or outside the current plan

- **Build Advisor:** no active implementation or automatic return is planned. Verified Global build references and useful online/offline tools are welcome; any future revival needs a separate scope and data-quality review.
- **Hybrid recorded-route playback:** excluded from the first zonal F9 implementation; no delivery date is assigned.
- **A complete new terrain pathfinder:** not part of the present local obstacle-recovery work.
- **Underwater gathering:** not included in v0.0.5-beta.1.
- **Full automatic quest leveling / SL:** not part of this BA2 release roadmap.

BE 3.2 has its own roadmap. A feature or fix in one product is not automatically implemented or released in the other.
