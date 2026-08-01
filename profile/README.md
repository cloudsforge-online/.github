# CloudsForge

**One crypto world. Mine it, trade it, make it, sell it, play in it, and stake on what happens next.**

Six products, one account, one wallet. The point of the account is not the
account — it is that the balance you earn or buy once works everywhere, and that
the coin you mine on your own laptop is the same coin that funds the rest.

## The loop

Mine EMBER at home → deposit it → convert it to Shards → spend it across
everything we make.

Hearth's proof-of-work is CPU-only and non-outsourceable. No farms, no pools, no
rigs — the work has to happen on the machine that owns the reward. What you mine
is what funds a trading bot, a token launch, a listing, a season in a world that
ends, or a position on a market that settles on chain.

## The six

|  | | |
| --- | --- | --- |
| **Mine** | **Forge Network** | A CPU-mined, ASIC-resistant proof-of-work coin. Homefire PoW, UTXO + Ed25519, 15-second blocks, no premine. EMBER is the unit. → [repository](https://github.com/cloudsforge-online/hearth) |
| **Trade** | **Forge Trade** | Strategies backtested against real market history with fees and slippage charged — because a strategy that only works for free does not work. What survives goes to paper, and only then to money. Not an exchange. |
| **Make** | **Forge Create** | Token deployment across the EVM majors and Solana. Real OpenZeppelin contracts, testnet by default, mainnet when you mean it. |
| **Sell** | **Forge Market** | Listings, offers, auctions and escrow. The escrow is a reservation in the ledger rather than a balance we hold, and every royalty split sums exactly to the sale price. |
| **Play** | **Forge Worlds** | Worlds built to end. *Ninety Days After* is one shared map with resources that genuinely run out and a 90-day season that seals into history. *Emberkin* is a monster-collecting RPG where how you raise a creature decides what it becomes. |
| **Predict** | **Forge Foresight** | Parimutuel markets on future events, staked in EMBER and **settled by the contract, not by us**. If our database vanished, every stake would still be in the contract and every winner could still claim. |

Beneath those, and deliberately not products: identity, custody, the ledger,
settlement, the chain indexer, pricing, billing, policy and the operator tooling.
An account is not something you choose. It is something you are given.

## What we will not do

Refusals, not gaps on a roadmap.

- **We are not an exchange.** No order book, no market making, no custody of
  anyone else's trading pairs. The other side of that line is a different company
  with a different regulatory posture.
- **A game never sells an advantage.** Cosmetics and seasons are entitlements. No
  creature is a token, and nothing you buy changes a stat.
- **Product analytics is ours and pseudonymous by construction, never a
  third-party tag.** Our frontend CI fails the build if a Google, Segment, Hotjar
  or Mixpanel tag appears in a bundle.
- **No service holds money.** Value moves as a double-entry ledger posting, and
  the ledger's *database* refuses an unbalanced journal — not the service, the
  database, so it holds even against a caller with a connection.

## How this is built

One repository per deployable, each with its own database and no access to
anybody else's. Services talk over HTTP typed by published contracts, and events
go Postgres outbox → signed HTTP → inbox. There is no message broker and no
shared schema.

Every repository runs the same reusable CI: typecheck, its own suite against a
real Postgres — a suite that *skips* its database tests fails the build — the
estate rules that repository walls used to enforce for free, a container that
must boot and answer `/livez`, and a secret-hygiene sweep.

The engineering log is public to the people who work here: twenty documents
covering the architecture and security decisions, the domain model, the testing
strategy, and a build ledger that records what is actually true rather than what
was planned — including the defects found on the way and the ones deliberately
left open.

## Status

**Pre-launch, and honest about it.** The services are built and tested; nothing
is serving the public yet. Hearth runs on a testnet. Where a product page says a
capability is in build, it means exactly that.

Most repositories are private while the estate settles. Hearth is public, takes
outside contributors, and is where the interesting cryptography lives; the
developer SDK becomes public with the API it describes.

Nothing here promises a return. Backtests describe the past. Mining yields depend
on difficulty. A parimutuel payout depends on the pool at settlement. A coin with
no mainnet has no price.
