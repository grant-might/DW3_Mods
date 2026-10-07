# DW3 Mods

A mod index for the recompiled Digimon World 3 builds. The two builds this
index covers are the Europe release (disc id `SLES-03936`) and the USA release
(disc id `SLUS-01436`).

Every mod here is a package a build can install. The packages are listed in
[`index.json`](index.json), and each one has a folder under [`mods/`](mods/)
holding the package file and its own notes.

## Mods

| Mod | What it does | Targets | Version |
|---|---|---|---|
| [Warp / Teleport Tool (Fast Travel)](mods/dev.warp-tool/README.md) | Opens a destination list in the field and teleports you there | `SLES-03936`, `SLUS-01436` | 1.0.1 |

## Read this first: how a mod actually gets onto this recomp

This is the part people get wrong, so it is at the top.

Installing a mod is two jobs, not one:

1. **Install the package.** The package is a ZIP. The build keeps it under
   `mods/packages/<id>/<version>/`, and a small file at `mods/state.toml` says
   whether the mod's feature is switched on. This half is data only. It is
   quick and it is reversible.
2. **Relink.** A mod that changes how the game behaves does it with native
   code that has to be compiled into the game's executable. The package ships
   that code as source, and the build compiles it in and links a new
   executable. This is the relink step.

There is no drop-in option. The runtime only activates plugin code that was
registered by a constructor compiled into the executable itself. At launch it
checks the selected packages and refuses to start if one names a plugin the
executable does not already contain (you get `trusted plugin is unavailable`).
There is no loader that can pick a plugin binary out of a package at run time,
so putting a file next to the game cannot add behaviour.

**How long it takes.** With the build tree already set up, the relink is an
incremental build and takes seconds, because only the mod's one source file is
recompiled and the executable is relinked. Building a tree from nothing
(compiler, CMake, and the game's generated code) takes much longer, up to about
half an hour.

**What the launcher does.** For a mod package the launcher can install the ZIP
into `mods/packages/<id>/<version>/`, write `mods/state.toml` when you switch a
feature on or off, remove a package, and report what it finds. That covers the
data half.

**What the launcher does not do by itself.** The relink. That is a build step,
needed once per build you want the mod in. The two ready-to-launch builds this
index was made for already have the Fast Travel plugin linked in, so on those
builds you only need the data half.

### Manual install, step by step

1. Get the package ZIP for the version you want from
   [`mods/<id>/<version>/`](mods/) in this repo.
2. Unpack the ZIP so its root lands in the build's mod folder as
   `mods/packages/<id>/<version>/` (the archive root holds `manifest.toml`).
   Or let the launcher install the ZIP for you.
3. Put the package's `plugin/<name>.c` where the build compiles plugin sources
   from, and make sure the build target includes it with the define for the
   right region.
4. Relink (`cmake --build <build dir> --config Release --parallel`).
5. Copy the rebuilt executable into the build folder, keeping the same
   executable name so the launcher still finds it.
6. Enable the feature in `mods/state.toml`.

`docs/PACKAGE-FORMAT.md` has the exact paths, the manifest fields and the state
file format. Each package also carries an `INSTALL.md` with the same steps.

## What a build needs to run a mod

- A recompiled build of the right disc. Fast Travel targets both `SLES-03936`
  and `SLUS-01436` from one package. A mod carries one address table per region
  and the build picks it with a compile define, so a build for the wrong region
  does not quietly misbehave: the plugin refuses and switches itself off.
- The build compiled with the mod's plugin linked in (the relink above).
- The package under `mods/packages/<id>/<version>/`, plus a `mods/state.toml`
  that enables the feature. With no state file at all, features run at their
  manifest default, which is off for Fast Travel, so a fresh install does
  nothing until you switch it on.

## Repo layout

    README.md                     this file
    index.json                    machine-readable list of the mods
    docs/PACKAGE-FORMAT.md        the package and state file contract
    mods/<id>/README.md           per-mod notes (what it does, controls, install)
    mods/<id>/<version>/          the installable package ZIP for that version

## Fast Travel: how to use it in game

Open the list in the field (not while a menu or a battle is up) by holding
**L2 and R2** together. The field freezes while the list is open, and is put
back exactly as it was when you close it.

- **D-pad up / down**: move the highlighted destination
- **L1 / R1**: page up and down through the list
- **Cross**: warp to the highlighted destination
- **Triangle or Start**: close the list and stay where you are

The list holds 239 destinations: every field stage in the game's own scene
table, grouped by area (Asuka City, Wire Forest & Coast, Chinlon & Tyranno,
Suzaku, Byakko, Genbu & Krohn, Magasta, and the Amaterasu alternate maps). The
header shows your place in the list, for example `7/239`.
