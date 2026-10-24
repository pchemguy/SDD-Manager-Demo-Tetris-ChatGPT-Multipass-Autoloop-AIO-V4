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
