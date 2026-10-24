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
