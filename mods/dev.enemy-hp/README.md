# Enemy HP in battle

- Package id: `dev.enemy-hp`
- Current version: `1.0.0`
- Targets: `SLES-03936` (Europe), `SLUS-01436` (USA)
- Type: plugin

## What it does

Shows the enemy's current and max HP on the battle HUD. The game's own battle
screen draws the enemy's HP bar but never the enemy's numbers, so this adds the
pair the same way the player's row reads: current first, then a slash, then max.

The number is the game's own battle value. It comes from the live battle state
(`FIGHTSTG_battle.fighters[1][active[1]].hp` and `.maxHp`), the same field the
game writes at battle start, drops as damage lands and restores on heal. It is
not recomputed from stats, so it falls the instant a hit connects.

The pair is drawn on the free row directly under the enemy's HP bar,
right-aligned to that bar's right end, clear of the enemy's name and its party
icons.

## Controls

None in game. Two settings in the mod's options if you want to nudge it:

- `y` (default 46): the screen row the numbers are drawn on
- `right` (default 143): the right edge the numbers are aligned to

## Notes

- The digits are drawn with the runtime's own font in the game's HP amber with a
  dark outline. The style is close to, but not identical to, the game's own
  numbers.
- The enemy number updates instantly, while your own row eases its number over a
  few frames, so the two rows can look slightly different while a number is
  changing.

## One package, two regions

The plugin carries one address table for Europe and one for the USA, and the
build picks the right one with a compile define. If a build ends up with the
wrong region's table, the plugin notices, writes a loud line to its log and
disables itself rather than writing through the wrong table.

## Install

Use the launcher. Open the **Mods** tab, pick your build, press **Load mod
package...**, choose `1.0.0/dev.enemy-hp-1.0.0.zip`, press **Install**, then
press the rebuild button for your region (**Rebuild (USA)** or
**Rebuild (EUR)**) and press Play. The full steps are in the
[root README](../../README.md).

Enemy HP is a plugin, so the rebuild is the step that compiles its
`plugin/enemy_hp.c` into that build's executable. It takes seconds when the
build already exists.

## Files in the package

    manifest.toml           package metadata, feature, options, plugin id
    INSTALL.md              install notes for this package
    plugin/enemy_hp.c       the plugin source the build compiles in
