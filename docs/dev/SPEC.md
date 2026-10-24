# System specification

The game defined in [PROJECT](PROJECT.md) is a static desktop-browser application. [DECOMPOSITION](DECOMPOSITION.md) assigns component ownership; contracts below are authoritative for observable behavior. All requirements are intended, not implementation claims.

## C1 — Board, pieces and lifecycle

The visible board has 10 columns and 20 rows. Coordinates are integers with x=0 at left, y=0 at top. Locked cells are empty or carry their originating piece color: ordinary shapes use non-white colors, and penetrator-origin cells use white. The seven tetrominoes I, J, L, O, S, T and Z each occupy four distinct cells. A piece has integer origin and an orientation; all occupied cells must lie within the visible board when spawned or locked. Spawn centered horizontally in its canonical orientation at the topmost fitting row; occupied cells begin at row 0. Define canonical offsets in geometry fixtures before consumers use them. A spawn overlapping locked cells ends the game without modifying those cells.

Starting opens a running game with empty board and score zero. Only one active piece exists while running. Game over freezes descent and movement, shows a labeled status and permits restart. Restart clears the board, score, active state, elapsed time and held inputs, then spawns one ordinary piece. It must not multiply listeners or animation loops. No session resume across reload is required.

## C2 — Controls and event ownership

Left/Right move one column; ArrowUp rotates clockwise; ArrowDown performs one soft-drop step; Space hard-drops; C holds; P activates a penetrator bonus; R or the labeled Restart button restarts. Prevent browser scrolling for gameplay keys only while the game surface owns focus and the event is handled. Ignore gameplay keys in input, select, textarea and editable controls. Rotation, hard drop, Hold, penetrator activation and restart activate once per physical press; repeated horizontal/soft-drop keydown events each represent one step. Unknown commands and gameplay input after game over are no-ops except restart. Render visible control instructions and a focusable game surface.

## C3 — Ordinary movement, rotation and descent

An ordinary candidate transform succeeds only if every occupied cell is in bounds and does not overlap the locked board. Rejected horizontal movement/rotation leaves the state unchanged. Rotation is clockwise around the canonical piece origin. Try kick offsets in this order, with y increasing downward: (0,0), (-1,0), (1,0), (-2,0), (2,0), (0,-1), (0,-2). Accept the first fully legal candidate, otherwise reject without mutation. The same bounded search applies to all shapes; O preserves occupied cells and does not translate on rotation. Use canonical integer offset fixtures and four orientation states, including I; the offset search is the complete required wall-kick policy. Shape generation follows C7.

Gravity attempts one downward row each 1000 ms of simulation time. Soft drop does not award points and does not restart the gravity accumulator. On a failed downward attempt, apply the selected C12 general locking policy. Under interval locking the failed attempt does not lock early or refresh the timer. Newly spawned pieces receive a fresh gravity accumulator; unused elapsed time from a settlement is not carried into another piece. After tab/window blur, discard accumulated held input and suspension time; require a fresh scheduled frame before advancing. Frame processing must remain bounded after a long suspension rather than replaying unbounded background ticks.

## C4 — Settlement, clearing and score

Validate all four cells before writing any locked cell. Settlement merges the active piece once, detects all full rows, removes them simultaneously, moves remaining rows down while preserving their order, and fills vacated top rows with empties. Clear 0/1/2/3/4 rows for 0/100/300/500/800 points respectively; no level multiplier or speed progression. After scoring, generate and spawn the replacement piece or enter game over on a blocked spawn. No command may double-score a settlement or partially merge an invalid placement.

## C5 — Presentation and delivery

Draw ordinary cells in distinguishable non-white shape colors on a contrasting grid; C10 owns the explicit white appearance of penetrator-origin cells. Present score, controls, game-over status and Restart as readable HTML. Labels and focus indicators are visible; operation is keyboard-first. Full nonvisual representation of the board, mobile touch controls, audio, accounts and multiplayer are outside scope. The built application runs as static assets from a local HTTP server without a backend. A minimum desktop viewport of 900×700 CSS pixels must expose the board, status and controls without overlap; smaller layouts may scroll.

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

Generate each bag as one uniformly shuffled permutation of the seven shapes, using injected random values for a descending Fisher–Yates shuffle. Consume one shape per ordinary spawn; refill only when exhausted. Two adjacent bags may repeat a shape across their boundary. Restart creates a fresh empty bag/source state, empty Hold and available Hold eligibility. Expose one stable next-shape preview under C12 without consuming the bag cursor. Preparing an exhausted bag for a future peek may cache a shuffle, but toggling preview must not reshuffle or alter the eventual draw sequence.

For an ordinary piece, C holds once per active-piece lifecycle when Hold is enabled. While penetrator mode is active C is a no-op, retaining held/source state; Hold eligibility for subsequent ordinary pieces follows the usual once-per-lock rule. If Hold is empty, store the active shape and consume exactly one bag entry for its replacement. Otherwise swap stored and active shapes without advancing the bag. The replacement uses canonical spawn orientation/position with fresh gravity/grounding state. A successful Hold disables Hold until the next piece spawned after a lock. A blocked replacement spawn ends the game; it does not overwrite locked cells or refund a source draw. Ignored repeated Hold changes neither source nor held state. Render the held shape in a labeled side area.

Acceptance: every aligned seven draws contains each shape once; deterministic shuffle replay is stable; empty Hold draws once, swap draws zero, second Hold before lock is ignored, and a blocked swapped spawn causes game over.

## C8 — Landing, hard drop and lock interval

An ordinary landing is the maximum downward translation reachable by repeated legal one-row moves without crossing occupied cells or leaving the board. Ghost cells occupy exactly that landing; draw them as a distinguishable outline when C9 permits visibility, including a visible zero-distance landing. Never merge the ghost into board state. Space moves the active piece to exactly the ghost landing and scores no drop-distance bonus. Hard drop is ignored when disabled. When enabled, its landing is unchanged; locking follows C12’s hard-drop policy.

For ordinary pieces grounding begins immediately when a legal active piece cannot move one row down; for penetrators it begins when the active position equals C10’s deepest legal landing, including after spawn, Hold, movement, rotation or hard drop. Capture the gravity interval (1000 ms) and a simulation timestamp on that transition. Under interval locking, settle when grounded elapsed time reaches that interval, independently of when the next gravity step falls. Grounded shifts/rotations, repeated hard drops, failed downward attempts and soft drops do not refresh a running timer. Leaving support cancels it; later grounding starts a new full interval. A new piece starts with fresh timing. Gravity and lock expiry are serialized; at equal time the already grounded piece locks once before any later command is processed. Blur/suspension time is excluded by C3; no pause feature is required.

Acceptance: landings match ghost and hard drop at empty floor, stack and overhang; 999 ms of grounded time does not lock and the next 1 ms does. A move retaining support keeps the original deadline; leaving support cancels it, and re-grounding gets a fresh interval. A zero-distance repeated hard drop cannot extend the deadline. Hold resets timing for its replacement; game-over/restart clear pending timers.

## C9 — Conditional ghost visibility

With zero-based rows 0–19, the upper half is 0–9 and the lower half is 10–19. Show the ghost exactly when a game is running and the maximum occupied landing-cell row is at least 10. Thus a projection wholly above row 10 is hidden, while one straddling rows 9/10 is shown. Use occupied cells, not a shape bounding-box origin or Canvas pixel position. Re-evaluate after each move, rotation, Hold, spawn, settlement and restart. Hiding a ghost changes only drawing, never the projected landing or hard-drop result.

Acceptance: a landing whose lowest cell is row 9 hides; a lowest cell at row 10 shows; straddling rows 9/10 shows; transformations returning an upper-only landing hide it again. Changing viewport size does not change the result. Hard drop still reaches the same projection while it is hidden. A game-over state draws no ghost.

## C10 — Penetrator traversal and locking

The session begins with zero bonuses and an ordinary active piece. P activates only while running, with an ordinary active piece, an available bonus, and at least one in-bounds non-overlapping landing at or below its current origin for its current shape/orientation/x. Acceptance spends exactly one bonus and marks only that active piece as a penetrator until it locks. Repeated P, P without a bonus, and activation after game over are no-ops. Activation cannot toggle the ability off or spend again. Restart resets bonuses and mode.

A penetrator’s intermediate falling cells may overlap the locked board, but must remain within board bounds. Define its landing as the greatest integer y at or below its current origin for which all four occupied cells are in bounds and non-overlapping. Scan past occupied intermediate positions; continuity of collision-free placement is not required. The current y is a candidate only if legal. An activation with no candidate is rejected without spending. A horizontal/rotation candidate must be in bounds and admit at least one legal landing at or below that candidate y; otherwise reject it unchanged. For rotation, apply C3’s ordered kick offsets using this mode-aware candidate rule. Do not impose ordinary occupied-cell rejection on a penetrator’s intermediate cells.

Gravity/soft drop advances a penetrator one row toward its current deepest landing, including through overlap; it never advances beyond that landing. Hard drop moves directly to the same landing. Recompute the projection after a successful transform. A penetrator is grounded exactly at its legal deepest landing, even if the next row is in bounds but has no legal eventual placement. Apply C12’s selected policy at that landing. Activation recomputes the landing and grounding immediately, cancelling an ordinary support timer if the penetrator can descend farther. Under interval locking, moving away from that landing cancels grounding, while remaining grounded does not refresh it. No timer may lock an overlapping intermediate position. Final merge independently validates all cells before any write and occurs once; traversal does not alter, clear or recolor pre-existing cells. Ordinary row clearing after a legal merge still removes full rows.

A penetrator locks as white cells; those locked cells retain white as visual provenance but have the same blocking behavior as other locked cells. The next spawned piece is ordinary. Ordinary pieces and held ordinary previews never use white; an active penetrator is entirely white, and its visible ghost is a distinguishable white outline. Dynamic visibility applies C9 to this mode-aware landing.

## C11 — Bonus awards and indicator

Clearing exactly four rows in one settlement awards one bonus, for ordinary or penetrator placements, capped at three. Other clear counts award none. Apply the award after settlement/clear without duplicating it when a frame or input repeats. A spent bonus can be earned again by that piece’s four-line clear. Restart clears all bonuses. Display exactly one visible symbol such as a lightning bolt per available bonus in a labeled side panel, with a readable numeric count for accessibility; update in the same render as spending/earning.

Acceptance: a penetrator passes through a blocking shelf into a deeper legal cavity without changing shelf cells; hard drop and ghost select the same deepest legal y. No landing, wall/out-of-bounds transforms and repeated P are harmless rejections. Overlapping intermediate frames cannot lock; legal landing and 999/1000 ms timing settle once. Hold is ignored while active without a bag draw. Four-line clears award once at counts 0/2/3 (results 1/3/3); spending reduces once and updates symbols. Restart resets bonuses/mode, white is confined to penetrator-origin cells, and the next ordinary spawn restores normal collision rules.

## C12 — Session-only settings

Show labeled HTML controls in the side panel for these settings. Initialize each to its default on page load. Keep them for the current page session, including restart and game over; reload restores defaults. Do not read/write cookies or query parameters for settings. Do not add local/session storage persistence.

| Setting | Allowed values / visible meaning | Default |
| --- | --- | --- |
| Ghost visibility | hide / dynamic / show | dynamic |
| Hard drop | on / off | on |
| Next-piece preview | on / off | on |
| Hard-drop locking | follow / immediate | follow |
| General locking | failed-down / interval | interval |
| Hold | on / off | on |

Ghost hide draws no ghost; show draws any running-game projection; dynamic uses C9. Always use the same ordinary/penetrator landing regardless of visibility mode. Hard drop off ignores Space entirely; on performs C8/C10 placement. Hard-drop locking follow applies the selected general policy; immediate settles once at the legal landing on that hard-drop command. This policy affects future commands, never retroactively locks a piece dropped earlier.

General failed-down settles on the next failed downward movement attempt, from gravity, soft drop or an enabled hard drop at its landing. Hard drop counts as a downward attempt to its landing; therefore follow + failed-down locks at that command’s landing, including zero distance. Interval uses C8’s full gravity interval from grounding. On an actual general-policy change, cancel the previous timer: if the new policy is interval and the active piece is grounded, start a fresh full interval; otherwise no timer runs. Switching to failed-down waits for the next failed downward attempt. Selecting the same value is a no-op and cannot refresh time.

Hold off ignores C without losing the stored shape, consuming source state or changing eligibility. Hold on restores access subject to the existing once-per-piece and penetrator restrictions. Preview on shows exactly the next shape that a source-consuming spawn would use; off hides it. A held-piece swap does not consume that preview. Repeated preview toggles do not advance/shuffle the draw stream. Both visible previews use ordinary non-white shape colors.

A setting change updates controls and relevant drawing immediately and affects subsequent commands without changing score, board, bonuses or active geometry. Only the specified policy switch changes timing. Settings changed after game over apply to the next restart without resuming play. Keep game key handling away from focused form controls. Invalid programmatic/DOM values are rejected without changing the current valid setting. The configuration panel must fit the supported desktop layout without covering the board.

Acceptance: exercise each value and all combinations of general policy × hard-drop policy × hard-drop enablement on ordinary and penetrator landings. Check 999/1000 ms after a real switch, same-value no-op, a switch while airborne/overlapping/game over, and a future hard drop after policy change. Verify ghost modes at rows 9/10, Hold-off retention, ignored active-penetrator Hold, stable preview at bag boundaries, preserved board/score/bonus state and restart retention. Reload restores all defaults and leaves cookies/query state untouched.
