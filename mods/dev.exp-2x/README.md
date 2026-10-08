# Experience x2

- Package id: `dev.exp-2x`
- Current version: `1.0.0`
- Targets: `SLES-03936` (Europe), `SLUS-01436` (USA)
- Type: plugin

## What it does

Multiplies the EXP a battle awards by 2.

The value is applied at the award point, before the game's own work. The
post-battle report reads the reward from its own table (`STFGTREP_rewards`), so
this scales the two EXP columns of that table while it is resident, and the
game's own split between fighters, the accessory boost, the level-up check and
the number shown all run on the multiplied value. The report, the rolling bar
and the level you gain agree with each other.

Rounding is explicit integer round-to-nearest.

## Controls

None. It is passive while enabled.

## Conflicts

Enable only one EXP multiplier. `dev.exp-1.2x`, `dev.exp-1.5x` and `dev.exp-2x`
each declare the other two as conflicts, and the runtime refuses a launch with
two of them enabled at once. That is deliberate: they all set the same single
value, so no combination of them means anything. See the
[root README](../../README.md) for the conflicts rule.

## One package, two regions

The reward table is compiled per region (Europe and USA) and the build picks the
right one. If the table at the overlay address is not the one it was built for,
the mod logs a warning instead of scaling.

## Install

Use the launcher. Open the **Mods** tab, pick your build, press **Load mod
package...**, choose `1.0.0/dev.exp-2x-1.0.0.zip`, press **Install**, then press
the rebuild button for your region (**Rebuild (USA)** or **Rebuild (EUR)**) and
press Play. The full steps are in the [root README](../../README.md).

## Files in the package

    manifest.toml           package metadata, feature, plugin id
    INSTALL.md              install notes for this package
    plugin/exp_tool.c       the plugin source the build compiles in
