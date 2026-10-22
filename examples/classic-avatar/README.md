# Classic Stack-chan avatar

Procedural 90-frame matrix (6 faces × 3 eyes × 5 mouths) in the official
black-screen, two-big-eyes style. Load it at runtime with `load_avatar_set`.
No firmware rebuild.

The generator is the source of truth. The RGB565 payload and PNG previews
are build outputs and are gitignored.

## Build

```bash
uv run --with pillow python examples/classic-avatar/make_classic.py
```

Writes `classic-matrix.rgb565` (3,456,000 bytes) and preview PNGs under `png/`.

## Load onto a connected device

Gateway must be running (`http://127.0.0.1:8767/mcp`) and the robot connected.
If the gateway was started with `STACKCHAN_TOKEN` (or the legacy
`BEARER_TOKEN`), export that same value first. The loader sends it as
`Authorization: Bearer …` and does not print it.

```bash
uv run --with pillow python examples/classic-avatar/load_classic.py
```

The set lives in PSRAM. A device reboot falls back to the firmware face until
you load it again.

## Autoload on every connect

Point the gateway at the built file so it reloads after hello — including
reconnects while the gateway is already running. When this path is set,
the same hook turns blink back on (firmware starts with blink off).
The built-in idle face shows as soon as the device is ready. The classic
face replaces it when the transfer finishes; a missing file or a timeout
leaves the built-in face in place.

```bash
export STACKCHAN_AVATAR_SET_PATH="$PWD/examples/classic-avatar/classic-matrix.rgb565"
```

Optional: `STACKCHAN_AVATAR_SET_MODE=matrix` (inferred only from an exact
size match) and `STACKCHAN_AVATAR_SET_TIMEOUT=180`. Export these in the
same environment that starts `stackchan-mcp serve` (the uv-installed CLI
does not reliably pick up a cwd `.env`). This hook is in the current
checkout; published PyPI builds pick it up on the next gateway release.
