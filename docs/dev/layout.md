# Physical layout

This maps the intended [component model](DECOMPOSITION.md) to source ownership. Except existing repository policies, package files and documentation, listed implementation paths are planned and need not exist yet.

| Paths | Owner and purpose | Checks / boundary |
| --- | --- | --- |
| `src/game/types.ts`, `geometry.ts` | Model records, tetromino offsets, transforms, collision queries | `tests/unit/geometry.test.ts`; no browser imports |
| `src/game/board.ts` | Locked grid, merge/clear/compaction | `tests/unit/board.test.ts` |
| `src/game/source.ts` | Injected random piece source | `tests/unit/source.test.ts` |
| `src/game/session.ts` | Session transitions, score, lifecycle and timing state | `tests/unit/session.test.ts`; `tests/fixtures/` owns deterministic board/source fixtures |
| `src/browser/input.ts`, `controller.ts` | DOM input/focus and animation lifecycle | Focus/teardown integration checks; imports game and UI only in the outward direction |
| `src/ui/renderer.ts`, `status.ts`, `styles.css` | Canvas geometry, HTML status/control rendering and styles | `tests/browser/` owns app interaction/rendering checks |
| `src/main.ts`, `index.html` | Composition root and static entry | Build/browser smoke path |
| `package.json`, lockfile, `tsconfig.json`, `vite.config.ts`, `playwright.config.ts` | Pinned toolchain and reproducible commands | Tooling owner; no global installs or speculative command claims |
| `README.md`, `docs/dev/` | User entry and canonical development documents/reports | Adjacent review reports own document QC; phase reports use `docs/dev/reports/phases/<id>/` |
| `.github/workflows/` | Reproducible CI/build checks and authorized release automation where needed | Same dependency lock and browser policy as local checks |

## Generated and retained material

Ignore `node_modules/`, `dist/`, dedicated browser binaries/profiles, test results and screenshots; retain reproducible evidence summaries in owning task/review reports. Keep credentials excluded by root `*.tkn`. Do not modify bundled plugin files as part of game development. Manual conversations belong under docs/ai-logs only when explicitly recorded; exclude them from ordinary source/QC inputs.

Tests mirror responsibilities, not every file. Shared fixtures have no production consumers. No test-only state controls appear in the public player UI. Root [AGENTS](../../AGENTS.md) and browser policy remain controlling; commands become documentation only after validated setup.

## Piece assistance ownership

`src/game/geometry.ts` owns kick candidate order and the landing query, with focused geometry fixtures; `source.ts` owns bag state. `session.ts` owns Hold and lock timing, tested by `tests/unit/session.test.ts`. `src/ui/renderer.ts` draws the ghost; `status.ts` draws Hold; `input.ts` supplies C/Space actions. `tests/browser/` covers Hold/input, landing equality and visible ghost output. These extend existing cohesive owners rather than duplicate projections in the view.
