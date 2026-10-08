# Starter packs

There are 53 starter packs. Each one puts a team of three into the
Digimon Online registration screen, a starting team the game itself never
offers. They are one family, so the pattern is described once here. The 53
packages are listed in [`index.json`](../index.json), one entry each.

## How a pack behaves

- A pack fills exactly one of the three choices on the registration screen.
  The other two keep the game's own packs, with their own names and
  descriptions.
- In game every custom pack is named **"Special Pack"**, with its team named
  in the description line under it. The name slot on that screen is a short
  fixed-width field, so all of them share the name and the team lives in the
  description.
- The file name of each package is the team, so you can tell them apart when
  you pick one to install.

## Apply only one pack at a time

A player picks one starting team, once. The plugin stays safe if several
packs are enabled (one pack per slot, in package id order, no corruption),
but that is defensive design and not a supported feature. Install one
starter pack and leave the rest out.

## The lab screen

The lab's Digimon loading and generation screen still shows the game's
original three packs while a pack is installed. Once the player exits the
initial lab, the game corrects to the mod's team. That is expected, not a
broken install.

## The 53 teams

| Package id | File (team) | In-game description |
|---|---|---|
| `dev.starter-pack.00` | `Kotemon-Kumamon-Monmon-pack-1.0.0.zip` | Kotemon Kumamon Monmon |
| `dev.starter-pack.01` | `Kotemon-Kumamon-Agumon-pack-1.0.0.zip` | Kotemon Kumamon Agumon |
| `dev.starter-pack.02` | `Kotemon-Kumamon-Veemon-pack-1.0.0.zip` | Kotemon Kumamon Veemon |
| `dev.starter-pack.03` | `Kotemon-Kumamon-Guilmon-pack-1.0.0.zip` | Kotemon Kumamon Guilmon |
| `dev.starter-pack.04` | `Kotemon-Kumamon-Renamon-pack-1.0.0.zip` | Kotemon Kumamon Renamon |
| `dev.starter-pack.05` | `Kotemon-Kumamon-Patamon-pack-1.0.0.zip` | Kotemon Kumamon Patamon |
| `dev.starter-pack.06` | `Kotemon-Monmon-Agumon-pack-1.0.0.zip` | Kotemon Monmon Agumon |
| `dev.starter-pack.07` | `Kotemon-Monmon-Veemon-pack-1.0.0.zip` | Kotemon Monmon Veemon |
| `dev.starter-pack.08` | `Kotemon-Monmon-Guilmon-pack-1.0.0.zip` | Kotemon Monmon Guilmon |
| `dev.starter-pack.09` | `Kotemon-Monmon-Renamon-pack-1.0.0.zip` | Kotemon Monmon Renamon |
| `dev.starter-pack.10` | `Kotemon-Monmon-Patamon-pack-1.0.0.zip` | Kotemon Monmon Patamon |
| `dev.starter-pack.11` | `Kotemon-Agumon-Veemon-pack-1.0.0.zip` | Kotemon Agumon Veemon |
| `dev.starter-pack.12` | `Kotemon-Agumon-Guilmon-pack-1.0.0.zip` | Kotemon Agumon Guilmon |
| `dev.starter-pack.13` | `Kotemon-Agumon-Renamon-pack-1.0.0.zip` | Kotemon Agumon Renamon |
| `dev.starter-pack.14` | `Kotemon-Agumon-Patamon-pack-1.0.0.zip` | Kotemon Agumon Patamon |
| `dev.starter-pack.15` | `Kotemon-Veemon-Guilmon-pack-1.0.0.zip` | Kotemon Veemon Guilmon |
| `dev.starter-pack.16` | `Kotemon-Veemon-Renamon-pack-1.0.0.zip` | Kotemon Veemon Renamon |
| `dev.starter-pack.17` | `Kotemon-Veemon-Patamon-pack-1.0.0.zip` | Kotemon Veemon Patamon |
| `dev.starter-pack.18` | `Kotemon-Guilmon-Renamon-pack-1.0.0.zip` | Kotemon Guilmon Renamon |
| `dev.starter-pack.19` | `Kotemon-Guilmon-Patamon-pack-1.0.0.zip` | Kotemon Guilmon Patamon |
| `dev.starter-pack.20` | `Kumamon-Monmon-Agumon-pack-1.0.0.zip` | Kumamon Monmon Agumon |
| `dev.starter-pack.21` | `Kumamon-Monmon-Veemon-pack-1.0.0.zip` | Kumamon Monmon Veemon |
| `dev.starter-pack.22` | `Kumamon-Monmon-Guilmon-pack-1.0.0.zip` | Kumamon Monmon Guilmon |
| `dev.starter-pack.23` | `Kumamon-Monmon-Renamon-pack-1.0.0.zip` | Kumamon Monmon Renamon |
| `dev.starter-pack.24` | `Kumamon-Monmon-Patamon-pack-1.0.0.zip` | Kumamon Monmon Patamon |
| `dev.starter-pack.25` | `Kumamon-Agumon-Veemon-pack-1.0.0.zip` | Kumamon Agumon Veemon |
| `dev.starter-pack.26` | `Kumamon-Agumon-Guilmon-pack-1.0.0.zip` | Kumamon Agumon Guilmon |
| `dev.starter-pack.27` | `Kumamon-Agumon-Renamon-pack-1.0.0.zip` | Kumamon Agumon Renamon |
| `dev.starter-pack.28` | `Kumamon-Agumon-Patamon-pack-1.0.0.zip` | Kumamon Agumon Patamon |
| `dev.starter-pack.29` | `Kumamon-Veemon-Guilmon-pack-1.0.0.zip` | Kumamon Veemon Guilmon |
| `dev.starter-pack.30` | `Kumamon-Veemon-Renamon-pack-1.0.0.zip` | Kumamon Veemon Renamon |
| `dev.starter-pack.31` | `Kumamon-Veemon-Patamon-pack-1.0.0.zip` | Kumamon Veemon Patamon |
| `dev.starter-pack.32` | `Kumamon-Guilmon-Renamon-pack-1.0.0.zip` | Kumamon Guilmon Renamon |
| `dev.starter-pack.33` | `Kumamon-Renamon-Patamon-pack-1.0.0.zip` | Kumamon Renamon Patamon |
| `dev.starter-pack.34` | `Monmon-Agumon-Veemon-pack-1.0.0.zip` | Monmon Agumon Veemon |
| `dev.starter-pack.35` | `Monmon-Agumon-Guilmon-pack-1.0.0.zip` | Monmon Agumon Guilmon |
| `dev.starter-pack.36` | `Monmon-Agumon-Patamon-pack-1.0.0.zip` | Monmon Agumon Patamon |
| `dev.starter-pack.37` | `Monmon-Veemon-Guilmon-pack-1.0.0.zip` | Monmon Veemon Guilmon |
| `dev.starter-pack.38` | `Monmon-Veemon-Renamon-pack-1.0.0.zip` | Monmon Veemon Renamon |
| `dev.starter-pack.39` | `Monmon-Veemon-Patamon-pack-1.0.0.zip` | Monmon Veemon Patamon |
| `dev.starter-pack.40` | `Monmon-Guilmon-Renamon-pack-1.0.0.zip` | Monmon Guilmon Renamon |
| `dev.starter-pack.41` | `Monmon-Guilmon-Patamon-pack-1.0.0.zip` | Monmon Guilmon Patamon |
| `dev.starter-pack.42` | `Monmon-Renamon-Patamon-pack-1.0.0.zip` | Monmon Renamon Patamon |
| `dev.starter-pack.43` | `Agumon-Veemon-Guilmon-pack-1.0.0.zip` | Agumon Veemon Guilmon |
| `dev.starter-pack.44` | `Agumon-Veemon-Renamon-pack-1.0.0.zip` | Agumon Veemon Renamon |
| `dev.starter-pack.45` | `Agumon-Veemon-Patamon-pack-1.0.0.zip` | Agumon Veemon Patamon |
| `dev.starter-pack.46` | `Agumon-Guilmon-Renamon-pack-1.0.0.zip` | Agumon Guilmon Renamon |
| `dev.starter-pack.47` | `Agumon-Guilmon-Patamon-pack-1.0.0.zip` | Agumon Guilmon Patamon |
| `dev.starter-pack.48` | `Agumon-Renamon-Patamon-pack-1.0.0.zip` | Agumon Renamon Patamon |
| `dev.starter-pack.49` | `Veemon-Guilmon-Renamon-pack-1.0.0.zip` | Veemon Guilmon Renamon |
| `dev.starter-pack.50` | `Veemon-Guilmon-Patamon-pack-1.0.0.zip` | Veemon Guilmon Patamon |
| `dev.starter-pack.51` | `Veemon-Renamon-Patamon-pack-1.0.0.zip` | Veemon Renamon Patamon |
| `dev.starter-pack.52` | `Guilmon-Renamon-Patamon-pack-1.0.0.zip` | Guilmon Renamon Patamon |
