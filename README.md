# THUG2 THPG Level Converter

> **Not affiliated with or endorsed by Activision, Neversoft or any other rights holder.** A non-commercial fan project
> for preservation, interoperability and education. No game files are included: the tools work only on copies of the
> games you own. Game names are trademarks of their owners. See LICENSE (MIT for the project's own work; no licence for material
> from the games); third-party parts (downloaded tools) are listed in THIRD_PARTY_NOTICES.md. Rights holders: open an
> issue and it will be dealt with promptly.

Converts Proving Ground's network levels and its bedroom from your own copy of Tony Hawk's Proving Ground (Xbox 360) to the formats of Tony Hawk's Underground 2 (PC), for its level
select. Nothing of either game is in this repository: the files are made from your copies each time. How it works,
what is converted and what isn't: [thpg_levels/README.md](thpg_levels/README.md).

- `thpg_levels/` - the converter (`thpg_levels.ts`: `convert`, `selfcheck`, `list`): Proving Ground's game profile and its own readers (layered materials, light maps, scenes).

## Dependencies

Each pinned to a tagged commit in package.json (`git+https://git@github.com/...#<commit>`; one copy of each: every repository that uses one pins the same commit):

- [thug2-level-converter-core](https://github.com/CanuckAussieKev/thug2-level-converter-core): THUG2's formats (scenes, collision, textures, archives, scripts), the level port's common steps.
- [thug2-thp8-level-converter](https://github.com/CanuckAussieKev/thug2-thp8-level-converter): THPG is built on Project 8's engine - this converter uses that converter's readers (`thp8/`) and runs its converter (`thp8/thp8_levels.ts`) with THPG's profile.
- [thug2-thaw-level-converter](https://github.com/CanuckAussieKev/thug2-thaw-level-converter): THAW's formats under Project 8's - its tests read the converted scripts with `thaw/` (the copy Project 8's converter uses).

## Public API

None: nothing here is imported by another repository (package.json exports no module). The converter is used through its commands.

Commands (package.json's `commands`: the tools with a command line - importing a module never runs one): `thpg_levels/thpg_levels.ts`.

## Usage

Needs Node 24 and NeverScript's `ns` (the QB script compiler): `npx thug2-neverscript fetch` downloads it for your
system (Windows, Linux, macOS - from [our NeverScript fork](https://github.com/CanuckAussieKev/NeverScript)'s release,
SHA-256 checked) and the converter finds it there; or pass `--ns <ns>` / set `NEVERSCRIPT=<ns>`. Inside the
[THUG2 decompilation](https://github.com/CanuckAussieKev/thug2-decomp) its own build is used
(`tools\build_neverscript.ps1`). The converter needs only THUG2's data files, not its exe.

    git clone https://github.com/CanuckAussieKev/thug2-thpg-level-converter
    cd thug2-thpg-level-converter
    npm ci
    npx thug2-neverscript fetch
    node thpg_levels/thpg_levels.ts convert thpg_bedroom --thpg "C:\Games\Tony Hawk's Proving Grounds\ProvingGround" --thug2 C:\Games\THUG2
    node thpg_levels/thpg_levels.ts selfcheck --thug2 C:\Games\THUG2

`convert` writes `<thug2>\thug2-decomp-thpg\` (not read by the game) - or `--out <folder>`, which must be new, empty or an earlier output (any other folder is refused,
nothing in it touched); naming levels converts only those and keeps the others; without level names it converts the default
set (thpg_levels/README.md). The [THUG2 content pipeline](https://github.com/CanuckAussieKev/thug2-content-pipeline)'s
`menu_level/make_menu_level.ts` then installs every converted game's levels into the game's data overlay and level
select. In the decomp, where the pipeline installs this converter, the same commands are

    node tools/pipeline/run.ts thpg_levels/thpg_levels.ts convert --thug2 C:\Games\THUG2
    node tools/pipeline/menu_level/make_menu_level.ts --game C:\Games\THUG2

Checks: `npm run typecheck`, `npm run lint`, `npm test` (the tests that need a game install skip without it;
`package.test.ts` checks package.json's exports, commands and pins). GitHub Actions runs them on every push
(`.github/workflows/ci.yml`).

## Versions

Releases are tagged `v<major>.<minor>.<patch>`; this is `v1.1.4` (core v5.2.0 (goal-only kills left out), thaw v1.4.0, thp8 v1.2.2); `v1.1.3` (imports from core v5.0.0's numbers.ts / text.ts / json.ts / cli.ts (its py.ts split, names for what they do); behaviour unchanged); `v1.1.2` (core v4.0.0, THP8 v1.2.0); `v1.1.1` (core v3.1.0: runs on Linux and macOS with the same output as on Windows; CI on all three); `v1.1.0` (ns found or fetched without the decomp; core v3.0.0, THP8 v1.1.0); `v1.0.1` (core v2.0.0, THAW's converter v1.1.0, Project 8's v1.0.1). The repositories that use it pin the tagged commit.
