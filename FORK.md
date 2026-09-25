# frostyard/hotedge fork

This fork of `jdoda/hotedge` is built by Snow from a pinned `frostyard/hotedge` commit via `frostyard/snosi`.

## Upstream and fork point

The upstream repository is `jdoda/hotedge` on GitHub. The fork point (merge-base of `origin/main` and `upstream/main`) is `ca68bc5` ("Add preference to turn off animation"), dated 2025-10-23.

## Commits on this fork that upstream does not have

There is exactly one commit on the fork's main that upstream does not have: `5411c3802803dbaafc6d8014b3a983027dab4704` ("Add compiled gschema file"), authored by Kyle Gospodnetich on 2025-10-29. It adds the binary `schemas/gschemas.compiled` (805 bytes). Upstream's `.gitignore` excludes `schemas/gschemas.compiled` and `build/`; upstream never commits this file.

**Inferred reason, not recorded rationale:** Reading `shared/snow/scripts/build/hotedge.chroot` in snosi against upstream's Makefile suggests why the commit exists. The snosi script downloads the pinned source archive, runs `tar -xzf`, and copies the tree straight into the image with `cp -a`; it never runs `glib-compile-schemas`. Upstream's Makefile `install` target depends on `schemas/gschemas.compiled`, built via `glib-compile-schemas schemas/`, before `gnome-extensions install` runs. Because snosi skips that step, the extension's settings schema would not be usable at runtime unless the compiled schema file already ships in the tree. The fork's sole commit supplies that build artifact directly in git for the raw-copy install. No commit message, PR description, or issue explicitly states this rationale.

## Is the pinned commit the fork's current main?

Yes. The fork's `origin/main` tip is `5411c3802803dbaafc6d8014b3a983027dab4704`, exactly the hotedge commit pinned in snosi's `shared/download/image-checksums.json`. There is nothing newer on the fork that Snow does not already ship.

## How far upstream has moved

The fork point is dated 2025-10-23; upstream's `upstream/main` tip is `1a92b82` ("fix readme for #44"), dated 2026-08-29. Upstream has moved for over 10 months since the fork point, while the fork added only its one gschema commit a few days later (2025-10-29). The fork lacks these 10 upstream commits:

- `d500d9c` Add support for Shell 50
- `02e859b` feat: Suppress overview when right or middle mouse button held
- `83994f4` feat: Add separate settings entry for each mouse button
- `0612efe` feat: Group button preferences in expander
- `1647af2` style: Make preference titles simpler
- `a8689f8` refactor: Rename new preferences
- `ee8627f` docs: Update readme
- `b234c80` fix: Make global mouse suppression toggle work
- `90e9cdd` revert: Remove per-button config
- `1a92b82` fix readme for #44

In particular, the fork lacks Shell 50 support and the mouse-button-suppression feature work. The fork's `metadata.json` declares `"shell-version": ["45", "46", "47", "48", "49"]`; upstream's current `metadata.json` declares `"shell-version": ["45", "46", "47", "48", "49", "50"]`. Upstream added Shell 50 support in `d500d9c`, which the fork/Snow does not yet have.

## Why the fork exists at all

**Unknown/unrecorded.** No rationale for creating the fork rather than using upstream directly is recorded in the consulted material (`frostyard/hotedge`, `frostyard/snosi`, or elsewhere). Brian recalls only that Kyle did the original hotedge integration work and that it implements something also done in Bazzite; no more detail is available. That recollection is not a recorded explanation for creating the fork.

## Recommendation

Rebase the fork on current upstream, bringing in Shell 50 support and the mouse-button-suppression changes, while keeping only the one gschema-compilation commit on top. Alternatively, change snosi's build step to run `glib-compile-schemas` itself; in that case the fork could be dropped entirely and Snow could point straight at `jdoda/hotedge`. Changing snosi's build step or its pin is out of scope for this document and must go through Murbella via Odrade.

This document makes no change to `extension.js`, `prefs.js`, `metadata.json`, schemas, the Makefile, or `frostyard/snosi`.
