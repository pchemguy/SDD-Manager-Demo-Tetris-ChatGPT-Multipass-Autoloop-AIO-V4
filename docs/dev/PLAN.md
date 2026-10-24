# Delivery plan

Deliver a usable static browser game through bounded capability increments. Each phase preserves the preceding playable path; only its listed contract scope and explicit phase defaults govern its exit. SPEC describes the target system, while each phase records the exact accepted specification baseline needed to assess that increment. Later capability contracts do not retroactively broaden earlier exits.

Milestone completion requires code review, tests, blocker repair and its report. Phase review has a dedicated final milestone. No phase starts until its predecessor is verified, integrated and published; when tracking is enabled, required predecessor issue/milestone closures precede dependent work. Establish required hosted objects before task execution. Every task commits result, evidence and initial checkbox together and is pushed/read back before progression.

Versioning uses SemVer: first playable v0.1.0, subsequent feature phases increment the minor version, compatible repairs increment patch. Publish a release only after a subsequently authorized implementation phase is complete, verified and integrated on the actual default branch. Root RELEASE-NOTES.md, eligible release commit/tag and GitHub body must agree. Documentation preparation itself publishes no product release.

Browser verification uses the root policy: isolated Chromium headless shell primary and Playwright-managed Firefox secondary. Provisioning, model tests, actual app interaction and visible rendering each require their own evidence. No browser installation is required for document-only planning. Layout owns physical placement; TASKS derives executable units.

## Phase 1 — Playable classic game

No predecessor. Contract baseline: SPEC SHA-256 `a42f0f4f81623c13b197aff5ecb4d9cde4a8bd33154345bf3b29f91cd64688ff`, C1–C6.

### Milestone 1.1 — Playable falling-piece slice

Outcome/exit: Empty-board falling, movement and rotation are visible through the actual application; isolated browser provisioning is demonstrated before dependent UI checks. Begins after the predecessor phase gate. Conclude with code review, applicable tests, blocker repair and a committed milestone report. Demonstrate the resulting capability and retain usability/risk evidence for any human continue/amend/simplify/stop decision.

### Milestone 1.2 — Completed game sessions

Outcome/exit: Settling, row clearing, scoring, blocked spawn and restart form a complete playable loop. Depends on milestone 1.1 completion and closure. Conclude with code review, applicable tests, blocker repair and a committed milestone report. Demonstrate the resulting capability and retain usability/risk evidence for any human continue/amend/simplify/stop decision.

### Milestone 1.3 — Static browser delivery

Outcome/exit: The built application is playable with documented controls and verified visible output on both required browsers. Depends on milestone 1.2 completion and closure. Conclude with code review, applicable tests, blocker repair and a committed milestone report. Demonstrate the resulting capability and retain usability/risk evidence for any human continue/amend/simplify/stop decision.

### Milestone 1.4 — Phase review

Exactly one phase code review/testing/report task, dependent on all delivery milestones being completed and closed. Verify cross-milestone interactions, applicable regressions, exits and TODO disposition; update HANDOFF in the owning completion commit.

Phase exit: A static build supports a complete classic session and restart on both targets; all delivery reviews and the phase report pass. Scope is fixed-gravity, independent random generation, blocked rotation and failed-down locking; no modern assistance or settings. First playable version v0.1.0. Publish the completed boundary, integrate explicitly into main, verify merged state and publish before any authorized successor or release.

## Phase 2 — Bag, kicks and Hold

Depends on verified, integrated and published Phase 1, with required issue/milestone closures. Contract baseline: SPEC SHA-256 `347dd3035fb3ed5dab05177f74ac3389fe05bec2f38e68dfce6183d1983880a8`, C1–C7; C8 delay is deferred to Phase 3.

### Milestone 2.1 — Reliable piece controls

Outcome/exit: 7-bag generation and deterministic wall kicks integrate into the classic playable loop. Begins after the predecessor phase gate. Conclude with code review, applicable tests, blocker repair and a committed milestone report. Demonstrate the resulting capability and retain usability/risk evidence for any human continue/amend/simplify/stop decision.

### Milestone 2.2 — Held-piece control

Outcome/exit: Hold storage, once-per-piece eligibility and blocked replacement behavior are visible and tested. Depends on milestone 2.1 completion and closure. Conclude with code review, applicable tests, blocker repair and a committed milestone report. Demonstrate the resulting capability and retain usability/risk evidence for any human continue/amend/simplify/stop decision.

### Milestone 2.3 — Phase review

Exactly one phase code review/testing/report task, dependent on all delivery milestones being completed and closed. Verify cross-milestone interactions, applicable regressions, exits and TODO disposition; update HANDOFF in the owning completion commit.

Phase exit: Playable 7-bag/kick/Hold controls with deterministic source and lifecycle evidence on both browsers; fixed gravity and failed-down locking remain this phase’s timing baseline. Ghost, hard drop and full-interval lock are deferred to Phase 3. Feature version v0.2.0. Publish the completed boundary, integrate explicitly into main, verify merged state and publish before any authorized successor or release.

## Phase 3 — Landing assistance and delayed lock

Depends on verified, integrated and published Phase 2, with required issue/milestone closures. Contract baseline: SPEC SHA-256 `347dd3035fb3ed5dab05177f74ac3389fe05bec2f38e68dfce6183d1983880a8`, C8 plus regression of C1–C7.

### Milestone 3.1 — Consistent landing assistance

Outcome/exit: One landing projection drives visible ghost and hard drop. Begins after the predecessor phase gate. Conclude with code review, applicable tests, blocker repair and a committed milestone report. Demonstrate the resulting capability and retain usability/risk evidence for any human continue/amend/simplify/stop decision.

### Milestone 3.2 — Grounded interval locking

Outcome/exit: A grounded active piece locks after one full simulation gravity interval, including hard drop. Depends on milestone 3.1 completion and closure. Conclude with code review, applicable tests, blocker repair and a committed milestone report. Demonstrate the resulting capability and retain usability/risk evidence for any human continue/amend/simplify/stop decision.

### Milestone 3.3 — Phase review

Exactly one phase code review/testing/report task, dependent on all delivery milestones being completed and closed. Verify cross-milestone interactions, applicable regressions, exits and TODO disposition; update HANDOFF in the owning completion commit.

Phase exit: Ghost and hard drop share an exact ordinary projection, and all relevant actions obey full-interval locking. Both-browser rendering/interaction evidence and source/Hold/classic regressions pass. Feature version v0.3.0. Publish the completed boundary, integrate explicitly into main, verify merged state and publish before any authorized successor or release.

## Phase 4 — Conditional ghost display

Depends on verified, integrated and published Phase 3, with required issue/milestone closures. Contract baseline: SPEC SHA-256 `5ce6a6128863ba069de2ec09465164645f646b40dd81efd9d5f24c4a9dbe2394`, C9 and C8 projection regression.

### Milestone 4.1 — Midline visibility

Outcome/exit: Upper-only projections hide while lower/straddling landings show; hiding never changes hard-drop placement. Begins after the predecessor phase gate. Conclude with code review, applicable tests, blocker repair and a committed milestone report. Demonstrate the resulting capability and retain usability/risk evidence for any human continue/amend/simplify/stop decision.

### Milestone 4.2 — Phase review

Exactly one phase code review/testing/report task, dependent on all delivery milestones being completed and closed. Verify cross-milestone interactions, applicable regressions, exits and TODO disposition; update HANDOFF in the owning completion commit.

Phase exit: Dynamic visibility obeys the occupied-cell midline predicate while all landing/locking/controls remain usable. One narrow delivery milestone is intentional: separate phases for a predicate and its integration would fragment this small behavior. Feature version v0.4.0. Publish the completed boundary, integrate explicitly into main, verify merged state and publish before any authorized successor or release.

## Phase 5 — Penetrator bonuses

Depends on verified, integrated and published Phase 4, with required issue/milestone closures. Contract baseline: SPEC SHA-256 `95fa2d52f33ceba291209612c84a2b3eba9290b97b8bbd7bb3ab8e9f1f935ee8`, C10–C11 plus C7–C9 interaction regression.

### Milestone 5.1 — Legal penetrator traversal

Outcome/exit: The model can cross occupied intermediate cells and still identify a safe deepest final placement. Begins after the predecessor phase gate. Conclude with code review, applicable tests, blocker repair and a committed milestone report. Demonstrate the resulting capability and retain usability/risk evidence for any human continue/amend/simplify/stop decision.

### Milestone 5.2 — Bonus and active-mode lifecycle

Outcome/exit: Activation spends one bonus; four-line settlement awards once up to three, and Hold is suppressed while active. Depends on milestone 5.1 completion and closure. Conclude with code review, applicable tests, blocker repair and a committed milestone report. Demonstrate the resulting capability and retain usability/risk evidence for any human continue/amend/simplify/stop decision.

### Milestone 5.3 — Integrated penetrator gameplay

Outcome/exit: The built game demonstrates safe traversal, matching projection and stable classic/modern behavior. Depends on milestone 5.2 completion and closure. Conclude with code review, applicable tests, blocker repair and a committed milestone report. Demonstrate the resulting capability and retain usability/risk evidence for any human continue/amend/simplify/stop decision.

### Milestone 5.4 — Phase review

Exactly one phase code review/testing/report task, dependent on all delivery milestones being completed and closed. Verify cross-milestone interactions, applicable regressions, exits and TODO disposition; update HANDOFF in the owning completion commit.

Phase exit: Penetrator traversal never destroys occupied cells or commits overlap; solver equality, active Hold restriction, bonus cap/spending and white rendering are verified with full gameplay regressions. Feature version v0.5.0. Publish the completed boundary, integrate explicitly into main, verify merged state and publish before any authorized successor or release.
