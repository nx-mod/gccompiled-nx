# gccompiled-nx

GameCube games running natively on Nintendo Switch — statically recompiled from
your own discs, not emulated. A game's PowerPC code is translated to C++ ahead
of time and compiled for the Switch, with the console's system software answered
natively rather than simulated.

Game code and data are never in this repository. You build from your own disc.

```
your disc  →  translator  →  game code + runtime  →  NRO
```

## The engine is the Wii's

There is no second translator and no second runtime. The GameCube and the Wii
share a CPU core, a graphics pipeline, a sound chip and most of an SDK, so a
GameCube game runs on the same engine a Wii game does, with a different console
profile underneath.

| Library | Role |
|---|---|
| [libdol-nx](https://github.com/nx-mod/libdol-nx) | the machine both consoles are: CPU, GX, DSP, the shared SDK, the translator, the disc reader |
| [libgc-nx](https://github.com/nx-mod/libgc-nx) | what only a GameCube has: ARAM, memory cards, DTK, its boot path |
| [aurora-nx](https://github.com/nx-mod/aurora-nx) | GX on WebGPU |
| [dawn-nx](https://github.com/nx-mod/dawn-nx) | WebGPU on Switch |
| [nxvk](https://github.com/nx-mod/nxvk) | the Vulkan driver underneath |

## Games

One folder per game in [gcgames-nx](gcgames-nx), holding what that game needs of
its own: its symbol table, its bindings, and any native code only it requires.

Candidates, by how much is already known about each:

| Game | Disc | Why it is a good first target |
|---|---|---|
| Mario Kart: Double Dash!! | `GM4E01` | the sibling of the engine most of the natives were written against, with a CC0 decompilation |
| Super Mario Sunshine | `GMSE01` | CC0 decompilation, JSystem throughout, and a THP video path already replaced natively |
| The Wind Waker | `GZLE01` | CC0 decompilation, and its instruction coverage is proven by a separate port |
| Twilight Princess | `GZ2E01` | the most complete decompilation of any GameCube game |

## Homebrew

GameCube homebrew built on libogc needs no translation at all: the same source
compiles against the platform layer directly and links as an NRO. Homebrew whose
source was never released goes through the translator like a game.

## License

GPL-3.0-or-later. No game code or data is included or distributed.
