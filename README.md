# Jetsam Testnet (JTMT)

A public test chain for [Jetsam](https://jetsamchain.com). **Its coins are worth
nothing, and that is the point.**

> ### ⚠️ JTMT is not JTM
>
> If someone offers to sell you JTMT, or sends you coins claiming they are
> Jetsam (JTM), you are being lied to. Test coins are given away for free by
> the faucet below. There is no reason to ever buy one.
>
> Two checks settle it in seconds:
>
> - **A real Jetsam address starts with `j1`. A testnet address starts with `tj1`.**
>   A mainnet wallet cannot even read a `tj1…` address — it refuses with
>   *"wrong address network"*. They are not interchangeable, at all.
> - A real transaction appears on
>   [explorer.jetsamchain.com](https://explorer.jetsamchain.com/). A test
>   transaction appears only on
>   [the testnet view](https://explorer.jetsamchain.com/?network=testnet),
>   which carries a red banner on every page.

## What this chain is for

Testing releases before they reach mainnet: consensus changes, hard forks,
wallets, pools and miners. It runs **the same code and the same proof matrices**
as mainnet — only its identity differs. A separate codebase would drift within
weeks and stop proving anything.

| | mainnet | testnet |
|---|---|---|
| ticker | JTM | **JTMT** |
| addresses | `j1…` | **`tj1…`** |
| P2P / RPC ports | 9700 / 9701 | **9710 / 9711** |
| data directory | `jetsam` | **`jetsam-testnet`** |
| genesis | `6e592c07…448c5f44` | **`b3efb3c1…110f996d`** |
| coin value | a market decides | **none, by design** |

The two networks cannot connect to each other: different genesis means a
different network profile, and nodes with different profiles part at the
handshake, before a single block is offered.

## Running a node

Linux x86-64 only for now.

```bash
tar xzf jetsam-testnet-linux-x86_64.tar.gz
./jetsam --data-dir ~/.jetsam-testnet \
         --p2p-listen 0.0.0.0:9710 --rpc-listen 127.0.0.1:9711
./jetsam-cli status          # talks to 9711 by default in this build
```

Add `--mode miner --cpu-threads N` to mine. Note that block cadence here is set
by **proof time, not proof of work**: each block carries a recursive proof, and
that proof is what your CPU count actually buys.

## Faucet

Ask on Discord with your `tj1…` address. Free, on request, no limit worth
gaming — the whole point is that these coins stay worthless.

## Reporting something

Consensus bugs found here are exactly what this chain is for. Open an issue on
[jetsam-chain/jetsam](https://github.com/jetsam-chain/jetsam) with the block
height and the node log.
