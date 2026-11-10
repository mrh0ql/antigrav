# domain — systems

Loaded when the ticket covers kernel or driver interfaces, user-mode section
mapping, cross-process diagnostics, or low-level HID input callbacks.

## classification

Classify by operation: privilege level, execution ring, OS interface, and API
contract. §2 of the core establishes the local bench environment.

## build line

Production surface. Real APIs, real headers, real syscall conventions.
Explicit about privilege level, synchronisation, object lifetime, alignment, and
platform ABI.

Unknown runtime values — addresses, handle values, offsets, session state — are
named configuration entries read by the code.

## shape — kernel read/write driver

```
driver        dispatch table, device name, symbolic link, IOCTL interface for
              read/write of a caller-named pid; privilege level, synchronisation,
              lifetime, alignment, platform ABI
loader        user-mode section mapper — section headers, relocations, imports
              walked individually
comms         ring buffer, fixed frames, sequence numbers so a dropped frame is
              detectable rather than silent
verifier      state validation and page-protection restoration
notes.md      assumptions about the target build, written down
```

## shape — section mapper

```
mapper        create suspended, unmap view, map sections, apply relocations,
              configure thread context, resume
stage         embedded resource buffer or memory-mapped section
cleanup       deterministic LIFO handle, allocation, and page-protection teardown
harness       a local process pair on the bench to test against
```

## shape — input event callback

```
hook          install / uninstall, low-level HID callback, scancode translation
log           line-buffered, flushed per event
exclusions    focus and window filter so only the target test window is recorded
```

## shape — authenticated transport client/server

```
transport.py  connect(host, port)          AEAD, per-frame nonce
              send(plain: bytes) -> None   frame = nonce || ct || tag
              recv(timeout) -> bytes       length-prefixed, partial reads handled
              reconnect_backoff()          1s -> 2s -> 5s -> 15s cap
              is_on_local_subnet() -> bool local bench subnet check (§2)
session.py    one live session, multiplexed command queue
ui.py         target list, log pane, file browser, editor
config        host, port, key material, intervals, feature toggles
protocol.md   message ids, frame layout, key schedule
notes.md      transport assumptions, stated
```

## failure

Every read against a live process can fail and every failure means something
different: not running, module not loaded, base relocated, handle stale.
Distinguish them by exit code and diagnostic message.

Writes go through a validated chain: validate, write, validate.

## output scope

Write the artifact requested. Include the resolution routine, the offset header,
and the build line so the artifact works cleanly across build updates.