# Save Anywhere

- Package id: `dev.save-anywhere`
- Current version: `1.0.0`
- Targets: `SLES-03936` (Europe), `SLUS-01436` (USA)
- Type: plugin

## What it does

Adds a small panel you can open while you are out in the field, which lets you
save at any point instead of walking back to a save point. Confirm it and the
**game's own memory-card save screen** opens: the real slot picker, the real
checksums, the real card write. You come back to the field exactly where you
were standing.

The mod does not reimplement the save format and does not rebuild any save UI.
It drives the game's own field task into its leave transition with the next mode
set to the game's own memory-card mode and a negative argument, which is exactly
how the game's own stage-select data enters that screen (`0xC00`,
`0x80000000` = save). Everything after that - which slot, the checksums, the
MEMCARD commands - is the game's own code doing its normal job.

It is only offered in the field with the field task live and the field idle, so
it never steals input from a menu, a battle or a cutscene. The field freezes
while the panel is open and is put back exactly as it was when you close it.

## Controls

- Hold **SELECT** and press **SQUARE** to open the panel (keyboard: hold
  **Right Shift**, press **Z**). Both are free in the field: Fast Travel owns
  L2+R2, the runtime owns SELECT+R3 and SELECT+R1, and the game's own field menu
  uses the D-pad, Cross, Triangle and Start.
- **D-pad up / down**: move between `SAVE GAME` and `CANCEL`
- **Cross**: confirm the highlighted row
- **Triangle or Start**: close the panel and stay where you are

Choosing `SAVE GAME` runs the game's own save screen, so you pick the slot and
confirm there just as you would at a save point. The field freezes while the
panel is up, and closing the panel restores it.

## It opens the game's own save screen

To be plain about it: Save Anywhere does not have a save menu of its own. The
panel it opens is only a two-row opener (`SAVE GAME` / `CANCEL`); the screen that
actually writes your file is the game's own memory-card screen, reached through
the game's own field transition. That is deliberate - it means the save is
written by the game, with the game's own checksums and slot handling, so saves
made this way load normally from the game's own title-screen load menu and are
readable by the save editor like any other.

## Coexists with Fast Travel

Save Anywhere and [Fast Travel](../dev.warp-tool/README.md) are meant to be
installed together and are chosen to never share an input:

- **Fast Travel** opens on **L2+R2**
- **Save Anywhere** opens on **SELECT+SQUARE**

While either one's panel is up the field is frozen, so the two cannot open at
the same moment. Install both, enable both features, and they simply live side
by side.

## One package, two regions

The plugin carries one address table for Europe and one for the USA, and the
build picks the right one with a compile define. Install the same package in
either build. If a build ends up with the wrong region's table, the plugin does
not misbehave: it notices the mismatch once the game's tasks are live, writes a
loud line to its log, and disables itself (no panel, no game memory writes).

## Install

Use the launcher. In **DW3 Recompiled+**, open the **Mods** tab, pick your
build, press **Load mod package...** and choose
`1.0.0/dev.save-anywhere-1.0.0.zip`, press **Install**, then press the rebuild
button for your region:

- **Rebuild (USA)** for a build made from the USA disc
- **Rebuild (EUR)** for a build made from the European disc

The rebuild is the code step: Save Anywhere is a plugin, so its
`plugin/save_tool.c` has to be compiled into that build's executable. It takes
seconds when that build already exists. Then press Play. If you are installing
it alongside Fast Travel, install and rebuild once - the rebuild links every
loaded package's plugin into the build.

Both rebuild buttons are greyed out until a package is loaded, so you cannot
press them by mistake.

### Why the rebuild, in one line

The runtime only activates plugin code whose constructor is compiled into the
executable, so a package alone can add data but not behaviour. The launcher's
rebuild does the compile and the relink for you.

### By hand, if you are not using the launcher

1. Unpack `1.0.0/dev.save-anywhere-1.0.0.zip` so its root lands on
   `<exe dir>/mods/packages/dev.save-anywhere/1.0.0/`.
2. Copy `plugin/save_tool.c` into the build's plugin source folder, make the
   runtime target compile it with `DW3_GAME_REGION_EU` or `DW3_GAME_REGION_US`
   for that build's disc, and relink
   (`cmake --build <build dir> --config Release --parallel`).
3. Copy the rebuilt executable into the build folder.
4. Enable it in `<exe dir>/mods/state.toml`:

       format_version = 2

       [[package]]
       id = "dev.save-anywhere"
       version = "1.0.0"

       [[feature]]
       package_id = "dev.save-anywhere"
       id = "save"
       enabled = true

Set `enabled = false` to turn it off. With no state file at all it stays off,
which is the intended default. The two ready-to-launch builds this index was
made for have the plugin linked in, so on those the package is enough.

## Files in the package

    manifest.toml            package metadata, features, options, plugin id
    INSTALL.md               install notes for this package
    plugin/save_tool.c       the plugin source the build compiles in
