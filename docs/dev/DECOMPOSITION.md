# Component decomposition

[ARCHITECTURE](ARCHITECTURE.md) owns the block model. These components refine its responsibilities; [SPEC](SPEC.md) owns exact behavior.

| Component | Provides / owns | Requires / excludes | Verification seam |
| --- | --- | --- | --- |
| Geometry | Tetromino cell offsets, candidate transforms, bounds and collision queries | Read-only board and piece input; does not mutate session state or score | Table-driven shape, rotation and boundary fixtures |
| Board | Fixed-size locked-cell grid; legal merge, simultaneous row removal and compaction | Valid piece placement; does not generate or move the active piece | Occupied cell fixtures and before/after row snapshots |
| Piece source | Next shape from injected random values | Random input in [0,1); owns source state; no rendering or global random access | Seeded/deterministic source sequences |
| Session reducer | Active piece, board, score, spawn/game-over/restart; ordered actions and atomic settle/clear/spawn | Geometry, board operations and piece source; no browser resources | Explicit state/action to state transition tests |
| Input adapter | Keyboard mapping, focus guards, repeat handling and semantic restart actions | DOM events and controller action sink; no geometry calculations | Browser key/focus interactions |
| Frame controller | Monotonic frame deltas, one loop, serialization and lifecycle disposal | Reducer and renderer; no duplicated gameplay state | Fake elapsed time and browser mount/restart/teardown cycles |
| Renderer and status view | Canvas cell drawing, score, game-over text and control labels | Read-only session snapshot; no writes to session | Pixel/shape assertions plus visible HTML checks |

## Collaboration and lifecycle

The input adapter delivers one semantic action at a time. The frame controller supplies simulation deltas, applies the reducer, and passes its resulting snapshot to the view. A rejected transform returns the unchanged state; it is not an exception. Programmer-invalid state is detected at the model boundary in tests rather than silently corrupting the board. Browser teardown releases listeners and animation subscriptions owned by the controller.

Board settlement validates all piece cells before any write. Completed rows are identified together and compacted once; score and the next spawn are computed from that result. Source state is advanced only when a piece is actually needed. Model test fixtures may construct a state directly, but production UI cannot mutate it.

## Design interface shapes

Use simple TypeScript data records for Board, Piece and GameState, and a closed Action union. A pure `step(state, action, random)` boundary or equivalent explicit dependency injection is sufficient; no inheritance hierarchy is required. Expose immutable views to presentation. Keep coordinate conversion in the renderer, board coordinates in geometry, and timing units in the controller/model command contract. SPEC settles exact coordinate, timing and rejection rules before implementation.

## Assisted piece lifecycle

Geometry provides ordered rotation candidates and the ordinary landing projection. Piece source owns a shuffled seven-shape bag and cursor. Session owns held shape, per-piece Hold eligibility, grounded-since timestamp and gravity accumulator; it clears timing at spawn and settles atomically. Renderer reads a derived ghost and optional held-shape snapshot; it has no authority to advance the bag. Hold uses the same spawn validation as normal generation. The input adapter maps Space to hard drop and C to Hold, filtering repeated activation. Test seams cover bag boundaries, blocked swaps, kick selection, landing equality and exact lock expiry.

## Ghost visibility responsibility

A pure presentation predicate consumes landing cell rows and returns show/hide. Geometry remains the sole landing owner; the view never substitutes another landing when hidden. Cover upper-only, lower-only and straddling fixtures, plus movement/Hold/restart updates. The board remains 20 rows, so its midpoint is independent of Canvas dimensions.
