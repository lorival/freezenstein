# ADR-0001: Which Wolf3D data format do we target?

## Context

Wolf3D exists in several versions (shareware, full game, Spear of Destiny). Each version
uses different data files and different slot counts. Before we write tools or content, we
must pick one version so packers and the reference engine all follow the same rules.

This choice is about the **file format only**. It does not decide how many levels we will
build. The first content goal is Episode 1 (ten levels). Unused map slots may stay as
simple stubs.

## Options

| Option | Pros | Cons |
|--------|------|------|
| **v1.4 full (`.WL6`)** | Matches [Wolf4SDL](https://github.com/fabiangreffrath/wolf4sdl) full-game build. Same idea as [Freedoom](https://freedoom.github.io/) (replacement data for a known format). Contract can be extracted from open engine source. | Large slot manifest. All eight output files must exist even when many slots are still placeholders. |
| Shareware (`UPLOAD`) | Smaller on-disk contract | Different files and engine build. Not Wolf4SDL full. Would need a separate baseline and tool chain. |
| Spear of Destiny | | Different game files. Effectively a different project. |
| Custom format | Total freedom | Needs engine changes. Breaks the data-only goal. |

## Decision

We will target **Wolfenstein 3D v1.4 full (registered)**:

- Eight `.WL6` data files in the layout expected by Wolf4SDL with `CARMACIZED` and `GOODTIMES`
- A machine-readable **baseline manifest** generated from Wolf4SDL source (slot names, limits, chunk order). Never from commercial Wolf3D data.
- A **compatibility manifest** that maps each stock slot to a planned Freezenstein replacement
- Validators and tests that fail if output drifts from the baseline

## Consequences

- All packers and validators will assume v1.4 full slot counts and chunk order
- We may ship Episode 1 content first. Other map slots can be minimal valid stubs
- Shareware and Spear layouts will not be supported
