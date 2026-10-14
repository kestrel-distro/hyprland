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

The recipe builds this fork's own tree. Packages are named `hyprland` to match
Arch, with `pkgrel` above Arch's so the build wins in the ISO build-time
repository; a later Arch bump takes over on `pacman -Syu`.

| Variable | Default | Meaning |
|---|---|---|
| `_kestrel_march` | `x86-64-v3` | CPU target; `x86-64-v4` or `native` to retarget |
| `_kestrel_lto` | `thin` | `thin` or `none` |

## Standards

- Changes ship with tests.
- English in commits, code and documentation.
- History is never rewritten.
