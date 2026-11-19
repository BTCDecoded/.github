# BLVM and Bitcoin Commons System Status

**Last Verified**: 2026-11-19  
**Status**: Phase 1 (Infrastructure Building) - Not Yet Activated

The phase timeline below was revised on 2026-11-11 and is kept as written then. On 2026-11-19 the component inventory was re-checked from shallow clones of the public BTCDecoded repositories: Rust file counts under each crate's primary `src/` and `tests/` directories, public modules listed in `lib.rs`, CLI binaries and dependency pins in `Cargo.toml`, SQL migrations in blvm-commons, and a spot-check of the blvm-commons enforcement caveats already in this file. This pass did not re-run test suites or measure line coverage.

## Executive Summary

**BLVM** is the Bitcoin implementation (blvm-consensus, blvm-protocol, blvm-node, blvm-sdk, and the runtime modules). **Bitcoin Commons** is the governance framework (the governance model in the `governance` repository, and the blvm-commons app). The system is in **Phase 1 (Infrastructure Building)**. Core repositories exist and contain substantial source and tests, but governance rules are **not yet enforced** and the system uses **test keys only**.

### Current Phase: Phase 1 (Infrastructure Building)

- ⚠️ **Not Yet Activated**: Governance rules are not enforced
- 🔧 **Test Keys Only**: No real cryptographic enforcement
- 📋 **Development Phase**: System is in rapid AI-assisted development

**Timeline:** Activation begins as BLVM moves from alpha to beta, on no fixed timeline. Phase 1 is current. Infrastructure is built, and governance rules are not enforced. Phase 2 begins with community validation and adoption, not on a calendar date. Phase 3 is later operation after that, with no fixed date.

---

## Component Status

### Core Components

The implementation and governance pieces are organized as follows:

1. **blvm-consensus** - Pure mathematical consensus implementation
2. **blvm-protocol** - Protocol abstraction layer
3. **blvm-node** - Full node implementation with module system
4. **blvm-sdk** - Developer SDK and governance primitives
5. **blvm-commons** - GitHub App. Enforcement is off. The app is one host for a Designed quorum, and that quorum is not Running.
6. **governance** - Governance configuration

### Runtime Modules overview

Runtime modules extend blvm-node functionality via process-isolated components:

- **blvm-lightning** - Lightning Network payment processing
- **blvm-mesh** - Payment-gated routing and network state management
- **blvm-governance** - Governance webhook delivery and proposal events
- **blvm-stratum-v2** - Stratum V2 mining protocol support

See the [Runtime Modules](#runtime-modules) section for detailed information.

File counts below are `.rs` files under each repository's primary `src/` and `tests/` directories on the default branch, from a shallow clone on 2026-11-19. Fuzz, benches, examples, tools, and workspace member crates are listed separately where they are substantial. They are not included in the Source / Test totals. Crate versions are those on the default branch at that check.

---

### 1. blvm-consensus (Consensus Proof)

**Status**: Implemented in-tree (crate `0.1.84`). This pass counted files and modules; it did not re-run the test or spec-lock jobs.

**Implementation Details:**

- **Source Files**: 62 Rust files (`src/`)
- **Test Files**: 207 Rust test files (`tests/`)
- **Also in tree**: 63 fuzz `.rs` files, 9 benches, 1 example
- **Modules Exported**: 35 public modules in `src/lib.rs` (including `test_utils` and profiling helpers). These include `script`, `transaction`, `block`, `economic`, `pow`, `mempool`, `mining`, `segwit`, `taproot`, `reorganization`, and others such as `utxo_overlay`, `activation`, and BIP helpers.
- **Spec-lock verification**: Orange Paper contracts on `#[spec_locked]` functions via **blvm-spec-lock** (`cargo spec-lock verify`; optional Z3 for full contract checks)
- **CI**: The consensus `ci.yml` workflow runs tests and `cargo-spec-lock` check-drift / verify on the main pipeline (blvm-spec-lock 0.1.63 is pinned in that workflow)

**Key Features** (symbols present in `src/`; this pass did not re-run the tests that exercise them):

- Transaction validation (`check_transaction`)
- Block validation (`connect_block`)
- Script execution (`eval_script`, `verify_script`)
- Economic model (`get_block_subsidy`, `total_supply`)
- Proof of Work (`check_proof_of_work`, `get_next_work_required`)
- Mempool operations (`accept_to_memory_pool`, `is_standard_tx`, `replacement_checks`)
- Mining (`create_new_block`, `mine_block`)
- Chain reorganization (`reorganization` module)
- SegWit and Taproot modules
- `utxo-commitments` Cargo feature (empty feature flag in this crate; see also `utxo_overlay` and the protocol crate's `utxo_commitments` module)

**Tests in tree:**

- Unit tests inside `src/`
- Integration tests under `tests/`, including historical block and differential-named files (for example `historical_consensus.rs`, `height_script_differential.rs`, `mainnet_blocks.rs`)
- Property-based tests (`proptest` appears in 14 files under `tests/`)
- Fuzzing: 63 targets under `fuzz/fuzz_targets/`

**Note**: Older docs sometimes cited model-checking counts; the **authoritative** consensus assurance story is **tests + blvm-spec-lock** on `#[spec_locked]` surfaces—see **blvm-consensus** README and **blvm-spec-lock**. This refresh does not restate those counts.

---

### 2. blvm-protocol (Protocol Engine)

**Status**: Implemented in-tree (crate `0.1.55`). This pass counted files and modules; it did not re-run tests.

**Implementation Details:**

- **Source Files**: 41 Rust files (`src/`)
- **Test Files**: 33 Rust test files (`tests/`)
- **Also in tree**: 81 fuzz `.rs` files
- **Modules Exported**: 50 public modules in `src/lib.rs`. Many are re-exports from blvm-consensus. Protocol-owned modules include `economic`, `features`, `genesis`, `network_params`, `validation`, `variants`, plus others such as `network`, `utxo_commitments`, `spam_filter`, and BIP helpers.

**Key Features** (present in the tree):

- Protocol variants (`BitcoinV1` / mainnet, `Testnet3`, `Regtest`, and `Signet`)
- Network parameters (magic bytes, ports, genesis blocks)
- Feature activation parameters (SegWit, Taproot, RBF, CTV — CTV is marked not yet activated in `features.rs` for mainnet)
- Economic parameters abstraction
- Protocol-specific validation rules

**Dependencies:**

- blvm-consensus (`>=0.1, <1`, not an exact-version pin)

---

### 3. blvm-node (Reference Node)

**Status**: Implemented in-tree (crate `0.1.102`). This pass counted files and modules; it did not re-run tests.

**Implementation Details:**

- **Source Files**: 343 Rust files (`src/`)
- **Test Files**: 254 Rust test files (`tests/`)
- **Also in tree**: 9 fuzz `.rs` files, 26 examples, plus `build.rs`, two tools, and `crates/blvm-storage-fence`
- **Modules Exported**: 10 public modules in `src/lib.rs`: `bip21`, `cli`, `config`, `module`, `network`, `node`, `rpc`, `storage`, `utils`, `validation`

**Key Features** (present in the tree; this pass did not re-run the tests that exercise them):

- Block validation (uses blvm-consensus via blvm-protocol)
- Storage layer (`src/storage`; feature-gated backends include redb, sled, rocksdb, and heed3)
- P2P networking with a transport abstraction (`tcp_transport`, `quinn_transport`, and optional `iroh_transport`)
- RPC interface (JSON-RPC; optional `rest-api` feature)
- Optional `bip70-http` feature (requires `rest-api`)
- Module system (process-isolated modules under `src/module`)
- Runtime module support for blvm-lightning, blvm-mesh, blvm-governance, and blvm-stratum-v2
- Compact blocks and package relay files under `src/network/` (`compact_blocks.rs`, `package_relay.rs`, BIP331-named sources)
- BIP21 module; BIP157/BIP158 handlers under `src/network/`
- Dandelion++ files and a `dandelion` Cargo feature (feature-gated)
- Node events can be consumed by other modules (the event publisher documents `blvm-zmq` as one consumer; ZMQ is not a top-level module in this crate)

**Dependencies:**

- blvm-protocol (`>=0.1, <1`)
- blvm-consensus (`>=0.1, <1`, optional in `Cargo.toml`, pulled in by features such as `production`)

---

### 4. blvm-sdk (Developer SDK)

**Status**: Implemented in-tree (crate `0.1.27`). This pass counted files and modules; it did not re-run tests or measure coverage.

**Implementation Details:**

- **Source Files**: 54 Rust files (`src/`)
- **Test Files**: 20 Rust test files (`tests/`)
- **Also in tree**: 5 `.rs` files in `crates/blvm-sdk-macros`, 6 examples, 2 fuzz `.rs` files
- **Modules Exported**: 4 public modules in `src/lib.rs`: `cli`, `composition`, `governance`, `module`
- **CLI Tools**: `blvm-keygen`, `blvm-sign`, `blvm-verify`, `blvm-compose`, `blvm-registry`, `blvm-sign-binary`, `blvm-verify-binary`, `blvm-aggregate-signatures`, and `timer-poll`

**Key Features** (present in the tree):

- Cryptographic key management modules (`bip32`, `bip39`, `bip44` under `src/governance/`)
- Signature creation and verification via **blvm-secp256k1** (not a crates.io `secp256k1 = 0.28.2` pin)
- Multisig threshold logic and governance message formats (under `src/governance/`)
- Composition framework (module registry, lifecycle management)
- PSBT module (`src/governance/psbt.rs`)

**Dependencies:**

- blvm-secp256k1 (`>=0.1, <1`)
- Optional `node` feature (on by default) depends on blvm-node (`>=0.1, <1`)
- `blvm-protocol` is a dev-dependency (`>=0.1, <1`)

---

### 5. blvm-commons (GitHub App)

**Status**: The app is implemented (crate `0.1.0`). It posts a `governance/merge` status. A branch-protection update leaves admin enforcement off, and a failed update still returns success. That is not a completed enforcement engine.

**Implementation Details:**

- **Source Files**: 123 Rust files (`src/`)
- **Test Files**: 33 Rust test files (`tests/`)
- **Also in tree**: 3 fuzz `.rs` files
- **Modules Exported**: 20 public modules in `src/lib.rs`: `audit`, `backup`, `build`, `config`, `crypto`, `database`, `enforcement`, `error`, `fork`, `github`, `governance`, `governance_review`, `node_registry`, `nostr`, `reporting`, `resilience`, `services`, `validation`, `webhooks`, `ots`
- **Database Migrations**: 20 SQL files in `migrations/` (001 through 020, including two no-op migrations). A smaller parallel set lives in `migrations-postgres/`, and two additional SQL files are under `src/database/migrations/`.

**Key Features:**

- GitHub webhook integration
- Signature verification (uses blvm-sdk)
- Status check posting
- The app can post a failing `governance/merge` status. That status does not stop a merge.
- A node registry module exists. Vote aggregation sets `veto_blocks` to false, and the merge path is maintainer-only. Zap veto votes are reported only and do not block.
- Governance fork module
- Audit logging (tamper-evident hash chains)
- Nostr status publishes on a timer. The default interval is 3600 seconds. That is not a live feed of every governance action.
- `stamp()` writes a `MOCK_OTS_PROOF` and does not post the hash to a calendar. A non-mock proof verifies as pending.
- Server authorization module (present under `src/`; also listed from `src/main.rs`)

**Database:**

- SQLite and PostgreSQL backends are both declared (`sqlx` features `sqlite` and `postgres`; a `postgres` crate feature exists)
- This pass did not verify production deployment of either backend

**Dependencies:**

- blvm-sdk (`>=0.1, <1`, not an exact-version pin)
- blvm-protocol (`>=0.1, <1`)
- crates.io `secp256k1 = "0.28"`

---

### 6. governance (Configuration)

**Status**: Governance configuration repository. Rules are written here; they are not enforced (Phase 1, test keys only).

**Key Features** (files present in the repository):

- 5-tier governance model configuration (`config/action-tiers.yml`)
- Layer-based signature thresholds (`config/repository-layers.yml`, `LAYER_TIER_MODEL.md`)
- Repository-specific configurations under `config/`
- Maintainer configurations by layer (`config/maintainers`)
- Emergency tier system (`config/emergency-tiers.yml`)
- Governance fork configuration (`config/governance-fork.yml`)
- Cross-layer dependency rules (`config/cross-layer-rules.yml`)

---

### Build and CI orchestration

There is no `commons` orchestration repository, and `commons/versions.toml` is not present. Crate versions are declared in each repository's `Cargo.toml`. A file named `versions.toml` exists in **blvm-commons**, but its `[versions]` map still lists `v0.1.0` / `v0.1.0-locked` pins under `release_set` `set-2025-01A`, so it is not treated here as a current source of truth.

Related repositories that are in the tree:

- **rust-ci**: shared GitHub Actions composite actions for BTCDecoded Rust repositories (toolchain install, runner cache and disk guards, spec-lock setup). It contains no crates.
- **blvm-workspace**: a development checkout with git submodules and a Cargo workspace for cross-crate BLVM work. Individual crates remain separate repositories.

---

## Runtime Modules

Runtime modules are process-isolated components that extend blvm-node functionality. They communicate via IPC (Unix domain sockets) and run in separate processes for security and stability.

### Module System Architecture

Modules are process-isolated components that communicate with the node via IPC (Unix domain sockets). Each module runs in a separate process for security and stability.

**Loading a Module:**

```rust
// Load a module programmatically
let metadata = ModuleMetadata {
    name: "blvm-lightning".to_string(),
    version: "0.1.0".to_string(),
    description: "Lightning Network payment processor".to_string(),
    author: "BTCDecoded".to_string(),
    capabilities: vec!["read_blockchain".to_string(), "subscribe_events".to_string()],
    dependencies: HashMap::new(),
    optional_dependencies: HashMap::new(),
    entry_point: "blvm-lightning".to_string(),
};

node.module_manager_mut()
    .unwrap()
    .load_module(
        "blvm-lightning",
        &binary_path,
        metadata,
        config
    )
    .await?;
```

The module manager lives at `blvm-node/src/module/manager.rs`. The snippet above is illustrative and is not a line-accurate excerpt.

**Module Communication Interface:**

Modules communicate with the node through the `NodeAPI` trait in `blvm-node/src/module/traits.rs`, which provides access to blockchain data, events, and node state (including `get_chain_tip`, `get_block`, and `get_block_height`).

**Module Manifest Example:**

```toml
# module.toml
name = "blvm-lightning"
version = "0.1.0"
description = "Lightning Network payment processor"
author = "BTCDecoded"
entry_point = "blvm-lightning"

capabilities = [
    "read_blockchain",
    "subscribe_events",
]

[dependencies]
# Hard dependencies - module cannot load without these
"blvm-mesh" = ">=0.1.0"

[optional_dependencies]
# Soft dependencies - module can work without these
"blvm-governance" = ">=0.1.0"
```

Modules are discovered automatically from the `modules/` directory or can be installed via the P2P module registry.

### Available Runtime Modules

#### 8. blvm-lightning (Lightning Network Module)

**Status**: In-tree Lightning module (crate `0.1.8`) with IPC integration and multiple payment providers. This is not a claim of mainnet Lightning readiness.

**Purpose**: Lightning Network payment processing

**Implementation Details:**

- **Source Files**: 15 Rust files (`src/`)
- **Test Files**: 2 Rust test files (`tests/`)
- **Modules Exported**: 9 public modules in `src/lib.rs`: `api`, `config`, `error`, `invoice`, `invoice_store`, `module`, `processor`, `provider`, `storage`

**Key Features** (as documented in the crate README and present in `src/`):

- IPC connection and event subscription
- Providers: LNBits (REST), LDK (`ChannelManager` in-process), and a stub provider (the documented default)
- Invoice creation and BOLT11 signing via the LDK provider; HTLC forwarding and claim handling as described in that README
- The README states that mainnet channel funds are out of scope until a restart and a reorg deeper than the LDK tip are in the test record. `pay_invoice` succeeding means LDK queued the send, not that a payment has settled.

---

#### 9. blvm-mesh (Mesh Networking Module)

**Status**: In-tree mesh module (crate `0.1.0`) with routing, payment verification, and related components in `src/`. This pass did not re-run its tests.

**Purpose**: Payment-gated routing and network state management

**Implementation Details:**

- **Source Files**: 23 Rust files (`src/`)
- **Test Files**: 10 Rust test files (`tests/`)
- **Also in tree**: a workspace with member crates under `modules/` (`blvm-onion`, `blvm-bridge`, `blvm-mining-pool`, `blvm-messaging`); those members are not included in the counts above
- **Modules Exported**: 22 public modules in `src/lib.rs`, including `manager`, `routing`, `routing_policy`, `verifier`, `replay`, `network`, `packet`, and `payment_proof`
- **Entrypoint**: `bin/main.rs` (binary name `blvm-mesh`)

**Key Features** (present in the tree / documented in the crate README):

- Routing policy system (`bitcoin_only`, `payment_gated`, and `open` modes)
- Payment verifier and payment-proof modules
- Replay prevention
- Routing table and discovery modules
- Packet structure and network integration
- `MeshManager` coordination

**Routing Modes:**

- `bitcoin_only`: Only route Bitcoin transactions (free)
- `payment_gated`: Require payment proof for routing (default)
- `open`: No payment required (experimental)

---

#### 10. blvm-governance (Governance Module)

**Status**: In-tree governance webhook and proposal module (crate `0.1.6`). A fresh node does not load it automatically.

**Purpose**: Governance webhook delivery to a configured URL, and optional forwarding toward blvm-commons

**Implementation Details:**

- **Source Files**: 10 Rust files (`src/`)
- **Test Files**: 3 Rust test files (`tests/`)
- **Modules Exported**: 8 public modules in `src/lib.rs`: `api`, `config`, `error`, `module`, `proposals`, `proxy`, `storage`, `webhook`

**Key Features** (as documented in the crate README and present in `src/`):

- IPC connection and event subscription
- Posts events to a configured webhook URL
- On one machine it can listen and forward block posts to blvm-commons
- It does not start blvm-commons, and it does not accept GitHub webhooks

---

#### 11. blvm-stratum-v2 (Stratum V2 Module)

**Status**: In-tree mining module (crate `0.1.3`) with a miner server, pool, and template modules. This is not a live pool and is not a complete public Stratum V2 implementation.

**Purpose**: Stratum V2-framed mining protocol support for blvm-node

**Implementation Details:**

- **Source Files**: 12 Rust files (`src/`)
- **Test Files**: 13 Rust test files (`tests/`)
- **Modules Exported**: 10 public modules in `src/lib.rs`: `config`, `error`, `jd`, `messages`, `module`, `module_api`, `pool`, `protocol`, `server`, `template`

**Key Features** (as documented in the crate README and present in `src/`):

- IPC connection and event subscription
- Miner server, channels, jobs, and share submission on a 6-byte Stratum V2 frame
- The README states that the module does not perform the Noise handshake, so a miner speaking public Stratum V2 will not complete a connection. Block bytes inside the process use bincode.
- Pool, job, and share-validation modules
- Merge-mining-related tests exist (`tests/merge_mining_api.rs`)

---

### Module Installation

Modules can be installed in several ways:

**1. Local Installation:**

```bash
# Copy module binary to modules directory
cp target/release/blvm-lightning modules/blvm-lightning/target/release/
# Create module.toml manifest
# Node will auto-discover on startup
```

**2. P2P Module Registry:**

```rust
// Modules can be discovered and installed via P2P network
let registry = node.module_registry().unwrap();
let module = registry.fetch_module("blvm-lightning", "0.1.0").await?;
// Module is automatically verified and installed
```

**3. Runtime Loading:**

```rust
// Load module at runtime via RPC or API
node.module_manager_mut()
    .unwrap()
    .load_module("blvm-lightning", &path, metadata, config)
    .await?;
```

### Module Security Model

All modules are designed to run with process isolation. The module system lives under `blvm-node/src/module/` (including `mod.rs`, `manager.rs`, `traits.rs`, `sandbox.rs`, and `security.rs`).

**Security Features** (modules and comments present in that tree; this pass did not audit their effectiveness):

- Process isolation (separate process / memory space)
- IPC communication
- Capability-based permissions declared in `module.toml`
- Sandboxed / isolated module storage APIs
- Crash isolation as a design goal (module crashes should not take down the node)
- Dependency validation (hard/soft dependencies)

---

## Test Infrastructure

### Test File Counts (Verified 2026-11-19)

Counts are `.rs` files under each repository's primary `src/` and `tests/` directories only.

| Component | Source Files | Test Files | Total Files |
|-----------|--------------|------------|-------------|
| blvm-consensus | 62 | 207 | 269 |
| blvm-protocol | 41 | 33 | 74 |
| blvm-node | 343 | 254 | 597 |
| blvm-sdk | 54 | 20 | 74 |
| blvm-commons | 123 | 33 | 156 |
| blvm-lightning | 15 | 2 | 17 |
| blvm-mesh | 23 | 10 | 33 |
| blvm-governance | 10 | 3 | 13 |
| blvm-stratum-v2 | 12 | 13 | 25 |
| **Total** | **683** | **575** | **1258** |

Not included in the table: consensus fuzz (63) and benches (9); protocol fuzz (81); node fuzz (9), examples (26), and extra crate/tool files; sdk macros crate (5), examples, and fuzz; commons fuzz (3); mesh workspace member crates.

### Formal and empirical verification

- **blvm-spec-lock**: Links Orange Paper section IDs to `#[spec_locked("…")]` implementations. Z3 checks the Rust in `cargo spec-lock verify`. Lean checks the spec model in the `lean-evidence` workflow and does not check the Rust.
- **Tests**: Unit, integration (including historical replay / differential-named checks in blvm-consensus), property tests, and fuzzing — see blvm-consensus CI and README. This refresh did not re-run those jobs or restate any divergence result.

**Note**: Treat **spec-lock + tests** as the primary documented assurance path for consensus. Ignore stale references elsewhere to legacy model-checking storylines unless a given crate explicitly documents them.

### Test Coverage

- **blvm-consensus**: CONTRIBUTING.md still states a >95% target for consensus-critical code. That is a target, not a measured figure from this pass.
- No published line-coverage percentage was re-measured on 2026-11-19. A previous 77.30% figure for blvm-sdk is not restated here.

---

## Build System Status

### Dependency Graph (Verified 2026-11-19)

```
blvm-consensus
    ↓
blvm-protocol (depends on blvm-consensus, pin >=0.1, <1)
    ↓
blvm-node (depends on blvm-protocol, pin >=0.1, <1;
           blvm-consensus is an optional direct dependency)

blvm-sdk (depends on blvm-secp256k1; default `node` feature depends on blvm-node)
    ↓
blvm-commons (depends on blvm-sdk, pin >=0.1, <1;
              also blvm-protocol >=0.1, <1 and crates.io secp256k1 = "0.28")
```

### Version Coordination

- **Current versions**: See `Cargo.toml` in each repository (values recorded in the component sections above are from the 2026-11-19 default-branch clones)
- **blvm-commons/versions.toml**: exists, but its version map is the stale `set-2025-01A` / `v0.1.0` pins and is not used as a current source of truth here
- **blvm-workspace**: optional local workspace for multi-crate development

---

## Governance System Status

### Phase 1: Infrastructure Building (Current)

**Status**: Phase 1. Components and configuration exist in the repositories. Governance is not activated.

**What's Implemented:**

- Governance configuration (the `governance` repository) and the blvm-commons app
- Database schema and migrations (see migration counts above)
- GitHub integration and signature verification modules
- Status checks
- The app can post a failing `governance/merge` status. That status does not stop a merge.
- Audit logging
- Nostr integration (timer-based; default interval 3600 seconds)
- `stamp()` writes a mock proof. It is not a Bitcoin confirmation.

**What's NOT Active:**

- ⚠️ Governance rules are not enforced
- ⚠️ Test keys only (no production keys)
- ⚠️ Not battle-tested in production

### Governance Tiers

These figures are taken from the `governance` repository README and `config/action-tiers.yml` as of the 2026-11-19 clone:

1. **Tier 1**: Routine Maintenance (3-of-5, 7 days)
2. **Tier 2**: Feature Changes (4-of-5, 30 days)
3. **Tier 3**: Consensus-Adjacent (5-of-5, 90 days)
4. **Tier 4**: Emergency Actions (4-of-5, 0 days review period). This is the maintainer pool in `config/action-tiers.yml`, not the keyholder classes.
5. **Tier 5**: two written pairs, neither chosen here: Meta-Governance is 5-of-7 plus 2-of-3 over 90 days; `tier_5_governance` is 5-of-5 over 180 days.

Combined with a repository layer, the longer review wins. An emergency-classed change in blvm-node is 4-of-5 and 60 days. Keyholder activation is `config/emergency-tiers.yml`: the critical class signs 4-of-7 and activates 5-of-7.

### Governance Layers

From the `governance` repository README as of the 2026-11-19 clone:

| Layer | Repository | Signatures | Review Period |
|-------|------------|------------|---------------|
| 1 | blvm-spec | 6-of-7 | 180 days (365 for consensus) |
| 2 | blvm-consensus | 6-of-7 | 180 days (365 for consensus) |
| 3 | blvm-protocol | 4-of-5 | 90 days |
| 4 | blvm-node / blvm | 3-of-5 | 60 days |
| 5 | blvm-sdk | 2-of-3 | 14 days |

---

## Known Gaps and Limitations

### Incomplete or qualified features

1. **UTXO Commitments**
   - A `utxo-commitments` feature exists in blvm-consensus and blvm-protocol
   - The protocol crate has a `utxo_commitments` module, including `network_integration.rs`
   - No completion percentage is claimed here

2. **Runtime Modules**
   - blvm-lightning, blvm-mesh, blvm-governance, and blvm-stratum-v2 all have substantial source in tree (see the sections above)
   - Lightning and Stratum V2 carry documented caveats (Lightning mainnet channel funds out of scope in the crate README; Stratum V2 server has no Noise handshake)
   - blvm-governance is not loaded by a fresh node

3. **Module System** (blvm-node)
   - Core module manager, IPC, registry, and related modules are present under `src/module/`
   - Some TODOs may remain in lifecycle management; they were not inventoried in this pass

### Experimental or optional components

- **Dandelion++**: Privacy relay (feature-gated)
- **Iroh Transport**: Optional QUIC-based P2P (`iroh` Cargo feature)

### Documentation Gaps

- **Verification narrative**: Consolidate on **blvm-spec-lock** + tests; remove stale references to obsolete proof counts
- **Status Documents**: Multiple status documents in related repos can still conflict with this file
- **Test Coverage**: No line-coverage percentage was re-measured for this refresh

---

## Phase 1 vs Phase 2 Distinction

### Phase 1 (Current): Infrastructure Building

**What This Means:**

- The core BLVM implementation repositories and the Bitcoin Commons governance repositories exist in public source
- Governance rules are written but not activated
- Test keys are used (no production keys)
- The system is not battle-tested
- This refresh did not re-run builds or test suites, so it does not claim that every component compiles, that all tests pass, or that the software is production-ready

**What Doesn't Work:**

- Governance rules are not enforced (dry-run / enforcement-off)
- No real cryptographic enforcement of governance
- Not tested in adversarial conditions as a deployed system

### Phase 2 (Future): Activation

**What Will Happen:**

- Production key generation ceremony
- Governance rules activated
- Real cryptographic enforcement
- Battle testing
- Community validation

**Timeline**: Activation begins as BLVM moves from alpha to beta, on no fixed timeline, and only with community validation and adoption.

---

## Verification Methodology

This 2026-11-19 refresh was produced by examining shallow clones of the public BTCDecoded repositories listed in the component sections (plus blvm-spec-lock, blvm-workspace, rust-ci, blvm-docs, and blvm-spec as supporting context):

1. Counted `.rs` files under each crate's primary `src/` and `tests/` directories
2. Read public `mod` declarations in `lib.rs`, binaries and dependency pins in `Cargo.toml`, and SQL files under blvm-commons `migrations/`
3. Spot-checked blvm-commons enforcement caveats (`MOCK_OTS_PROOF`, Nostr default interval 3600, `veto_blocks = false`, maintainer-only merge path)
4. Aligned the governance tier and layer tables with the current `governance` README and `config/action-tiers.yml`
5. Did not re-run `cargo test`, CI, or coverage, and did not invent or keep numbers that could not be re-derived from those clones

---

## Next Steps

1. Keep this file aligned with crate inventories as the repositories move
2. Consolidate remaining conflicting status language in other public docs
3. Continue work on qualified features (UTXO commitments network path, Lightning and Stratum V2 caveats)
4. Prepare for Phase 2 only after security review, a key ceremony, and community validation — not on a fixed date

---

## Summary

**Overall Status**: Phase 1 (Infrastructure Building) — not activated, test keys only

- **Code**: BLVM implementation repositories and the Bitcoin Commons governance repositories are in tree with the file counts above
- **Tests**: Large test trees are present; this pass did not re-run them or publish a coverage percentage
- **Governance**: Configuration and the blvm-commons app exist, but rules are not enforced
- **Documentation**: Other status documents may still conflict and need consolidation
- **Production Readiness**: Not ready (Phase 1, test keys only). This document does not claim mainnet readiness.

**Key Achievement**: A working tree for a Bitcoin implementation (BLVM) together with a written governance framework (Bitcoin Commons). That is infrastructure for a later Phase 2, not a claim that activation is ready.
