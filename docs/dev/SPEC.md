# System specification

The game defined in [PROJECT](PROJECT.md) is a static desktop-browser application. [DECOMPOSITION](DECOMPOSITION.md) assigns component ownership; contracts below are authoritative for observable behavior. All requirements are intended, not implementation claims.

## C1 — Board, pieces and lifecycle

The visible board has 10 columns and 20 rows. Coordinates are integers with x=0 at left, y=0 at top. Locked cells are empty or carry a non-white piece color. The seven tetrominoes I, J, L, O, S, T and Z each occupy four distinct cells. A piece has integer origin and an orientation; all occupied cells must lie within the visible board when spawned or locked. Spawn centered horizontally in its canonical orientation at the topmost fitting row; occupied cells begin at row 0. Define canonical offsets in geometry fixtures before consumers use them. A spawn overlapping locked cells ends the game without modifying those cells.

Starting opens a running game with empty board and score zero. Only one active piece exists while running. Game over freezes descent and movement, shows a labeled status and permits restart. Restart clears the board, score, active state, elapsed time and held inputs, then spawns one ordinary piece. It must not multiply listeners or animation loops. No session resume across reload is required.

## C2 — Controls and event ownership

Left/Right move one column; ArrowUp rotates clockwise; ArrowDown performs one soft-drop step; Space hard-drops; C holds; R or the labeled Restart button restarts. Prevent browser scrolling for gameplay keys only while the game surface owns focus and the event is handled. Ignore gameplay keys in input, select, textarea and editable controls. Rotation, hard drop, Hold and restart activate once per physical press; repeated horizontal/soft-drop keydown events each represent one step. Unknown commands and gameplay input after game over are no-ops except restart. Render visible control instructions and a focusable game surface.

## C3 — Ordinary movement, rotation and descent

An ordinary candidate transform succeeds only if every occupied cell is in bounds and does not overlap the locked board. Rejected horizontal movement/rotation leaves the state unchanged. Rotation is clockwise around the canonical piece origin. Try kick offsets in this order, with y increasing downward: (0,0), (-1,0), (1,0), (-2,0), (2,0), (0,-1), (0,-2). Accept the first fully legal candidate, otherwise reject without mutation. The same bounded search applies to all shapes; O preserves occupied cells and does not translate on rotation. Use canonical integer offset fixtures and four orientation states, including I; the offset search is the complete required wall-kick policy. Shape generation follows C7.

Gravity attempts one downward row each 1000 ms of simulation time. Soft drop does not award points and does not restart the gravity accumulator. On a failed downward attempt, remain grounded and apply C8’s lock delay; the failed attempt does not lock early or refresh the timer. Newly spawned pieces receive a fresh gravity accumulator; unused elapsed time from a settlement is not carried into another piece. After tab/window blur, discard accumulated held input and suspension time; require a fresh scheduled frame before advancing. Frame processing must remain bounded after a long suspension rather than replaying unbounded background ticks.

## C4 — Settlement, clearing and score

Validate all four cells before writing any locked cell. Settlement merges the active piece once, detects all full rows, removes them simultaneously, moves remaining rows down while preserving their order, and fills vacated top rows with empties. Clear 0/1/2/3/4 rows for 0/100/300/500/800 points respectively; no level multiplier or speed progression. After scoring, generate and spawn the replacement piece or enter game over on a blocked spawn. No command may double-score a settlement or partially merge an invalid placement.

## C5 — Presentation and delivery

Draw locked and active cells in distinguishable non-white shape colors on a contrasting grid. Present score, controls, game-over status and Restart as readable HTML. Labels and focus indicators are visible; operation is keyboard-first. Full nonvisual representation of the board, mobile touch controls, audio, accounts and multiplayer are outside scope. The built application runs as static assets from a local HTTP server without a backend. A minimum desktop viewport of 900×700 CSS pixels must expose the board, status and controls without overlap; smaller layouts may scroll.

## C6 — State, failures and evidence

Geometry and model transitions accept injected randomness and simulation time and must not read DOM, storage or global wall time. Public snapshots expose board, active shape/orientation/position, score and running/game-over state. Invalid user actions are harmless no-ops. Browser listeners and timers have one owner and must be released on teardown. Provisioning/test errors are verification failures, not game-over outcomes.

Browser verification uses isolated Chromium headless shell as primary and Playwright-managed Firefox as secondary under [BROWSER_DEV_POLICY](../../BROWSER_DEV_POLICY.md). Exercise the actual built app: keyboard movement/rotation/drop, occupied-cell collision, multirow clear/scoring, blocked spawn, restart and repeated mount/teardown. Inspect Canvas rendering with screenshots or meaningful pixel/shape assertions; DOM score text alone does not prove the board rendered. Browser binaries and generated evidence are untracked. Model tests cover deterministic source boundaries, all shapes, edges, rejected rotations, row compaction and atomic settlement. Exact commands/versions are established during authorized tooling work, not claimed here.

## Acceptance examples

| Contract | Objective examples |
| --- | --- |
| C1–C3 | Seven shapes each occupy four cells; a wall/stack blocks an ordinary move without mutation; 999 ms causes no gravity step and the next 1 ms causes one. A rejected downward attempt leaves a grounded active piece until its lock interval expires, then settles once. |
| C4 | A fixture with two completed rows earns 300 points, compacts remaining rows in order, and spawns once. A blocked next spawn ends play while preserving locked cells. |
| C2, C5 | Actual browser keys move/rotate the visible piece while focused, do not hijack form controls, and Restart returns an empty running board with score zero. |
| C6 | Replaying a seeded source and ordered action/time stream yields identical snapshots; repeated restart and teardown do not add loops/listeners; both required browser targets execute the chosen app checks. |

## C7 — Bag and Hold

Generate each bag as one uniformly shuffled permutation of the seven shapes, using injected random values for a descending Fisher–Yates shuffle. Consume one shape per ordinary spawn; refill only when exhausted. Two adjacent bags may repeat a shape across their boundary. Restart creates a fresh empty bag/source state, empty Hold and available Hold eligibility. No next-piece preview is required.

C holds once per active-piece lifecycle. If Hold is empty, store the active shape and consume exactly one bag entry for its replacement. Otherwise swap stored and active shapes without advancing the bag. The replacement uses canonical spawn orientation/position with fresh gravity/grounding state. A successful Hold disables Hold until the next piece spawned after a lock. A blocked replacement spawn ends the game; it does not overwrite locked cells or refund a source draw. Ignored repeated Hold changes neither source nor held state. Render the held shape in a labeled side area.

Acceptance: every aligned seven draws contains each shape once; deterministic shuffle replay is stable; empty Hold draws once, swap draws zero, second Hold before lock is ignored, and a blocked swapped spawn causes game over.

## C8 — Landing, hard drop and lock interval

An ordinary landing is the maximum downward translation reachable by repeated legal one-row moves without crossing occupied cells or leaving the board. Ghost cells occupy exactly that landing; show them as a distinguishable outline while a game is running, including a zero-distance landing. Never merge the ghost into board state. Space moves the active piece to exactly the ghost landing and scores no drop-distance bonus. After hard drop the piece remains active under the general delay.

Grounding begins immediately when a legal active piece cannot move one row down, including after spawn, Hold, movement, rotation or hard drop. Capture the gravity interval (1000 ms) and a simulation timestamp on that transition. Settle when grounded elapsed time reaches that interval, independently of when the next gravity step falls. Grounded shifts/rotations, repeated hard drops, failed downward attempts and soft drops do not refresh a running timer. Leaving support cancels it; later grounding starts a new full interval. A new piece starts with fresh timing. Gravity and lock expiry are serialized; at equal time the already grounded piece locks once before any later command is processed. Blur/suspension time is excluded by C3; no pause feature is required.

Acceptance: landings match ghost and hard drop at empty floor, stack and overhang; 999 ms of grounded time does not lock and the next 1 ms does. A move retaining support keeps the original deadline; leaving support cancels it, and re-grounding gets a fresh interval. A zero-distance repeated hard drop cannot extend the deadline. Hold resets timing for its replacement; game-over/restart clear pending timers.
