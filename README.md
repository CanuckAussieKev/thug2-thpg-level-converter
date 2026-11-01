# THUG2 THPG Level Converter

> **Not affiliated with or endorsed by Activision, Neversoft or any other rights holder.** A non-commercial fan project
> for preservation, interoperability and education. No game files are included: the tools work only on copies of the
> games you own. Game names are trademarks of their owners. See LICENSE (MIT for the project's own work; no licence for material
> from the games); third-party parts (downloaded tools) are listed in THIRD_PARTY_NOTICES.md. Rights holders: open an
> issue and it will be dealt with promptly.

Converts Proving Ground's network levels and its bedroom from your own copy of Tony Hawk's Proving Ground (Xbox 360) to the formats of Tony Hawk's Underground 2 (PC), for its level
select. Nothing of either game is in this repository: the files are made from your copies each time. How it works,
what is converted and what isn't: [thpg_levels/README.md](thpg_levels/README.md).

It runs Project 8's converter (`thp8/` in thug2-level-converter-core) with Proving Ground's own game profile and readers (its layered materials, light maps and scenes).

- `thpg_levels/` - the converter (`thpg_levels.ts`: `convert`, `selfcheck`, `list` where it has one) and its tools.

It depends only on [thug2-level-converter-core](https://github.com/CanuckAussieKev/thug2-level-converter-core) (pinned
to a commit in package.json): THUG2's formats and what the converters share.

## Usage

Needs Node 24 and NeverScript's `ns.exe` (QB scripts compiled): the
[THUG2 decompilation](https://github.com/CanuckAussieKev/thug2-decomp) builds it (`tools\build_neverscript.ps1` ->
`out\neverscript\ns.exe`, found there when this converter is used inside it) - or pass `--ns <ns.exe>`.

    git clone https://github.com/CanuckAussieKev/thug2-thpg-level-converter
    cd thug2-thpg-level-converter
    npm install
    node thpg_levels/thpg_levels.ts convert thpg_bedroom --thpg "C:\Games\Tony Hawk's Proving Grounds\ProvingGround" --thug2 C:\Games\THUG2 --ns <ns.exe>
    node thpg_levels/thpg_levels.ts selfcheck --thug2 C:\Games\THUG2

`convert` writes `<thug2>\thug2-decomp-thpg\` (not read by the game); without level names it converts the default
set (thpg_levels/README.md). The [THUG2 content pipeline](https://github.com/CanuckAussieKev/thug2-content-pipeline)'s
`menu_level/make_menu_level.ts` then installs every converted game's levels into the game's data overlay and level
select. In the decomp, where the pipeline installs this converter, the same commands are

    node tools/pipeline/run.ts thpg_levels/thpg_levels.ts convert --thug2 C:\Games\THUG2
    node tools/pipeline/menu_level/make_menu_level.ts --game C:\Games\THUG2

Checks: `npm run typecheck`, `npm run lint`, `npm test` (the tests that need a game install skip without it).
