---
name: aftereffect-remove
description: Set up a clean macOS/Windows computer and teach removal of a wall-mounted object (sign, window, plate) from moving footage with Mocha AE planar tracking, independent OpenCV checks, local ECC refinement, a one-frame clean plate and a Python-generated After Effects project. Use in this repository for a student's shot; no AE MCP.
---

# Aftereffect Remove

This is a repository-bound teaching skill. Run from the repository root that
contains `scripts/setup_environment.py`. Rules live in
[rules](../../../docs/rules.md); lesson order in [workflow](../../../docs/workflow.md).

## First interaction

1. Inspect the actual OS/shell and repository root. If Python or tools are
   missing, start with [START-HERE](../../../START-HERE.md) and read only the matching
   setup: [macOS](../../../docs/setup-macos.md) or [Windows](../../../docs/setup-windows.md).
2. Run `setup_environment.py --check`. Install only what is missing from the
   official sources in the setup document. After Effects (with bundled Mocha AE)
   is installed, signed in and licensed by the student.
3. Locate the student's clip and create `work/<shot>`. Keep the original intact.
   A first clip: short, constant frame rate, target on one flat textured wall.
4. Explain the next step in two or three sentences, then do only that step.

## Non-negotiable rules

- Planar tracking starts with Mocha AE. OpenCV is for independent checks and
  local refinement of Mocha, not a replacement tracker.
- Write `contract.json` (visible frames, removal region, margin, gate) before
  tracking. Do not change the gate or shrink the region after seeing results.
- Score with source pixels never used for fitting. Keep every visible frame in the
  denominator; unmeasurable frames are UNKNOWN, not zero. Report pooled and
  worst-frame p95/max.
- Do not use Mocha keys after a stop or "center unavailable" warning. End the
  patch when the target leaves the frame; check any re-entry separately.
- Refinement is per-frame ECC homography against the reference frame on the
  same wall around the target, initialized from Mocha. Check that fitting
  support surrounds the target. No temporal smoothing, frozen off-screen
  corners, whole-plate warps or extra feather/blur to hide error.
- Generate the clean plate once from the reference frame, register it with
  SIFT/RANSAC, color-match on unchanged wall and use only the removal region.
- Pixels outside the patch alpha must equal the source. Do not apply Corner Pin
  again to an already tracked full-canvas RGBA patch.
- Build the AE project from Python-generated JSX run by the student through
  File > Scripts > Run Script File. No MCP, persistent bridge, overwrite of open
  projects, automatic save or render.
- A result that misses the gate is a review render. Do not make a breakdown or
  public video from it.

## Context and long work

Summarize large arrays with Python and read short JSON reports. Run long
computations with the agent's own background facility, one job at a time, and
check results instead of polling logs. After each lesson write
`work/<shot>/next.md`: files, what was verified, visible problems, one next step.

## Growing the skill

Move a step into `scripts/` only after it worked on a real shot. Keep shot
values (margins, thresholds, frame ranges) in `contract.json`, not in code.
Add new failure patterns and fixes to `docs/rules.md` with the shot they came from.
