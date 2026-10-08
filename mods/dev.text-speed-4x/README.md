# Faster Dialogue Text (4x)

- Package id: `dev.text-speed-4x`
- Current version: `1.0.0`
- Targets: `SLES-03936` (Europe), `SLUS-01436` (USA)
- Type: plugin

## What it does

Makes the game's typed text print four times as fast.

The game's typewriter reveals one character every few frames for talk boxes,
message boxes and the field area banner. This reveals 4 characters per tick
instead of 1, so the text appears at about four times the speed. The tick
cadence is not touched, so prompts, waits for the player, page breaks, portraits
and the typing sound keep their normal timing. Each tick just shows more
characters.

The speed-up is exactly 4x with no rounding, because it changes whole characters
per tick.

## Controls

None. It is passive while enabled.

## Conflicts

Enable exactly one text speed. `dev.text-speed-2x` and `dev.text-speed-4x`
declare each other as conflicts, and the runtime refuses a launch with both
enabled. That is deliberate: both patch the same single instruction, so running
them together is meaningless. See the [root README](../../README.md) for the
conflicts rule.

## One package, two regions

The patch address is compiled per region (Europe and USA) and the build picks
the right one. The plugin refuses to patch if the live instruction is not the
expected original.

## Install

Use the launcher. Open the **Mods** tab, pick your build, press **Load mod
package...**, choose `1.0.0/dev.text-speed-4x-1.0.0.zip`, press **Install**,
then press the rebuild button for your region (**Rebuild (USA)** or
**Rebuild (EUR)**) and press Play. The full steps are in the
[root README](../../README.md).

## Files in the package

    manifest.toml           package metadata, feature, plugin id
    INSTALL.md              install notes for this package
    plugin/text_speed.c     the plugin source the build compiles in
