# KytyPS5 – Ghost of Yōtei (AMD test build)

Work-in-progress test build of KytyPS5 for **Ghost of Yōtei (PPSA26344)** on **AMD graphics cards**.
Tested on an RX 9060 XT. **This is not a finished result.**

**Download:** see [Releases](../../releases) (Windows build + source archive).

## Current status

- Boots and reaches gameplay (night village scene)
- About **5 fps** (200 ms/frame)
- Water, trees/foliage, flowers and ground fog do not render yet
- Very high memory use (about 15 GB VRAM), possible GPU reset in busy scenes

## What I found so far

- The game runs its water and grass passes and the emulator draws them, so the problem is wrong data or output, not missing draw calls.
- Most frame time goes into GPU compute shaders (translated PS5 shaders) and CPU–GPU sync waits (`COND_EXEC` stalls ~40 ms per frame).
- Ruled out: texture placeholders, compute chain, BC buffer writes, timestamp scale.
- RenderDoc cannot be used (the layer causes a device loss at start).

## How to run

```
kyty_emulator.exe --game <path>\PPSA26344-app\eboot.bin --readback-linear-images true
```

`--readback-linear-images true` is required for correct textures. The first start compiles shaders and stutters; close the game window normally so the pipeline cache is saved.
You need a large pagefile (system commit limit of about 60 GB worked for me).

## Help wanted

If you have fixes or findings for Ghost of Yōtei (missing effects, sync waits, shader cost, memory use), please share what you changed and where, or open an issue. Working together is the fastest way.

## Legal

Based on [KytyPS5](https://github.com/KytyPS5/KytyPS5) and the Ghost of Yōtei fork. License: GPL-2.0 (see `LICENSE`).
No game files are included. Use only game files that you own. Not affiliated with Sony or PlayStation.
