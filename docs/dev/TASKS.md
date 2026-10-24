# Implementation tasks

All work is planned and unchecked. [PLAN](PLAN.md) owns phase/milestone boundaries and phase-specific specification baselines; [layout](layout.md) owns paths. [SPEC](SPEC.md) owns current behavior. Each delivery task includes relevant tests, fixes and documentation; review tasks add independent code review, boundary verification and reports. No task is selected for execution by this list alone.

## Phase 1 — Playable classic game

- [ ] Phase 1 — Playable classic game
    - [ ] Milestone 1.1 — Playable falling-piece slice
        - [ ] T-001 — Establish isolated toolchain and browser capability
            Scope: package/tooling configuration, dedicated browser environment and reproducible setup documentation. Depends on: none; Phase 1 entry gates apply.
            Contract: SPEC C1–C6 under this phase’s recorded baseline; PLAN Phase 1 and layout ownership. Evidence: Pin TypeScript/Vite, unit runner and Playwright dependencies/lockfile; declare scripts after validating them. Follow central browser policy for isolated Chromium/Firefox provisioning, proactive diagnostics, repeated contexts/pages, rendering and teardown. Record versions, commands and limits before dependent browser work.
        - [ ] T-002 — Implement geometry and deterministic piece source
            Scope: src/game/types.ts, geometry.ts, source.ts and focused unit fixtures. Depends on: T-001.
            Contract: SPEC C1–C6 under this phase’s recorded baseline; PLAN Phase 1 and layout ownership. Evidence: Verify seven four-cell shapes, orientation transforms, bounds/collision, random interval boundaries and unchanged rejected candidates.
        - [ ] T-003 — Connect a minimal running game surface
            Scope: src/game/session.ts, src/browser/, src/ui/, src/main.ts and index.html. Depends on: T-002.
            Contract: SPEC C1–C6 under this phase’s recorded baseline; PLAN Phase 1 and layout ownership. Evidence: Render and keyboard-control one falling piece; fake-time gravity and actual browser input prove frame/controller integration without duplicated loops.
        - [ ] T-004 — Review, test and report milestone 1.1
            Depends on: T-001, T-002, T-003. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/1/1.1.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 1.2 — Completed game sessions
        - [ ] T-005 — Implement atomic settlement and row clearing
            Scope: src/game/board.ts and board fixtures. Depends on: T-004.
            Contract: SPEC C1–C6 under this phase’s recorded baseline; PLAN Phase 1 and layout ownership. Evidence: Verify legal merge, simultaneous 1–4 row clears, ordered compaction and no partial writes.
        - [ ] T-006 — Integrate score and session lifecycle
            Scope: src/game/session.ts and transition tests. Depends on: T-005.
            Contract: SPEC C1–C6 under this phase’s recorded baseline; PLAN Phase 1 and layout ownership. Evidence: Apply fixed 1000 ms gravity, failed-down settlement, 0/100/300/500/800 clear scores, replacement spawn and blocked-spawn game over exactly once.
        - [ ] T-007 — Deliver restart and visible session status
            Scope: src/ui/status.ts, controller/input adapters and browser checks. Depends on: T-006.
            Contract: SPEC C1–C6 under this phase’s recorded baseline; PLAN Phase 1 and layout ownership. Evidence: Show score/game over/instructions and labeled Restart; R/button reset all session timing and held input. Repeated restarts keep one loop and teardown releases resources.
        - [ ] T-008 — Review, test and report milestone 1.2
            Depends on: T-005, T-006, T-007. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/1/1.2.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 1.3 — Static browser delivery
        - [ ] T-009 — Verify focused keyboard and rendering behavior
            Scope: tests/browser/, input/controller and UI fixes within accepted scope. Depends on: T-008.
            Contract: SPEC C1–C6 under this phase’s recorded baseline; PLAN Phase 1 and layout ownership. Evidence: Exercise focused keys versus editable controls, wall/stack rejection, visible cells and a 900×700 layout; inspect Canvas screenshots/pixels in Chromium and Firefox.
        - [ ] T-010 — Exercise complete deterministic browser sessions
            Scope: tests/browser/, deterministic fixtures and owning game modules. Depends on: T-009.
            Contract: SPEC C1–C6 under this phase’s recorded baseline; PLAN Phase 1 and layout ownership. Evidence: Verify clear/scoring, blocked spawn, restart, blur handling and two isolated lifecycle cycles through the actual server path; repair contract failures.
        - [ ] T-011 — Prepare reproducible static delivery
            Scope: build/CI configuration, README and relevant module documentation. Depends on: T-010.
            Contract: SPEC C1–C6 under this phase’s recorded baseline; PLAN Phase 1 and layout ownership. Evidence: Validate typecheck, unit tests, both browser suites and production build; document actual setup/controls and limitations. Test output/assets remain ignored.
        - [ ] T-012 — Review, test and report milestone 1.3
            Depends on: T-009, T-010, T-011. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/1/1.3.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 1.4 — Phase review
        - [ ] T-013 — Review, test and report phase 1
            Depends on: milestone 1.1 completed/closed, milestone 1.2 completed/closed, milestone 1.3 completed/closed. Review cross-milestone code, run phase exits/regressions, repair blockers, aggregate TODOs and update HANDOFF. Report: docs/dev/reports/phases/1/PHASE-REPORT.md. Publish before review issue/milestone closure; integrate only the complete verified phase. Release/version preparation follows the authorized release gate.

## Phase 2 — Bag, kicks and Hold

- [ ] Phase 2 — Bag, kicks and Hold
    - [ ] Milestone 2.1 — Reliable piece controls
        - [ ] T-014 — Implement deterministic seven-piece bags
            Scope: src/game/source.ts and source fixtures. Depends on: Phase 1 completion, closure and published integration.
            Contract: SPEC C1–C7; C8 delay is deferred to Phase 3 under this phase’s recorded baseline; PLAN Phase 2 and layout ownership. Evidence: Verify permutation, refill, bag-boundary repetition and seeded shuffle consumption.
        - [ ] T-015 — Implement ordered rotation kicks
            Scope: src/game/geometry.ts and geometry fixtures. Depends on: T-014.
            Contract: SPEC C1–C7; C8 delay is deferred to Phase 3 under this phase’s recorded baseline; PLAN Phase 2 and layout ownership. Evidence: Verify ordered legal candidates for all shapes, wall/floor/stack rejection and O invariance; retain earlier movement behavior.
        - [ ] T-016 — Integrate source and kicks into play
            Scope: session/controller and browser regressions. Depends on: T-015.
            Contract: SPEC C1–C7; C8 delay is deferred to Phase 3 under this phase’s recorded baseline; PLAN Phase 2 and layout ownership. Evidence: Run classic sessions with bag/source changes and visible wall kicks; preserve scoring, restart and the current failed-down phase baseline before Phase 3 timing.
        - [ ] T-017 — Review, test and report milestone 2.1
            Depends on: T-014, T-015, T-016. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/2/2.1.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 2.2 — Held-piece control
        - [ ] T-018 — Implement Hold state and transitions
            Scope: src/game/session.ts and state tests. Depends on: T-017.
            Contract: SPEC C1–C7; C8 delay is deferred to Phase 3 under this phase’s recorded baseline; PLAN Phase 2 and layout ownership. Evidence: Cover empty-store draw, no-draw swap, canonical replacement, once-per-lock gate and blocked-spawn game over.
        - [ ] T-019 — Expose Hold control and display
            Scope: input adapter, status view and browser checks. Depends on: T-018.
            Contract: SPEC C1–C7; C8 delay is deferred to Phase 3 under this phase’s recorded baseline; PLAN Phase 2 and layout ownership. Evidence: C activates once per physical press; labeled held shape matches stored state; game-over input remains guarded.
        - [ ] T-020 — Verify Hold interactions and delivery docs
            Scope: unit/browser regressions and README. Depends on: T-019.
            Contract: SPEC C1–C7; C8 delay is deferred to Phase 3 under this phase’s recorded baseline; PLAN Phase 2 and layout ownership. Evidence: Cover restart/source consumption and listener lifecycle while retaining the playable baseline; document actual controls.
        - [ ] T-021 — Review, test and report milestone 2.2
            Depends on: T-018, T-019, T-020. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/2/2.2.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 2.3 — Phase review
        - [ ] T-022 — Review, test and report phase 2
            Depends on: milestone 2.1 completed/closed, milestone 2.2 completed/closed. Review cross-milestone code, run phase exits/regressions, repair blockers, aggregate TODOs and update HANDOFF. Report: docs/dev/reports/phases/2/PHASE-REPORT.md. Publish before review issue/milestone closure; integrate only the complete verified phase. Release/version preparation follows the authorized release gate.

## Phase 3 — Landing assistance and delayed lock

- [ ] Phase 3 — Landing assistance and delayed lock
    - [ ] Milestone 3.1 — Consistent landing assistance
        - [ ] T-023 — Define the ordinary landing query
            Scope: src/game/geometry.ts and projection fixtures. Depends on: Phase 2 completion, closure and published integration.
            Contract: SPEC C8 plus regression of C1–C7 under this phase’s recorded baseline; PLAN Phase 3 and layout ownership. Evidence: Verify floor, stacks, overhangs and zero-distance landings; projection leaves state unchanged.
        - [ ] T-024 — Render ghost and wire hard drop
            Scope: src/ui/renderer.ts, input/session and browser checks. Depends on: T-023.
            Contract: SPEC C8 plus regression of C1–C7 under this phase’s recorded baseline; PLAN Phase 3 and layout ownership. Evidence: Ghost outline matches projected cells; Space moves to the same row without adding drop score; repeat is filtered.
        - [ ] T-025 — Verify assistance against classic controls
            Scope: unit/browser interaction checks and docs. Depends on: T-024.
            Contract: SPEC C8 plus regression of C1–C7 under this phase’s recorded baseline; PLAN Phase 3 and layout ownership. Evidence: Cover Hold replacement, blocked spawn, restart, visible ghost and hard-drop landing equality in both targets.
        - [ ] T-026 — Review, test and report milestone 3.1
            Depends on: T-023, T-024, T-025. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/3/3.1.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 3.2 — Grounded interval locking
        - [ ] T-027 — Implement grounded timer transitions
            Scope: src/game/session.ts and fake-time tests. Depends on: T-026.
            Contract: SPEC C8 plus regression of C1–C7 under this phase’s recorded baseline; PLAN Phase 3 and layout ownership. Evidence: Capture grounding, preserve supported-move deadline, cancel on ungrounding and settle exactly once at expiry.
        - [ ] T-028 — Integrate delayed locking into controller actions
            Scope: controller/session and interaction fixtures. Depends on: T-027.
            Contract: SPEC C8 plus regression of C1–C7 under this phase’s recorded baseline; PLAN Phase 3 and layout ownership. Evidence: Failed down and hard drop retain active state; restart/Hold/blur handle timing and equal-time ordering correctly.
        - [ ] T-029 — Validate delay boundaries and playable delivery
            Scope: unit/browser suites and README. Depends on: T-028.
            Contract: SPEC C8 plus regression of C1–C7 under this phase’s recorded baseline; PLAN Phase 3 and layout ownership. Evidence: Exercise 999/1000 ms, repeated drop, shift/rotation, leave/re-ground and full sessions on both browsers; document timing.
        - [ ] T-030 — Review, test and report milestone 3.2
            Depends on: T-027, T-028, T-029. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/3/3.2.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 3.3 — Phase review
        - [ ] T-031 — Review, test and report phase 3
            Depends on: milestone 3.1 completed/closed, milestone 3.2 completed/closed. Review cross-milestone code, run phase exits/regressions, repair blockers, aggregate TODOs and update HANDOFF. Report: docs/dev/reports/phases/3/PHASE-REPORT.md. Publish before review issue/milestone closure; integrate only the complete verified phase. Release/version preparation follows the authorized release gate.

## Phase 4 — Conditional ghost display

- [ ] Phase 4 — Conditional ghost display
    - [ ] Milestone 4.1 — Midline visibility
        - [ ] T-032 — Implement occupied-cell midline visibility
            Scope: src/ui/ghost.ts, renderer and unit fixtures. Depends on: Phase 3 completion, closure and published integration.
            Contract: SPEC C9 and C8 projection regression under this phase’s recorded baseline; PLAN Phase 4 and layout ownership. Evidence: Verify row-9/10 boundary and all shape orientations using occupied cells; no pixel-coordinate dependence.
        - [ ] T-033 — Verify visibility across gameplay transitions
            Scope: browser/unit fixtures, renderer and README. Depends on: T-032.
            Contract: SPEC C9 and C8 projection regression under this phase’s recorded baseline; PLAN Phase 4 and layout ownership. Evidence: Cover move/rotation/Hold/spawn/restart, hidden hard drop, resize and game over in both browsers; inspect actual ghost raster output.
        - [ ] T-034 — Review, test and report milestone 4.1
            Depends on: T-032, T-033. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/4/4.1.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 4.2 — Phase review
        - [ ] T-035 — Review, test and report phase 4
            Depends on: milestone 4.1 completed/closed. Review cross-milestone code, run phase exits/regressions, repair blockers, aggregate TODOs and update HANDOFF. Report: docs/dev/reports/phases/4/PHASE-REPORT.md. Publish before review issue/milestone closure; integrate only the complete verified phase. Release/version preparation follows the authorized release gate.

## Phase 5 — Penetrator bonuses

- [ ] Phase 5 — Penetrator bonuses
    - [ ] Milestone 5.1 — Legal penetrator traversal
        - [ ] T-036 — Implement mode-aware landing and transforms
            Scope: src/game/geometry.ts, types and cavity fixtures. Depends on: Phase 4 completion, closure and published integration.
            Contract: SPEC C10–C11 plus C7–C9 interaction regression under this phase’s recorded baseline; PLAN Phase 5 and layout ownership. Evidence: Cover separated legal cavities, bounds, rotations, no-landing rejection and unchanged ordinary projection.
        - [ ] T-037 — Integrate penetrator descent and safe merge
            Scope: src/game/session.ts, board merge and unit tests. Depends on: T-036.
            Contract: SPEC C10–C11 plus C7–C9 interaction regression under this phase’s recorded baseline; PLAN Phase 5 and layout ownership. Evidence: Advance through overlap toward deepest landing; forbid intermediate lock and partial/overlapping final writes.
        - [ ] T-038 — Verify penetrator landing and lock timing
            Scope: geometry/session integration fixtures. Depends on: T-037.
            Contract: SPEC C10–C11 plus C7–C9 interaction regression under this phase’s recorded baseline; PLAN Phase 5 and layout ownership. Evidence: Ghost/hard-drop use the same solver, full-interval grounding occurs only at legal target, and transforms update/cancel timers correctly.
        - [ ] T-039 — Review, test and report milestone 5.1
            Depends on: T-036, T-037, T-038. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/5/5.1.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 5.2 — Bonus and active-mode lifecycle
        - [ ] T-040 — Implement activation and bounded bonus awards
            Scope: session/types and lifecycle fixtures. Depends on: T-039.
            Contract: SPEC C10–C11 plus C7–C9 interaction regression under this phase’s recorded baseline; PLAN Phase 5 and layout ownership. Evidence: Cover zero/one/three bonus states, repeat activation, no-landing no-spend and exact four-line award/cap.
        - [ ] T-041 — Integrate active-mode Hold and restart rules
            Scope: session/source integration and focused checks. Depends on: T-040.
            Contract: SPEC C10–C11 plus C7–C9 interaction regression under this phase’s recorded baseline; PLAN Phase 5 and layout ownership. Evidence: Ignored active Hold does not draw/swap; lock spawns ordinary; restart clears mode/count; earning after spending is atomic.
        - [ ] T-042 — Expose activation, white pieces and bonus indicator
            Scope: input/status/renderer and browser checks. Depends on: T-041.
            Contract: SPEC C10–C11 plus C7–C9 interaction regression under this phase’s recorded baseline; PLAN Phase 5 and layout ownership. Evidence: P activates once per press; all active penetrator cells/ghost are white; locked white cells remain blocking; symbol count updates on earn/spend.
        - [ ] T-043 — Review, test and report milestone 5.2
            Depends on: T-040, T-041, T-042. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/5/5.2.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 5.3 — Integrated penetrator gameplay
        - [ ] T-044 — Exercise penetrator gameplay in both browsers
            Scope: tests/browser/ and deterministic cavity fixtures. Depends on: T-043.
            Contract: SPEC C10–C11 plus C7–C9 interaction regression under this phase’s recorded baseline; PLAN Phase 5 and layout ownership. Evidence: Inspect crossing shelf, legal lock, dynamic ghost, white output, ignored Hold and indicator updates through actual application events.
        - [ ] T-045 — Run ordinary and penetrator regression scenarios
            Scope: unit/browser suites and owning module fixes. Depends on: T-044.
            Contract: SPEC C10–C11 plus C7–C9 interaction regression under this phase’s recorded baseline; PLAN Phase 5 and layout ownership. Evidence: Cover restart/game over, boundaries, no bonus, cap, bag/Hold, ordinary wall kicks and non-destructive traversal; repair violations.
        - [ ] T-046 — Document and package bonus gameplay
            Scope: README, module docs and build validation. Depends on: T-045.
            Contract: SPEC C10–C11 plus C7–C9 interaction regression under this phase’s recorded baseline; PLAN Phase 5 and layout ownership. Evidence: Describe activation/award/cap/Hold restriction and validate the reproducible static build with both-browser evidence.
        - [ ] T-047 — Review, test and report milestone 5.3
            Depends on: T-044, T-045, T-046. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/5/5.3.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 5.4 — Phase review
        - [ ] T-048 — Review, test and report phase 5
            Depends on: milestone 5.1 completed/closed, milestone 5.2 completed/closed, milestone 5.3 completed/closed. Review cross-milestone code, run phase exits/regressions, repair blockers, aggregate TODOs and update HANDOFF. Report: docs/dev/reports/phases/5/PHASE-REPORT.md. Publish before review issue/milestone closure; integrate only the complete verified phase. Release/version preparation follows the authorized release gate.

## Phase 6 — Session configuration

- [ ] Phase 6 — Session configuration
    - [ ] Milestone 6.1 — Settings controls and stable preview
        - [ ] T-049 — Define typed settings and validated defaults
            Scope: src/game/settings.ts, types and unit tests. Depends on: Phase 5 completion, closure and published integration.
            Contract: SPEC C12 and C7–C11 settings interactions under this phase’s recorded baseline; PLAN Phase 6 and layout ownership. Evidence: Cover all six value domains, defaults, invalid/same-value rejection and session-owned values.
        - [ ] T-050 — Expose stable next-piece peek
            Scope: source/session/status and sequence fixtures. Depends on: T-049.
            Contract: SPEC C12 and C7–C11 settings interactions under this phase’s recorded baseline; PLAN Phase 6 and layout ownership. Evidence: Preview one upcoming shape without consuming draws; cover bag boundaries, Hold draws/swaps and repeated visibility toggles.
        - [ ] T-051 — Build the side-panel settings controls
            Scope: src/ui/settings.ts, styles and browser checks. Depends on: T-050.
            Contract: SPEC C12 and C7–C11 settings interactions under this phase’s recorded baseline; PLAN Phase 6 and layout ownership. Evidence: Label each control, reflect session values, fit desktop layout and preserve keyboard focus ownership; no storage/query writes.
        - [ ] T-052 — Review, test and report milestone 6.1
            Depends on: T-049, T-050, T-051. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/6/6.1.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 6.2 — Live behavior and timing
        - [ ] T-053 — Integrate enablement and ghost modes
            Scope: input/session/ghost/status and unit/browser tests. Depends on: T-052.
            Contract: SPEC C12 and C7–C11 settings interactions under this phase’s recorded baseline; PLAN Phase 6 and layout ownership. Evidence: Hard-drop/Hold toggles gate commands, stored Hold persists and penetrator Hold remains disabled; preview/ghost toggles do not change projection or source.
        - [ ] T-054 — Implement selected lock policies and transitions
            Scope: session/controller and exact-time fixtures. Depends on: T-053.
            Contract: SPEC C12 and C7–C11 settings interactions under this phase’s recorded baseline; PLAN Phase 6 and layout ownership. Evidence: Cover general failed-down/interval × follow/immediate drop, actual versus same-value changes, grounded/airborne/penetrator state and no retroactive drop lock.
        - [ ] T-055 — Validate session settings across complete games
            Scope: browser suites, README and static build. Depends on: T-054.
            Contract: SPEC C12 and C7–C11 settings interactions under this phase’s recorded baseline; PLAN Phase 6 and layout ownership. Evidence: Exercise all controls in both browsers, preserved board/score/bonuses, restart retention, reload defaults and untouched URL/cookies; repair regressions and document defaults.
        - [ ] T-056 — Review, test and report milestone 6.2
            Depends on: T-053, T-054, T-055. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/6/6.2.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 6.3 — Phase review
        - [ ] T-057 — Review, test and report phase 6
            Depends on: milestone 6.1 completed/closed, milestone 6.2 completed/closed. Review cross-milestone code, run phase exits/regressions, repair blockers, aggregate TODOs and update HANDOFF. Report: docs/dev/reports/phases/6/PHASE-REPORT.md. Publish before review issue/milestone closure; integrate only the complete verified phase. Release/version preparation follows the authorized release gate.

## Phase 7 — Persistent configuration

- [ ] Phase 7 — Persistent configuration
    - [ ] Milestone 7.1 — Independent settings restoration
        - [ ] T-058 — Define persistence tokens and pure source resolution
            Scope: src/game/settings.ts and per-setting fixtures. Depends on: Phase 6 completion, closure and published integration.
            Contract: SPEC C13 plus C12 behavior and C1–C11 regression under this phase’s recorded baseline; PLAN Phase 7 and layout ownership. Evidence: Cover exact keys/tokens, all six independent precedence paths, invalid/duplicate/empty inputs, cookie decode errors and mixed sources.
        - [ ] T-059 — Implement scoped cookie and startup adapters
            Scope: src/browser/persistence.ts, controller initialization and adapter checks. Depends on: T-058.
            Contract: SPEC C13 plus C12 behavior and C1–C11 regression under this phase’s recorded baseline; PLAN Phase 7 and layout ownership. Evidence: Read one input snapshot; save every valid explicit URL value including defaults; leave cookies for omitted/invalid parameters untouched; resolve before rendering.
        - [ ] T-060 — Verify actual restored startup state
            Scope: tests/browser/ and isolated cookie/URL fixtures. Depends on: T-059.
            Contract: SPEC C13 plus C12 behavior and C1–C11 regression under this phase’s recorded baseline; PLAN Phase 7 and layout ownership. Evidence: Reload with URL, cookie and default mixtures on both targets; selected controls and gameplay match resolved values while board/score/bonuses start fresh.
        - [ ] T-061 — Review, test and report milestone 7.1
            Depends on: T-058, T-059, T-060. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/7/7.1.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 7.2 — Durable player changes and failure tolerance
        - [ ] T-062 — Integrate canonical URL and cookie updates
            Scope: persistence/controller/settings UI and focused checks. Depends on: T-061.
            Contract: SPEC C13 plus C12 behavior and C1–C11 regression under this phase’s recorded baseline; PLAN Phase 7 and layout ownership. Evidence: Use History replacement without reload, preserve unrelated query pairs/fragment and collapse only the changed key; apply model update before side effects.
        - [ ] T-063 — Handle independent persistence failures
            Scope: browser adapter, settings status view and failure fixtures. Depends on: T-062.
            Contract: SPEC C13 plus C12 behavior and C1–C11 regression under this phase’s recorded baseline; PLAN Phase 7 and layout ownership. Evidence: Catch cookie/History read/write errors and silent cookie refusal; attempt paths independently, retain in-memory settings and expose concise nonblocking feedback.
        - [ ] T-064 — Validate persistence and complete static delivery
            Scope: unit/browser suites, README, module docs and build checks. Depends on: T-063.
            Contract: SPEC C13 plus C12 behavior and C1–C11 regression under this phase’s recorded baseline; PLAN Phase 7 and layout ownership. Evidence: Exercise all six changes, explicit-default synchronization, omitted-cookie retention, invalid inputs, reload/restart and both failure paths with gameplay regressions on both browsers; document the final settings interface.
        - [ ] T-065 — Review, test and report milestone 7.2
            Depends on: T-062, T-063, T-064. Review code and tests separately, exercise milestone exits/regressions, repair blockers and document TODO disposition. Report: docs/dev/reports/phases/7/7.2.md. Refresh HANDOFF; commit/push/read back before issue then milestone closure.
    - [ ] Milestone 7.3 — Phase review
        - [ ] T-066 — Review, test and report phase 7
            Depends on: milestone 7.1 completed/closed, milestone 7.2 completed/closed. Review cross-milestone code, run phase exits/regressions, repair blockers, aggregate TODOs and update HANDOFF. Report: docs/dev/reports/phases/7/PHASE-REPORT.md. Include final docs/dev/reports/IMPLEMENTATION-REPORT.md with all remaining/deferred TODOs and provenance. Publish before review issue/milestone closure; integrate only the complete verified phase. Release/version preparation follows the authorized release gate.
