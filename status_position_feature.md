# Positions & Formations — Status

Status: **Built on branch `feature/positions-formations`, committed locally, not pushed.**
User is testing further; will push to `main` (and let GitHub Pages redeploy) when ready.

## What shipped

- **Setup flow order**: Format & Formation → Team Members → Assign Positions → Game Settings.
- **Format & Formation** are horizontal button rows (not dropdowns) — 5v5/7v7/9v9/11v11,
  each with its own set of named formations. 5v5 offers 2-2, 1-2-1, 2-1-1 (1-1-2 dropped,
  "(Diamond)" label shortened to just "1-2-1"). 7v7/9v9/11v11 unchanged from the original
  plan (3 formations each, e.g. 4-4-2, 4-3-3, 3-5-2 for 11v11).
- **Assign Positions**: one `<select>` per slot (GK + formation slots), mutual exclusion
  recomputed live, read-only Bench preview, Start Game disabled until every slot is filled.
- **`positions` map** (`{ name: slotLabel }`) + `activeFormationSlots` threaded through every
  mutation point: rotation subs, manual swap, GK swap (both directions), add/remove extra
  player, Match Reset, Full Reset. Lineup renders `Name (pos: LABEL)`.
- **Swap Positions mode**: tap two on-pitch players to swap their labels, independent of
  subs/bench.
- **Match Reset modal**: each on-pitch row gets a position `<select>` for reshuffling at a
  break.
- **Substitution confirmation modal** (added after initial build, not in the original plan):
  - Leaving/entering rows show `(pos: LABEL)`.
  - When 2+ players are subbing on at once, each "COMING ON" row gets a `<select>` of the
    vacated labels. Picking a label someone else already holds **swaps** the two players'
    picks rather than hiding the option — the naive mutual-exclusion approach (same pattern
    used in Setup/Match Reset) deadlocks for exactly-2-player swaps since each row would hide
    the other's label. The swap-on-conflict fix generalizes to any number of simultaneous
    subs.

## Verification

Headless Chromium via Playwright, driving `index.html` directly over `file://`. Covered:
setup flow (slot exclusion, bench preview, Start Game gating), lineup rendering, rotation
inheritance, Swap Positions, GK swap, Match Reset reassignment, Add/Remove Extra Player
(confirmed no stray `positions` entries), and the multi-sub reassignment swap (including the
deadlock case). All passing as of last run.

## Not yet done

- Not pushed to `origin/main` — user wants to test locally first.
- No drag-and-drop pitch view (this was always scoped as the stepping stone before that).
