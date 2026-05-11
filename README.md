# Telescope

Split proxy in C with libuv.

```
[SOCKS5 app] --SOCKS5----------------+
                                     +--> [local] --custom--> [remote] --> [destination]
[browser] --HTTP--> [http] --SOCKS5--+
```

## Protocol (local ↔ remote)

```
L ---- [13-byte pad] ---------------------------->> R
L <<-- [13-byte pad] ----------------------------- R

L ---- ATYP(1) + NADDR(1) + NAME(N) + PORT(2) -->> R
                                                    R <<-- [handshake] -->> destination
L <<-- ATYP(1) + BND.ADDR(4) + BND.PORT(2) ------- R

[pipe]
```

## Build

Requires [libuv](https://libuv.org/) for `local` and `remote`, and pthreads for `http`.

```bash
make        # builds dist/local, dist/remote, dist/http
make debug  # adds -g -O0 -Wall
```

## Setup

**Remote server (Linux):**
```bash
bash install-linux.sh  # installs dist/remote as a systemd service on port 3030
```

**Local machine (macOS):**
```bash
bash install-macos.sh  # installs dist/local and dist/http as LaunchAgents
```

Or run manually:
```bash
dist/remote <port>
dist/local <local-port> <remote-ip> <remote-port>
dist/http <local-port> <remote-ip> <remote-port>
```