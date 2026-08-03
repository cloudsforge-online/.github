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

**The word "earn" in that diagram is still doing more work than the code is.** One
surface credits a seller today — Forge Market, at
[`market/src/orders.ts:324`](https://github.com/cloudsforge-online/micro-market). The rest
spend. See row 6 of the scorecard, which is the honest version.

## The six products, the control centre, and the developer surface

Everything else is **spine** — identity, the ledger, custody, the indexer, policy,
activity, notifications, billing, the gateway, Lantern and Beacon. Spine never
appears in a product grid as a peer, because an account is not something a person
chooses; it is something they are given.

| Verb | Surface | What it is |
| --- | --- | --- |
| **Mine** | **[Forge Network](https://github.com/cloudsforge-online/hearth)** | The EMBER chain: node, mining, explorer, faucet, RPC and SDK. Homefire PoW, CPU-mined, 15-second blocks, no premine. Consensus on the **account model** is merged and the original UTXO chain is being retired. |
| **Make** | **[Forge Create](https://github.com/cloudsforge-online/micro-mint)** | Brand generation, token deployment, project pages, the launch flow. Real OpenZeppelin contracts, testnet by default, mainnet when you mean it. |
| **Trade** | **[Forge Trade](https://github.com/cloudsforge-online/micro-trade)** | Backtesting, the strategy catalogue, paper and live bots, performance reporting. Free until it makes money: the only charge is a share of a **live** bot's gains against a high-water mark. Not an exchange. |
| **Sell** | **[Forge Market](https://github.com/cloudsforge-online/micro-market)** | Discovery, listings, auctions, offers, escrow, creator and project profiles. The escrow is a reservation in the ledger rather than a balance we hold. |
| **Play** | **[Forge Worlds](https://github.com/cloudsforge-online/micro-worlds)** | The game platform, not a game — one player profile, one inventory, seasons and entitlements across every title. Four titles: *[Ninety Days After](https://github.com/cloudsforge-online/micro-nda)*, *[Emberkin](https://github.com/cloudsforge-online/micro-emberkin)*, *[Aetherholm](https://github.com/cloudsforge-online/micro-aetherholm)* and *[Tessera](https://github.com/cloudsforge-online/micro-tessera)*. |
| **Predict** | **[Forge Foresight](https://github.com/cloudsforge-online/micro-foresight)** | Markets on future events, staked and settled in EMBER **on the chain itself**. The service orchestrates; the contract is the custodian. |
| **Spend** | **[Wallet](https://github.com/cloudsforge-online/micro-wallet)** | Balance, receive, send, convert, history. Presented **inside Forge Hub**, deliberately not as a destination — nobody wakes up wanting to visit a payments product. |
| **Build** | **[Developer Platform](https://github.com/cloudsforge-online/micro-devplatform)** | Projects, API keys, OAuth clients, webhooks, quotas, and the [SDK and CLI](https://github.com/cloudsforge-online/micro-sdk). |
| — | **[Forge Hub](https://github.com/cloudsforge-online/micro-hub-api)** | The control centre: dashboard, portfolio, wallet, activity, settings, security. Where you land after signing in, and the container the rest sit inside. It sells nothing. |

### Tessera, the newest thing here

A persistent, user-made world you enter in a browser tab: claim ground, fire
objects out of a prompt, open a place people go to. Land is free and abundant;
**location** is scarce, because attention is. It is isometric, tile-based and
painterly rather than 3D, and the design says why in as many words: neither of the
image models we use emits a mesh, a UV layout or a rig, and *"the substitution is
not a downgrade"* — a diffusion model outputs finished art, so user content in
Tessera cannot look bad, because the thing making it is a painter. What is given up
— first-person immersion, free-look, standing at human scale in a space someone
built — is named once in the design rather than discovered in week three.

The service binds port 4022, runs 97 tests against a real Postgres, and the
generation path works end to end. **It is designed as the first surface where EMBER
is earned, and that half is not built yet** — the payout function exists
(`tessera/src/ledgerclient.ts:346`) and nothing calls it. Selling is a listing that
goes `sold`; the money does not move. The page will say so until it does.

## Is it actually one platform?

The test is not whether the products share a logo. It is whether these hold. This
is the same scorecard the engineering log keeps, reproduced rather than summarised,
and every row was re-read against source for this revision rather than carried
forward.

| # | One platform means | Today |
| --- | --- | --- |
| 1 | One account signs into everything, once. | **True** |
| 2 | One identity — the same profile and handle everywhere. | Partly — one user row, no profile beyond a handle |
| 3 | One wallet experience, whichever product you came from. | **True** — one wallet service, one set of screens |
| 4 | One portfolio — a single number that is the truth about what you hold. | **True** — composed by `hub-api` |
| 5 | One activity history across money, assets, play and governance. | **True** — `activity` owns the canonical record |
| 6 | One internal economy — the same money spends **and earns** identically everywhere. | Partly, and less far than this page used to imply — see below |
| 7 | Assets created in one product are usable in the others. | Partly — the entitlement bridge exists; no title consumes it yet |
| 8 | One set of notifications, one preference page. | **True** |
| 9 | One operator view — any question answered from one place. | **True** — and it now has a producer for every topic it consumes |
| 10 | One financial source of truth that reconciles against the chain. | Partly — the ledger asks the chain now, but nothing in the estate can answer yet |
| 11 | A third party can build on all of it. | Partly — the platform and SDK exist; nothing is serving yet |

Three of these were true when the programme started.

**Row 6 moved backwards on inspection, not forwards.** This page previously said
"universal, but little earns them yet". Two things are truer than that. First, one
surface genuinely credits a seller — `market/src/orders.ts:324` posts a balanced
entry into a seller's `payout_due`. Tessera, designed to be the first, is not it:
its market client and ledger client are imported by nothing, so a Tessera sale
changes a row's status and moves no money. Second, the money itself is mid-change:
**Shards are being replaced estate-wide by EMBER with a subunit called Sparks**, and
that work is early — `'SHARD'` is still a live asset code in 21 repositories and in
the shared contract (`contracts/packages/chain/src/index.ts:19`), while "Sparks"
exists in exactly two files, both Tessera's. The rule driving it is the owner's:
**no balance may exist that the chain does not back.** Today Shards are explicitly
outside that guarantee, which is the whole reason the change is happening.

**Row 10 moved, and the reason is worth reading.** Reconciliation used to compare
the ledger against itself — vacuous, and worse than vacuous, because a self-
referential run recorded `clean`, and `clean` is the status that *lifts* a freeze.
The check that could not fail outranked the one that could. Three things landed
since: the ledger's migration 11 makes a self-referential run **a database error**,
not a service-level guard (`ledger/src/migrations.ts:755` — *"comparing this ledger
against itself proves nothing about the chain"*); the indexer serves a
confirmed-only custody total that **refuses a partial sum**, erroring rather than
returning a smaller number if a single address is unreadable
(`indexer/src/custody.ts:293`); and the ledger's fifteen-minute job now actually
makes the call (`ledger/src/jobs.ts:251`). It is still **Partly** for one reason: no
deployment mints the ledger a credential for `indexer:read`, so the call 401s, the
run records `unavailable`, and EMBER freezes. The service logs that at `fatal` on
boot and names the remedy. **Failing closed is not the same as working**, and a
frozen asset is the correct behaviour, not the finished one.

**Row 9 stayed True and got its evidence.** The operator console's tamper-evident
audit mirror was a consumer with no producer — it subscribed to `*.audit.recorded`
topics nothing in the estate ever emitted, so the mirror was structurally always
empty. It now consumes 31 existing domain topics
(`contracts/packages/events/src/audit.ts`), and every one of the 31 has a real emit
site in a real service.

## Why the empty rooms are a design problem

A parimutuel market with one bettor is a refund machine. Zero listings begets zero
buyers begets zero listings. A season with an unfunded reward budget cannot pay a
single reward. **Someone has to move first, and the only party with a reason to is
the platform.**

So there is an **engagement treasury**: bounded, disclosed, and made of ordinary
double-entry ledger accounts rather than a service holding money. Two things were
refused outright in designing it. Synthetic bids, ghost bettors and invisible house
positions — *"it is the one form of this that costs nothing and it is fraud"*. And a
consensus carve-out, a share of early block rewards, because the public copy says
**no premine** and it is going to keep saying it. Every unit the platform puts into
a room is labelled as the platform's, on the surface where users see it. Raising a
cap takes two operators and a fresh approval the schema itself checks for; lowering
one takes one, because a cap the capped programme can quietly raise is not a cap.

## What we will not do

Refusals, not gaps on a roadmap.

- **We are not an exchange.** No order book, no market making, no custody of
  anyone else's trading pairs. The other side of that line is a different company
  with a different regulatory posture.
- **A game never sells an advantage.** Cosmetics and seasons are entitlements. No
  creature is a token, and nothing you buy changes a stat.
- **No premine, and no consensus carve-out to fund the platform's own marketing.**
  See above; it was proposed and rejected.
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

**Some invariants belong to the estate and to no repository in it**, so they run
across a full clone of all of it rather than inside any one repo. There are four:
every service must agree on the ledger account types it names; every registered
auth scope must be demanded by a gate somewhere, and every gate must demand a
registered one; every event topic must have both a producer and a consumer that
agree it exists; and no route in the estate may return private key material — a
body scan over roughly 498 routes in 29 servers. **Each of the four carries a
canary: a deliberately broken case that the check must go red on.** A check that
cannot fail is not a check, which is the same lesson row 10 taught the hard way.

Frontends used to have tests that never rendered anything — *"there is no DOM in
this suite on purpose"*. That position has been abandoned. Fourteen of the sixteen
frontends now render: six drive a real headless Chromium, eight use an in-process
DOM, and the catalogue behind it is 318 browser scenarios decomposed from the
twenty-one user journeys. `admin-web` and `hub-web` still have neither and are the
two exceptions, which is why the number is fourteen and not sixteen.

The engineering log is public to the people who work here: twenty-three documents
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
| [`micro-market`](https://github.com/cloudsforge-online/micro-market) | **Forge Market.** Listings, offers, bids, auctions, orders, escrow references, collections, moderation and disputes. Escrow is a ledger reservation, never a balance held here — and its settlement is the one place in the estate that credits a seller today. |
| [`micro-worlds`](https://github.com/cloudsforge-online/micro-worlds) | **Forge Worlds.** The title registry, shared player profile, inventory, achievements, seasons and the entitlement bridge — the platform the games sit on, not a game. |
| [`micro-foresight`](https://github.com/cloudsforge-online/micro-foresight) | **Forge Foresight.** Markets, the AI idea pipeline with cited provenance, on-chain settlement and disputes. The service holds no stake. |

### The titles inside Forge Worlds

A title is not a product. It registers with [`micro-worlds`](https://github.com/cloudsforge-online/micro-worlds), shares one player profile, and sells
nothing but cosmetics and seasons.

| Repository | Scope |
| --- | --- |
| [`micro-nda`](https://github.com/cloudsforge-online/micro-nda) | *Ninety Days After*: the shared map, tiles, players, actions and the resolution engine. Ported so a day resolves byte-identically to its ancestor. |
| [`micro-emberkin`](https://github.com/cloudsforge-online/micro-emberkin) | *Emberkin*: the monster-collecting RPG. Its ported RNG reproduces the original bit-for-bit, so recorded battles replay exactly. |
| [`micro-aetherholm`](https://github.com/cloudsforge-online/micro-aetherholm) | *Aetherholm*: a sky-island strategy MMO — mine Aether, command airship fleets, contest an archipelago that seals when the season ends. The first title designed inside the standards rather than migrated up to them, and the first to implement the provisioning contract Worlds calls. |
| [`micro-tessera`](https://github.com/cloudsforge-online/micro-tessera) | *Tessera*: a persistent, user-made isometric world in a browser tab — wards, parcels, the Kiln that fires an object out of a prompt, venues and bookings, listings and a ward's government. Binds 4022. Serves exactly the two routes Worlds actually calls, because a service that can serve a route Worlds does not call has invented an integration. |

### The control centre, the wallet and the developer surface

Customer-facing, but not products — the vision document is explicit that an account is not
something a person chooses, and that Forge Pay stopped being a destination because nobody wakes
up wanting to visit a payments product.

| Repository | Scope |
| --- | --- |
| [`micro-hub-api`](https://github.com/cloudsforge-online/micro-hub-api) | **Forge Hub**'s BFF: dashboard aggregation, portfolio composition, unified search and suggested actions. One dead upstream costs one tile, not the dashboard. |
| [`micro-wallet`](https://github.com/cloudsforge-online/micro-wallet) | Wallet registry, deposit addresses, withdrawals, conversions, transfers and the portfolio read. Holds no balances; composes ledger, custody and indexer. The engine under Hub's wallet tab. |
| [`micro-devplatform`](https://github.com/cloudsforge-online/micro-devplatform) | Developer organisations, projects, API keys, OAuth clients, webhooks, usage and quotas. Its database refuses a fast hash, and a quota the quota'd party can raise is not a quota. |

### The spine

Never a product, and never in a product grid as a peer.

| Repository | Scope |
| --- | --- |
| [`micro-identity`](https://github.com/cloudsforge-online/micro-identity) | Accounts, credentials, MFA, sessions, devices, refresh families, signing keys and JWKS. The root of trust. |
| [`micro-ledger`](https://github.com/cloudsforge-online/micro-ledger) | Double-entry accounting: chart of accounts, journal entries, postings, balances, reservations, reconciliation. Its database refuses an unbalanced journal — and, since migration 11, refuses to reconcile an on-chain asset against itself. |
| [`micro-custody`](https://github.com/cloudsforge-online/micro-custody) | HD seeds, key generation, the encryption envelope, signing policy and key lifecycle. It has no reveal endpoint, by deletion rather than by guard. |
| [`micro-settlement`](https://github.com/cloudsforge-online/micro-settlement) | Treasuries, sweeps, outbound transaction building, broadcast and confirmation tracking. The lease names the nonce, not the row. |
| [`micro-indexer`](https://github.com/cloudsforge-online/micro-indexer) | Blocks, transactions, receipts, logs, address activity, balances, reorgs and provider health. Reorg safety is the whole job — and its custody total refuses a partial sum rather than returning a smaller number. |
| [`micro-pricing`](https://github.com/cloudsforge-online/micro-pricing) | Market sources, the median oracle, administered prices, spread policy and rate history. A rate that cannot be quoted is an error, never a default. |
| [`micro-billing`](https://github.com/cloudsforge-online/micro-billing) | Products, prices, entitlements, subscriptions, usage, invoices, refunds, creator payouts and the engagement treasury's fee recycle. |
| [`micro-policy`](https://github.com/cloudsforge-online/micro-policy) | Rules, limits, velocity counters, trusted addresses, cooling-off, approvals and freezes. It decides, callers enforce, and the fail-closed/fail-open split cannot drift. |
| [`micro-activity`](https://github.com/cloudsforge-online/micro-activity) | The canonical activity record and the unified feed, written only from the bus. |
| [`micro-notify`](https://github.com/cloudsforge-online/micro-notify) | Preferences, templates, notifications, deliveries, digests and developer webhooks. You cannot opt out of being told your key left. |
| [`micro-community`](https://github.com/cloudsforge-online/micro-community) | Communities, membership, proposals, votes, delegations, timelocks and treasury executions — governance across products rather than a product of its own. A treasury is a ledger account. |
| [`micro-admin-api`](https://github.com/cloudsforge-online/micro-admin-api) | The operator BFF: cross-service actions, approval queues, the engagement treasury's caps, and a tamper-evident audit mirror fed by 31 real domain topics. |
| [`micro-studio`](https://github.com/cloudsforge-online/micro-studio) | Asset generation as a service on FLUX 2 Pro: brand kits, asset specs, leased generation jobs and assets whose provenance is complete. Spend is capped by a conditional UPDATE before the model call, not by a prompt. |
| [`micro-analytics`](https://github.com/cloudsforge-online/micro-analytics) | A pseudonymised product event store, funnels, cohorts and retention. A raw subject cannot be stored, even with the service bypassed. |

### The things people look at

Sixteen frontends. Fourteen now render in a test; the two that do not are named in the table.

| Repository | Scope |
| --- | --- |
| [`micro-site`](https://github.com/cloudsforge-online/micro-site) | The marketing site. No number on it that is not checkable against something real. |
| [`micro-hub-web`](https://github.com/cloudsforge-online/micro-hub-web) | Forge Hub: dashboard, portfolio, wallet, activity, security, entitlements. **Still has no DOM in its suite** — one of the two frontends browser coverage has not reached. |
| [`micro-market-web`](https://github.com/cloudsforge-online/micro-market-web) | Forge Market's storefront, listings, orders and disputes. |
| [`micro-foresight-web`](https://github.com/cloudsforge-online/micro-foresight-web) | Forge Foresight's public markets. It recomputes the question hash in the browser. |
| [`micro-foresight-admin-web`](https://github.com/cloudsforge-online/micro-foresight-admin-web) | The Foresight operator console, its own bundle by design. |
| [`micro-emberkin-web`](https://github.com/cloudsforge-online/micro-emberkin-web) | The Emberkin game client. It deletes the battle engine it inherited: a client that can resolve a battle can lie about one. |
| [`micro-aetherholm-web`](https://github.com/cloudsforge-online/micro-aetherholm-web) | The Aetherholm game client: islands, fleets, the archipelago and the season clock. Drives a real headless Chromium in its suite. |
| [`micro-tessera-web`](https://github.com/cloudsforge-online/micro-tessera-web) | The Tessera client: the isometric renderer, the Kiln, wards and parcels. It has a render budget its tests enforce, and it states plainly which routes the service does not serve yet rather than rendering an empty screen. |
| [`micro-status-web`](https://github.com/cloudsforge-online/micro-status-web) | The public status page. Green-on-unknown is structurally unreachable. |
| [`micro-admin-web`](https://github.com/cloudsforge-online/micro-admin-web) | The operator console: approvals, the action catalogue, the engagement treasury's caps, the audit log and its chain verification, flags and broadcasts. It never calls the audit-write route — a browser holds neither the signing secret nor the scope. **The other frontend with no DOM in its suite.** |
| [`micro-mint-web`](https://github.com/cloudsforge-online/micro-mint-web) | Forge Create's console: the catalogue, token orders, payment and the deploy lifecycle. Deploy answers 202 and reaches no chain, so nothing renders "deployed" from a mutation result. |
| [`micro-trade-web`](https://github.com/cloudsforge-online/micro-trade-web) | Forge Trade: strategies, backtests, bots, fills and settlements. Draws the equity curve against buy-and-hold, because a strategy curve with nothing to compare it to is a number with no scale. |
| [`micro-worlds-web`](https://github.com/cloudsforge-online/micro-worlds-web) | Forge Worlds' surface: the title registry, the shared player profile, inventory, seasons and provisions. States the title gap plainly rather than behind a spinner. |
| [`micro-explorer-web`](https://github.com/cloudsforge-online/micro-explorer-web) | The chain explorer: blocks, transactions, addresses and token state, read anonymously from the index. It states the head every depth was measured against, and never says a thing is final. |
| [`micro-network-site`](https://github.com/cloudsforge-online/micro-network-site) | Forge Network's front door: what Hearth is, how to run a node, the state of the network, and the faucet. Every figure is fetched or absent — there is no path from an absence to a digit, no price and no yield. |
| [`micro-devportal-web`](https://github.com/cloudsforge-online/micro-devportal-web) | The Developer Platform console: organisations, projects, API keys, OAuth clients, webhooks, usage and quotas. A key is shown once, and the screen says so before the request is made. |

### Operations

| Repository | Scope |
| --- | --- |
| [`micro-beacon`](https://github.com/cloudsforge-online/micro-beacon) | Synthetic probes, journeys, incidents, SLOs and error budgets. **The release gate** — an unknown refuses, and it declares no journey it cannot actually run. |
| [`micro-lantern`](https://github.com/cloudsforge-online/micro-lantern) | Log triage: OTLP ingest, error grouping, browser errors and trace lookup. Credentials are scrubbed before anything is stored. |
| [`micro-faucet`](https://github.com/cloudsforge-online/micro-faucet) | The EMBER faucet. It refuses to start against a chain that is not the testnet — and there is no public testnet yet, so it has nothing to point at. |
| [`micro-deploy`](https://github.com/cloudsforge-online/micro-deploy) | The composed estate, the telemetry stack, the gateway configuration, the service-token grants and the public API route map. |
| [`micro-org`](https://github.com/cloudsforge-online/micro-org) | The reusable CI every repository calls, the four estate-wide invariants and their canaries, the contract-compatibility checker, `cfctl` and the README template. |

### Libraries and machinery

| Repository | Scope |
| --- | --- |
| [`micro-runtime`](https://github.com/cloudsforge-online/micro-runtime) | `@cloudsforge/lifecycle`, `-http`, `-jobs`, `-db`, `-auth`, `-telemetry`. The six copies of the same file that used to drift. |
| [`micro-contracts`](https://github.com/cloudsforge-online/micro-contracts) | The typed contracts services agree on, split by bounded context, at 1.0.0. `-chain` is exact-pinned, because a skew credits money at the wrong depth. |
| [`micro-ui`](https://github.com/cloudsforge-online/micro-ui) | The design system: tokens, chrome, the product accents that can actually be told apart, and the validated chart palette. |
| [`micro-sdk`](https://github.com/cloudsforge-online/micro-sdk) | The public developer SDK and CLI. Zero runtime dependencies; every route cites the line that serves it. |
| [`micro-service-template`](https://github.com/cloudsforge-online/micro-service-template) | A working service skeleton with every runtime library wired. |
| [`micro-web-template`](https://github.com/cloudsforge-online/micro-web-template) | The same for a frontend, including the guards a frontend keeps forgetting, and an honest 404. |

### Assets and record

Every asset in every one of these is AI-generated, and says so — in the repository, on each manifest
entry, and in the licence string the asset itself carries.

| Repository | Scope |
| --- | --- |
| [`micro-brand`](https://github.com/cloudsforge-online/micro-brand) | 93 generated brand assets across 14 surfaces with per-asset provenance, and the numeric ground normaliser that makes them one family. |
| [`micro-emberkin-assets`](https://github.com/cloudsforge-online/micro-emberkin-assets) | 83 generated images and 51 derivatives for Emberkin, prompted from the game's own visual spec. |
| [`micro-aetherholm-assets`](https://github.com/cloudsforge-online/micro-aetherholm-assets) | 96 generated images and 5 derivatives for Aetherholm, on FLUX 2 Pro. |
| [`micro-tessera-assets`](https://github.com/cloudsforge-online/micro-tessera-assets) | The Tessera art set: 392 assets specified — 288 generated, 104 derived — produced by **both** FLUX 2 Pro and Qwen-Image 2512 and judged against criteria fixed in `COMPARISON.md` **before either set existed**. The generation run is still going. |
| [`micro-docs`](https://github.com/cloudsforge-online/micro-docs) | Twenty-three documents: the architecture and security decisions, the domain model, the testing strategy, the browser-journey catalogue, the engagement treasury, Aetherholm, Tessera, and a build ledger recording what is actually true. |
| [`micro-conformance`](https://github.com/cloudsforge-online/micro-conformance) | A recorded corpus of real interactions — the behavioural baseline a successor has to match. |
| [`.github`](https://github.com/cloudsforge-online/.github) | This page. |

### Predecessors

[`platform`](https://github.com/cloudsforge-online/platform), [`forge-pay`](https://github.com/cloudsforge-online/forge-pay), [`forge-keyvault`](https://github.com/cloudsforge-online/forge-keyvault), [`forge-mint`](https://github.com/cloudsforge-online/forge-mint), [`crucible`](https://github.com/cloudsforge-online/crucible), [`ninety-days-after`](https://github.com/cloudsforge-online/ninety-days-after),
[`shared-libs`](https://github.com/cloudsforge-online/shared-libs), [`asset-forge`](https://github.com/cloudsforge-online/asset-forge), [`stack`](https://github.com/cloudsforge-online/stack). **Nothing was deleted, archived or renamed.** They remain
deployable and are the rollback target; the estate above was built beside them.

**That is all seventy repositories in this organisation** — nine predecessors, `hearth`, this one,
and fifty-nine `micro-` repositories. Every one is listed above exactly once.

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
- **No route in the estate returns private key material**, and that is a check across all of it
  rather than a rule each repository is trusted to keep — a body scan over roughly 498 routes in
  29 servers, with a canary proving the scan can still go red.
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

**Pre-launch, and honest about it.** Fifty-nine `micro-` repositories plus the chain, carrying
617 test files, every service suite running against a real Postgres and failing the build if it
skips them.

**We cannot currently tell you anything about CI, and will not pretend otherwise.** GitHub Actions
is billing-blocked across this organisation. Runs fail in about four seconds having executed zero
steps, and produce no logs — which is not a red suite, it is no suite. **Everything landed today is
verified locally and unverified remotely**, and any statement on this page about a check passing
means it passed on a developer's machine. When billing is restored the history will show the
four-second failures, because **red runs are never deleted here** — the record of what failed is the
only evidence a suite ever had teeth.

**Hearth has no testnet and no mainnet.** Its own map is explicit: "there is no live endpoint, no
testnet and no mainnet". Consensus on the account model is merged and blocks are produced,
validated and reorged across real nodes — but the three-node stack binds `127.0.0.1`, nothing
routes it, and no genesis outlives a `docker compose down -v`. This page once said "Hearth runs on
a testnet", which was wrong.

**Two mining claims this page used to make are retracted, because Hearth's own documents retract
them.** The proof-of-work is *not* non-outsourceable: the private key is used after a nonce wins
rather than inside the hash loop, so a pool operator can hand out work under its own public key,
collect nonces, and sign the blocks itself — `docs/mining.md` says so in as many words, and calls
it deliberately open rather than overlooked. And ASIC-resistance rests on a production scratchpad
of around 2 GiB; what ships today is the development size, 8,192 words. Neither is a defect being
hidden. Both were this page describing an intention as a property.

**The chain-backed solvency loop is wired and frozen, on purpose.** The ledger asks the indexer for
a confirmed-only custody total every fifteen minutes; no deployment yet holds the credential to
answer, so EMBER reconciliation records `unavailable`, fails, and freezes withdrawals for that
asset. The service shouts about it at boot rather than letting it be discovered by noticing an
absence. That is the correct behaviour and it is not the finished one.

**Nothing is serving the public yet, and almost nothing is deployed.** Services run composed
against real databases, and the estate's event bus delivers across them — the first cross-service
event took six defects that no single repository's suite could see. The rest exist as code that
passes its own tests. Where a product page says a capability is in build, it means exactly that.

Most repositories are private while the estate settles. Hearth is public, takes outside
contributors, and is where the interesting cryptography lives; the developer SDK becomes public
with the API it describes.

Nothing here promises a return. Backtests describe the past. Mining yields depend on difficulty. A
parimutuel payout depends on the pool at settlement. A coin with no mainnet has no price.
