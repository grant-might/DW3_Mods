# Footstep volume 50%

- Package id: `dev.footstep-major`
- Current version: `1.0.0`
- Targets: `SLES-03936` (Europe), `SLUS-01436` (USA)
- Type: plugin

## What it does

Makes your own footstep sound half as loud. Nothing else changes: music,
ambience and every other sound effect are untouched.

The player's footstep is the game's `SOUND_PLAYER00` sample, played by the
field code. The game passes no volume of its own when it plays it, so the only
volume control is the SPU voice mix volume for that one tone. This asks the
runtime's own SPU mixer to scale exactly that sample's output volume to 50
percent at the instant it is keyed on, which is a straight linear scale of that
one voice. No other voice, sample, register, bank or piece of game code or data
is changed. Turn the mod off and the mixer is stock again.

## Controls

None. It is passive while enabled.

## Notes

Do not combine it with another footstep mod. There is no conflict declaration,
because this is the only footstep mod shipped.

## One package, two regions

The footstep sample sits at the same SPU address on both discs, so one package
serves Europe and the USA.

## Install

Use the launcher. Open the **Mods** tab, pick your build, press **Load mod
package...**, choose `1.0.0/dev.footstep-major-1.0.0.zip`, press **Install**,
then press the rebuild button for your region (**Rebuild (USA)** or
**Rebuild (EUR)**) and press Play. The full steps are in the
[root README](../../README.md).

## Files in the package

    manifest.toml           package metadata, feature, plugin id
    INSTALL.md              install notes for this package
    plugin/step_duck.c      the plugin source the build compiles in
