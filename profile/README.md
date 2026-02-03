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

Bitcoin Commons applies cryptographic enforcement to Bitcoin development governance, making it as robust as Bitcoin's consensus layer. Built on the BLVM technology stack with formal verification and forkable governance.

**🌐 [thebitcoincommons.org](https://thebitcoincommons.org)** | **📚 [Documentation](https://docs.thebitcoincommons.org)** | **📊 [System Status](https://github.com/BTCDecoded/.github/blob/main/SYSTEM_STATUS.md)**

## What We're Building

**BLVM** - Mathematical rigor through the Orange Paper specification and formal verification using blvm-spec-lock. Enables safe alternative Bitcoin implementations.

**Bitcoin Commons** - Forkable governance framework applying Elinor Ostrom's commons management principles. Enables coordination without civil war.

Together, they solve Bitcoin's governance asymmetry: making development governance 6x harder to capture than Bitcoin Core's current model, with complete transparency and user-protective mechanisms.

## Architecture

![BLVM Stack Architecture](https://thebitcoincommons.org/assets/images/stack.png)

**6-Tier Architecture:**
1. **Orange Paper** (blvm-spec) - Mathematical foundation
2. **Consensus** (blvm-consensus) - Formally verified implementation with blvm-spec-lock
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
- [`blvm-consensus`](https://github.com/BTCDecoded/blvm-consensus) - Formally verified consensus implementation
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
3. **Explore**: Browse repository READMEs for component-specific information
4. **Status**: Check [SYSTEM_STATUS.md](https://github.com/BTCDecoded/.github/blob/main/SYSTEM_STATUS.md) for verified implementation status

## Support

- **Issues**: GitHub issues for bugs and feature requests
- **Discussions**: GitHub discussions for questions
- **Security**: Report security issues privately to maintainers

## License

MIT License - see individual repository LICENSE files for details.
