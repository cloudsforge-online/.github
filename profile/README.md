<img src="https://raw.githubusercontent.com/cloudsforge-online/.github/main/profile/assets/avatar.png" alt="" width="96" align="right" />

# CloudsForge

**One crypto world. Mine it, trade it, make it, sell it, play in it, and stake on what happens next.**

Six products, one account, one wallet. The point of the account is not the
account — it is that the balance you earn or buy once works everywhere, and that
the coin you mine on your own laptop is the same coin that funds the rest.

## The loop

Mine EMBER at home → deposit it → convert it to Shards → spend it across
everything we make → and what you earn back lands in the same wallet.

Hearth's proof-of-work is CPU-only and non-outsourceable. No farms, no pools, no
rigs — the work has to happen on the machine that owns the reward.

That is the whole point of one account. The coin you mined funds a backtest, and
the bot that survives it is charged from the same balance. A token you launch is
listed in the same marketplace, and the royalty comes back to the same wallet. A
season in a world that ends is bought with the same Shards as a position on a
market that settles on chain — and a community can hold a treasury that spends
the same way, because a treasury here is a ledger account like any other.

**Forge Hub** is where all of it is visible at once: one dashboard, one portfolio,
one search across the six. It is deliberately not a seventh product — it is the
container the other six sit inside, which is why it sells nothing.

## The six

Each row links to the repository that owns it. **Only Forge Network's is public**
— the other five are private, so they open for members of this organisation and
404 for everybody else.

|  | | |
| --- | --- | --- |
| **Mine** | **[Forge Network](https://github.com/cloudsforge-online/hearth)** | A CPU-mined, ASIC-resistant proof-of-work coin. Homefire PoW, UTXO + Ed25519, 15-second blocks, no premine. EMBER is the unit. Public, and takes outside contributors. |
| **Trade** | **[Forge Trade](https://github.com/cloudsforge-online/micro-trade)** | Strategies backtested against real market history with fees and slippage charged — because a strategy that only works for free does not work. What survives goes to paper, and only then to money. Not an exchange. |
| **Make** | **[Forge Create](https://github.com/cloudsforge-online/micro-mint)** | Token deployment across the EVM majors and Solana. Real OpenZeppelin contracts, testnet by default, mainnet when you mean it. |
| **Sell** | **[Forge Market](https://github.com/cloudsforge-online/micro-market)** | Listings, offers, auctions and escrow. The escrow is a reservation in the ledger rather than a balance we hold, and every royalty split sums exactly to the sale price. |
| **Play** | **[Forge Worlds](https://github.com/cloudsforge-online/micro-worlds)** | The platform games are built on, not a game: one player profile, one inventory, seasons and entitlements shared across every title. Two run on it — *[Ninety Days After](https://github.com/cloudsforge-online/micro-nda)*, one shared map whose resources genuinely run out and whose 90-day season seals into history, and *[Emberkin](https://github.com/cloudsforge-online/micro-emberkin)*, a monster-collecting RPG where how you raise a creature decides what it becomes. |
| **Predict** | **[Forge Foresight](https://github.com/cloudsforge-online/micro-foresight)** | Parimutuel markets on future events, staked in EMBER and **settled by the contract, not by us**. If our database vanished, every stake would still be in the contract and every winner could still claim. |

Beneath those, and deliberately not products: identity and custody, the ledger,
the wallet, settlement, the chain indexer, pricing, billing, policy, notifications,
activity, the developer platform, analytics, asset generation, the operator tooling
— and the governance layer, where a community's proposals, votes and treasury run
across the six rather than inside any one of them. **Every one of them is listed
below with what it owns.** An account is not something you choose; it is something
you are given.

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

> **Every `micro-` link below is internal.** Those repositories are private, so they open for
> members of this organisation and 404 for everybody else — the 404 means "not yours to read", not
> "not there". The public ones are [`hearth`](https://github.com/cloudsforge-online/hearth) and the
> predecessors at the bottom.

### The six products

| Repository | Scope |
| --- | --- |
| [`hearth`](https://github.com/cloudsforge-online/hearth) | **Forge Network.** The chain itself — Homefire PoW, UTXO + Ed25519, node, miner, EVM layer and contracts. **Public**, and takes outside contributors. |
| [`micro-trade`](https://github.com/cloudsforge-online/micro-trade) | **Forge Trade.** The strategy catalogue, backtests, bots, fills and fee settlement. A backtest replays byte-identically from a seed. |
| [`micro-mint`](https://github.com/cloudsforge-online/micro-mint) | **Forge Create.** Token orders, the deployment lifecycle, the token registry and contract templates. A deploy leaves the request; nothing is held for three minutes. |
| [`micro-market`](https://github.com/cloudsforge-online/micro-market) | **Forge Market.** Listings, offers, bids, auctions, orders, escrow references, collections, moderation and disputes. Escrow is a ledger reservation, never a balance held here. |
| [`micro-worlds`](https://github.com/cloudsforge-online/micro-worlds) | **Forge Worlds.** The title registry, shared player profile, inventory, achievements, seasons and the entitlement bridge — the platform the games sit on, not a game. |
| [`micro-foresight`](https://github.com/cloudsforge-online/micro-foresight) | **Forge Foresight.** Markets, the AI idea pipeline with cited provenance, on-chain settlement and disputes. The service holds no stake. |

### The titles inside Forge Worlds

A title is not a product. It registers with [`micro-worlds`](https://github.com/cloudsforge-online/micro-worlds), shares one player profile, and sells
nothing but cosmetics and seasons.

| Repository | Scope |
| --- | --- |
| [`micro-nda`](https://github.com/cloudsforge-online/micro-nda) | *Ninety Days After*: the shared map, tiles, players, actions and the resolution engine. Ported so a day resolves byte-identically to its ancestor. |
| [`micro-emberkin`](https://github.com/cloudsforge-online/micro-emberkin) | *Emberkin*: the monster-collecting RPG. Its ported RNG reproduces the original bit-for-bit, so recorded battles replay exactly. |

### The platform beneath them

| Repository | Scope |
| --- | --- |
| [`micro-identity`](https://github.com/cloudsforge-online/micro-identity) | Accounts, credentials, MFA, sessions, devices, refresh families, signing keys and JWKS. The root of trust. |
| [`micro-ledger`](https://github.com/cloudsforge-online/micro-ledger) | Double-entry accounting: chart of accounts, journal entries, postings, balances, reservations, reconciliation. Its database refuses an unbalanced journal. |
| [`micro-custody`](https://github.com/cloudsforge-online/micro-custody) | HD seeds, key generation, the encryption envelope, signing policy and key lifecycle. It has no reveal endpoint, by deletion rather than by guard. |
| [`micro-wallet`](https://github.com/cloudsforge-online/micro-wallet) | Wallet registry, deposit addresses, withdrawals, conversions, transfers and the portfolio read. Holds no balances; composes ledger, custody and indexer. |
| [`micro-settlement`](https://github.com/cloudsforge-online/micro-settlement) | Treasuries, sweeps, outbound transaction building, broadcast and confirmation tracking. |
| [`micro-indexer`](https://github.com/cloudsforge-online/micro-indexer) | Blocks, transactions, receipts, logs, address activity, balances, reorgs and provider health. Reorg safety is the whole job. |
| [`micro-pricing`](https://github.com/cloudsforge-online/micro-pricing) | Market sources, the median oracle, administered prices, spread policy and rate history. A rate that cannot be quoted is an error, never a default. |
| [`micro-billing`](https://github.com/cloudsforge-online/micro-billing) | Products, prices, entitlements, subscriptions, usage, invoices, refunds and creator payouts. |
| [`micro-policy`](https://github.com/cloudsforge-online/micro-policy) | Rules, limits, velocity counters, trusted addresses, cooling-off, approvals and freezes. Fail-closed and fail-open are separated deliberately. |
| [`micro-activity`](https://github.com/cloudsforge-online/micro-activity) | The canonical activity record and the unified feed. |
| [`micro-notify`](https://github.com/cloudsforge-online/micro-notify) | Preferences, templates, notifications, deliveries, digests and developer webhooks. A critical notification ignores preferences. |
| [`micro-hub-api`](https://github.com/cloudsforge-online/micro-hub-api) | **Forge Hub**'s BFF: dashboard aggregation, portfolio composition, unified search and suggested actions. Hub is the container the six products sit inside, not a seventh product. |
| [`micro-community`](https://github.com/cloudsforge-online/micro-community) | Communities, membership, proposals, votes, delegations, timelocks and treasury executions — governance across products rather than a product of its own. A treasury is a ledger account. |
| [`micro-devplatform`](https://github.com/cloudsforge-online/micro-devplatform) | Developer organisations, projects, API keys, OAuth clients, webhooks, usage and quotas. Its database refuses a fast hash. |
| [`micro-admin-api`](https://github.com/cloudsforge-online/micro-admin-api) | The operator BFF: cross-service actions, approval queues and a tamper-evident audit mirror. |
| [`micro-studio`](https://github.com/cloudsforge-online/micro-studio) | Asset generation as a service: brand kits, asset specs, leased generation jobs and assets whose provenance is complete. Spend is capped by a conditional UPDATE before the model call, not by a prompt. |
| [`micro-analytics`](https://github.com/cloudsforge-online/micro-analytics) | A pseudonymised product event store, funnels, cohorts and retention. A raw subject cannot be stored, even with the service bypassed. |

### The things people look at

| Repository | Scope |
| --- | --- |
| [`micro-site`](https://github.com/cloudsforge-online/micro-site) | The marketing site. No number on it that is not checkable against something real. |
| [`micro-hub-web`](https://github.com/cloudsforge-online/micro-hub-web) | Forge Hub: dashboard, portfolio, wallet, activity, security, entitlements. |
| [`micro-market-web`](https://github.com/cloudsforge-online/micro-market-web) | Forge Market's storefront, listings, orders and disputes. |
| [`micro-foresight-web`](https://github.com/cloudsforge-online/micro-foresight-web) | Forge Foresight's public markets. It recomputes the question hash in the browser. |
| [`micro-foresight-admin-web`](https://github.com/cloudsforge-online/micro-foresight-admin-web) | The Foresight operator console, its own bundle by design. |
| [`micro-emberkin-web`](https://github.com/cloudsforge-online/micro-emberkin-web) | The Emberkin game client. It deletes the battle engine it inherited: a client that can resolve a battle can lie about one. |
| [`micro-status-web`](https://github.com/cloudsforge-online/micro-status-web) | The public status page. Green-on-unknown is structurally unreachable. |
| [`micro-admin-web`](https://github.com/cloudsforge-online/micro-admin-web) | The operator console: approvals, the action catalogue, the audit log and its chain verification, flags and broadcasts. It never calls the audit-write route — a browser holds neither the signing secret nor the scope. |

### Operations

| Repository | Scope |
| --- | --- |
| [`micro-beacon`](https://github.com/cloudsforge-online/micro-beacon) | Synthetic probes, journeys, incidents, SLOs and error budgets. **The release gate** — an unknown refuses. |
| [`micro-lantern`](https://github.com/cloudsforge-online/micro-lantern) | Log triage: OTLP ingest, error grouping, browser errors and trace lookup. Credentials are scrubbed before anything is stored. |
| [`micro-faucet`](https://github.com/cloudsforge-online/micro-faucet) | The testnet EMBER faucet. It refuses to start against a chain that is not the testnet. |
| [`micro-deploy`](https://github.com/cloudsforge-online/micro-deploy) | The telemetry stack, the gateway configuration and the public API route map. |

### Libraries and machinery

| Repository | Scope |
| --- | --- |
| [`micro-runtime`](https://github.com/cloudsforge-online/micro-runtime) | `@cloudsforge/lifecycle`, `-http`, `-jobs`, `-db`, `-auth`, `-telemetry`. The six copies of the same file that used to drift. |
| [`micro-contracts`](https://github.com/cloudsforge-online/micro-contracts) | The typed contracts services agree on, split by bounded context. `-chain` is exact-pinned, because a skew credits money at the wrong depth. |
| [`micro-ui`](https://github.com/cloudsforge-online/micro-ui) | The design system: tokens, chrome, the product accents and the validated chart palette. |
| [`micro-sdk`](https://github.com/cloudsforge-online/micro-sdk) | The public developer SDK and CLI. Zero runtime dependencies; every route cites the line that serves it. |
| [`micro-org`](https://github.com/cloudsforge-online/micro-org) | The reusable CI every repository calls, the contract-compatibility checker, `cfctl` and the README template. |
| [`micro-service-template`](https://github.com/cloudsforge-online/micro-service-template) | A working service skeleton with every runtime library wired. |
| [`micro-web-template`](https://github.com/cloudsforge-online/micro-web-template) | The same for a frontend, including the guards a frontend keeps forgetting. |

### Assets and record

| Repository | Scope |
| --- | --- |
| [`micro-brand`](https://github.com/cloudsforge-online/micro-brand) | 93 generated brand assets with per-asset provenance, and the numeric ground normaliser that makes them one family. |
| [`micro-emberkin-assets`](https://github.com/cloudsforge-online/micro-emberkin-assets) | 83 generated assets for Emberkin, prompted from the game's own visual spec. |
| [`micro-docs`](https://github.com/cloudsforge-online/micro-docs) | Twenty documents: the architecture and security decisions, the domain model, the testing strategy, and a build ledger recording what is actually true. |
| [`micro-conformance`](https://github.com/cloudsforge-online/micro-conformance) | A recorded corpus of real interactions — the behavioural baseline a successor has to match. |

### Still in build

These do not exist yet — the links above are to repositories that do.

`micro-mint-web`, `micro-trade-web`, `micro-worlds-web`, `micro-explorer-web`,
`micro-network-site`, `micro-devportal-web`.

### Predecessors

[`platform`](https://github.com/cloudsforge-online/platform), [`forge-pay`](https://github.com/cloudsforge-online/forge-pay), [`forge-keyvault`](https://github.com/cloudsforge-online/forge-keyvault), [`forge-mint`](https://github.com/cloudsforge-online/forge-mint), [`crucible`](https://github.com/cloudsforge-online/crucible), [`ninety-days-after`](https://github.com/cloudsforge-online/ninety-days-after),
[`shared-libs`](https://github.com/cloudsforge-online/shared-libs), [`asset-forge`](https://github.com/cloudsforge-online/asset-forge), [`stack`](https://github.com/cloudsforge-online/stack). **Nothing was deleted, archived or renamed.** They remain
deployable and are the rollback target; the estate above was built beside them.


## Security

**Hearth is money, and a consensus bug is not a defect report — it is a loss of funds.** The
disclosure policy lives with the chain:
[SECURITY.md](https://github.com/cloudsforge-online/hearth/blob/main/SECURITY.md). It covers the
whole estate, not only the node.

Please report privately and give us time to fix it. Do not open a public issue for anything
touching consensus, custody, the ledger, or authentication.

What we have already decided, so you know what you are looking at:

- **The custody service has no key-reveal endpoint.** Not guarded — deleted. There is no
  authenticated path to a private key.
- **Signing shapes are closed.** A custody key may produce a value transfer or a contract creation
  and nothing else; widening that would make the key a signing oracle.
- **Money invariants live in the database.** The ledger's deferred constraint refuses an unbalanced
  journal against a caller holding a connection, not merely against a caller using the service.
- **The first admin is a manual database update**, deliberately. A service that can mint its own
  first administrator is a service whose compromise grants the estate.

## The stack

TypeScript on Node 22, ESM, strict — `noUncheckedIndexedAccess` and `exactOptionalPropertyTypes`
everywhere. Postgres per service. `node:test` and nothing else. React 19 and Vite for the
frontends, nginx for serving them, Traefik at the edge, and OpenTelemetry into Prometheus, Tempo,
Loki and Grafana.

No message broker. No shared database. No ORM. Money is `bigint`; a float anywhere near an amount
is a defect.

## Where to start reading

- **The chain** — [`hearth`](https://github.com/cloudsforge-online/hearth) is public and is where
  the interesting cryptography is.
- **The reasoning** — `micro-docs` carries the architecture and security decisions with the
  alternatives that were rejected, and a build ledger recording the defects found on the way,
  including the ones deliberately left open.
- **The shape of a service** — `micro-service-template` is a working skeleton; every service is cut
  from it.

## Status

**Pre-launch, and honest about it.** Forty-eight repositories are built and
tested — 383 test files, every service suite running against a real Postgres and
failing the build if it skips them. Hearth runs on a testnet.

Forty-seven are green in CI. `micro-sdk` is red: a check that compares the
OpenAPI description against the gateway's route map reads that map out of
`micro-deploy`, and its workflow does not check that repository out as a sibling.
The check is right and the workflow is wrong; it is being fixed rather than
weakened. **Red runs are never deleted here** — the history of what failed is the
only evidence that a suite ever had teeth.

**Nothing is serving the public yet, and almost nothing is deployed.** Two
services have been run together against a real database to prove the seams; the
rest exist as code that passes its own tests. Where a product page says a
capability is in build, it means exactly that.

Most repositories are private while the estate settles. Hearth is public, takes
outside contributors, and is where the interesting cryptography lives; the
developer SDK becomes public with the API it describes.

Nothing here promises a return. Backtests describe the past. Mining yields depend
on difficulty. A parimutuel payout depends on the pool at settlement. A coin with
no mainnet has no price.
