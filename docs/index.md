---
title: Home
---

# DUNE Spack Project

Documentation, training material, issue tracking, and project coordination for
Spack development and use across DUNE — the docs/training/issues hub that was
missing alongside the existing spack config and package repos.

!!! info "Proof of principle"
    This site is an initial skeleton, built to validate the local Zensical build
    before committing to the full scope and a GitHub Pages deploy. Content below
    is a starting set, not exhaustive.

## Get started

- **Use the DUNE software stack** — start with the [DUNE Spack Cheat-Sheet](https://dune.github.io/FAQ/Spack) for everyday commands, then the [DUNE Computing Basics setup guide](https://dune.github.io/computing-basics/setup.html) for the CVMFS entry point and `dune-prototype` environment.
- **Develop or package DUNE software** — see the [DUNE Spack repositories](#dune-spack-repositories) below and the [Fermilab upstream and DUNE deployment](#fermilab-upstream-and-dune-deployment) section for where things live.
- **Learn Spack** — see [Spack tutorials](#spack-tutorials).
- **Report a problem or request documentation** — see [Issues](#issues).

## DUNE Spack repositories

| Repo | Purpose |
|---|---|
| [`dune-spack-config`](https://github.com/DUNE/dune-spack-config) | Standard configuration files for DUNE spack |
| [`dune-spack-envs`](https://github.com/DUNE/dune-spack-envs) | Environment configurations for DUNE spack |
| [`dune_spack`](https://github.com/DUNE/dune_spack) | The DUNE-specific Spack package repository |
| [`dune-release-configs`](https://github.com/DUNE/dune-release-configs) | Spack configuration files for DUNE releases |

## Fermilab upstream and DUNE deployment

Spack 1.x splits core from package recipes, so no single link is "the DUNE Spack instance" — these together describe where DUNE's deployment comes from:

- [FNALssi/spack](https://github.com/FNALssi/spack) — the Fermilab fork/source of Spack core that DUNE builds on
- [FNALssi/spack-at-fnal](https://github.com/FNALssi/spack-at-fnal) — Fermilab usage documentation
- [FNALssi/fermi-spack-tools](https://github.com/FNALssi/fermi-spack-tools) — tools for managing Fermilab Spack instances
- [DUNE Computing Basics setup guide](https://dune.github.io/computing-basics/setup.html) — the DUNE-specific CVMFS entry point and `dune-prototype` environment

## Spack tutorials

Curated by audience — for a DUNE training event, pin a tested tutorial edition and Spack version rather than linking `/latest/` directly.

- [Spack 101](https://spack-tutorial.readthedocs.io/en/latest/) — general entry point, reusable training container
- [Basic installation](https://spack-tutorial.readthedocs.io/en/latest/tutorial_basics.html) — new users
- [Environments](https://spack-tutorial.readthedocs.io/en/latest/tutorial_environments.html) — most relevant to DUNE users
- [Package creation](https://spack-tutorial.readthedocs.io/en/latest/tutorial_packaging.html) — DUNE package maintainers
- [Stacks](https://spack-tutorial.readthedocs.io/en/latest/tutorial_stacks.html) — release/deployment maintainers

## Migration tracking

Tracking DUNE's transition to newer Spack releases (Spack 1.0/1.1):
[Spack 1.0 migration project](https://github.com/orgs/DUNE/projects/23)
(org-wide GitHub Project board).

## Issues

Not yet decided where DUNE Spack dev/use issues should be filed — new `dune-spack-project` Issues once this repo exists on GitHub, or one of the existing spack repos (`dune-spack-config`, `dune_spack`). This section will point to the chosen location once settled.

## FAQ

DUNE computing FAQ lives in its own repo/site: [`DUNE/FAQ`](https://github.com/DUNE/FAQ)
(tracked as Project #19 in the org-wide GitHub Projects — see that repo for
Spack-adjacent questions not yet covered here).

## Related

- [Official Spack documentation](https://spack.readthedocs.io/) — upstream reference for Spack itself, not DUNE-specific
- [`dune-ghpandp-doc`](https://github.com/DUNE/dune-ghpandp-doc) — DUNE Collaboration GitHub Organization Policy and Procedures
