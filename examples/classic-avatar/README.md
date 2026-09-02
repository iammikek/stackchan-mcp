# Classic Stack-chan avatar

Procedural 90-frame matrix (6 faces × 3 eyes × 5 mouths) in the official
black-screen, two-big-eyes style. Load it at runtime with `load_avatar_set`.
No firmware rebuild.

For the full factory-kit → gateway → Cursor path, see
[docs/first-run-from-factory.md](../../docs/first-run-from-factory.md).

The generator is the source of truth. The RGB565 payload and PNG previews
are build outputs and are gitignored.

## Build

```bash
uv run --with pillow python examples/classic-avatar/make_classic.py
```

Writes `classic-matrix.rgb565` (3,456,000 bytes) and preview PNGs under `png/`.

## Load onto a connected device

Gateway must be running (`http://127.0.0.1:8767/mcp`) and the robot connected.

```bash
uv run --with pillow python examples/classic-avatar/load_classic.py
```

The set lives in PSRAM. A device reboot falls back to the firmware face until
you load it again.
