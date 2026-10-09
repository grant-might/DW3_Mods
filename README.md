# DW3 Mods

A mod index for the recompiled Digimon World 3 builds. The two builds this
index covers are the Europe release (disc id `SLES-03936`) and the USA release
(disc id `SLUS-01436`).

Join our discord server for community forum, assistance, to make suggestions, etc. https://discord.gg/rJv3uVsgx

Every mod here is a package a build can install. The packages are listed in
[`index.json`](index.json), and each one has a folder under [`mods/`](mods/)
holding the package file and its own notes.

**Each package covers both builds.** Every shipping package names both
`SLES-03936` and `SLUS-01436` and works on either, from one ZIP. Your build
picks the right region's address table with a compile define, so you install the
same package in the Europe build or the USA one. The single exception is the
60 Hz NTSC patch, which is Europe only because the USA disc is NTSC already. A
build made from the wrong region's disc is not left to misbehave: a plugin
notices the mismatch and switches itself off instead of writing through the
wrong table.

## Mods

| Mod | What it does | Targets | Version |
|---|---|---|---|
| [Warp / Teleport Tool (Fast Travel)](mods/dev.warp-tool/README.md) | Opens a destination list in the field and teleports you there | `SLES-03936`, `SLUS-01436` | 1.0.1 |
| [Save Anywhere](mods/dev.save-anywhere/README.md) | Opens a panel in the field that runs the game's own save screen | `SLES-03936`, `SLUS-01436` | 1.0.0 |
| [Enemy HP in battle](mods/dev.enemy-hp/README.md) | Draws the enemy's current and max HP on the battle HUD | `SLES-03936`, `SLUS-01436` | 1.0.0 |
| [Experience x1.2](mods/dev.exp-1.2x/README.md) | Multiplies battle EXP by 1.2 | `SLES-03936`, `SLUS-01436` | 1.0.0 |
| [Experience x1.5](mods/dev.exp-1.5x/README.md) | Multiplies battle EXP by 1.5 | `SLES-03936`, `SLUS-01436` | 1.0.0 |
| [Experience x2](mods/dev.exp-2x/README.md) | Multiplies battle EXP by 2 | `SLES-03936`, `SLUS-01436` | 1.0.0 |
| [Faster Dialogue Text (2x)](mods/dev.text-speed-2x/README.md) | Prints dialogue text at twice the speed | `SLES-03936`, `SLUS-01436` | 1.0.0 |
| [Faster Dialogue Text (4x)](mods/dev.text-speed-4x/README.md) | Prints dialogue text at four times the speed | `SLES-03936`, `SLUS-01436` | 1.0.0 |
| [Footstep volume 50%](mods/dev.footstep-major/README.md) | Makes your own footstep sound half as loud | `SLES-03936`, `SLUS-01436` | 1.0.0 |
| [60 Hz NTSC](mods/dev.ntsc-60hz/README.md) | Runs the European build at NTSC timing | `SLES-03936` only | 1.0.0 |
| [Starter packs (53 teams)](docs/STARTER-PACKS.md) | Puts a team of three the game never offers into the registration screen | `SLES-03936`, `SLUS-01436` | 1.0.0 |

The [modded disc patch](disc-patch/README.md) is separate: it is a `.bps` you
apply to your own retail USA disc to get a finished image with six of these mods
already inside it.

Fast Travel and Save Anywhere are made to be installed together and never share
an input: Fast Travel opens on **L2+R2**, Save Anywhere on **SELECT+SQUARE**.
While either panel is up the field is frozen, so they cannot both open at once.

## Install a mod (the normal way: the launcher does it)

You need the launcher, **DW3 Recompiled+**, and one build made from your own
disc for the region you play. Make that build on the launcher's **Play** tab
first if you have not already.

1. Open the launcher and go to the **Mods** tab.
2. Pick your build at the top: **USA** or **Europe**.
3. Press **Load mod package...** and pick the package ZIP from this repo
   (`mods/<id>/<version>/<name>.zip`). The tab shows what it is: id, version,
   name, description, the discs it targets and its features.
4. Press **Install**. That unpacks it into the build and switches its features
   on.
5. Press the rebuild button for your region: **Rebuild (USA)** or
   **Rebuild (EUR)**. This is the step that puts the mod's code into the game,
   and it is why installing the ZIP alone is not enough (see below).
6. Press **Play**.

Both rebuild buttons stay greyed out until a mod package is loaded, so the tab
tells you when this step is available.

If that region's build already exists, step 5 is an incremental relink and takes
**seconds**. If it does not exist yet the tab says so and does nothing, and the
fix is step 0: build the disc on the Play tab first, which takes 10 to 20
minutes.

To turn a mod off later, use **Disable** in the same tab (that writes
`mods/state.toml`); to take it out, press **Remove** and rebuild.

If you install several plugin mods at once, install them all and rebuild once:
the rebuild links every loaded package's plugin into the build.

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
plugin the executable does not contain (`trusted plugin is unavailable`). There
is no loader that can pick a plugin binary out of a package at run time, so
putting a file next to the game cannot add behaviour.

That is exactly why the launcher's Mods tab has the two **Rebuild** buttons: the
relink is a build step, it is needed once per build, and it is the step no
player should have to do by hand. The launcher stages the package's source into
that build, sets the region define for the disc that build was made from,
rebuilds and relinks with the same build path the Play tab uses, and replaces
the executable. Everything else in the build folder, your cards, your settings
and your mods, is left alone.

A package with no `plugin/` folder is data only (byte patches, disc overlays).
Those need no rebuild; install and play. Of the mods here, only the 60 Hz NTSC
patch is data only.

## Starter packs

The 53 starter packs are one family, so they are described once here rather than
53 times. Each pack puts one starting team of three into the Digimon Online
registration screen, a team the game itself never offers. The full list of the
53 teams, and which package id is which team, is in
[`docs/STARTER-PACKS.md`](docs/STARTER-PACKS.md) and in
[`index.json`](index.json).

**Apply only one starter pack at a time.** A pack fills one of the three choices
on the registration screen. That is the supported way to use them: a player
picks one starting team, once. The plugin is written to stay safe if several
packs are enabled (one pack per slot, in package id order, no corruption), but
that is defensive design and not a supported feature.

**What you see in game.** Every custom pack is named **"Special Pack"** on the
registration screen, with its team named in the description line under it, for
example `Kotemon Kumamon Monmon`. The name slot on that screen is a short
fixed-width field, so the shared name is used for all of them and the team lives
in the description. The file name of each package is the team, so you can tell
them apart when you pick one to install.

**Only the slot a pack takes changes.** A pack fills one of the three choices;
the other two keep the game's own packs, with their own names and descriptions.
Enable one pack and you get your team plus the game's two originals.

**The lab screen looks "wrong" for a moment, and that is expected.** The lab's
Digimon loading and generation screen still shows the game's original three
packs while a starter pack is installed. Once the player exits the initial lab,
the game corrects to the mod's team. A player who sees the original three on
that screen does not have a broken install: the team is applied on exit.

## Conflicts

Two pairs of mods are mutually exclusive, and the runtime enforces it:

- The EXP multipliers: `dev.exp-1.2x`, `dev.exp-1.5x` and `dev.exp-2x`. Enable
  only one.
- The text speeds: `dev.text-speed-2x` and `dev.text-speed-4x`. Enable only one.

Each of these declares the other as a conflict in its manifest. If you enable
both of a pair and then press Play, the runtime **refuses the launch** instead
of running them together. That is deliberate, not a fault: the two EXP
multipliers would fight over the same reward value, and the two text speeds
patch the same single instruction, so there is no sane combination of either
pair. Turn one off and press Play again.

Every other package here carries no conflict declaration, so nothing else can
block a legitimate combination. Install as many of the rest as you like.

## The modded disc patch

[`disc-patch/`](disc-patch/) holds `DW3-USA-Complete.bps`, a patch that turns
your own retail **Digimon World 3 (USA)** disc image into a finished image with
six of these mods already inside it (Save Anywhere, Fast Travel, a starter pack
that offers Veemon, Enemy HP, text speed 4x and EXP 2x). It is for people who
want the mods baked into the disc, for a PS4 fPKG or an emulator, without the
recomp launcher.

The patch contains no game data and no disc image. It applies only to your own
retail USA disc. See [`disc-patch/README.md`](disc-patch/README.md) for the base
file it expects, the resulting image's hash so you can check it, and how to
apply it.

## What a build needs to run a mod

- A recompiled build of the right disc. Each mod targets `SLES-03936` and
  `SLUS-01436` from one package. A mod carries one address table per region and
  the build picks it with a compile define, so a build for the wrong region does
  not quietly misbehave: the plugin refuses and switches itself off.
- The build compiled with the mod's plugin linked in (the rebuild above). Data
  only packages skip this.
- The package under `mods/packages/<id>/<version>/`, plus a `mods/state.toml`
  that enables the feature. With no state file at all, features run at their
  manifest default, which is off.

## Repo layout

    README.md                     this file
    LICENSE.md                    the licence
    index.json                    machine-readable list of every mod
    docs/PACKAGE-FORMAT.md        the package and state file contract
    docs/STARTER-PACKS.md         the 53 starter packs and their teams
    disc-patch/                   the USA disc patch and its notes
    mods/<id>/README.md           per-mod notes (what it does, controls, install)
    mods/<id>/<version>/          the installable package ZIP for that version

## Controls, quick reference

- **Fast Travel**: hold **L2+R2** in the field. D-pad moves, L1/R1 page, Cross
  warps, Triangle or Start closes. 239 destinations, grouped by area. The list
  header shows your place, for example `7/239`.
- **Save Anywhere**: hold **SELECT** and press **SQUARE** in the field
  (keyboard: hold **Right Shift**, press **Z**). D-pad picks `SAVE GAME` or
  `CANCEL`, Cross confirms, Triangle or Start closes. Confirming opens the
  game's own memory-card save screen, where the buttons are the game's own:
  Cross for the slot, the save row, the overwrite and the dismiss, Triangle to
  leave. If the panel does not open, you are not standing in a plain live field:
  it refuses in a menu, a battle, a cutscene and mid-transition.
- **Enemy HP**, **EXP multipliers**, **text speeds**, **footstep** and **60 Hz
  NTSC**: no in-game controls. They are passive while enabled.
- **Starter packs**: no in-game controls. Enable the `pack` feature of the one
  pack you want.

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
file format. Each plugin package also carries an `INSTALL.md` with the same
steps.

## Licence

PolyForm Noncommercial License 1.0.0 with additional terms. See
[`LICENSE.md`](LICENSE.md). Use it, change it and share it free of charge for
anything that is not commercial.
