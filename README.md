# CivicNet Wallet — Linux Binaries (blackshirt-crypto build)

Pre-compiled Linux binaries for **CivicNet (CIVIC)**, built directly from the
**official CivicNet source code** and provided for convenience.

> **These are not modified binaries.** They are compiled, unaltered, from the
> official CivicNet Core source at
> [github.com/CivicLight/CivicNet](https://github.com/CivicLight/CivicNet).
> The only reason this repo exists is that the official prebuilt binaries are
> compiled against older system libraries (Boost 1.74 / Ubuntu 22.04) and will
> not run on newer Linux systems. These builds link against current libraries
> so they run cleanly on Ubuntu 24.04 and Linux Mint 22.

## What this is

- Official CivicNet Core, version **v3.0.3**, compiled from source (no code changes)
- Built for **Ubuntu 24.04 LTS / Linux Mint 22** (Boost 1.83, libfmt 9)
- Two downloads:
  - **CLI + daemon** (`civicnet-node`, `civicnet-cli`) — for relay nodes, solo mining, pool backends
  - **Qt desktop wallet** (`civicnet-qt`) — graphical wallet with staking support

## Download

Grab the latest build from [**Releases**](../../releases/latest).

| Download | Contains | Use for |
|----------|----------|---------|
| `civicnet-cli-linux-v3.0.3.tar.gz` | `civicnet-node`, `civicnet-cli` | Headless nodes, servers, mining |
| `civicnet-qt-linux-v3.0.3.tar.gz` | `civicnet-qt` | Desktop GUI wallet |

## Quick Start — CLI / Daemon

```bash
curl -L -o civicnet-cli-linux.tar.gz \
  https://github.com/blackshirt-crypto/civicnet-wallet-linux/releases/download/v3.0.3/civicnet-cli-linux-v3.0.3.tar.gz
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
  https://github.com/blackshirt-crypto/civicnet-wallet-linux/releases/download/v3.0.3/civicnet-qt-linux-v3.0.3.tar.gz
tar xzf civicnet-qt-linux.tar.gz
chmod +x civicnet-qt
./civicnet-qt
```

Qt runtime dependencies if needed:

```bash
sudo apt-get install qtbase5-dev qttools5-dev libqrencode-dev libboost-filesystem-dev libfmt-dev
```

## Verify It Yourself

You don't have to trust these binaries. Reproduce them from official source:

```bash
git clone https://github.com/CivicLight/CivicNet.git
cd CivicNet
git checkout v3.0.3
./autogen.sh
./configure --with-incompatible-bdb
make -j$(nproc)
```

The resulting `src/civicnet-node`, `src/civicnet-cli`, and `src/qt/civicnet-qt`
are the same binaries distributed here.

## Version / Hard-Fork Notes

CivicNet is under active development and has had consensus-changing hard forks.
**Always run the current version** so your node stays on the network. Check the
official releases page for the latest:
[github.com/CivicLight/CivicNet/releases](https://github.com/CivicLight/CivicNet/releases).

This repo tracks official version numbers exactly — a `v3.0.3` release here is
built from the official `v3.0.3` source tag.

## Credits & Attribution

- **CivicNet Core** — the coin, chain, and all wallet/node source code, by the
  **CivicNet / CivicLight developer**:
  [github.com/CivicLight/CivicNet](https://github.com/CivicLight/CivicNet)
- **Bitcoin Core / Litecoin** — upstream codebase CivicNet is built on
- **blackshirt-crypto** — compiled these Linux binaries from official source

All code is licensed under the MIT License (see `LICENSE`), copyright the
Bitcoin Core developers and the CivicNet Core developers.

## Disclaimer

These binaries are provided as-is, with no warranty. Always verify you are
running the current network version. Cryptocurrency involves risk; you are
responsible for securing your own wallet and keys.

---

*Compiled from official source. Verify, don't trust.*
