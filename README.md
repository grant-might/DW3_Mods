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
| [Save Anywhere](mods/dev.save-anywhere/README.md) | Opens a panel in the field that runs the game's own save screen | `SLES-03936`, `SLUS-01436` | 1.0.0 |

The two mods are made to be installed together and never share an input: Fast
Travel opens on **L2+R2**, Save Anywhere on **SELECT+SQUARE**. While either
panel is up the field is frozen, so they cannot both open at once.

## Install a mod (the normal way: the launcher does it)

You need the launcher, **DW3 Recompiled+**, and one build made from your own
disc for the region you play. Make that build on the launcher's **Play** tab
first if you have not already.

1. Open the launcher and go to the **Mods** tab.
2. Pick your build at the top: **USA** or **Europe**.
3. Press **Load mod package...** and pick the package ZIP from this repo
   (`mods/<id>/<version>/<id>-<version>.zip`). The tab shows what it is: id,
   version, name, description, the discs it targets and its features.
4. Press **Install**. That unpacks it into the build and switches its features
   on.
5. Press the rebuild button for your region: **Rebuild (USA)** or
   **Rebuild (EUR)**. This is the step that puts the mod's code into the game,
   and it is why installing the ZIP alone is not enough (see below).
6. Press **Play**.

Both rebuild buttons stay greyed out until a mod package is loaded, so the tab
tells you when this step is available.

If that region's build already exists, step 5 is an incremental relink and
takes **seconds**. If it does not exist yet the tab says so and does nothing,
and the fix is step 0: build the disc on the Play tab first, which takes 10 to
20 minutes.

To turn the mod off later, use **Disable** in the same tab (that writes
`mods/state.toml`); to take it out, press **Remove** and rebuild.

## Why there is a rebuild step, and why the launcher does it

Installing a package and making the mod work are two different jobs, and only
the first one is a file copy.

- **Install the package.** The package is a ZIP whose root holds
  `manifest.toml`. The build keeps it under `mods/packages/<id>/<version>/`,
  and `mods/state.toml` says whether each feature is switched on. This half is
  data only, quick and reversible.
- **Relink.** A mod that changes how the game behaves does it with native code
  that has to be compiled into the game's executable. The package ships that
  code as source (`plugin/<name>.c`), and the build compiles it in and links a
  new executable.

There is no drop-in option, by design: the runtime only activates plugin code
that was registered by a constructor compiled into the executable itself. At
launch it checks the selected packages and refuses to start if one names a
plugin the executable does not contain (`trusted plugin is unavailable`).
There is no loader that can pick a plugin binary out of a package at run time,
so putting a file next to the game cannot add behaviour.

That is exactly why the launcher's Mods tab gained the two **Rebuild** buttons:
the relink is a build step, it is needed once per build, and it is the step no
player should have to do by hand. The launcher stages the package's source into
that build, sets the region define for the disc that build was made from,
rebuilds and relinks with the same build path the Play tab uses, and replaces
the executable. Everything else in the build folder - your cards, your settings,
your mods - is left alone.

A package with no `plugin/` folder is data only (byte patches, disc overlays).
Those need no rebuild; install and play.

## The builds this index was made for

Two ready-to-launch builds existed before the launcher could rebuild, and they
already have both the Fast Travel and Save Anywhere plugins linked in. On those
exact builds the packages alone are enough and the rebuild is a no-op you can
skip. On any other build, including one you make yourself from your disc, press
the rebuild button for your region.

## What a build needs to run a mod

- A recompiled build of the right disc. Both mods target `SLES-03936` and
  `SLUS-01436` from one package each. A mod carries one address table per region
  and the build picks it with a compile define, so a build for the wrong region
  does not quietly misbehave: the plugin refuses and switches itself off.
- The build compiled with the mod's plugin linked in (the rebuild above).
- The package under `mods/packages/<id>/<version>/`, plus a `mods/state.toml`
  that enables the feature. With no state file at all, features run at their
  manifest default, which is off for both mods.

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

## Save Anywhere: how to use it in game

Open the panel in the field (not while a menu or a battle is up) by holding
**SELECT** and pressing **SQUARE** (keyboard: hold **Right Shift**, press **Z**).
The field freezes while the panel is up and is put back exactly as it was when
you close it.

- **D-pad up / down**: move between `SAVE GAME` and `CANCEL`
- **Cross**: confirm the highlighted row
- **Triangle or Start**: close the panel and stay where you are

Choosing **SAVE GAME** opens the **game's own memory-card save screen** - the
real slot picker and the real card write, not a rebuilt one. Pick a slot and
confirm there just as you would at a save point, and the game writes the file
itself; you then come back to the field where you were standing. The panel
itself is only the two-row opener; the screen that saves is the game's.

In that screen the buttons are the game's own, so they work exactly as they
do at a save point:

- **Cross**: the slot, then the save row, then confirm the overwrite, then
  dismiss the confirmation
- **Triangle**: leave the screen, which puts you back in the field where you
  were standing

If the panel does not open, you are not standing in a plain field. It refuses
in a menu, in a battle, in a cutscene and mid-transition, and waits until the
field is live and idle.

Save Anywhere and Fast Travel do not collide: Fast Travel opens on **L2+R2**,
Save Anywhere on **SELECT+SQUARE**, and either way the field is frozen while a
panel is up, so the two cannot open at the same moment.

## Appendix: installing by hand (no launcher, or an older build)

The launcher is the supported route. Do this only if you are building the game
yourself or using a build that predates the Mods tab.

1. Get the package ZIP for the version you want from
   [`mods/<id>/<version>/`](mods/) in this repo.
2. Unpack the ZIP so its root lands in the build's mod folder as
   `mods/packages/<id>/<version>/` (the archive root holds `manifest.toml`).
3. Put the package's `plugin/<name>.c` where the build compiles plugin sources
   from, and make the build target include it with the define for the right
   region.
4. Relink (`cmake --build <build dir> --config Release --parallel`).
5. Copy the rebuilt executable into the build folder, keeping the same
   executable name so the launcher still finds it.
6. Enable the feature in `mods/state.toml`.

`docs/PACKAGE-FORMAT.md` has the exact paths, the manifest fields and the state
file format. Each package also carries an `INSTALL.md` with the same steps.
