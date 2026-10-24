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
