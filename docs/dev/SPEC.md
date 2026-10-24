# System specification

The game defined in [PROJECT](PROJECT.md) is a static desktop-browser application. [DECOMPOSITION](DECOMPOSITION.md) assigns component ownership; contracts below are authoritative for observable behavior. All requirements are intended, not implementation claims.

## C1 — Board, pieces and lifecycle

The visible board has 10 columns and 20 rows. Coordinates are integers with x=0 at left, y=0 at top. Locked cells are empty or carry a non-white piece color. The seven tetrominoes I, J, L, O, S, T and Z each occupy four distinct cells. A piece has integer origin and an orientation; all occupied cells must lie within the visible board when spawned or locked. Spawn centered horizontally in its canonical orientation at the topmost fitting row; occupied cells begin at row 0. Define canonical offsets in geometry fixtures before consumers use them. A spawn overlapping locked cells ends the game without modifying those cells.

Starting opens a running game with empty board and score zero. Only one active piece exists while running. Game over freezes descent and movement, shows a labeled status and permits restart. Restart clears the board, score, active state, elapsed time and held inputs, then spawns one ordinary piece. It must not multiply listeners or animation loops. No session resume across reload is required.

## C2 — Controls and event ownership

Left/Right move one column; ArrowUp rotates clockwise; ArrowDown performs one soft-drop step; R or the labeled Restart button restarts. Prevent browser scrolling for gameplay keys only while the game surface owns focus and the event is handled. Ignore gameplay keys in input, select, textarea and editable controls. Rotation/restart activate once per physical press; repeated horizontal/soft-drop keydown events each represent one step. Unknown commands and gameplay input after game over are no-ops except restart. Render visible control instructions and a focusable game surface.

## C3 — Ordinary movement, rotation and descent

An ordinary candidate transform succeeds only if every occupied cell is in bounds and does not overlap the locked board. Rejected horizontal movement/rotation leaves the state unchanged. Rotation is clockwise around the canonical piece origin with no offset search; O rotation preserves occupied cells. Independent uniform shape generation maps each injected random value in [0,1) into one of seven equal intervals.

Gravity attempts one downward row each 1000 ms of simulation time. Soft drop does not award points and does not restart the gravity accumulator. On a failed downward attempt, settle the piece immediately. Newly spawned pieces receive a fresh gravity accumulator; unused elapsed time from a settlement is not carried into another piece. After tab/window blur, discard accumulated held input and suspension time; require a fresh scheduled frame before advancing. Frame processing must remain bounded after a long suspension rather than replaying unbounded background ticks.

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
| C1–C3 | Seven shapes each occupy four cells; a wall/stack blocks an ordinary move without mutation; 999 ms causes no gravity step and the next 1 ms causes one. A rejected downward attempt settles once. |
| C4 | A fixture with two completed rows earns 300 points, compacts remaining rows in order, and spawns once. A blocked next spawn ends play while preserving locked cells. |
| C2, C5 | Actual browser keys move/rotate the visible piece while focused, do not hijack form controls, and Restart returns an empty running board with score zero. |
| C6 | Replaying a seeded source and ordered action/time stream yields identical snapshots; repeated restart and teardown do not add loops/listeners; both required browser targets execute the chosen app checks. |
