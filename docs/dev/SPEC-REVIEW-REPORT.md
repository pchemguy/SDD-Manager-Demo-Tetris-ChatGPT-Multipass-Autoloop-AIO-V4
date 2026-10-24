# SPEC review report

## Current gate

State: Ready for the reviewed spec boundary. Pass 5. `PROJECT.md` SHA-256 `d8e46642abf6a5649038fc64ead12a4a5df19dea1e84eaa484108cfdffac957f`; `ARCHITECTURE.md` SHA-256 `c7b959f5e8382f31cc60529c54e9681b8d12c3f1b01435841750d0bc96cedca1`; `DECOMPOSITION.md` SHA-256 `e3af347a18ef90d72f1a960e3a6a37caa9bff27affe728f5206ea99e4dce27e2`; `SPEC.md` SHA-256 `c3ad6a470df4d3a8e2262c8c10dc32f98b1a103d20f053a993e0b64de0ff4a5b`.

C12 maps value validation to settings, policy transitions to session, command gating to input/session and presentation to controls/ghost/status. Reviewed complete finite domains and defaults, full policy cross-product, no retroactive locking, same-value timing, preview/hold source stability, form focus, restart/reload and no persistence. Located C5 color wording ambiguity (S-COLOR-01): made the ordinary/penetrator exception explicit; rechecked C1/C5/C10 white contracts, resolved. Activation grounding is explicitly recalculated. No remaining blockers; prior PLAN/TASKS ranges are unchanged.

## Initial review

2026-10-24, artifact owner review. Pass 1. `PROJECT.md` SHA-256 `e6e2c214d62582f96f847144decc7168608cf7efdb3dd47fc6d912c8eb40cf37`; `ARCHITECTURE.md` SHA-256 `67f5f3d4f77c3b8a0cca8308dd56ab441fa67c5f28a43ec2e9c10094ad3746ff`; `DECOMPOSITION.md` SHA-256 `f5e7e083c318ff1a6f25cb18bd933852aeaee1f8e8afe418dd5ec36b6af4ff58`; `SPEC.md` SHA-256 `a42f0f4f81623c13b197aff5ecb4d9cde4a8bd33154345bf3b29f91cd64688ff`.

C1–C4 map to geometry, board, source and session; C2/C5 map to input/controller and view; C6 maps to dependency injection, resource ownership and isolated browser evidence. Reviewed baseline-only coverage, coordinates, seeded source boundaries, rejection semantics, scoring, spawn/restart, timing, blur and rendering acceptance. No future enhancement contracts adopted. No unresolved findings.

Confirmed unresolved findings: none. This is per-document conformance review; no implementation, browser execution or whole-package QC is claimed.

## Revision 1

Pass 2 authoring/recheck. `PROJECT.md` SHA-256 `350b6d8ddd0d98b41db504cd99401fd334261d40be412d9ed5ef30e7f45b8f9e`; `ARCHITECTURE.md` SHA-256 `4ef69dd2f9d355a89e2535690c24d3889d10c746810399f31841f40c508da7d4`; `DECOMPOSITION.md` SHA-256 `741b52e96c4d874ba171771dc174e0fe3f68345cc2da355828f0f4cb8dc9b3c2`; `SPEC.md` SHA-256 `347dd3035fb3ed5dab05177f74ac3389fe05bec2f38e68dfce6183d1983880a8`.

C7 maps bag generation to source and Hold state to session/status; C8 maps projection to geometry and timer ownership to session, with controller-supplied simulation time. C1–C6 were checked for current rotation/source/timing consistency. Covered bag boundaries, once-per-piece Hold, blocked replacement, ghost/drop equality, supported shifts, ungrounding, timer expiry and event serialization. No unresolved findings; controls, score and browser policy remain coherent.

Confirmed unresolved findings: none. No runtime checks performed; document readiness only.

## Revision 2

Pass 3 authoring/recheck. `PROJECT.md` SHA-256 `1ce5266517d6432f04353d807ad5642fa78d2c57be013352a28103b4b071abe3`; `ARCHITECTURE.md` SHA-256 `81fee8c3f146ac0dc0af9e51cdc2c490f8336613a6015479df36c27f047d6e24`; `DECOMPOSITION.md` SHA-256 `d764fc0e6064547b3d8847894c41321adbbf3706f449aa1d31b4e13dc8ec32f6`; `SPEC.md` SHA-256 `5ce6a6128863ba069de2ec09465164645f646b40dd81efd9d5f24c4a9dbe2394`.

C9 maps to the presentation predicate and existing geometry projection. Reviewed exact row-9/10 and straddling semantics, transition refresh, no ghost after game over, pixel-independence and hidden hard-drop equality. C1–C8 ownership and behavior remain consistent; no new model state or unresolved findings.

Confirmed unresolved findings: none. No runtime checks performed; document readiness only.

## Revision 3

Pass 4 authoring/recheck. `PROJECT.md` SHA-256 `be0c581fa00ac968b75f73920a77e534bf152d54ecea9dca271e1637954bbb88`; `ARCHITECTURE.md` SHA-256 `c1e46db70939fccbe3f7df53adb56b1bf1c001cf6df8a99859edfaec8d39067a`; `DECOMPOSITION.md` SHA-256 `ff3243f1e576416dcfb68c15e99f45a0b1702c17b5c873cc8c2e791f0e2374f7`; `SPEC.md` SHA-256 `95fa2d52f33ceba291209612c84a2b3eba9290b97b8bbd7bb3ab8e9f1f935ee8`.

C10 maps mode-aware projection to geometry, safe final merge to board, mode/Hold/timer transitions to session and white drawing to renderer. C11 maps awards/cap to session and symbol count to status. Rechecked C1 color constraint, input mapping, C7 Hold restriction and C8 grounding so they cannot contradict traversal through overlap. Shelf/cavity/no-landing fixtures, repeated activation, restart, award cap and ghost/drop/visibility interactions have objective acceptance. No unresolved findings.

Confirmed unresolved findings: none. No runtime checks performed; document readiness only.

## Revision 4

Pass 5 authoring/recheck. `PROJECT.md` SHA-256 `d8e46642abf6a5649038fc64ead12a4a5df19dea1e84eaa484108cfdffac957f`; `ARCHITECTURE.md` SHA-256 `c7b959f5e8382f31cc60529c54e9681b8d12c3f1b01435841750d0bc96cedca1`; `DECOMPOSITION.md` SHA-256 `e3af347a18ef90d72f1a960e3a6a37caa9bff27affe728f5206ea99e4dce27e2`; `SPEC.md` SHA-256 `c3ad6a470df4d3a8e2262c8c10dc32f98b1a103d20f053a993e0b64de0ff4a5b`.

C12 maps value validation to settings, policy transitions to session, command gating to input/session and presentation to controls/ghost/status. Reviewed complete finite domains and defaults, full policy cross-product, no retroactive locking, same-value timing, preview/hold source stability, form focus, restart/reload and no persistence. Located C5 color wording ambiguity (S-COLOR-01): made the ordinary/penetrator exception explicit; rechecked C1/C5/C10 white contracts, resolved. Activation grounding is explicitly recalculated. No remaining blockers; prior PLAN/TASKS ranges are unchanged.

Confirmed unresolved findings: none. No runtime checks performed; document readiness only.
