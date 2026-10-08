# 60 Hz NTSC (Europe)

- Package id: `dev.ntsc-60hz`
- Current version: `1.0.0`
- Targets: `SLES-03936` (Europe only)
- Type: data patch (no plugin, no rebuild)

## What it does

Runs the European build at NTSC timing and removes the 24-line PAL display
offset, by setting two of the game's own region flags at boot: `NTSC_MODE = 1`
and `SHIFT_PAL_SCREEN = 0`.

Those two flags are the two small-data words the standard PAL to NTSC disc
conversion flips inside the retail `SLES_039.36` executable (file `0x4D4AC`
`00 -> 01` and file `0x4D4B0` `01 -> 00`). Mapped through the byte-matching
decomp they are the initialisers of `NTSC_MODE` (`0x8005CCAC`) and
`SHIFT_PAL_SCREEN` (`0x8005CCB0`).

`NTSC_MODE` drives the video mode, the game clock step, the field actor and
flight speeds, and the sound tick and fade durations. `SHIFT_PAL_SCREEN` is what
adds the 24-line downward screen offset in the European build, and it also
selects the card-game layout tables.

On the recompiled build the host frame pacer already runs at about 60 Hz, so the
un-patched European game clock runs about 20 percent fast for a 50 Hz title.
Setting `NTSC_MODE = 1` is what brings the clock, the actor speeds and the
offset back to the NTSC figures the patch is known for.

## Europe only

This package targets `SLES-03936` only. The USA build (`SLUS-01436`) is NTSC
already and has neither flag, so there is nothing to patch there.

## Controls

None. It is passive while enabled.

## Note on the form

This package is a data-only patch. It carries no plugin and needs no rebuild:
the runtime applies the two words to the loaded game image, with an expected-byte
guard, the first time the game starts, and re-applies them after a savestate or
rewind restore. Install and play, no rebuild step.

## Install

Use the launcher. Open the **Mods** tab, pick your **Europe** build, press
**Load mod package...**, choose `1.0.0/dev.ntsc-60hz-1.0.0.zip`, press
**Install**, then press Play. Because it is data only, the rebuild step is not
needed for this one.

## Files in the package

    manifest.toml           package metadata, feature, and the two data patches
    INSTALL.md              install notes for this package
