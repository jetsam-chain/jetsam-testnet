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
| proof of work | Poseidon2b sponge | **TowerWalk, since block 4650** |
| coin value | a market decides | **none, by design** |

The two networks cannot connect to each other: different genesis means a
different network profile, and nodes with different profiles part at the
handshake, before a single block is offered.

## Running a node

Linux x86-64 only for now. Copy this whole block:

```bash
curl -LO https://github.com/jetsam-chain/jetsam-testnet/releases/latest/download/jetsam-testnet-linux-x86_64.tar.gz
tar xzf jetsam-testnet-linux-x86_64.tar.gz
cd jetsam-testnet-linux-x86_64

./jetsam --data-dir ~/.jetsam-testnet \
         --p2p-listen 0.0.0.0:9710 --rpc-listen 127.0.0.1:9711 \
         --seed 193.160.130.205:9710
```

The node prints its network on the first line. It must say **`· testnet`** —
if it says `mainnet`, you are running the wrong binary, stop there.

In a second terminal, from the same directory:

```bash
./jetsam-cli balance         # what you have
./jetsam-cli mining          # hashrate, difficulty, block reward
./jetsam-cli status          # height and chain tip
```

The CLI finds the daemon on 9711 by itself; `--rpc` is only for a node
somewhere else. Mining prints two lines worth watching, and they are coloured
so they do not disappear into the sync traffic:

```
⛏  8.30 kH/s  16 threads · 1188132 hashes total
✅ BLOCK WON — block accepted #5629   50 JTM · pow 20.5s · proof 12.4s
```

Every release is listed at
[github.com/jetsam-chain/jetsam-testnet/releases](https://github.com/jetsam-chain/jetsam-testnet/releases).
Download the **`.tar.gz` asset** — not the "Source code" link, which contains
no binaries.

Add `--mode miner --cpu-threads N` to mine.

### What you are testing

Since block 4650 this chain runs **TowerWalk**, a cache-resident proof of work.
Each attempt walks a 512 KiB scratchpad through 524 288 dependent reads: the
address of one read cannot be computed until the previous one has returned. No
amount of parallelism removes that chain — only memory latency at the innermost
cache level does.

That working set is chosen to sit inside a CPU's private L2 cache and outside
what a GPU can give each of the thousands of threads it needs to keep its
arithmetic units busy. **On this chain a CPU is the sensible machine to mine
with, and that is the property under test.** A GPU miner built for the previous
digest is refused by the node, every time, with
`proof of work: digest is not below the target`.

A block still carries a recursive proof, and proving still costs wall-clock
time. The difference since 4650 is that the search now costs something too.

## Faucet

Ask on Discord with your `tj1…` address. Free, on request, no limit worth
gaming — the whole point is that these coins stay worthless.

## Reporting something

Consensus bugs found here are exactly what this chain is for. Open an issue on
[jetsam-chain/jetsam](https://github.com/jetsam-chain/jetsam) with the block
height and the node log.
