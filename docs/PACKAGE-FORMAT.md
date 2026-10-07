# Package and state file format

This is the contract a mod package and the runtime follow. It is here so the
index can grow: a new mod package only has to match this to work.

## Paths

Everything lives next to the game executable:

    <exe dir>/mods/state.toml                                  selection and options
    <exe dir>/mods/packages/<package id>/<version>/manifest.toml
    <exe dir>/mods/.staging/                                   transient install dir

The mod folder is resolved next to the executable, not from the working
directory. Each build folder has its own `mods/`.

## The package file

A package is a **ZIP whose archive root contains `manifest.toml`**, not nested
in a subdirectory:

    dev.warp-tool-1.0.1.zip
      manifest.toml
      INSTALL.md
      plugin/warp_tool.c

Installing unpacks the whole archive into
`mods/packages/<manifest id>/<manifest version>/`. The installer refuses if
that directory already exists, so bump the version to publish a change, or
remove the old directory first.

## The manifest

TOML, `format_version = 5`. Fields:

| field | required | notes |
|---|---|---|
| `format_version` | yes | `5` |
| `id` | yes | lowercase; must match the install directory name |
| `version` | yes | must match the install directory name |
| `name` | yes | shown in mod lists |
| `description` | no | one paragraph |
| `author` | no | free text |
| `license` | no | free text |
| `resolver` | no | defaults to `declarative` |

Feature ids, option ids and package ids must be lowercase. A manifest with an
invalid id does not load.

### Targets

A package names the discs it works on. The runtime only applies a package to a
matching game:

    [[target]]
    game_id = "SLES-03936"

    [[target]]
    game_id = "SLUS-01436"

### Features

A feature is one switchable unit. It has an id, a name, and the default state
used when there is no state file:

    [[feature]]
    id = "warp"
    name = "Fast Travel"
    description = "Open the FAST TRAVEL screen in the field with L2+R2."
    group = "Tools"
    default_enabled = false

Keep `default_enabled = false` for anything that changes the game. Then a
missing, empty or malformed state file leaves the mod off instead of turning it
on by accident.

### Options

An option is a value a feature reads at run time:

    [[option]]
    feature = "warp"
    id = "place"
    label = "Destination field stage mode (e.g. 518 = Digimon Lab)"
    type = "integer"
    min = 0
    max = 65535
    step = 1
    default = 518

Option values are stored per feature in the state file under
`[feature.values]`.

### Plugins

A feature that does more than patch data is backed by a plugin:

    [[plugin]]
    feature = "warp"
    id = "dev.warp-tool"

This block is an **id reference only**. It carries no file path. See the code
step below.

## Installing and enabling

Two independent things: the package has to be present, and the feature has to
be enabled.

Install a package file: unpack it so the archive root lands on
`mods/packages/<id>/<version>/`. The public launcher (DW3 Recompiled+) does
this for you - **Mods** tab, **Load mod package...**, **Install** - writing the
files directly, and it switches the package's features on at the same time; use
its **Disable** / **Enable** buttons from then on. A launcher that links the
runtime's own mod provider instead calls `install(<path to zip>)`, which does
the same thing and then re-scans.

Enable or disable a feature through `mods/state.toml`, `format_version = 2`:

    format_version = 2

    [[package]]
    id = "dev.warp-tool"
    version = "1.0.1"

    [[feature]]
    package_id = "dev.warp-tool"
    id = "warp"
    enabled = true

The runtime reads this file on every launch and rewrites it in this shape after
a mod commit (atomic write), so writing it between runs from outside is safe.
Set `enabled = false` to switch a feature off. The launcher path is
`feature_enable(package_id, feature_id, 0|1)`.

### What happens when the state file is missing or broken

| `state.toml` | result |
|---|---|
| missing | every feature runs at its manifest `default_enabled`. A line is printed to stderr about it. For a mod with `default_enabled = false`, that is **off**. |
| empty | `mods unavailable` (a missing `format_version`), **all mods off**, the game still runs |
| malformed | `mods unavailable` with a parse error, **all mods off**, the game still runs |

A feature that is switched off is not just inactive, its callbacks are never
called at all: no drawing, no game memory writes.

## The code step: plugins are not loaded from the package

The `[[plugin]]` block is an id, not code. Native plugin callbacks are
registered by constructors compiled into the executable. At launch the runtime
checks the selected packages against the plugins it already has and refuses to
start if one is missing:

    cannot launch with selected mods: dev.warp-tool/warp: trusted plugin is unavailable: dev.warp-tool

So a package that ships a `[[plugin]]` needs the plugin linked into the build.

**The launcher does this step.** Its Mods tab has **Rebuild (USA)** and
**Rebuild (EUR)** (disabled until a package is loaded). Pressing the one for
your region copies the package's `plugin/<name>.c` into that build's plugin
source folder, adds it to the build with the region define for that disc,
rebuilds, and replaces the executable. It reuses the launcher's own build path,
so it needs the same toolchain the Play tab already required, and it is
incremental - seconds - whenever that build tree already exists.

By hand it is:

1. Copy `plugin/<name>.c` into the build's plugin source folder.
2. Make the build target compile it, with the region define for the disc:

       target_sources(runtime PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/mods_src/warp_tool.c")
       target_compile_definitions(runtime PRIVATE DW3_GAME_REGION_EU)   # or _US

3. `cmake --build <build dir> --config Release --parallel`
4. Copy the new executable into the build folder.

Data-only mods (byte patches and disc overlays) need no relink. Only packages
with a `[[plugin]]` do.

## The launcher API

This is the API the runtime's own mod provider exposes, for a launcher that
links the provider and calls into it:

    install(path)                         install a package ZIP, then re-scan
    feature_enable(package_id, feature_id, 0|1)
    remove(...)                           remove an installed package
    select(...)                           choose which installed version is active
    set_option(...)                       write an option value
    commit(...)                           persist state.toml
    diagnostics(...)                      report what is installed
    archive_extension / archive_description   for the launcher's file picker

`install` maps to unpacking the archive to `mods/packages/<id>/<version>/` and
refuses an existing version.

The public launcher (DW3 Recompiled+) is Python and does not link the provider,
so it does NOT call this API. It implements the same file contract directly:
unpack the archive, and write `mods/state.toml` in the format above. The two
routes produce identical files, which is why a build cannot tell them apart.
