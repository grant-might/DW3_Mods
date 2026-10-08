# BPS patch for the Digimon World 3 (USA) modded disc

This folder holds the patch that turns your own retail Digimon World 3 (USA)
disc image into the modded image.

The patch is distributed from this repo. You are free to download it and use it
on your own copy of the game.

## What a .bps patch is

BPS (Beat Patching System) is a patch format that stores the steps needed to
rebuild one file from another. A .bps patch carries the source file's size, the
target file's size, the CRC32 of the source, the CRC32 of the target, and a
CRC32 of the patch itself. A patcher checks those CRCs, so it refuses to apply
the patch to the wrong base file instead of writing a corrupt image.

## What is in this folder

| File | What it is |
|---|---|
| `DW3-USA-Complete.bps` | the patch, 31,669 bytes, sha1 `23743657ca467b20f6eb748d627ccbb93fa8398b` |
| `README.md` | this file |

## What the patch contains

It contains no game data and no disc image. It is only the difference between
your retail disc and the finished one. The patched image is already the finished
game, with six mods inside it:

- Save Anywhere (open the game's own save screen from the field)
- Warp / fast travel (hold L2 and R2 in the field to pick a destination)
- a starter pack that offers Veemon instead of Patamon
- Enemy HP shown on the battle HUD
- text speed x4
- EXP x2

Nothing else about the disc changes. The image is the same size as retail and
differs from it only in the sectors the mods touch.

## The base file you must own

The patch applies to the retail USA disc image and nothing else:

| | |
|---|---|
| file | `Digimon World 3 (USA).bin` (your own retail copy of the disc) |
| size | 647,526,768 bytes |
| CRC32 | `E3D73A16` |
| sha1 | `f0b022f9be53cbce14640abd8f01beaadcb35208` |

Your copy may have a different file name, which does not matter. What matters is
the size and the CRC32. If you have a `.cue` and `.bin` pair, the patch goes on
the `.bin`.

## The result, and how to check it

Applying the patch produces the modded image:

| | |
|---|---|
| file | `Digimon World 3 (USA) - Complete.bin` |
| size | 647,526,768 bytes |
| CRC32 | `69DEBBBF` |
| sha1 | `0044bd5ddc1d67bd79a3e675a06617d091188d72` |

Check the result before you use it:

```
sha1sum "Digimon World 3 (USA) - Complete.bin"
```

On Windows, `certutil -hashfile "Digimon World 3 (USA) - Complete.bin" SHA1`.
The hash must be `0044bd5ddc1d67bd79a3e675a06617d091188d72`.

## Which tool to use

**Use FLIPS (Floating IPS).** It reads and writes images of this size without
trouble, and it checks the base file's CRC32 before it writes anything.

Do not use RomPatcher.js. It is a browser tool that loads both files into memory
as JavaScript, and it does arithmetic on the patch's action lengths in 32 bits.
This patch is one long action over a 650 MB image, so that arithmetic overflows
and the tool cannot handle it. It is a limitation of a browser JavaScript tool,
not of the patch.

The DW3 Recompiled+ launcher also has an apply card for this patch, which is the
simplest route if you already use it.

## How to apply it with FLIPS

Command line:

```
flips --apply DW3-USA-Complete.bps "Digimon World 3 (USA).bin" "Digimon World 3 (USA) - Complete.bin"
```

Graphical version: open the retail `.bin` as the ROM and the `.bps` as the
patch, or use "Apply Patch".

If FLIPS reports that the patch is not intended for this ROM, then the base file
is not the retail USA image. Check its size and CRC32 against the table above.
FLIPS refuses rather than producing a wrong image.

## Building the PS4 fPKG

The fPKG must be built from the patched image. A package built from a retail
image contains none of the mods and is just the unmodified game.

1. Apply the patch to your own retail image as above, and check the sha1.
2. Use the patched `.bin` (with its `.cue`) as the input to a PSX to PS4 fPKG
   tool, for example PSX-FPKG or PS Classics fPKG Builder.
3. Install the resulting fPKG on an exploited PS4.
