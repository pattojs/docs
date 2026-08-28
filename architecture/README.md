# Architecture

This section covers the "why" and "how" behind Pattojs. It is optional
reading; you can use Pattojs happily without it. For tutorials and
reference material, return to the [main documentation](../README.md).

## How it works

When you press **Play**, your script runs inside an isolated sandbox with
limited file and environment access but open network. Every run starts
fresh: previous state, timers, and requests are gone. Pressing **Stop**
kills the process immediately.

Loop guards are injected to keep the playground responsive, and output is
safely captured so that large or circular objects never crash the UI.

## Design philosophy

- **Sandboxed by default.** File and environment access are restricted;
  network is open for API work.
- **Deterministic runs.** Each execution is independent and self-contained.
- **Safe output.** Logging is protected against circular references and
  oversized data.

## Limitations

- Loops are capped at 2000 iterations.
- No DOM access (no `document`, `localStorage`, etc.).
- Remote modules re-download on cold starts.
- Props values are strings only; parse numbers/JSON yourself.
- Only one run at a time.
