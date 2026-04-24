# Bitcoin Commons System Status

**Status**: Phase 1 (Infrastructure Building) - Not Yet Activated

## Executive Summary

Bitcoin Commons is a comprehensive Bitcoin implementation ecosystem with cryptographic governance. The system is in **Phase 1 (Infrastructure Building)** - all core components are implemented, but governance rules are **not yet enforced** and the system uses **test keys only**.

### Current Phase: Phase 1 (Infrastructure Building)

- ✅ **Infrastructure Complete**: All core components implemented
- ⚠️ **Not Yet Activated**: Governance rules are not enforced
- 🔧 **Test Keys Only**: No real cryptographic enforcement
- 📋 **Development Phase**: System is in rapid AI-assisted development

**Timeline:**
- **Phase 2 Activation**: Governance enforcement begins
- **Phase 3 Full Operation**: Mature, stable system

### Crate versions (source: each repo `Cargo.toml`, 2026-04)

| Crate | Version |
|-------|---------|
| blvm-consensus | 0.1.10 |
| blvm-protocol | 0.1.4 |
| blvm-primitives | 0.1.7 |
| blvm-spec-lock | 0.1.5 |
| blvm-node | 0.1.0 |
| blvm-sdk | 0.1.0 |
| blvm-commons | 0.1.0 |

`blvm/versions.toml` is still pinned at **0.1.0** for several names—treat the table above as **what is actually released on crates.io** until the manifest is bumped in a coordinated release sweep.

### Recent engineering (git history, ~Jan–Apr 2026)

- **blvm-consensus:** Shipped **0.1.9 → 0.1.10**. Fuzz targets expanded (incl. sequence locks, sigop validation, economic validation edge cases); **release workflow gated on fuzz-build**. **Removed vendored Orange Paper tree**—canonical spec is repo **`blvm-spec`**. CI uses **BTCDecoded/rust-ci** compositing and spec-lock install paths.
- **blvm-protocol:** **BIP155 (addrv2)**, **BIP324** v2 encrypted transport, **framed TCP P2P** refactor. **`economic-node` P2P messages and wire paths removed** (breaking cleanup). **Protocol fuzz crate** + sharded fuzz CI. UTXO commitments **integration tests** live here; dependency alignment (e.g. smallvec) for hyper stack.
- **blvm-node:** **`economic-node` governance handlers and IPC hooks removed** (matches protocol). **RocksDB 0.24**, P2P parity fixes, IPC length codec. **Node fuzz crate** + sharded CI. UTXO commitments **integration tests** (Redb in tests). P2P **getdata**, selective withhold, module IPC policy. RPC/ZMQ and **QUIC JSON-RPC** docs; book links → **docs.thebitcoincommons.org**.
- **blvm-stratum-v2:** Separate crate; integrates with node via NodeAPI (Stratum still **feature-gated** on `blvm-node`).
- **blvm-sdk:** **Composition/macros** path, **WASM** modules, registry/tar tooling; **SDK fuzz** + CI. Crates.io deps on BLVM crates use **`>=0.1, <1`** with sibling patches for dev.
- **blvm-commons:** **`economic-node` and `economic-veto` enforcement surfaces removed**; rust-ci alignment; manifest-only lockfile policy matches other repos.
- **blvm-spec:** Orange Paper **TOC / display-math / spec-lock ID** hardening; **governance narrative removed from Orange Paper** (spec focuses on consensus/protocol); PROTOCOL sections expanded.
- **governance:** Ruleset/docs aligned with **maintainer-only** governance narrative; Bitcoin Commons **Compact** doc; canonical links to tier repos and BLVM book.

---

## Component Status

### 1. blvm-consensus (Consensus Proof)

**Status**: ✅ **Implemented** - Core consensus functions complete

**Implementation Details:**
- **Modules**: Core consensus modules (constants, script, transaction, block, economic, pow, mempool, mining, segwit, taproot, utxo_commitments, etc.)
- **Formal Verification**: Active with blvm-spec-lock verification linking code to Orange Paper specifications

**Key Features:**
- ✅ Transaction validation (CheckTransaction)
- ✅ Block validation (ConnectBlock)
- ✅ Script execution (EvalScript, VerifyScript)
- ✅ Economic model (GetBlockSubsidy, TotalSupply)
- ✅ Proof of Work (CheckProofOfWork, GetNextWorkRequired)
- ✅ Mempool operations (AcceptToMemoryPool, IsStandardTx, ReplacementChecks)
- ✅ Mining (CreateNewBlock, MineBlock)
- ✅ Chain reorganization
- ✅ SegWit and Taproot support
- ✅ UTXO commitments (feature-gated)
- ✅ blvm-spec-lock formal verification - All consensus-critical functions linked to Orange Paper specifications
- ✅ BIP validation (BIP30, BIP34, BIP66, BIP90, BIP147, BIP119, BIP65, BIP113)
- ✅ Peer consensus protocol (section 13.4)
- ✅ Spam filtering
- ✅ Optimization passes (constant folding, memory layout, SIMD vectorization)

**Tests / CI:** Unit + integration (historical replay, differential harnesses in **blvm-bench**). Proptest. **Fuzz:** many `cargo-fuzz` targets; **release gated on fuzz-build** in CI.

**Repo layout:** Spec markdown is **not** vendored here; clone **`blvm-spec`** for Orange Paper / PROTOCOL.

---

### 2. blvm-protocol (Protocol Engine)

**Status**: ✅ **Implemented** - Protocol abstraction layer complete

**Implementation Details:**
- **Modules**: Protocol abstraction modules (economic, features, genesis, network_params, validation, variants, plus re-exports from consensus)

**Key Features:**
- ✅ Protocol variants (BitcoinV1/mainnet, Testnet3, Regtest)
- ✅ Network parameters (magic bytes, ports, genesis blocks)
- ✅ Feature activation (SegWit, Taproot, RBF, CTV)
- ✅ Economic parameters abstraction
- ✅ Protocol-specific validation rules
- ✅ FIBRE protocol support
- ✅ Commons-specific extensions (UTXO commitments, ban list sharing)
- ✅ **BIP155 (addrv2)**, **BIP324** (v2 encrypted transport), framed TCP P2P stack
- ✅ NODE_GOVERNANCE / governance-related P2P capability flags ( **`economic-node` wire removed** in 2026 )
- ✅ BIP support (BIP152, BIP157, BIP158, BIP173/350/351)
- ✅ **Protocol fuzz** crate + sharded fuzz CI

**Dependencies:**
- blvm-consensus (crates.io semver; workspace uses `patch.crates-io` for siblings)

---

### 3. blvm-node (Reference Node)

**Status**: ✅ **Implemented** - Full node implementation complete

**Implementation Details:**
- **Modules**: Full node modules (storage, network, rpc, node, config, module, bip21, bech32m, bip157, bip158, bip70)

**Key Features:**
- ✅ Block validation (uses blvm-consensus)
- ✅ Storage layer (multiple backends: redb, sled, rocksdb)
- ✅ P2P networking (TCP, Iroh/QUIC transport abstraction)
- ✅ RPC interface (JSON-RPC 2.0, Bitcoin Core compatible)
- ✅ Mining coordination
- ✅ Module system (process-isolated, sandboxed module loading)
- ✅ BIP support (BIP21, BIP70, BIP157, BIP158)
- ✅ Compact blocks
- ✅ **Stratum V2:** separate **`blvm-stratum-v2`** crate; **`stratum-v2`** feature on node
- ✅ Dandelion++ privacy relay (feature-gated)
- ✅ RBF support (4 configurable modes)
- ✅ Mempool policies (5 eviction strategies)
- ✅ CTV (CheckTemplateVerify) payment processing
- ✅ Advanced indexing (address and value range)
- ✅ Governance-related P2P surfaces ( **`economic-node` IPC/handlers removed** in 2026 )
- ✅ QUIC JSON-RPC support (documented alongside TCP RPC)

**Network Features:**
- ✅ Transport abstraction (TCP, Quinn QUIC, Iroh)
- ✅ Protocol adapter and message bridge
- ✅ UTXO commitments network integration
- ✅ Compact block relay
- ✅ Erlay transaction relay

**Dependencies:**
- blvm-protocol, blvm-consensus (crates.io semver ranges in `Cargo.toml`; workspace patches for monorepo dev)

---

### 4. blvm-sdk (Developer SDK)

**Status**: ✅ **Implemented** - Governance infrastructure complete

**Implementation Details:**
- **Modules**: SDK modules (cli, governance, composition)
- **CLI Tools**: Governance and composition tools (blvm-keygen, blvm-sign, blvm-verify, blvm-compose, blvm-sign-binary, blvm-verify-binary, blvm-aggregate-signatures)

**Key Features:**
- ✅ Cryptographic key management (BIP32, BIP39, BIP44)
- ✅ Signature creation and verification (secp256k1)
- ✅ Multisig threshold logic
- ✅ Governance message formats
- ✅ Composition framework (module registry, lifecycle management)
- ✅ PSBT support
- ✅ Binary signing and verification (blvm-sign-binary, blvm-verify-binary)
- ✅ Signature aggregation (blvm-aggregate-signatures)
- ✅ Nested multisig support

**Tests / CI:** Governance crypto tests; **fuzz crate**; line coverage → **Codecov** via tarpaulin workflow. Composition **macros** crate; **WASM** module path for composition/registry tooling.

**Dependencies:**
- Standalone (no consensus dependencies)
- secp256k1 = 0.28.2 (exact version)

---

### 5. blvm-commons (Governance Enforcement)

**Status**: ✅ **Implemented** - Governance enforcement engine complete

**Implementation Details:**
- **Modules**: Governance enforcement modules (config, crypto, database, enforcement, github, validation, webhooks, nostr, ots, audit, authorization, fork)
- **Database**: SQLite (development/testnet), PostgreSQL (production) with schema migrations

**Key Features:**
- ✅ GitHub webhook integration
- ✅ Signature verification (uses blvm-sdk)
- ✅ Status check posting
- ✅ Merge blocking logic
- ✅ Governance fork capability
- ✅ Audit logging (tamper-evident hash chains)
- ✅ Nostr integration (real-time transparency)
- ✅ OpenTimestamps anchoring (Bitcoin blockchain proof)
- ✅ Server authorization system
- ✅ Layer + Tier governance enforcement
- ✅ Review period tracking
- ✅ Multisig threshold validation
- ✅ Tier classification automation
- ✅ PR status management
- **`economic-node` / `economic-veto` surfaces removed** (2026); enforcement is tier/layer + GitHub integration only

**Database:**
- SQLite (development/testnet)
- PostgreSQL (production)
- Schema migrations covering: initial schema, emergency mode, audit log, governance fork, key metadata, tier overrides, signature reasoning

**Dependencies:**
- blvm-sdk (crates.io semver in `Cargo.toml`; workspace patches when developed alongside SDK)

---

### 6. governance (Configuration)

**Status**: ✅ **Implemented** - Governance configuration complete

**Source of truth:** YAML under `governance/config/` in repo **`governance`**—especially `action-tiers.yml`, `repository-layers.yml`, `emergency-tiers.yml`. Numbers below are a **summary**; if they drift, fix the YAML first, then this doc.

**Key Features:**
- ✅ 5-tier governance model configuration
- ✅ Layer-based signature thresholds
- ✅ Repository-specific configurations
- ✅ Maintainer configurations by layer
- ✅ Emergency tier system
- ✅ Governance fork configuration
- ✅ Cross-layer dependency rules

---

### 7. blvm (Build orchestration / release set)

**Status**: ✅ **Implemented** - Meta-repo for coordinated releases

**Key Features:**
- ✅ Version coordination manifest (`versions.toml`)
- ✅ Unified build scripts
- ✅ Reusable GitHub Actions workflows
- ✅ Release automation
- ✅ Deterministic builds
- ✅ Artifact management

---

### 8. Published docs, web, and benchmarks

| Deliverable | Repo | Notes |
|-------------|------|--------|
| BLVM book (mdBook) | **blvm-docs** | Architecture, consensus, protocol, development (fuzzing, differential testing, etc.); published **docs.thebitcoincommons.org**. |
| Public site | **commons-website** | **thebitcoincommons.org** — positioning, FAQ, embedded spec viz, links to docs/status. |
| Benchmarks + differential guides | **blvm-bench** | Perf vs reference node, differential testing docs; CI publishes JSON / **benchmarks.thebitcoincommons.org** (per bench repo README). |

---

## Test Infrastructure

### Testing Approach

All components include comprehensive testing:
- **Unit tests**: Component-level validation
- **Integration tests**: Cross-component validation, historical block replay, differential testing
- **Property-based tests**: Randomized testing with proptest
- **Fuzzing**: Multiple fuzzing targets for security-critical paths
- **Formal verification**: blvm-spec-lock for consensus-critical functions

### Formal verification & coverage

Scope, limits, and methodology: `blvm-consensus/docs/VERIFICATION.md`. Spec-lock CLI/macros: `blvm-spec-lock`. Proof counts: use that repo’s docs, not ad-hoc numbers here.

---

## Build System Status

### Dependency Graph (Verified)

**Bitcoin validation stack** (what ships in a node; versions from each repo `Cargo.toml` / workspace patches):

```
blvm-primitives ──┐
                  ├──► blvm-consensus ──► blvm-protocol ──► blvm-node
blvm-muhash ──────┘
```

- **blvm-consensus** pulls **blvm-primitives** + **blvm-muhash** (and optional **blvm-spec-lock** for verification workflows).
- **blvm-protocol** pulls **blvm-consensus** + **blvm-primitives**.
- **blvm-node** pulls **blvm-protocol** + **blvm-consensus** + **blvm-muhash** (and optional **blvm-spec-lock**); **blvm-primitives** appears via sibling patches in the monorepo layout.

**Governance enforcement service** (Phase 2 blocks merges; **not** on the block-validation hot path):

```
blvm-sdk (no BLVM crates; secp256k1 / bitcoin message standards + serde stack)
    ↓
blvm-commons (GitHub webhooks, signatures, DB; policy thresholds align with **governance** repo YAML)
```

**Policy config:** repo **`governance`** — `governance/config/*.yml` (tiers, layers, emergency). **`blvm-commons`** implements enforcement against that policy when deployed.

**Satellite:** `blvm-spec` (Orange Paper), `blvm-spec-lock`, `blvm-bench` (differential / replay vs reference RPC). **`blvm-primitives`** shared types (published **0.1.7**).

**Shared CI:** **`rust-ci`** — composite GitHub Actions (`install-rust-toolchain`, `strip-patch-crates-io`, `setup-blvm-spec`, verify composites, etc.) used across BTCDecoded repos.

**Extended product repos (outside the minimal runtime graph):** `blvm-lightning`, `blvm-mesh`, `blvm-miningos`, `blvm-selective-sync`, `blvm-datum` — active; depend on or extend the stack (e.g. mesh routing, selective sync). Track releases separately.

### Version coordination

- Manifest: `blvm/versions.toml` (often **behind** crate patch releases—see crate version table at top).

---

## Governance System Status

### Phase 1: Infrastructure Building (Current)

**Status**: ✅ Infrastructure complete, not activated

**What's Implemented:**
- ✅ All governance components (blvm-commons, governance config)
- ✅ Database schema and migrations
- ✅ GitHub integration
- ✅ Signature verification
- ✅ Status checks
- ✅ Merge blocking logic
- ✅ Audit logging
- ✅ Nostr integration
- ✅ OpenTimestamps anchoring

**What's NOT Active:**
- ⚠️ Governance rules are not enforced
- ⚠️ Test keys only (no production keys)
- ⚠️ Not battle-tested in production

### Governance Tiers

(Routine numbered tiers — `action-tiers.yml` keys `tier_1_routine` … `tier_5_governance`.)

1. **Tier 1**: Routine Maintenance (3-of-5, 7 days) — docs/perf/non-consensus fixes; no emergency override.
2. **Tier 2**: Feature Changes (4-of-5, 30 days) — **requires_specification** (e.g. new RPC/P2P/config).
3. **Tier 3**: Consensus-Adjacent (5-of-5, 90 days) — **requires_specification** + **requires_audit**.
4. **Tier 4**: Emergency Actions (4-of-5, 0 days) — **emergency_override**; **requires_post_mortem** (routine fast path, distinct from `emergency-tiers.yml` activation flow).
5. **Tier 5**: Governance Changes (**5-of-5**, **180 days**) — **requires_public_comment** + **requires_rationale**; see `tier_5_governance`.

**Additional action classes** (same file, path-based / elevated scrutiny): **security_critical** (7-of-7, 180d), **cryptographic** (6-of-7, 90d), **security_enhancement** (5-of-7, 30d) — not the same as the five numbered routine tiers.

**Emergency response** (separate): `emergency-tiers.yml` — e.g. critical path **4-of-7** signatures, **0-day** initial review window when emergency tier activated (distinct from routine Tier 4 “Emergency Actions” above).

### Governance Layers

**Repos per layer** (`repository-layers.yml`): **L1** `blvm-spec`, **L2** `blvm-consensus`, **L3** `blvm-protocol`, **L4** `blvm-node` + `blvm`, **L5** `blvm-sdk`.

1. **Layer 1–2** (Constitutional): 6-of-7 maintainers, 180 days (L1/L2 also define longer **consensus_review_period_days** in YAML for spec/consensus-class changes)
2. **Layer 3** (Implementation): 4-of-5 maintainers, 90 days
3. **Layer 4** (Application): 3-of-5 maintainers, 60 days
4. **Layer 5** (Extension): 2-of-3 maintainers, 14 days

---

## Known Gaps and Limitations

### Incomplete Features

1. **UTXO commitments** (consensus + protocol + node)
   - Consensus / protocol logic: ✅ in tree; **integration tests** in **blvm-protocol** and **blvm-node** (2026).
   - **Remaining:** live-network edge cases (e.g. async response / routing polish)—treat %-complete estimates as stale; use issues and tests as ground truth.

2. **Stratum V2** (`blvm-stratum-v2`, integrated with `blvm-node`)
   - Implementation: ✅ Complete
   - Status: Feature-gated on node; server crate is separate repo

3. **Module System** (blvm-node)
   - Core: ✅ Complete
   - Some TODOs remain in lifecycle management

### Experimental Components

- **Dandelion++**: Privacy relay (feature-gated, experimental)
- **Iroh Transport**: QUIC-based P2P (production-ready but optional)

### Documentation gaps

- Formal verification: historical proof-count chatter; use `VERIFICATION.md` as source of truth.
- Some components lack a pinned public coverage badge (SDK uses Codecov).

---

## Phase 1 vs Phase 2 Distinction

### Phase 1 (Current): Infrastructure Building

**What This Means:**
- All code is implemented and functional
- Components are production-quality in structure
- Governance system is complete but not activated
- Test keys are used (no production keys)
- System is not battle-tested

**What Works:**
- All components compile and run
- Tests pass
- Governance logic is implemented
- GitHub integration works

**What Doesn't Work:**
- Governance rules are not enforced (dry-run mode)
- No real cryptographic enforcement
- Not tested in adversarial conditions

### Phase 2 (Future): Activation

**What Will Happen:**
- Production key generation ceremony
- Governance rules activated
- Real cryptographic enforcement
- Battle testing
- Community validation

**Timeline**: Phase 2 activation pending

---

## Next steps

1. Refresh **`blvm/versions.toml`** to match published crate set (or document intentional lag).
2. Finish **UTXO commitments** network/async path (see Known Gaps).
3. Reconcile any remaining **proof-count** mentions with `VERIFICATION.md` only.
4. **Phase 2:** security review, production keys, turn on enforcement in `blvm-commons` deployments.

---

## Summary

**Phase 1:** Runtime stack, spec, fuzz/CI gates, governance **code**, and **governance** YAML are in place; **enforcement not activated** (test keys).

**Follow:** crate table + **Recent engineering** above; **`governance/config/*.yml`** for thresholds; **`VERIFICATION.md`** for proof scope; **docs.thebitcoincommons.org** + **thebitcoincommons.org** for public narrative.

