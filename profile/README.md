# CloudsForge

A crypto platform built around a coin you can mine on your own computer: a chain, six products, an
operator console and a developer platform, sharing one account, one wallet and one double-entry
ledger.

Everything below is measured, not estimated. Where something does not work, it says so.

---

## The estate, counted

| | |
| --- | --- |
| Repositories, public | **61** |
| Repositories, archived read-only and private | 9 pre-migration predecessors, each naming its successors |
| Services that bind a port | **30**, plus a service template that never deploys |
| Frontends | **16** |
| TypeScript / TSX files | **1,927** |
| Lines of TypeScript | **556,610** |
| Test files (`node:test`, no other runner) | **642** |
| Distinct database tables | **221** |
| HTTP route declarations | **604** |
| Engineering documents | **27** |
| Browser scenarios specified | **314** |

Counted on 2026-08-03 from the working tree, excluding `node_modules` and build output.

---

## The chain

**Hearth** is written here rather than forked. It is a proof-of-work chain that speaks Ethereum.

| | |
| --- | --- |
| Consensus | Homefire proof-of-work — CPU-mined, memory-hard, ASIC-resistant |
| Account model | EVM, written from scratch: interpreter, opcodes, gas, memory, stack, `uint256`, `bn128`, `blake2f`, precompiles |
| Chain IDs | **7411** mainnet, **7412** testnet |
| Block time | 15 seconds |
| Decimals | 18 |
| Block reward | 6 EMBER at genesis, decaying to a perpetual tail of 0.3 EMBER |
| Premine | none |
| Ethereum VMTests | **609 / 609** |
| Ethereum GeneralStateTests | **20,077 / 20,077** |

Because it is EVM-compatible, ordinary Ethereum tooling works against it, and a seed phrase derived
at `m/44'/60'` restores in wallets nobody here wrote.

**A testnet is running now.** Chain ID 7412, a CPU miner producing blocks, currently past block
**1,065**. It is not a public network and EMBER has no price.

---

## What is running

**41 containers**, composed against real Postgres — one database per service, no shared schema, no
service reading another's tables. The estate boots from one command and includes the chain.

**Continuous integration: 58 green, 1 red.** Every repository runs the same reusable workflow:
typecheck, its own suite against a real Postgres, the estate rules, a container that must boot and
answer `/livez`, and a secret-hygiene sweep. **A suite that skips its database tests fails the
build.** (The remainder is the profile repository, which has no workflow.)

That number was **zero** this morning — not zero green, zero *runs*. Every job in the organisation
had been failing in under fifteen seconds with no steps executed, on a billing block. Making the
estate public restored unlimited Actions, and within the hour CI had caught a cross-repository
contract drift, a documentation gap, a container image job that had never once run for any service,
and a flaky test that was hiding a real defect in an erasure path.

---

## How it is built

**One repository per deployable.** A service owns its schema, its migrations, its tests and its
container. Nothing shares a database.

**Events go Postgres outbox → signed HTTP → inbox**, deduplicated on `(topic, event_id)`. There is
no message broker. Currently **55 registered topics from 15 producers**, with 126 emit sites
derived from source.

**Authorisation is scoped per caller.** **61 scopes**, 56 live and 5 deprecated, with 59 demanded by
gates across 27 repositories. A scope that no gate demands, and a gate demanding a scope that is not
registered, both fail the build.

**Invariants live in the schema**, not in handlers: `CHECK` constraints, deferred constraint
triggers, generated columns, partial unique indexes and GiST exclusion constraints. The ledger's
"debits equal credits, per entry, per asset" is a deferred trigger, so it holds against a `psql`
session and a future service, not only against code that went through the API.

**Four invariants belong to the estate rather than to any repository in it**, and run across a full
clone: every service must agree on the ledger account types it names; every registered scope must be
demanded somewhere; every event topic must have a producer and a consumer that agree it exists; and
no route may return private key material — a body scan across roughly 498 routes. **Each carries a
canary: a deliberately broken case the check must go red on.** A check that cannot fail is not a
check.

---

## What is true today, and what is not

Honest status of the claim that this is one platform rather than six products with a shared logo.

| | | |
| --- | --- | --- |
| One account signs into everything, once | **works** | |
| One wallet, whichever product you came from | **works** | one wallet service, one set of screens |
| One portfolio — a single number for what you hold | **works** | composed by `hub-api` |
| One activity history across money, assets and play | **works** | `activity` owns the canonical record |
| One set of notifications and preferences | **works** | |
| One operator view | **works** | every topic it consumes now has a producer |
| One ledger that reconciles against the chain | **works, with a caveat** | driven clean against the live testnet at drift **exactly 0**, and proven to refuse and re-freeze when custody was emptied. The four tests that prove it end-to-end need two checkouts, so CI does not run them |
| One identity — same profile everywhere | partial | one user row; no profile beyond a handle |
| Assets made in one product usable in others | partial | the entitlement bridge exists; consumption is starting |
| The same money earns everywhere, not just spends | partial | **two** surfaces credit a seller today, not one |
| A third party can build on all of it | partial | the platform and SDK exist; nothing is publicly serving |

**Nothing is serving the public.** Services run composed against real databases and the event bus
delivers across them. Where a product page says a capability is in build, it means exactly that.

**Money in flight is being unified.** Shards, an internal unit, are being removed in favour of EMBER
denominated in **Sparks** — one Spark is 10⁻⁶ EMBER, a display denomination and deliberately never a
second asset code, because the ledger balances per asset code and a second one would let the two
halves of the same money drift apart. The rule underneath: **no balance may exist that the chain
does not back.**

---

## The full ecosystem

Everything that exists, by what it owns. Descriptions are the registry's own, not a summary of them.

### Money and identity — the spine

Spine never appears in a product grid, because an account is not something a person chooses; it is
something they are given.

| Service | Owns |
| --- | --- |
| `micro-identity` | Accounts, credentials, MFA, sessions, devices, SSO exchange, JWKS, orgs, consents |
| `micro-ledger` | Chart of accounts, journal, postings, balances projection, reservations, reconciliation |
| `micro-wallet` | Wallet registry, external links, deposit addresses, withdrawals, conversions, portfolio |
| `micro-custody` | HD seeds, key generation, encryption envelope, signing policy, treasury pins, export |
| `micro-settlement` | Treasuries, sweeps, outbound transactions, broadcast, confirmation tracking |
| `micro-pricing` | Market sources, median oracle, administered prices, rate history, valuation |
| `micro-billing` | Products, prices, entitlements, subscriptions, usage, invoices, payouts, revenue share |
| `micro-policy` | Rules, limits, velocity counters, trusted addresses, cooling-off, approvals, freezes |
| `micro-indexer` | Blocks, transactions, receipts, logs, balances, transfers, reorgs, provider health |
| `micro-activity` | Canonical activity records, event inbox, feed cursors, feed query API |
| `micro-notify` | Preferences, templates, notifications, deliveries, digests, webhooks, broadcasts |
| `micro-analytics` | Pseudonymised product event store, funnels, cohorts, retention, metric definitions |

`micro-analytics` never stores a raw subject. A pseudonym is salted per person and the salt is the
only thing erasure destroys — a plain `HMAC(user_id, pepper)` would be a pure function of two values
that both survive, so it could not be erased at all.

### The products

| Sold as | Service | Owns |
| --- | --- | --- |
| Forge Network | `hearth` | The chain itself: node, mining, RPC, contracts, SDK |
| Forge Create | `micro-mint` | Token orders, deployment lifecycle, token registry, token pages, contract templates. Real OpenZeppelin contracts, testnet by default |
| Forge Trade | `micro-trade` | Strategy catalogue, backtests, bots, fills, allocations, fee settlement, performance. **Free until it makes money** — the only charge is a share of a live bot's gains above a high-water mark. Not an exchange |
| Forge Market | `micro-market` | Listings, offers, auctions, orders, escrow refs, collections, moderation, disputes. The escrow is a **ledger reservation**, not a balance anyone holds |
| Forge Foresight | `micro-foresight` | Prediction markets: registry and lifecycle, idea pipeline, contract deployment, positions, resolution, fees. Settled **on the chain** — `ForesightMarket.sol` takes stakes from `msg.sender` with no allowlist and pays claims to `msg.sender`, so a position is readable and claimable with the platform switched off |
| Forge Worlds | `micro-worlds` | Title registry, player profile, inventory, achievements, seasons, entitlement bridge. A platform, not a game |
| Forge Hub | `micro-hub-api` | The control centre you land on after signing in. It sells nothing |
| — | `micro-community` | Communities, roles, treasury accounts, proposals, votes, delegations, timelocks |
| — | `micro-studio` | Brand kits, asset specs, generation jobs, generated assets, generation credits |

### The four game titles

| Service | Owns |
| --- | --- |
| `micro-nda` | *Ninety Days After*: worlds, tiles, players, resolution engine, communes, objectives |
| `micro-emberkin` | *Kindred*: authoritative saves, campaign, party, catches, Resonance, battle engine |
| `micro-aetherholm` | World state, cities, economy, fleets, battles, seasons, the chronicle, the title contract |
| `micro-tessera` | Wards, parcels, claims, objects, placements, the Kiln, presence, the title contract, authorship anchoring |

### Aggregators and the developer surface

| Service | Owns |
| --- | --- |
| `micro-hub-api` | Forge Hub BFF: dashboard aggregation, portfolio composition, search, saved views |
| `micro-admin-api` | Operator BFF: cross-service actions, approvals, audit mirror, flags, broadcasts |
| `micro-devplatform` | Developer orgs, projects, API keys, OAuth clients, webhooks, quotas, directory |

### Operations

| Service | Owns |
| --- | --- |
| `micro-lantern` | Log triage: OTLP push ingest, fingerprinting, browser errors and RUM |
| `micro-beacon` | Synthetic monitoring, journeys, incidents, SLOs. **The release gate** |
| `micro-faucet` | Testnet EMBER faucet |
| `micro-conformance` | The characterisation corpus, and the estate-wide sweeps run against every repository |

### The sixteen frontends

| Frontend | Serves |
| --- | --- |
| `micro-hub-web` | Forge Hub: dashboard, portfolio, wallet, activity, settings, security, entitlements |
| `micro-site` | Marketing site |
| `micro-admin-web` | Operator console |
| `micro-mint-web` | Forge Create |
| `micro-trade-web` | Forge Trade |
| `micro-market-web` | Forge Market |
| `micro-worlds-web` | Forge Worlds client |
| `micro-foresight-web` | Browse, market detail with cited sources, stake, portfolio, claim |
| `micro-foresight-admin-web` | Operator panel: idea queue, open/close/resolve/void, disputes |
| `micro-emberkin-web` | The Kindred Three.js client, on estate conventions, with the generated art |
| `micro-aetherholm-web` | Archipelago map, city view, fleet control, battle reports, chronicle browser |
| `micro-tessera-web` | Isometric renderer, build and place tools, the Kiln, the ward map, Workshop pages |
| `micro-explorer-web` | Block explorer |
| `micro-network-site` | Forge Network marketing |
| `micro-devportal-web` | Developer console and docs |
| `micro-status-web` | Public status page, from Beacon's redacted projection |

### Shared code and machinery

| Repository | Owns |
| --- | --- |
| `micro-contracts` | Typed contracts per bounded context — auth, money, chain, market, worlds, create, events, devplatform |
| `micro-runtime` | Telemetry, HTTP, jobs, auth, database, lifecycle and policy-client packages |
| `micro-ui` | Design system, tokens, chrome, the surface registry and the validated chart layer |
| `micro-sdk` | The public SDK and CLI, generated from the public OpenAPI description |
| `micro-org` | Shared CI, the repository registry, and the four estate-wide invariants |
| `micro-deploy` | Compose, Kubernetes manifests, gateway configuration, telemetry stack, release manifests |
| `micro-docs` | The engineering log |
| `micro-brand`, `micro-emberkin-assets`, `micro-aetherholm-assets`, `micro-tessera-assets` | Generated art and its provenance |
| `micro-service-template`, `micro-web-template` | What `cfctl new` produces |

---

## Tessera

The newest title: a persistent, user-made world in a browser tab. Claim ground, generate objects
from a prompt, open a place people visit. Land is free and abundant; **location** is scarce, because
attention is. Isometric and painterly rather than 3D — neither image model in use emits a mesh, a UV
layout or a rig, and the design says so plainly instead of discovering it later.

**151 tests** against a real Postgres. Venue bookings reserve EMBER through the ledger, with the
booking's release enforced by a schema constraint so money cannot be stranded, and overlapping
bookings refused by a GiST exclusion constraint rather than by a handler.

---

## The art

**728 images ship**, every one generated with **FLUX 2 Pro** and recorded in a manifest with the
prompt that produced it, the model, the delivered dimensions and the cost.

| | |
| --- | --- |
| `micro-brand` | 98 |
| `micro-emberkin-assets` | 137 |
| `micro-aetherholm-assets` | 101 |
| `micro-tessera-assets` | 392 — of which 104 are derived rather than generated |

A second full set of **741 images** was generated with **Qwen-Image 2512** to compare against it.
Those live under `candidates/` in each repository and never ship, so the reference stays
byte-identical and the comparison stays honest. Criteria were fixed in writing before either set
existed.

Generated originals carry C2PA provenance metadata written by the model, so their origin is
checkable rather than merely claimed.

---

## Licence

Code is **MIT**. Artwork is **CC BY 4.0** — reuse it, remix it, ship it, with attribution.

**Trademarks are reserved.** The CloudsForge, Forge, Hearth and EMBER names, logos and wordmarks are
excluded from both grants. The art is permissive on purpose; a brand mark works by telling people
who made something, and a mark anybody may apply to anything has stopped doing its only job.

---

## Provenance

The code was written by **Claude Opus 5** and **Claude Fable 5**, assets generated with **FLUX 2
Pro**, under human direction and review. Every repository says so in its own README.

Nothing here is claimed to be hand-written that is not, and nothing is claimed to work that has not
been tested.

---

## Where to start

- **[micro-docs](https://github.com/cloudsforge-online/micro-docs)** — 27 documents: architecture
  and security decisions, the domain model, the testing strategy, and a build ledger that records
  what is actually true rather than what was planned, including defects found on the way and the
  ones deliberately left open.
- **[hearth](https://github.com/cloudsforge-online/hearth)** — the chain. Public, takes outside
  contributors, and where the cryptography is.
- **[micro-org](https://github.com/cloudsforge-online/micro-org)** — the shared CI and the four
  estate-wide invariants, each with its canary.

---

Nothing here promises a return. Backtests describe the past. Mining yields depend on difficulty. A
parimutuel payout depends on the pool at settlement. A coin with no mainnet has no price.
