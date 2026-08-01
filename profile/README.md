# CloudsForge

**One crypto world. Mine it, hold it, forge it, trade it, sell it, play in it,
predict on it, and build on it.**

Almost every consumer crypto platform is an exchange with features bolted on.
CloudsForge is the inverse: a set of things worth doing, funded by a currency you
can produce yourself on a laptop, with the account, the wallet and the ledger
shared across all of them.

## The loop

**The loop is the product.** A CPU-mineable coin that is the actual funding rail
for real products is the thing no one else can tell.

```
   mine EMBER on your own CPU            Hearth — Homefire PoW, CPU-mined, ASIC-resistant
              │
              ▼
   deposit into your wallet              custody mints the key, the indexer confirms it
              │
              ▼
   hold, convert, or reserve             one ledger, double-entry, one portfolio
              │
              ├──► forge a token or a brand      Forge Create
              ├──► run a strategy                Forge Trade
              ├──► play, earn, own               Forge Worlds
              ├──► sell it, buy someone else's   Forge Market
              ├──► stake on what happens next    Forge Foresight
              └──► build on all of it            Developer Platform
              │
              ▼
   withdraw back out on-chain, or to     one activity history, one set of
   your own external wallet              notifications
```

**That last arrow is not decoration.** *A user can always leave with their assets*
is a stated principle, not a feature: private-key access for a wallet you own is a
product requirement. The safeguards are ours to design; the right is not ours to
withhold.

## The six products, the control centre, and the developer surface

Everything else is **spine** — identity, the ledger, custody, the indexer, policy,
activity, notifications, billing, the gateway, Lantern and Beacon. Spine never
appears in a product grid as a peer, because an account is not something a person
chooses; it is something they are given.

| Verb | Surface | What it is |
| --- | --- | --- |
| **Mine** | **[Forge Network](https://github.com/cloudsforge-online/hearth)** | The EMBER chain: node, mining, explorer, faucet, RPC and SDK. Homefire PoW, CPU-mined, 15-second blocks, no premine. Consensus on the **account model** is merged and the original UTXO chain is being retired. |
| **Make** | **[Forge Create](https://github.com/cloudsforge-online/micro-mint)** | Brand generation, token deployment, project pages, the launch flow. Real OpenZeppelin contracts, testnet by default, mainnet when you mean it. |
| **Trade** | **[Forge Trade](https://github.com/cloudsforge-online/micro-trade)** | Backtesting, the strategy catalogue, paper and live bots, performance reporting. Fees and slippage are charged, because a strategy that only works for free does not work. Not an exchange. |
| **Sell** | **[Forge Market](https://github.com/cloudsforge-online/micro-market)** | Discovery, listings, auctions, offers, escrow, creator and project profiles. The escrow is a reservation in the ledger rather than a balance we hold. |
| **Play** | **[Forge Worlds](https://github.com/cloudsforge-online/micro-worlds)** | The game platform, not a game — one player profile, one inventory, seasons and entitlements across every title. *[Ninety Days After](https://github.com/cloudsforge-online/micro-nda)* is its first title, not its definition; *[Emberkin](https://github.com/cloudsforge-online/micro-emberkin)* is its second. |
| **Predict** | **[Forge Foresight](https://github.com/cloudsforge-online/micro-foresight)** | Markets on future events, staked and settled in EMBER **on the chain itself**. The service orchestrates; the contract is the custodian. |
| **Spend** | **[Wallet](https://github.com/cloudsforge-online/micro-wallet)** | Balance, receive, send, convert, history. Presented **inside Forge Hub**, deliberately not as a destination — nobody wakes up wanting to visit a payments product. |
| **Build** | **[Developer Platform](https://github.com/cloudsforge-online/micro-devplatform)** | Projects, API keys, OAuth clients, webhooks, quotas, and the [SDK and CLI](https://github.com/cloudsforge-online/micro-sdk). |
| — | **[Forge Hub](https://github.com/cloudsforge-online/micro-hub-api)** | The control centre: dashboard, portfolio, wallet, activity, settings, security. Where you land after signing in, and the container the rest sit inside. It sells nothing. |

## Is it actually one platform?

The test is not whether the products share a logo. It is whether these hold. This
is the same scorecard the engineering log keeps, reproduced rather than summarised.

| # | One platform means | Today |
| --- | --- | --- |
| 1 | One account signs into everything, once. | **True** |
| 2 | One identity — the same profile and handle everywhere. | Partly — one user row, no profile beyond a handle |
| 3 | One wallet experience, whichever product you came from. | **True** — one wallet service, one set of screens |
| 4 | One portfolio — a single number that is the truth about what you hold. | **True** — composed by `hub-api` |
| 5 | One activity history across money, assets, play and governance. | **True** — `activity` owns the canonical record |
| 6 | One internal economy — Shards and EMBER spend identically everywhere. | Partly — universal, but little earns them yet |
| 7 | Assets created in one product are usable in the others. | Partly — the entitlement bridge exists; no title consumes it yet |
| 8 | One set of notifications, one preference page. | **True** |
| 9 | One operator view — any question answered from one place. | **True** — `admin-api` and its console |
| 10 | One financial source of truth that reconciles against the chain. | Partly — reconciliation compares the ledger against itself |
| 11 | A third party can build on all of it. | Partly — the platform and SDK exist; nothing is serving yet |

Three of these were true when the programme started.

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

### The control centre, the wallet and the developer surface

Customer-facing, but not products — the vision document is explicit that an account is not
something a person chooses, and that Forge Pay stopped being a destination because nobody wakes
up wanting to visit a payments product.

| Repository | Scope |
| --- | --- |
| [`micro-hub-api`](https://github.com/cloudsforge-online/micro-hub-api) | **Forge Hub**'s BFF: dashboard aggregation, portfolio composition, unified search and suggested actions. The container the six sit inside. |
| [`micro-wallet`](https://github.com/cloudsforge-online/micro-wallet) | Wallet registry, deposit addresses, withdrawals, conversions, transfers and the portfolio read. Holds no balances; composes ledger, custody and indexer. The engine under Hub's wallet tab. |
| [`micro-devplatform`](https://github.com/cloudsforge-online/micro-devplatform) | Developer organisations, projects, API keys, OAuth clients, webhooks, usage and quotas. Its database refuses a fast hash. |

### The spine

Never a product, and never in a product grid as a peer.

| Repository | Scope |
| --- | --- |
| [`micro-identity`](https://github.com/cloudsforge-online/micro-identity) | Accounts, credentials, MFA, sessions, devices, refresh families, signing keys and JWKS. The root of trust. |
| [`micro-ledger`](https://github.com/cloudsforge-online/micro-ledger) | Double-entry accounting: chart of accounts, journal entries, postings, balances, reservations, reconciliation. Its database refuses an unbalanced journal. |
| [`micro-custody`](https://github.com/cloudsforge-online/micro-custody) | HD seeds, key generation, the encryption envelope, signing policy and key lifecycle. It has no reveal endpoint, by deletion rather than by guard. |
| [`micro-settlement`](https://github.com/cloudsforge-online/micro-settlement) | Treasuries, sweeps, outbound transaction building, broadcast and confirmation tracking. |
| [`micro-indexer`](https://github.com/cloudsforge-online/micro-indexer) | Blocks, transactions, receipts, logs, address activity, balances, reorgs and provider health. Reorg safety is the whole job. |
| [`micro-pricing`](https://github.com/cloudsforge-online/micro-pricing) | Market sources, the median oracle, administered prices, spread policy and rate history. A rate that cannot be quoted is an error, never a default. |
| [`micro-billing`](https://github.com/cloudsforge-online/micro-billing) | Products, prices, entitlements, subscriptions, usage, invoices, refunds and creator payouts. |
| [`micro-policy`](https://github.com/cloudsforge-online/micro-policy) | Rules, limits, velocity counters, trusted addresses, cooling-off, approvals and freezes. Fail-closed and fail-open are separated deliberately. |
| [`micro-activity`](https://github.com/cloudsforge-online/micro-activity) | The canonical activity record and the unified feed. |
| [`micro-notify`](https://github.com/cloudsforge-online/micro-notify) | Preferences, templates, notifications, deliveries, digests and developer webhooks. A critical notification ignores preferences. |
| [`micro-community`](https://github.com/cloudsforge-online/micro-community) | Communities, membership, proposals, votes, delegations, timelocks and treasury executions — governance across products rather than a product of its own. A treasury is a ledger account. |
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
| [`micro-mint-web`](https://github.com/cloudsforge-online/micro-mint-web) | Forge Create's console: the catalogue, token orders, payment and the deploy lifecycle. Deploy answers 202 and reaches no chain, so nothing renders "deployed" from a mutation result. |
| [`micro-trade-web`](https://github.com/cloudsforge-online/micro-trade-web) | Forge Trade: strategies, backtests, bots, fills and settlements. Draws the equity curve against buy-and-hold, because a strategy curve with nothing to compare it to is a number with no scale. |
| [`micro-worlds-web`](https://github.com/cloudsforge-online/micro-worlds-web) | Forge Worlds' surface: the title registry, the shared player profile, inventory, seasons and provisions. States the title gap plainly rather than behind a spinner. |
| [`micro-explorer-web`](https://github.com/cloudsforge-online/micro-explorer-web) | The chain explorer: blocks, transactions, addresses and token state, read anonymously from the index. |
| [`micro-network-site`](https://github.com/cloudsforge-online/micro-network-site) | Forge Network's front door: what Hearth is, how to run a node, the state of the network, and the faucet. Every figure is fetched or absent — there is no path from an absence to a digit. |
| [`micro-devportal-web`](https://github.com/cloudsforge-online/micro-devportal-web) | The Developer Platform console: organisations, projects, API keys, OAuth clients, webhooks, usage and quotas. A key is shown once, and the screen says so before the request is made. |

### Operations

| Repository | Scope |
| --- | --- |
| [`micro-beacon`](https://github.com/cloudsforge-online/micro-beacon) | Synthetic probes, journeys, incidents, SLOs and error budgets. **The release gate** — an unknown refuses. |
| [`micro-lantern`](https://github.com/cloudsforge-online/micro-lantern) | Log triage: OTLP ingest, error grouping, browser errors and trace lookup. Credentials are scrubbed before anything is stored. |
| [`micro-faucet`](https://github.com/cloudsforge-online/micro-faucet) | The EMBER faucet. It refuses to start against a chain that is not the testnet — and there is no public testnet yet, so it has nothing to point at. |
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

**Nothing.** Every repository in the plan is built, tested and green. The last was
`micro-network-site`; the estate is repository-complete and deployment-zero, which
is the honest way round to say it — see Status.

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

**Pre-launch, and honest about it.** Fifty-two repositories are built and
tested — 467 test files, every service suite running against a real Postgres and
failing the build if it skips them.

**Hearth has no testnet and no mainnet.** Its own map is explicit: "there is no
live endpoint, no testnet and no mainnet". Consensus on the account model is
merged and blocks are produced, validated and reorged across real nodes — but the
three-node stack binds `127.0.0.1`, nothing routes it, and no genesis outlives a
`docker compose down -v`. This page said "Hearth runs on a testnet", which was
wrong.

**Two mining claims this page used to make are retracted, because Hearth's own
documents retract them.** The proof-of-work is *not* non-outsourceable: the private
key is used after a nonce wins rather than inside the hash loop, so a pool operator
can hand out work under its own public key, collect nonces, and sign the blocks
itself — `docs/mining.md` says so in as many words, and calls it deliberately open
rather than overlooked. And ASIC-resistance rests on a production scratchpad of
around 2 GiB; what ships today is the development size, 8,192 words. Neither is a
defect being hidden. Both were this page describing an intention as a property.

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
