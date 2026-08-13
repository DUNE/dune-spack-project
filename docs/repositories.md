---
title: Repositories
---

# Spack-related repositories

What each repo actually holds, so you can tell them apart at a glance. All
are Apache-2.0 unless noted.

## DUNE-maintained

### [`dune-spack-config`](https://github.com/DUNE/dune-spack-config)
Core Spack configuration: `config.yaml` (general Spack settings),
`packages.yaml` (external/preferred packages), `repos.yaml` (which package
repositories Spack should pull recipes from). This is the base layer
everything else builds on — start here to see what Spack itself is
configured to know about.

### [`dune-spack-envs`](https://github.com/DUNE/dune-spack-envs)
Spack **environment** definitions — the actual sets of packages installed
together for a given purpose, split into `dune-development/` and
`dune-workflow/` directories, plus its own `packages.yaml`. Use this to see
or change what's installed in a specific DUNE Spack environment, as opposed
to Spack's general configuration (`dune-spack-config`, above).

### [`dune_spack`](https://github.com/DUNE/dune_spack)
The package repository itself: `package.py` build recipes for DUNE's
offline software (LArSoft-based stack). This is where you'd add a new
package or fix how an existing one builds. See also the
[`dunesw` wiki](https://github.com/DUNE/dunesw/wiki) for the broader
LArSoft-based software stack this repo builds.

### [`dune-release-configs`](https://github.com/DUNE/dune-release-configs)
Spack configuration scoped to tagged **releases** rather than day-to-day
development — `bin/` scripts plus `included_yaml` and `dune-release.yaml`.
Distinct from `dune-spack-envs`: that repo covers development/workflow
environments, this one covers what actually ships in a release.

## Fermilab-maintained (SSI)

### [`FNALssi/spack`](https://github.com/FNALssi/spack)
A genuine [fork](https://github.com/spack/spack) of upstream Spack,
maintained by Fermilab Scientific Software Infrastructure (SSI) — this is
the Spack core DUNE actually runs. See [Upstream Spack and the Fermilab
fork](index.md#upstream-spack-and-the-fermilab-fork) on the home page for
how it relates to `spack/spack`.

### [`FNALssi/spack-at-fnal`](https://github.com/FNALssi/spack-at-fnal)
Documentation describing Spack use at Fermilab generally (not DUNE-specific)
— useful background once you're past the DUNE-specific setup steps.

### [`FNALssi/fermi-spack-tools`](https://github.com/FNALssi/fermi-spack-tools)
Tooling for setting up and managing Spack instances at Fermilab — relevant
if you're standing up or administering a Spack instance rather than just
using one.

### [`FNALssi/spack-mpd`](https://github.com/FNALssi/spack-mpd)
The MPD (multi-package development) Spack extension — see
[MPD](index.md#mpd-multi-package-development) on the home page for what it
does and how it relates to the old `mrb` workflow.

## Issues

The four DUNE-maintained repos above, plus `FNALssi/spack-mpd`, feed the
[DUNE Spack & MPD Issues](https://github.com/orgs/DUNE/projects/37) board —
see [Issues](index.md#issues) on the home page. The other three
Fermilab-maintained repos don't: `FNALssi/spack` has issues disabled
entirely (report upstream to [spack/spack](https://github.com/spack/spack)
instead), and `spack-at-fnal`/`fermi-spack-tools` aren't currently tracked
on the board.
