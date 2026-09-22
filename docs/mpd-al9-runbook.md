---
title: MPD on AL9 runbook
---

# Developing a DUNE package with MPD on AL9

One path, start to finish, for building a DUNE/LArSoft package with Spack and
[MPD](https://github.com/FNALssi/spack-mpd) on an AL9 gpvm against a published
DUNE release environment. It was worked out between 8 and 19 September 2026
by rebuilding `dunereco` against `dunesw-10_22_00d01-justin-01_06_04-prototype`,
then checked with a clean rerun from scratch.

It came out of [#9](https://github.com/DUNE/dune-spack-project/issues/9) and
[FNALssi/spack-mpd#62](https://github.com/FNALssi/spack-mpd/issues/62). Thanks
to Patrick Gartung for the recipe-commit fix and to Kyle Knoepfel for the MPD
changes that followed.

!!! warning "Written against one release"
    The paths, commit and versions below are for
    `dunesw-10_22_00d01-justin-01_06_04-prototype` under
    `/cvmfs/dune.opensciencegrid.org/spack/v1.1.1`. The steps carry over to
    other releases; the values do not. If a step does not behave as described,
    comment on [#9](https://github.com/DUNE/dune-spack-project/issues/9).

## The four things that matter

Each makes every later step look broken until it is right.

1. **Create the subspack from `v1.1.1/setup-env.sh`.** The packages behind the
   `-prototype` environments live under `v1.1.1/opt/spack`. A subspack from the
   top-level (v1.2.2) script reuses nothing, tries to build ten packages and
   dies fetching `gcc-runtime@12.5.0` from the build cache.
2. **Check the `fnal_art` recipe repo out at the commit the release was built
   with.** For this release that is `f80d956792`. Newer recipes describe a
   dependency graph the installed tree does not contain, which produces
   `Missing dependency not in database` warnings and a view with no CLHEP in it.
3. **Do not pin versions by hand.** `require: ["@=..."]` entries taken from
   `spack find` name the newest *installed* version, not the one the
   environment uses (`larsim@10.20.06` where the environment has `10.20.05`),
   so they force rebuilds and shift every downstream hash.
4. **Force the step you changed.** `refresh` replays `local/spack.lock` and
   `build` skips configure when a build directory exists. Use `refresh -f`
   after any config change and `build --clean` after anything that affects
   configure.

!!! question "Open: is `-d cetmodules@3` required?"
    cetmodules 4 removed the deprecated CMake functions that legacy DUNE
    packages still call, so pinning cetmodules 3 was part of the first
    successful build. A later run appeared to succeed without it, but `-d` is
    stored in the MPD project config and had probably persisted from an earlier
    `refresh`. [Step 8](#8-the-cetmodules-experiment) settles it in a few
    minutes. Until then, keep the flag.

## Runbook

Set the variables once. Use a **new** `INST` for a clean run: instances are
cheap, and stale state is what costs hours.

```bash
ssh dunegpvmNN.fnal.gov
htgettoken -a htvaultprod.fnal.gov -i dune

INST=/exp/dune/data/users/$USER/mpd1
PROJ=my-project
ENVNAME=dunesw-10_22_00d01-justin-01_06_04-prototype
TAG=v10_22_00d01
PKG=dunereco
```

### 1. Subspack from the right parent

```bash
source /cvmfs/dune.opensciencegrid.org/spack/v1.1.1/setup-env.sh
echo "ROOT=$SPACK_ROOT"   # MUST contain /v1.1.1/
echo "ENV=[$SPACK_ENV]"   # MUST be empty, or subspack writes onto read-only CVMFS

mkdir -p "$INST" && cd "$INST"
spack subspack --with-padding "$PWD/spack"
source spack/setup-env.sh
echo "ROOT=$SPACK_ROOT"   # MUST now be $INST/spack
```

### 2. Make the release environments resolvable, then init MPD

Environment names are **instance-local**. `spack env list` on the current CVMFS
tree shows newer dunesw environments whose packages are not installed under
v1.1.1, and they are unusable here.

```bash
ln -s /cvmfs/dune.opensciencegrid.org/spack/v1.1.1/var/spack/environments/* \
      "$SPACK_ROOT/var/spack/environments/"   # not needed if you sourced v1.1.1
spack env list | grep -F "$ENVNAME"           # MUST print
spack env deactivate 2>/dev/null              # git-clone and refresh need no env active
spack mpd init
```

### 3. Pin the recipe repo

```bash
R=$(spack repo list | awk '/fnal_art/{print $NF}')
echo "$R"
git -C "$R" log -1 --format='%H %cd'   # record it, so you can go back
git -C "$R" fetch --all
git -C "$R" checkout f80d956792

spack config get packages | grep -c require   # expect no hand pins

spack spec larsim > spec.log 2>&1; head -3 spec.log   # expect [^] larsim@10.20.05
```

If the repo path is under `/cvmfs`, it is read-only: clone it yourself, check
out the commit, and `spack repo add` it ahead of the CVMFS copy.

!!! warning "Do not pipe Spack into `head`"
    `spack spec larsim | head -1` dies with `BrokenPipeError`. `head` exits
    after one line and closes the pipe while Spack is still writing, usually
    mid-fetch of the compiler cache. The solve itself is fine. Redirect to a
    file first, as above. The same applies to `grep -m1`; plain `grep`, `awk`
    and `sed -n 1p` are safe because they read to the end.

### 4. Project and source

```bash
spack mpd n -C gcc@12.5.0 -T "$INST/$PROJ" -E "$ENVNAME" -d cetmodules@3
spack mpd git-clone "$PKG"

cd "$INST/$PROJ/srcs/$PKG"
git fetch --tags
git checkout -b my-feature "$TAG"   # git-clone lands on develop
git describe --tags                 # MUST print exactly $TAG
cd "$INST"
```

!!! tip "Fail instead of silently rebuilding"
    Since [FNALssi/spack-mpd#62](https://github.com/FNALssi/spack-mpd/issues/62),
    MPD has a `--require-reuse` option for `new-project`. With it, `refresh`
    stops if the project's dependencies cannot be reused from the `-E`
    environment, rather than quietly choosing newer versions and rebuilding
    them. It needs a version of spack-mpd from 15 September 2026 or later.
    MPD's [Creation](https://github.com/FNALssi/spack-mpd/blob/main/doc/Creation.md)
    page explains what `-E` does.

### 5. Concretize, and read the log before answering

```bash
spack mpd refresh -f generator=ninja 2>&1 | tee refresh.log
grep -c "not in database" refresh.log   # expect 0
```

A correct run, checked on 18 September 2026:

- **Reused** from `/cvmfs/.../v1.1.1/opt/spack/`: `larsim@10.20.05`,
  `larreco@10.06.07`, `larana@10.02.07`, `larpandora@10.04.00`,
  `larrecodnn@10.04.00`, `artg4tk@13.00.00`, `art@3.14.04`, `geant4@11.2.2`,
  `cetmodules@3.27.04`, `clhep@2.4.7.1`.
- **Built from source**: `dunecore`, `dunecalib` and `duneprototypes` at
  `10.23.00d01`, about 8 minutes at 4 cores. Their recipes come from a
  different repo than `fnal_art`, so that layer is still unpinned.
- **Answer the core-count prompt with the number you want.** MPD keeps it in
  the project config, and `-j` on `build` does not override it.

### 6. Build

```bash
spack mpd build --clean -j12 2>&1 | tee build1.log
```

`--clean` is required on a first build, and again after adding any source
file, because `art_make` globs `.cxx` at configure time. Deprecation warnings
from cetmodules are expected. `-Werror` is on, so anything that stops the build
is a real error.

!!! note "There is no 'build complete' message, and the output arrives out of order"
    A finished build ends with the last `[N/N] Linking ...` line and nothing
    else. Under `| tee`, the `==> Configuring with command:` and
    `==> Building with command:` banners appear **after** the ninja output,
    which reads as if the build were starting over. MPD's Python messages are
    block-buffered when stdout is a pipe and flush at exit, while ninja writes
    straight to the terminal.

    Confirm completion instead of looking for a message:

    ```bash
    ninja -C "$INST/$PROJ/build"   # "ninja: no work to do." means every target is current
    ```

    One run built at `-j4` although 12 was given at the `refresh` prompt and
    `-j12` was passed to `build`. The first full `dunereco` build is 730
    targets, so this matters for wall time.

Then check the plugins are visible:

```bash
spack mpd select -p "$PROJ"
lar --print-available module | grep -i <YourModule>
echo "$CET_PLUGIN_PATH" | tr ':' '\n' | head   # project build/install first
```

### 7. Resuming after a break

The instance, the project and the repo checkout all persist. A fresh shell
needs only:

```bash
source "$INST/spack/setup-env.sh"   # NOT the CVMFS script
spack mpd list                      # project listed?
spack mpd select -p "$PROJ"         # marker moves to it
git -C "$(spack repo list | awk '/fnal_art/{print $NF}')" log -1 --format='%H'
```

`refresh` and `build` both refuse to run with no project selected, and
selection does not carry across instances.

### 8. The cetmodules experiment

A few minutes, and nothing compiles.

```bash
spack mpd n -C gcc@12.5.0 -T "$INST/probe-nocet" -E "$ENVNAME" -S "$INST/$PROJ/srcs"
spack mpd refresh -f generator=ninja 2>&1 | tee nocet.log   # answer n at the prompt
grep -c "not in database" nocet.log
grep -E "^\[.\]" nocet.log | grep -E "cetmodules|clhep"
spack mpd select -p "$PROJ"                                  # switch back
```

A clean result means `cetmodules@3` is not needed. Warnings coming back, or
`cetmodules@4` being chosen, means `-d cetmodules@3` is load-bearing and
belongs in every DUNE MPD recipe. Please post either answer on
[#9](https://github.com/DUNE/dune-spack-project/issues/9).

## When something looks broken

| Symptom | Cause | Fix |
|---|---|---|
| `gcc-runtime@12.5.0` in the install list | subspack has the wrong parent | start over at [step 1](#1-subspack-from-the-right-parent); do not work around it |
| LArSoft packages one patch ahead of the environment | `fnal_art` not at the release commit | [step 3](#3-pin-the-recipe-repo) |
| Nine packages to build, right versions, wrong hashes | a `-d` root or a hand pin perturbed the dependency graph | remove the pins; compare against `spack find -l` |
| `Missing dependency not in database`, `find_package(CLHEP)` fails | the same recipe drift, one layer down | [step 3](#3-pin-the-recipe-repo), then `refresh -f` |
| Identical output after a config change, hashes included | stale `local/spack.lock` | `spack mpd refresh -f` |
| `Project <name> is up-to-date` after a config edit | MPD compares only its own project config | `spack mpd refresh -f` |
| `ninja: error: loading 'build.ninja'`, no `Configuring with command:` above | build directory exists but configure never ran | `spack mpd build --clean` |
| No "build complete"; banners print after the ninja output | MPD's Python output is buffered under `tee` | `ninja -C build` should say "no work to do" |
| Builds at `-j4` though 12 was given | core count stored at the `refresh` prompt | open question |
| `spack spec ...` piped into `head` gives `BrokenPipeError` | `head` closes the pipe mid-fetch | redirect to a file first |
| `An MPD project must be selected` | new shell, or a different instance | `spack mpd select -p <project>` |
| Deprecated-CMake warning spam | DUNE packages call legacy cetmodules functions | expected; not a symptom |

## Still open

- **The `dune*` recipe layer is unpinned.** `dunecore`, `dunecalib` and
  `duneprototypes` build from source every time.
- **Recipe commits are not published.** Nothing in the environment directory
  records which `fnal_art` commit built it; it had to be looked up by hand.
- **Development-cycle cost.** Concretize plus build of one package took over
  30 minutes, the number to compare against the 1 October "Spack and MPD no
  worse than MRB" milestone.
- **Whether `cetmodules@3` is required** ([step 8](#8-the-cetmodules-experiment)).

## Fallback: SL7 Apptainer and `mrb`

The SL7 container with `mrb` still works (checked 7 September 2026). Use it
if the Spack path stalls.
