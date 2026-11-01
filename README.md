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

- [thug2-level-converter-core](https://github.com/CanuckAussieKev/thug2-level-converter-core): THUG2's formats (scenes, collision, textures, archives, scripts), the level port's common steps and the THUGPRO compatibility register.
- [thug2-thp8-level-converter](https://github.com/CanuckAussieKev/thug2-thp8-level-converter): THPG is built on Project 8's engine - this converter uses that converter's readers (`thp8/`) and runs its converter (`thp8/thp8_levels.ts`) with THPG's profile.
- [thug2-thaw-level-converter](https://github.com/CanuckAussieKev/thug2-thaw-level-converter): THAW's formats under Project 8's - its tests read the converted scripts with `thaw/` (the copy Project 8's converter uses).

## Public API

None: nothing here is imported by another repository (package.json exports no module). The converter is used through its commands.

Commands (package.json's `commands`: the tools with a command line - importing a module never runs one): `thpg_levels/thpg_levels.ts`.

## Usage

Needs Node 24 and NeverScript's `ns.exe` (QB scripts compiled): the
[THUG2 decompilation](https://github.com/CanuckAussieKev/thug2-decomp) builds it (`tools\build_neverscript.ps1` ->
`out\neverscript\ns.exe`, found there when this converter is used inside it) - or pass `--ns <ns.exe>`.

    git clone https://github.com/CanuckAussieKev/thug2-thpg-level-converter
    cd thug2-thpg-level-converter
    npm ci
    node thpg_levels/thpg_levels.ts convert thpg_bedroom --thpg "C:\Games\Tony Hawk's Proving Grounds\ProvingGround" --thug2 C:\Games\THUG2 --ns <ns.exe>
    node thpg_levels/thpg_levels.ts selfcheck --thug2 C:\Games\THUG2

`convert` writes `<thug2>\thug2-decomp-thpg\` (not read by the game); without level names it converts the default
set (thpg_levels/README.md). The [THUG2 content pipeline](https://github.com/CanuckAussieKev/thug2-content-pipeline)'s
`menu_level/make_menu_level.ts` then installs every converted game's levels into the game's data overlay and level
select. In the decomp, where the pipeline installs this converter, the same commands are

    node tools/pipeline/run.ts thpg_levels/thpg_levels.ts convert --thug2 C:\Games\THUG2
    node tools/pipeline/menu_level/make_menu_level.ts --game C:\Games\THUG2

Checks: `npm run typecheck`, `npm run lint`, `npm test` (the tests that need a game install skip without it;
`package.test.ts` checks package.json's exports, commands and pins). GitHub Actions runs them on every push
(`.github/workflows/ci.yml`).

## Versions

Releases are tagged `v<major>.<minor>.<patch>`; this is `v1.0.0`. The repositories that use it pin the tagged commit.
