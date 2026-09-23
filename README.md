# gccompiled-nx

Turning a GameCube disc into a Switch program.

A game's PowerPC code is translated to C++ ahead of time and compiled for the
Switch; the console's system software is answered natively rather than
simulated. This is the machinery that does it. Players want
[gc-nx](https://github.com/nx-mod/gc-nx) instead.

```
your disc  →  translator  →  game code + runtime  →  NRO
```

Game code and data are never in this repository, and a build happens on the
machine of whoever owns the disc.

## One command

```sh
tools/gccompiled build mygame.iso
```

reads the disc, writes the project, scans the game for the natives the libraries
can bind, translates its code, builds it, and leaves a program to copy to the
card beside the dump.

## The engine is the Wii's

There is no second translator and no second runtime: the two consoles share a
CPU core, a graphics pipeline, a sound chip and most of an SDK. A GameCube game
runs on the same engine a Wii game does, with a different console profile
underneath.

| Library | Role |
|---|---|
| [libdol-nx](https://github.com/nx-mod/libdol-nx) | the machine both consoles are: CPU, GX, DSP, the shared SDK, the translator, the disc reader |
| [libgc-nx](https://github.com/nx-mod/libgc-nx) | what only a GameCube has: ARAM, memory cards, DTK, its boot path |
| [aurora-nx](https://github.com/nx-mod/aurora-nx) | GX on WebGPU |
| [dawn-nx](https://github.com/nx-mod/dawn-nx) | WebGPU on Switch |
| [nxvk](https://github.com/nx-mod/nxvk) | the Vulkan driver underneath |

## Per-game projects

Each game gets a small project of its own, holding its symbol table, its
bindings and any native code only that game needs. Good first targets, by how
much is already known about each:

Which disc matters: a decompilation matches one build, and the symbols only line
up with that one.

| Game | Disc to dump | Why |
|---|---|---|
| Mario Kart: Double Dash!! | `GM4P01` PAL | ships a linker map, `debugInfoS.MAP`, and PAL is the retail build the CC0 decompilation matches - its other target is a Mario Club debug disc. The sibling of the engine most natives were written against |
| Super Mario Sunshine | `GMSP01` PAL | ships `marioEU.MAP`; the decompilation covers PAL and JPN and not USA. JSystem throughout, and a THP video path already replaced natively |
| The Wind Waker | `GZLE01` USA or `GZLP01` PAL | every retail disc ships full symbol maps, and the decompilation supports all of them. A separate static recompilation proves the instruction coverage |
| Twilight Princess | `GZ2E01` | the most complete decompilation of any GameCube game |

A map on the disc is worth more here than anything else: the symbol transfer
reads one directly, and on the Wii side that took a game from ten bound natives
to about three hundred and fifty.

## Homebrew

Homebrew built on libogc needs no translation: the same source compiles against
the platform layer and links as an NRO. Homebrew whose source was never released
goes through the translator like a game. See
[libdol-nx's notes](https://github.com/nx-mod/libdol-nx/blob/main/docs/homebrew.md).

## License

GPL-3.0-or-later. No game code or data is included or distributed.
