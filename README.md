# CivicNet Wallet — Linux Binaries (blackshirt-crypto build)

Pre-compiled Linux binaries for **CivicNet (CIVIC)**, built from the
official CivicNet v3.0.8 source code.

Built for Ubuntu 24.04 LTS / Linux Mint 22 (Boost 1.83, libfmt 9).
Includes full HVL (Hybrid Value Layer) token support.

## What this is

- CivicNet Core **v3.0.8**, compiled from official source
- Built for **Ubuntu 24.04 LTS / Linux Mint 22**
- CLI + daemon (civicnet-node, civicnet-cli) — relay nodes, pool backends, solo mining
- Qt desktop wallet (civicnet-qt) — coming soon for v3.0.8

## Download

Grab the latest build from the [Releases](../../releases/latest) page.

| Download | Contains | Use for |
|----------|----------|---------|
| civicnet-core-linux-v3.0.8.tar.gz | civicnet-node, civicnet-cli | Headless nodes, servers, mining |

## Quick Start

```bash
curl -L -o civicnet-core-linux-v3.0.8.tar.gz https://github.com/blackshirt-crypto/civicnet-wallet-linux/releases/download/v3.0.8/civicnet-core-linux-v3.0.8.tar.gz
tar xzf civicnet-core-linux-v3.0.8.tar.gz
chmod +x civicnet-node civicnet-cli
./civicnet-node -daemon
./civicnet-cli getblockchaininfo
```

## Upgrading from v3.0.7 or earlier

v3.0.8 requires a one-time reindex after installing:

```bash
./civicnet-node -daemon -reindex-chainstate
```

Wait until the log shows: HVL canonical state: READY

Then stop and restart normally without -reindex-chainstate.

## Upgrading from v3.0.5 (blackshirt-crypto patched build)

Our v3.0.5 patched build is now superseded. The Linux sync bug we fixed
in v3.0.5 was properly resolved by ruglover69 in v3.0.6 via the unified
PredictNextStakeTarget() function. v3.0.8 includes all fixes plus the
full HVL token layer. Upgrade immediately — nodes on v3.0.5 are on a
forked chain since August 26, 2026.

## Dependencies

```bash
sudo apt-get install libboost-filesystem-dev libboost-thread-dev libevent-dev libdb++-dev libfmt-dev
```

## Verify It Yourself

```bash
git clone https://github.com/CivicLight/CivicNet.git
cd CivicNet
git checkout v3.0.8
./autogen.sh
./configure --with-incompatible-bdb --disable-tests --disable-bench --without-gui
make -j$(nproc)
```

## Version History

| Version | Notes |
|---------|-------|
| v3.0.8 | HVL Authority v2, canonical token state, consensus hardening |
| v3.0.7 | HVL token layer introduced |
| v3.0.6 | PoS stake target unification via PredictNextStakeTarget() |
| v3.0.5* | blackshirt-crypto patched build - Linux sync bug fix (superseded) |
| v3.0.3 | Original Ubuntu 24.04 compatible build |

## Add Our Relay Node

addnode=172.245.139.245:9333

## Credits

- CivicNet Core by ruglover69 / CivicLight: https://github.com/CivicLight/CivicNet
- Bitcoin Core / Litecoin upstream codebase
- blackshirt-crypto Linux builds for Ubuntu 24.04

## Disclaimer

These binaries are provided as-is, with no warranty. Always verify you
are running the current network version.

Compiled from official source. Verify, dont trust.
