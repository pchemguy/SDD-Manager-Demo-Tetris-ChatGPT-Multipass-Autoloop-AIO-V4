# Main documentation preparation record

## Current control

- Mode: autodoc/autoloop, explicitly requested on 2026-10-24 for six predefined cumulative scopes.
- Delivery mode: strict multipass. No permission to change earlier phases/tasks has been supplied. Preserve their identities, scope, milestone membership, order, dependencies and exits; add subsequent ranges for subsequent scope.
- Current pass: 1; DOCS-003 accepted; next DOCS-004.
- Approval basis: first PROJECT explicitly approved by the user (“Approve”) on 2026-10-24. Later PROJECT approvals are waived, as announced in chat; subsequent document checkpoints and scope resets are preapproved by the selected mode after first approval. Newly discovered material choices are delegated under autoloop, with visible rationale. Initial cross-scope proposals were accepted in that same approval.
- Tracking: enabled for preparation and implementation by existing root AGENTS.md. Milestone 1 (`sdd-docs-main`) and issues 1–9 match bundled definitions and associations, verified before repository writes. No implementation objects activated.
- Publication: every owned commit is pushed and read back before progression. Reporting: Standard after Explicit bootstrap.
- Stop: complete the six scopes through final DOCS-008, stopping before DOCS-009, whole-package QC, documentation integration or implementation.

## Scope sequence

1. **Pass 1 — Playable classic baseline.** Falling tetrominoes, movement, rotation, gravity, line clearing, scoring, game over and restart. Minimal coherent playable game; no adoption of later scope into the initial brief.
2. **Pass 2 — Gameplay enhancements.** Hold, ghost, 7-bag randomization, wall kicks, hard drop, and default lock delay of one full gravity interval from grounding, including hard drop.
3. **Pass 3 — Conditional ghost visibility.** Hide projected landings in the upper half, and show them in the lower half; resolve straddling positions using the proposed rule below.
4. **Pass 4 — Penetrator mode.** Hotkey activation spends one available bonus for the current falling piece. Pass through occupied cells without destroying them; lock entirely in bounds without overlap. Hard drop reaches the lowest valid landing, and visible ghost previews it. Four-line clears award one bonus, capped at three. Render penetrators white; regular pieces never white. Side panel displays one symbol per bonus and updates on earning/spending. Disable Hold during activation under the accepted rule below.
5. **Pass 5 — Session-only configuration.** Side-panel controls: ghost hide/dynamic/show; hard drop on/off; next preview on/off; hard-drop locking follow-general/immediate; general locking failed-down/full-interval; Hold on/off. Add next preview. Sensible defaults; settings survive restart but reset on reload; no cookies/URL persistence yet.
6. **Pass 6 — Configuration persistence.** Resolve all six settings independently from valid explicit URL parameter, then valid cookie, then default. Player changes save cookie and update corresponding URL parameter. Save valid explicit URL values to cookies even when equal to defaults. Omitted URL parameters preserve saved cookies. Handle invalid values under the proposed fallback rule below.

## Per-pass decisions

All entries originate in initial exploration during Pass 1. They were proposed by the agent and accepted by the user’s first PROJECT approval on 2026-10-24. Future requirements remain here until their scope is adopted.

| Decision | Resolution and rationale | Basis / source | Applicability / status |
| --- | --- | --- | --- |
| Delivery and stack | Keyboard-first desktop browser game; TypeScript, Canvas board and HTML controls, static client with no backend. Canvas avoids a large DOM board; HTML supports conventional control focus and labels. Vanilla JavaScript is simpler to start but provides weaker state contracts; a UI framework adds little value for this small game. | Agent recommendation from browser scope and minimal baseline. | All scopes; accepted. |
| Baseline | 10×20 board, seven tetrominoes, simple rotation with blocked moves rejected, independent uniform random pieces, fixed gravity, failed-down locking, line-count-based scoring, restart. No Hold, ghost, bag, kicks, hard drop, preview or configuration. | Agent proposed defaults, preserving the user’s minimal baseline. | Pass 1; accepted. Pass 2 replaces randomization/rotation/locking behavior through new delivery ranges. |
| Midline | Show a dynamic ghost if its lowest occupied cell is in rows 10–19 (zero-based); hide only when every occupied cell lies in rows 0–9. A straddling projection therefore shows. | Agent proposal: any landing reaching the lower half qualifies. | Passes 3–6; accepted. |
| Lock timing | Capture one current gravity interval when grounding begins. Grounded translations/rotations do not refresh it; leaving support cancels it, and renewed grounding starts a fresh interval. Hard drop follows the same rule and cannot refresh an already running grounded timer. Pause, if introduced, freezes simulation time. | Agent proposal to avoid indefinite grounded resets while retaining actual ungrounding semantics. | Passes 2–6; accepted. Exact timing/level rules belong in SPEC. |
| Penetrator lifecycle | Activation spends exactly once and lasts until lock; Hold is disabled while active. Board cells are never removed by traversal. Use the lowest legal non-overlapping landing at or below the active position. Reject activation or transforms with no reachable legal landing, without spending a bonus for rejected activation. While descending through overlap, never merge into the board; stop at that legal landing. If a replacement piece cannot spawn, end the game normally. | Agent proposal resolving traversal, geometry and bonus safety; Hold restriction also aligns with established user project preference but is not claimed as a new V4 instruction. | Passes 4–6; accepted. |
| Live configuration | Visual/toggle settings apply immediately; toggling Hold off retains stored piece but disables use. Hard-drop disable ignores its input. Lock-policy changes cancel the previous timer and start a fresh interval if grounded under delay mode; switching to failed-down waits for the next failed downward attempt. Immediate hard-drop policy affects subsequent hard drops only. Settings survive restart; game state does not reset on change. | Agent proposal for predictable visible changes without board replacement or retroactive hard-drop locking. | Passes 5–6; accepted. |
| Defaults | Dynamic ghost; hard drop, preview and Hold on; full-interval general lock; hard drop follows general. Session-only settings initially; persistence is added separately. | Agent recommendation matching preceding gameplay behavior. | Passes 5–6; accepted. |
| Invalid persistence values | Validate URL and cookie independently. Invalid values count as absent, allowing the next valid source. Do not save invalid URL values to cookies. Duplicate URL keys count as invalid for that setting. Preserve unrelated query parameters. Cookie unavailability must not stop gameplay or in-page changes. | Agent proposal preserving per-setting precedence and graceful failure. | Pass 6; accepted. |
| Verification and releases | Chromium primary and Playwright-managed Firefox secondary, headless and isolated under root browser policy. Plan a tested static build. First playable release v0.1.0; subsequent bounded feature phases increment minor version, patch versions for compatible fixes. Release only after authorized implementation phase verification and published integration. | Browser policy, user release instruction, agent version recommendation. | All delivery scopes; accepted. Browser provisioning is not attempted for document-only work. |

## Cross-scope decisions

| Concern | Decision / applicable scopes | Reconsideration condition |
| --- | --- | --- |
| Feature isolation | Baseline excludes all future features; strict later ranges extend it. Product documents contain current intended scope, never pass history. | Explicit permission for flexible multipass or changed scope. |
| Ghost and penetrator | Midline uses the same landing projection as hard drop; penetrator projection ignores blocking cells during traversal but requires a legal final placement. See Midline and Penetrator lifecycle above; Passes 3–6. | Geometry proves the proposed landing rule inconsistent, or user steering changes visibility. |
| Lock and configuration | Lock timing and Live configuration above govern Passes 2–6; prevent grounded inputs or setting changes from implying retroactive hard drops. | Changed timing/reset policy or additional pause/level requirements. |
| Hold and penetrator | Active penetrator disables Hold regardless of configuration; normal Hold resumes for the next ordinary piece if enabled. | User explicitly changes active-penetrator Hold policy. |
| Persistence | Live behavior is retained when storage is added; valid values resolve per setting, not as one source-wide configuration. | Storage/source requirements change. |

## Decisions and document evidence

| Task | Current evidence / decision basis |
| --- | --- |
| DOCS-001 | Four pre-existing root files retained byte for byte against initial commit `ab6f437fc7b8d5bc1950502e0a28192fa1a684da`. Task list copied from running repository package with only DOCS-001 checked. Initialization commit `b4458065acc74d212b715c432378aaf45e84abef` pushed and exact ref read back; issue #1 closed as completed and read back. Connector writes returned integration-permission 403; supplied-token API client completed the supported closure. |
| DOCS-002 | Pass 1 [PROJECT proposal](PROJECT.md), SHA-256 `e6e2c214d62582f96f847144decc7168608cf7efdb3dd47fc6d912c8eb40cf37`; first human approval received on 2026-10-24; completion checkpoint records acceptance. Scope reviewed against Pass 1: no future enhancements adopted. HTML/Canvas/TypeScript direction, testability, browser policy and explicit exclusions are accepted. Cross-scope choices accepted in this first review. README contains truthful project status and root disclosure/usage links. |
| DOCS-003–008 | Not started. |
| DOCS-009 | Not selected; remains unchecked through the autoloop stop. |

Material unresolved decisions: none; first PROJECT approval accepted the brief and initial cross-scope proposals above. No implementation range is authorized.

| Pass 1, DOCS-002 | `PROJECT.md` SHA-256 `e6e2c214d62582f96f847144decc7168608cf7efdb3dd47fc6d912c8eb40cf37`; human approval: “Approve”, 2026-10-24; accepted proposal 50d61c969e4f5fa9d7bddcfc9ce05e5ab226fb29 unchanged. PROJECT scope and recorded cross-scope choices approved; proposed document hash unchanged. Required source links and scope separation checked. Publication and issue #2 closure must be read back before progression. |

| Pass 1, DOCS-003 | `ARCHITECTURE.md` SHA-256 `67f5f3d4f77c3b8a0cca8308dd56ab441fa67c5f28a43ec2e9c10094ad3746ff`; scoped autodoc/autoloop preapproval following first PROJECT approval. Reviewed ownership, pure model boundary, serialized settlement, browser resource lifecycle and accepted PROJECT conformance; no implementation claimed. Publication and issue #3 closure must be read back before progression. |

## Initialization and current reset

- Original root-file presence: all four existed: AI_DISCLOSURE.md, SDD-MANAGER.md, AGENTS.md, SKILL_DISCOVERY_PROTOCOL.md. Copied root files: none.
- Verification: byte comparison with their initial committed versions passed for all four. No mixed-presence pause applies.
- Manual provenance: default enabled. Base `ab6f437fc7b8d5bc1950502e0a28192fa1a684da`; scaffold seed `2647e55f939639533412b0a645cb2fe2150b4386` on retained `design-docs/ai-logs`; explicit main merge `38b5c4bf8d5c945a4eaf8dffb0a03610b8ace7f7`. Both pushes and exact remote readbacks succeeded with confirmed process cleanup. Two scaffold assets match package bytes. No conversations recorded.
- Documentation branch: design-docs/main from main at the verified scaffold merge above.
- Current reset: none. No prior-pass delivery ranges exist yet.

## Pending transitions

Publish/read back this DOCS-003 checkpoint, then close/read back issue #3. All prior checkpoint effects have been verified. Continue only after the required hosted effect is verified.
