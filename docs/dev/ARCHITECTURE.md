# Architecture

[PROJECT](PROJECT.md) defines the intended player experience. A static TypeScript client separates deterministic gameplay from browser input, scheduling and presentation.

## Blocks and dependency direction

| Block | Responsibility and permitted dependencies |
| --- | --- |
| Game model | Owns board, active piece, source state, score and lifecycle. Pure transitions depend only on geometry and an injected random stream; no DOM, Canvas, clock or storage imports. |
| Browser controller | Owns the animation frame subscription, focus and keyboard listeners. Converts events and monotonic elapsed time into model commands, then renders the returned snapshot. Depends on the model and presentation. |
| Presentation | Canvas draws the board and active piece; HTML presents score, instructions, restart and game-over status. Reads snapshots and emits semantic actions; never mutates game state. |
| Build and checks | TypeScript checking, a static development/build toolchain, model tests and isolated browser automation. Test drivers use the same public commands and snapshots as the controller. |

## State and interaction

One game session has a single model owner. The controller feeds ordered actions into synchronous transitions, receives immutable/read-only snapshots and paints after updates. A timer tick and an input action are serialized; line clear, scoring and replacement spawn form one atomic transition. Restart creates a fresh model and clears elapsed-time bookkeeping without adding another loop or listener set.

Inject the random source and elapsed time rather than reading global randomness or wall time inside rules. This makes geometry, line clearing and replayed input sequences deterministic under tests. The controller bounds catch-up after focus loss and clears held-key state, preventing background elapsed time or a stuck key from unexpectedly ending a game.

## Boundaries and tradeoffs

Canvas is a compact fit for the cell grid; standard HTML gives labels and focus behavior to controls. Full DOM cell rendering is possible but adds elements without improving the chosen keyboard experience. A framework or event bus is unnecessary: one controller and explicit state transitions keep ownership legible. Avoid a generic plugin system for game rules.

The game uses no network service and stores no game session. Binaries, browser profiles, build output and generated test evidence are outside tracked production source. Browser provisioning and execution obey the root browser policy.

## Architectural invariants

- Only the model writes the board and score. Failed commands leave state unchanged.
- Every locked cell belongs to an in-bounds board position; ordinary active pieces never overlap locked cells.
- Rendering and UI state cannot alter gameplay rules. A snapshot contains everything necessary to draw one frame.
- The controller disposes all owned resources on teardown and never schedules concurrent game loops.

Detailed responsibilities and verification seams are in [DECOMPOSITION](DECOMPOSITION.md); behavioral contracts belong in [SPEC](SPEC.md). These documents describe intended structure, not implemented modules.

## Piece controls and landing

The model owns the shuffled bag cursor, held shape and Hold eligibility. A geometry-owned landing projection is shared by the ghost and hard drop; the renderer does not independently scan the board. Lock timing belongs to session state and receives simulation time from the controller, so tests can exercise exact boundaries without a browser clock. Separate rotation candidate search, source generation and timing policy as focused functions, without a runtime rule-plugin framework.

## Projection visibility

Presentation derives ghost visibility from the geometry-owned landing cells using a pure visibility predicate. Recompute after every state transition; visibility has no effect on collision, hard drop, source or lock state. Use board coordinates rather than rendered pixel height so resizing does not alter the midline rule.

## Penetrator policy

Geometry owns both ordinary and penetrator placement/projection under an explicit piece-mode argument. Penetrator traversal ignores occupied cells but requires an in-bounds candidate with a legal future landing; final board merge always enforces non-overlap. The session owns bonus count and the active mode, spends/awards them atomically, and gates Hold. The view consumes mode and bonus state, drawing white penetrator cells and the symbol count. Keep traversal policy separate from board merge validation so permissive falling cannot leak into locked-state writes.
