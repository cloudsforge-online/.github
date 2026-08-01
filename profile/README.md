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

## Every repository, and what it owns

One repository per deployable. Each service owns exactly one database and reads no other — the
answer to needing another service's data is an HTTP call typed by a published contract, never a
second connection string, and CI greps for that.

### The products people use

| Repository | Scope |
| --- | --- |
| [`hearth`](https://github.com/cloudsforge-online/hearth) | The chain itself — Homefire PoW, UTXO + Ed25519, node, miner, EVM layer and contracts. **Public**, and takes outside contributors. |
| `micro-mint` | Forge Create: token orders, the deployment lifecycle, the token registry, contract templates. A deploy leaves the request; nothing is held for three minutes. |
| `micro-trade` | Forge Trade: the strategy catalogue, backtests, bots, fills and fee settlement. A backtest replays byte-identically from a seed. |
| `micro-market` | Forge Market: listings, offers, bids, auctions, orders, escrow references, collections, moderation and disputes. Escrow is a ledger reservation, never a balance held here. |
| `micro-worlds` | Forge Worlds: the title registry, shared player profile, inventory, achievements, seasons and the entitlement bridge. |
| `micro-nda` | *Ninety Days After*: the shared map, tiles, players, actions and the resolution engine. Ported so a day resolves byte-identically to its ancestor. |
| `micro-emberkin` | *Emberkin*: the monster-collecting RPG. Its ported RNG reproduces the original bit-for-bit, so recorded battles replay exactly. |
| `micro-foresight` | Forge Foresight: markets, the AI idea pipeline with cited provenance, on-chain settlement and disputes. The service holds no stake. |
| `micro-community` | Communities, membership, proposals, votes, delegations, timelocks and treasury executions. |

### The platform beneath them

| Repository | Scope |
| --- | --- |
| `micro-identity` | Accounts, credentials, MFA, sessions, devices, refresh families, signing keys and JWKS. The root of trust. |
| `micro-ledger` | Double-entry accounting: chart of accounts, journal entries, postings, balances, reservations, reconciliation. Its database refuses an unbalanced journal. |
| `micro-custody` | HD seeds, key generation, the encryption envelope, signing policy and key lifecycle. It has no reveal endpoint, by deletion rather than by guard. |
| `micro-wallet` | Wallet registry, deposit addresses, withdrawals, conversions, transfers and the portfolio read. Holds no balances; composes ledger, custody and indexer. |
| `micro-settlement` | Treasuries, sweeps, outbound transaction building, broadcast and confirmation tracking. |
| `micro-indexer` | Blocks, transactions, receipts, logs, address activity, balances, reorgs and provider health. Reorg safety is the whole job. |
| `micro-pricing` | Market sources, the median oracle, administered prices, spread policy and rate history. A rate that cannot be quoted is an error, never a default. |
| `micro-billing` | Products, prices, entitlements, subscriptions, usage, invoices, refunds and creator payouts. |
| `micro-policy` | Rules, limits, velocity counters, trusted addresses, cooling-off, approvals and freezes. Fail-closed and fail-open are separated deliberately. |
| `micro-activity` | The canonical activity record and the unified feed. |
| `micro-notify` | Preferences, templates, notifications, deliveries, digests and developer webhooks. A critical notification ignores preferences. |
| `micro-hub-api` | The Forge Hub BFF: dashboard aggregation, portfolio composition, search and suggested actions. |
| `micro-devplatform` | Developer organisations, projects, API keys, OAuth clients, webhooks, usage and quotas. Its database refuses a fast hash. |
| `micro-admin-api` | The operator BFF: cross-service actions, approval queues and a tamper-evident audit mirror. |
| `micro-analytics` | A pseudonymised product event store, funnels, cohorts and retention. A raw subject cannot be stored, even with the service bypassed. |

### The things people look at

| Repository | Scope |
| --- | --- |
| `micro-site` | The marketing site. No number on it that is not checkable against something real. |
| `micro-hub-web` | Forge Hub: dashboard, portfolio, wallet, activity, security, entitlements. |
| `micro-market-web` | Forge Market's storefront, listings, orders and disputes. |
| `micro-foresight-web` | Forge Foresight's public markets. It recomputes the question hash in the browser. |
| `micro-foresight-admin-web` | The Foresight operator console, its own bundle by design. |
| `micro-emberkin-web` | The Emberkin game client. It deletes the battle engine it inherited: a client that can resolve a battle can lie about one. |
| `micro-status-web` | The public status page. Green-on-unknown is structurally unreachable. |

### Operations

| Repository | Scope |
| --- | --- |
| `micro-beacon` | Synthetic probes, journeys, incidents, SLOs and error budgets. **The release gate** — an unknown refuses. |
| `micro-lantern` | Log triage: OTLP ingest, error grouping, browser errors and trace lookup. Credentials are scrubbed before anything is stored. |
| `micro-faucet` | The testnet EMBER faucet. It refuses to start against a chain that is not the testnet. |
| `micro-deploy` | The telemetry stack, the gateway configuration and the public API route map. |

### Libraries and machinery

| Repository | Scope |
| --- | --- |
| `micro-runtime` | `@cloudsforge/lifecycle`, `-http`, `-jobs`, `-db`, `-auth`, `-telemetry`. The six copies of the same file that used to drift. |
| `micro-contracts` | The typed contracts services agree on, split by bounded context. `-chain` is exact-pinned, because a skew credits money at the wrong depth. |
| `micro-ui` | The design system: tokens, chrome, the product accents and the validated chart palette. |
| `micro-sdk` | The public developer SDK and CLI. Zero runtime dependencies; every route cites the line that serves it. |
| `micro-org` | The reusable CI every repository calls, the contract-compatibility checker, `cfctl` and the README template. |
| `micro-service-template` | A working service skeleton with every runtime library wired. |
| `micro-web-template` | The same for a frontend, including the guards a frontend keeps forgetting. |

### Assets and record

| Repository | Scope |
| --- | --- |
| `micro-brand` | 93 generated brand assets with per-asset provenance, and the numeric ground normaliser that makes them one family. |
| `micro-emberkin-assets` | 83 generated assets for Emberkin, prompted from the game's own visual spec. |
| `micro-docs` | Twenty documents: the architecture and security decisions, the domain model, the testing strategy, and a build ledger recording what is actually true. |
| `micro-conformance` | A recorded corpus of real interactions — the behavioural baseline a successor has to match. |

### Still in build

`micro-admin-web`, `micro-mint-web`, `micro-trade-web`, `micro-worlds-web`, `micro-explorer-web`,
`micro-network-site`, `micro-devportal-web`.

### Predecessors

`platform`, `forge-pay`, `forge-keyvault`, `forge-mint`, `crucible`, `ninety-days-after`,
`shared-libs`, `asset-forge`, `stack`. **Nothing was deleted, archived or renamed.** They remain
deployable and are the rollback target; the estate above was built beside them.


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
