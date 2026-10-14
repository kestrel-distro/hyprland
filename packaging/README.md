# Kestrel Hyprland fork

This is `kestrel-distro/hyprland`, a fork of
[`hyprwm/Hyprland`](https://github.com/hyprwm/Hyprland). The fork carries the
small set of source changes Kestrel's lean build needs; the packaging recipe
that compiles it lives in `packaging/local/PKGBUILD`.

## Kestrel changes on top of upstream

- `CMakeLists.txt`: the global `add_compile_options(-fno-lto)` is removed so the
  distro build can use ThinLTO. Upstream disables LTO because it can break
  hyprpm plugins; Kestrel builds with `NO_HYPRPM=1`.
- `packaging/local/PKGBUILD`: the build recipe (Clang, ThinLTO, x86-64-v3,
  strip, no hyprpm).

## Keeping up to date

```
git remote add upstream https://github.com/hyprwm/Hyprland.git
git fetch upstream
git merge upstream/main
```

The GitHub "Sync fork" button does the same, merging upstream into `main`.
The only divergence is the one-line LTO change plus the packaging files, so
merges are normally clean.

## Building

```
cd packaging/local
makepkg -s -f --noconfirm --nocheck
```

The recipe builds the **pinned upstream release tarball** (currently `v0.56.2`),
not this fork's `main`: tracking upstream `main` needs the matching hypr*-git
libraries, which Kestrel does not ship, so the release is pinned to stay ABI-
compatible with Arch's stable hyprutils/hyprlang/aquamarine. Bump `pkgver` to the
next release that matches the Arch libraries. The package is named
`hyprland-kestrel` and conflicts with/replaces/provides `hyprland`, so Arch's
stock package can never install or overwrite it; updates come only from the
Kestrel `[kestrel]` repository. The installed binary and files are unchanged.

| Variable | Default | Meaning |
|---|---|---|
| `_kestrel_march` | `x86-64-v3` | CPU target; `x86-64-v4` or `native` to retarget |
| `_kestrel_lto` | `thin` | `thin` or `none` |
| `_kestrel_pgo` | `no` | `no`, `generate` (instrumented), or `use` (optimized); see `PGO.md` |

## Standards

- Changes ship with tests.
- English in commits, code and documentation.
- History is never rewritten.
