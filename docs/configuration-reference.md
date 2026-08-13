---
title: Configuration reference
---

# Configuration reference

!!! info "Hand-written, not generated"
    Neither Zensical nor the wider MkDocs ecosystem currently has a plugin
    that generates documentation from Spack YAML files — `mkdocstrings`
    (Zensical's docstring-autodoc integration) only covers Python source, not
    YAML. This page is written and kept up to date by hand; if it drifts
    from the actual files, the files are authoritative.

What each YAML file across `dune-spack-config`, `dune-spack-envs`, and
`dune-release-configs` actually controls — see [Repositories](repositories.md)
for what the repos themselves are for.

## `dune-spack-config`

Core Spack configuration, included by environments rather than used
standalone.

| File | Controls |
|---|---|
| `config.yaml` | General Spack settings — install tree root and path padding. |
| `packages.yaml` | Global package preferences — currently just target architecture (`x86_64_v3`). |
| `repos.yaml` | Which package repositories Spack pulls recipes from (see table below). |
| `included_yaml/packages-dune.yaml` | DUNE package version pins, keyed by short package names (`dune-ana`, `dune-core`, ...). **Actually referenced** by `dune-spack-envs`. |
| `included_yaml/specs-dune.yaml` | The `dune_specs` spec list built from those same packages. **Actually referenced** by `dune-spack-envs` (`specs: - $dune_specs`). |
| `included_yaml/dune-packages.yaml` | Identical content to `packages-dune.yaml` above — appears to be an unreferenced duplicate/older copy. |
| `included_yaml/dune-specs.yaml` | Same package list as `specs-dune.yaml`, but under a `specs_dune` key (not `dune_specs`) — not the key `dune-spack-envs` actually pulls in. Also appears unreferenced. |

`repos.yaml` package-repo pins:

| Name | Source | Branch |
|---|---|---|
| `dune` | [`DUNE/dune_spack`](https://github.com/DUNE/dune_spack) | `spack-v1.0` |
| `builtin` | [`FNALssi/spack-packages`](https://github.com/FNALssi/spack-packages) | `fnal-v2025.11` |
| `art` | [`FNALssi/fnal_art`](https://github.com/FNALssi/fnal_art) | `develop` |
| `larsoft` | [`LArSoft/larsoft-spack-recipes`](https://github.com/LArSoft/larsoft-spack-recipes) | `main` |
| `nusoft` | [`NuSoftHEP/nusofthep-spack-recipes`](https://github.com/NuSoftHEP/nusofthep-spack-recipes) | `spack-v1.0` |
| `phlex` | [`Framework-R-D/phlex-spack-recipes`](https://github.com/Framework-R-D/phlex-spack-recipes) | `main` |
| `artdaq` | [`art-daq/artdaq-spack`](https://github.com/art-daq/artdaq-spack) | `artdaq/Spack1.1` |
| `wirecell` | [`gartung/wire-cell-spack`](https://github.com/gartung/wire-cell-spack) | `master` |
| `sbn` | [`gartung/sbn-spack`](https://github.com/gartung/sbn-spack) | `spack-v1.0` |
| `scd` | [`fnal-fife/scd_recipes`](https://github.com/fnal-fife/scd_recipes) | `master` |

## `dune-spack-envs`

Two Spack **environment** manifests (`spack.yaml`), each installable on its
own with `spack env activate` — these are what you actually build from, as
opposed to the configuration they include.

### `dune-development/spack.yaml`
The general DUNE development environment. Includes external release-configs
pinned by tag (gcc-12 toolchain, LArSoft `larsoft-10.11.01`, NuSoftHEP
`nusofthep-3.17.01`, art `art-3.14.04`, FIFE `2.8.0.1`), each with a
`sha256` checksum, plus the two locally-referenced DUNE files above
(`packages-dune.yaml`, `specs-dune.yaml`). Uses `concretizer: unify: true`
(single consistent dependency resolution across the whole environment).

### `dune-workflow/spack.yaml`
A fully version-pinned LArSoft/art stack for workflow/production use — no
external includes, every package version and build option (compiler,
`build_type`, `cxxstd=17`, `generator=ninja`, variants like `~davix`) spelled
out directly in the file. This is the more concrete, "exactly what gets
built" counterpart to `dune-development`'s include-based approach.

## `dune-release-configs`

Release-scoped configuration and publishing tooling — distinct from
`dune-spack-envs` above, which covers day-to-day development/workflow, not
tagged releases.

| File | Controls |
|---|---|
| `dune-release.yaml` | The release environment definition — includes DUNE, LArSoft, NuSoftHEP, art, and FIFE configs each pinned to a specific **git tag** (not branch), e.g. `dunesw-10.22.00d01-justin-01.06.04`. Currently marked `config: deprecated: true` in the file itself. |
| `included_yaml/dune-packages.yaml` | DUNE package version pins for this release, keyed by full package names (`duneana`, `dunecore`, ...) — a newer/higher version set than `dune-spack-config`'s copy (e.g. `dunesw @=10.22.00d01` vs. `@=10.11.01d00`). |
| `included_yaml/dune-specs.yaml` | The matching `specs_dune` spec list, also including a few extra packages not in `dune-spack-config`'s version (`gfal2-util`, `r-m-dd-config`, `py-merge-utils`, `snakemake`). |
| `bin/add-dune-checksums.sh` | Adds version pins + checksums for a DUNE package (`dunesw`, `duneanaobj`, `dunedaqdataformats`, `dunedetdataformats`, `dunepdlegacy`) into the release recipes. |
| `bin/publish-dune-environment.sh` | Publishes a built Spack environment to CVMFS (`/cvmfs/dune.opensciencegrid.org/spack/...`) from a Jenkins build. |

## Open question

`dune-spack-config`'s `included_yaml/dune-packages.yaml` and
`dune-specs.yaml` look like stale duplicates of `packages-dune.yaml` and
`specs-dune.yaml` — same content, different filename/key, and not actually
referenced by either `dune-spack-envs` manifest. Worth confirming with the
repo maintainer whether they're safe to remove, rather than guessing here.
