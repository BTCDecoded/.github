# Bitcoin Commons

> ## 🚨 **WARNING: UNRELEASED SOFTWARE**
> 
> **This governance system is currently UNRELEASED and UNTESTED in production.**
> 
> - ⚠️ **Not Yet Activated**: Governance rules are not enforced
> - 🔧 **Test Keys Only**: No real cryptographic enforcement  
> - 📋 **Development Phase**: System is in rapid AI-assisted development
> - ⚡ **Use at Your Own Risk**: This is experimental software
> 
> **Do not deploy in production until Phase 2 activation.** See [System Status](https://github.com/BTCDecoded/.github/blob/main/SYSTEM_STATUS.md) for details.

**Coordination Without Authority: An Architectural Solution**

Bitcoin Commons applies intended cryptographic enforcement to Bitcoin development governance. Built on the **BLVM** stack (specs, consensus, protocol, node) with **BLVM Specification Lock**, tests, and related tooling—verify scope in **[System Status](https://github.com/BTCDecoded/.github/blob/main/SYSTEM_STATUS.md)** and per-repo READMEs.

**🌐 [thebitcoincommons.org](https://thebitcoincommons.org)** | **📚 [Documentation](https://docs.thebitcoincommons.org)** | **📜 [Compact](https://github.com/BTCDecoded/governance/blob/main/COMPACT.md)** | **📊 [System Status](https://github.com/BTCDecoded/.github/blob/main/SYSTEM_STATUS.md)**

## What we're building

**BLVM** - Specifications and implementations for alternative Bitcoin-compatible nodes: Orange Paper, **[blvm-consensus](https://github.com/BTCDecoded/blvm-consensus)**, **[blvm-spec-lock](https://github.com/BTCDecoded/blvm-spec-lock)**, and the rest of the tiered stack (see Architecture below).

**Bitcoin Commons** - Forkable governance framework applying Elinor Ostrom's commons management principles. Normative principles: **[Bitcoin Commons Compact](https://github.com/BTCDecoded/governance/blob/main/COMPACT.md)** (technical rules live in the [governance](https://github.com/BTCDecoded/governance) repo).

Together, they target Bitcoin’s governance asymmetry through **transparent rules**, **forkable process**, and **cryptographic checks** where implemented—see the **[Compact](https://github.com/BTCDecoded/governance/blob/main/COMPACT.md)** and **[System Status](https://github.com/BTCDecoded/.github/blob/main/SYSTEM_STATUS.md)** for what is verified and active today.

## Architecture

![BLVM Stack Architecture](https://thebitcoincommons.org/assets/images/stack.png)

**6-Tier Architecture:**
1. **Orange Paper** (blvm-spec) - Mathematical foundation
2. **Consensus** ([blvm-consensus](https://github.com/BTCDecoded/blvm-consensus)) - Consensus implementation (BLVM Specification Lock, tests, and related assurance—see repo and **[System Status](https://github.com/BTCDecoded/.github/blob/main/SYSTEM_STATUS.md)**)
3. **Protocol** (blvm-protocol) - Core protocol abstraction
4. **Node** (blvm-node) - Full Bitcoin node
5. **SDK** (blvm-sdk) - Tools and primitives
6. **Governance** (governance) - Forkable governance framework

## Current Status

**Phase 1: Infrastructure Complete** ✅

All core components implemented. Governance not yet activated. Production deployment pending Phase 2 activation.

## Repositories

### Core Infrastructure
- [`blvm-spec`](https://github.com/BTCDecoded/blvm-spec) - Orange Paper (mathematical foundation)
- [`blvm-spec-lock`](https://github.com/BTCDecoded/blvm-spec-lock) - Formal verification tooling
- [`blvm-consensus`](https://github.com/BTCDecoded/blvm-consensus) - Consensus layer ([blvm-spec-lock](https://github.com/BTCDecoded/blvm-spec-lock), tests, and related assurance—scope per repo and **System Status**)
- [`blvm-protocol`](https://github.com/BTCDecoded/blvm-protocol) - Protocol abstraction layer
- [`blvm-node`](https://github.com/BTCDecoded/blvm-node) - Full Bitcoin node implementation
- [`blvm-sdk`](https://github.com/BTCDecoded/blvm-sdk) - Developer toolkit
- [`blvm-commons`](https://github.com/BTCDecoded/blvm-commons) - Cryptographic governance enforcement
- [`governance`](https://github.com/BTCDecoded/governance) - Governance configuration and fork registry
- [`blvm`](https://github.com/BTCDecoded/blvm) - Binary wrapper

### Official Modules
- [`blvm-governance`](https://github.com/BTCDecoded/blvm-governance) - Governance module ([docs](https://docs.thebitcoincommons.org/modules/governance.html))
- [`blvm-lightning`](https://github.com/BTCDecoded/blvm-lightning) - Lightning Network integration ([docs](https://docs.thebitcoincommons.org/modules/lightning.html))
- [`blvm-mesh`](https://github.com/BTCDecoded/blvm-mesh) - Mesh networking module ([docs](https://docs.thebitcoincommons.org/modules/mesh.html))
- [`blvm-stratum-v2`](https://github.com/BTCDecoded/blvm-stratum-v2) - Stratum V2 mining protocol ([docs](https://docs.thebitcoincommons.org/modules/stratum-v2.html))

## Getting Started

1. **Learn**: Visit [thebitcoincommons.org](https://thebitcoincommons.org) to understand the governance model and principles
2. **Read**: [Documentation](https://docs.thebitcoincommons.org) for technical details
3. **Install**: Pre-built **`blvm`** binaries and packages — **[GitHub Releases (blvm)](https://github.com/BTCDecoded/blvm/releases)**; install walkthrough: [btcdecoded.org/install](https://btcdecoded.org/install)
4. **Explore**: Browse repository READMEs for component-specific information
5. **Status**: Check [SYSTEM_STATUS.md](https://github.com/BTCDecoded/.github/blob/main/SYSTEM_STATUS.md) for verified implementation status

## Support

- **Issues**: GitHub issues for bugs and feature requests
- **Discussions**: GitHub discussions for questions
- **Security**: Report security issues privately to maintainers

## License

MIT License - see individual repository LICENSE files for details.
