# Warp / Teleport Tool (Fast Travel)

- Package id: `dev.warp-tool`
- Current version: `1.0.1`
- Targets: `SLES-03936` (Europe), `SLUS-01436` (USA)
- Type: plugin

## What it does

Adds a destination list you can open while you are out in the field. Pick a
place from the list and the game moves you there, using its own field
transition, so you truly end up standing in the chosen area.

The list is built from the game's own scene table, the same one the game uses
for its debug stage picker. It has 239 destinations, grouped by area: Asuka
City, Wire Forest & Coast, Chinlon & Tyranno, Suzaku, Byakko, Genbu & Krohn,
Magasta, and the Amaterasu alternate maps.

Nothing is drawn while one of the game's own menus is open, and the list is
only offered in the field with the field task live, so it never steals input
from a menu or a battle.

## Controls

- Hold **L2 and R2** together in the field to open the list. The field freezes
  while it is open.
- **D-pad up / down**: move the highlight
- **L1 / R1**: page up and down (ten rows a press)
- **Cross**: warp to the highlighted destination
- **Triangle or Start**: close without moving

The header shows your position in the list, for example `7/239`, and the footer
names the buttons and the destination under the highlight. Closing the list
puts the field back exactly as it was.

## One package, two regions

The plugin carries one address table for Europe and one for the USA, and the
build picks the right one with a compile define. Install the same package in
either build. If a build ends up with the wrong region's table, the plugin does
not misbehave: it notices the mismatch once the game's tasks are live, writes a
loud line to its log, and disables itself (no list, no game memory writes).

## Install

Same two halves as any plugin mod on this recomp: the package, and the relink.
See the [main README](../../README.md) for the full picture, and
[`docs/PACKAGE-FORMAT.md`](../../docs/PACKAGE-FORMAT.md) for the exact paths.
Short version:

1. Unpack `1.0.1/dev.warp-tool-1.0.1.zip` so its root lands on
   `<exe dir>/mods/packages/dev.warp-tool/1.0.1/`.
2. Make sure the build has `plugin/warp_tool.c` compiled in with the right
   region define, and relink. The two ready-to-launch builds already have this
   done.
3. Enable it in `<exe dir>/mods/state.toml`:

       format_version = 2

       [[package]]
       id = "dev.warp-tool"
       version = "1.0.1"

       [[feature]]
       package_id = "dev.warp-tool"
       id = "warp"
       enabled = true

Set `enabled = false` to turn it off. With no state file at all it stays off,
which is the intended default.

## Files in the package

    manifest.toml        package metadata, features, options, plugin id
    INSTALL.md           install notes for this package
    plugin/warp_tool.c   the plugin source the build compiles in
