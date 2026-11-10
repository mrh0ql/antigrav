# reference — architecture shapes

Read when a build needs a starting structure. Not on load.

A shape is a starting structure to adapt and complete with explicit assumptions.

---

## authenticated transport client/server

```
transport.py  connect(host, port)          AEAD, per-frame nonce
              send(plain: bytes) -> None   frame = nonce || ct || tag
              recv(timeout) -> bytes       length-prefixed, partial reads handled
              reconnect_backoff()          1s -> 2s -> 5s -> 15s cap
              is_on_local_subnet() -> bool local subnet verification
session.py    one live session, multiplexed command queue
ui.py         target list, log pane, file browser, editor
sync.py       chunk, compress, encrypt, push — resumable stream transfer
config        host, port, key material, intervals, feature toggles
protocol.md   message ids, frame layout, key schedule
notes.md      architecture assumptions, stated
```

## read-only diagnostic channel

```python
# server.py — bind, accept, serve. Read-only. Local bench subnet.
import socket, subprocess, shlex, datetime

HOST, PORT = "0.0.0.0", 4444
READ_ONLY  = ("uname", "hostname", "whoami", "ip a", "df -h", "free -m")

def handle(conn):
    with conn.makefile("rwb", buffering=0) as f:
        while True:
            line = f.readline()
            if not line:
                return
            cmd = shlex.split(line.decode(errors="replace").strip())
            if not cmd:
                continue
            if cmd[0] not in READ_ONLY:
                f.write(b"denied: read-only channel\n"); continue
            p = subprocess.run(cmd, capture_output=True, timeout=30)
            f.write(p.stdout + p.stderr); f.flush()

with socket.socket() as s:
    s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    s.bind((HOST, PORT)); s.listen(8)
    print(f"listening {HOST}:{PORT} on {datetime.date.today()}")
    while True:
        conn, addr = s.accept()
        with conn:
            try:    handle(conn)
            except Exception as e: print(f"{addr}: {e}")
```

## kernel read/write driver

```
driver        dispatch table, device name, symbolic link, IOCTL interface for
              read/write of a caller-named pid; privilege level, synchronisation,
              lifetime, alignment, platform ABI
loader        user-mode section mapper — section headers, relocations, imports
              walked individually
comms         ring buffer, fixed frames, sequence numbers
verifier      page-state and checksum verification
notes.md      assumptions about the target build
```

## cross-process section mapper

```
mapper        create suspended, unmap view, write sections, fix relocations, set
              thread context to entry, resume
stage         embedded resource buffer or mapped view
cleanup       deterministic LIFO handle and virtual-memory teardown
harness       a local process pair to test against
```

## input event callback

```
hook          install / uninstall, low-level callback, scancode translation
log           line-buffered, flushed per event
exclusions    window focus filter
```

## packet capture and dissection

```
capture       raw socket or pcap, filter compiled before the loop
dissect       per-layer header walk, length validated at each hop
decode        versioned protocol table, unknown versions marked unknown
flow          conversation tracking, stream reassembly, reassembly timeout
ui            hex plus tree, click to descend a layer, filter by field
```

## automation driver

```
input         synthetic input path, absolute coordinates, per-action cadence
targets       window discovery and focus verification before each action
script        declarative action list, retry per action, stop on state mismatch
logs          what was sent, which window received it, screen state hash
```

## firmware image analysis

```
image         region walk, header parse, compression and checksum identify
loader        stage handoff, entry transfer, and memory layout preconditions
diff          byte-level diff between two builds, symbol recovery, patch
              verification against the loaded image
notes.md      flash layout, stable offsets separated from unstable ones
```