# CivicNet Wallet — Linux Binaries (blackshirt-crypto build)

Pre-compiled Linux binaries for **CivicNet (CIVIC)**, built from a
**patched fork of the official CivicNet v3.0.5 source code**.

> **Why a patched build?** The official v3.0.5 Linux binaries contain a
> consensus bug that causes all Linux nodes to stall at block 28427 — the
> first PoS block after the August 21, 2026 emergency stake target reset.
> Windows nodes are unaffected due to compiler differences (MSVC vs GCC).
> This build applies two targeted fixes to `pos_kernel.cpp` and
> `validation.cpp` that allow Linux nodes to sync and stay on the correct
> chain. The fixes have been reported to the CivicNet developer.

## What this is

- CivicNet Core **v3.0.5** with Linux sync bug fix, compiled from source
- Built for **Ubuntu 24.04 LTS / Linux Mint 22** (Boost 1.83, libfmt 9)
- Three downloads:
  - **CLI + daemon** (`civicnet-node`, `civicnet-cli`) — relay nodes, pool backends
  - **Qt desktop wallet** (`civicnet-qt`) — graphical wallet with staking support

## The Bug Fix (Technical Summary)

After the PoS emergency stake target reset activated at block 28426
(timestamp 1787302800), Linux nodes failed to accept block 28427's header
with `bad-diffbits, incorrect stake target`.

**Root cause:** `ComputeExpectedStakeTarget()` in `pos_kernel.cpp` read
`pindexPrev->nStakeTarget` from disk for the emergency reset block, which
still held the old stuck value (`0x1b0b3583`) rather than the reset value
(`0x1e0ffff0`) — because the in-memory reset had not been flushed to disk
before a node restart. Additionally, PoS `nBits` validation during
header-only sync used chain context not yet available, causing false
rejections for subsequent PoS blocks.

**Fixes applied:**
1. `pos_kernel.cpp` — force `baseTarget = 0x1e0ffff0` when `pindexPrev`
   is the emergency reset block, regardless of disk value
2. `validation.cpp` — guard PoS `nBits` header check with
   `BLOCK_HAVE_DATA` so it only runs when full block data is available

## Download

Grab the latest build from [**Releases**](../../releases/latest).

| Download | Contains | Use for |
|----------|----------|---------|
| `civicnet-cli-linux-v3.0.5.tar.gz` | `civicnet-node`, `civicnet-cli` | Headless nodes, servers, mining |
| `civicnet-qt-linux-v3.0.5.tar.gz` | `civicnet-qt` | Desktop GUI wallet |

## Quick Start — CLI / Daemon

```bash
curl -L -o civicnet-cli-linux.tar.gz \
  https://github.com/blackshirt-crypto/civicnet-wallet-linux/releases/download/v3.0.5/civicnet-cli-linux-v3.0.5.tar.gz
tar xzf civicnet-cli-linux.tar.gz
chmod +x civicnet-node civicnet-cli
./civicnet-node -daemon
./civicnet-cli getblockchaininfo
```

If you hit a missing-library error:

```bash
sudo apt-get install libboost-filesystem-dev libboost-thread-dev libevent-dev libdb++-dev libfmt-dev
```

## Quick Start — Qt Desktop Wallet

```bash
curl -L -o civicnet-qt-linux.tar.gz \
  https://github.com/blackshirt-crypto/civicnet-wallet-linux/releases/download/v3.0.5/civicnet-qt-linux-v3.0.5.tar.gz
tar xzf civicnet-qt-linux.tar.gz
chmod +x civicnet-qt
./civicnet-qt
```

Qt runtime dependencies if needed:

```bash
sudo apt-get install qtbase5-dev qttools5-dev libqrencode-dev libboost-filesystem-dev libfmt-dev
```

## Verify It Yourself

You can reproduce these binaries from our patched source:

```bash
git clone https://github.com/blackshirt-crypto/civicnet-wallet-linux.git
```

Or verify against the official source with patches applied manually —
see the bug fix description above for exact files and changes.

## Version / Hard-Fork Notes

CivicNet is under active development and has had consensus-changing hard
forks. **Always run the current version** so your node stays on the
network. Check the official releases page:
[github.com/CivicLight/CivicNet/releases](https://github.com/CivicLight/CivicNet/releases).

## Credits & Attribution

- **CivicNet Core** — coin, chain, and wallet/node source code by the
  CivicNet / CivicLight developer:
  [github.com/CivicLight/CivicNet](https://github.com/CivicLight/CivicNet)
- **Bitcoin Core / Litecoin** — upstream codebase
- **blackshirt-crypto** — Linux build + consensus bug fix

All code is licensed under the MIT License.

## Disclaimer

These binaries are provided as-is, with no warranty. Always verify you
are running the current network version. Cryptocurrency involves risk;
you are responsible for securing your own wallet and keys.

---

*Fixed build. Verify, don't trust.*
