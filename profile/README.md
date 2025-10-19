# BTCDecoded

**Direct mathematical implementation of Bitcoin consensus rules from the Orange Paper.**

BTCDecoded is a comprehensive Bitcoin implementation ecosystem consisting of five tiers that build upon the Orange Paper's mathematical specifications. The system provides direct mathematical implementation of consensus rules, protocol abstraction, a minimal reference implementation, and a developer-friendly SDK.

## 5-Tier Architecture

```
1. Orange Paper (mathematical foundation)
    ↓ (direct mathematical implementation)
2. consensus-proof (pure math: CheckTransaction, ConnectBlock, etc.)
    ↓ (protocol abstraction)
3. protocol-engine (Bitcoin abstraction: mainnet, testnet, regtest)
    ↓ (full node implementation)
4. reference-node (validation, storage, mining, RPC)
    ↓ (ergonomic API)
5. developer-sdk (governance infrastructure)
    ↓ (cryptographic governance)
6. governance + governance-app (enforcement engine)
```

## Repositories

### Core Implementation

- **[consensus-proof](https://github.com/BTCDecoded/consensus-proof)** - Direct mathematical implementation of Bitcoin consensus rules from the Orange Paper
- **[protocol-engine](https://github.com/BTCDecoded/protocol-engine)** - Bitcoin protocol abstraction layer for multiple variants and evolution
- **[reference-node](https://github.com/BTCDecoded/reference-node)** - Minimal Bitcoin node implementation using protocol-engine and consensus-proof

### Documentation

- **[the-orange-paper](https://github.com/BTCDecoded/the-orange-paper)** - Mathematical Bitcoin specification
- **[developer-sdk](https://github.com/BTCDecoded/developer-sdk)** - Governance infrastructure & composition framework

### Governance Infrastructure

- **[governance](https://github.com/BTCDecoded/governance)** - Central governance configuration and rules
- **[governance-app](https://github.com/BTCDecoded/governance-app)** - GitHub App for cryptographic governance enforcement

## Getting Started

### For Developers

1. **Start with consensus-proof**: Understand the mathematical foundation
2. **Explore protocol-engine**: Learn about Bitcoin protocol variants
3. **Build with reference-node**: Create Bitcoin applications
4. **Use developer-sdk**: Governance infrastructure for Bitcoin governance operations

### For Researchers

- **Orange Paper**: Mathematical specifications and proofs
- **consensus-proof**: Direct implementation of mathematical functions
- **protocol-engine**: Protocol evolution and variant support

### For Bitcoin Users

- **reference-node**: Run your own Bitcoin node
- **developer-sdk**: Build Bitcoin governance applications

### For Governance Participants

- **governance**: Understand governance rules and processes
- **governance-app**: Technical enforcement of governance requirements

## Key Features

- **Mathematical Accuracy**: Direct implementation of Orange Paper specifications
- **Protocol Evolution**: Support for multiple Bitcoin variants and future evolution
- **Production Ready**: Full Bitcoin node implementation
- **Developer Friendly**: Clean APIs and comprehensive documentation
- **Security First**: Exact version pinning and comprehensive testing
- **Cryptographic Governance**: Multi-signature requirements and transparent audit trails

## Design Principles

1. **Pure Functions**: All consensus functions are deterministic and side-effect-free
2. **Mathematical Accuracy**: Direct implementation of Orange Paper specifications
3. **Protocol Abstraction**: Support for multiple Bitcoin variants and evolution
4. **Production Ready**: Full Bitcoin node functionality
5. **Developer Experience**: Clean APIs and comprehensive documentation
6. **Cryptographic Governance**: Multi-signature enforcement and transparent audit trails

## Contributing

We welcome contributions! See our [contribution guidelines](https://github.com/BTCDecoded/.github/blob/main/CONTRIBUTING.md) for details.

## Security

Security is paramount in Bitcoin software. See our [security policy](https://github.com/BTCDecoded/.github/blob/main/SECURITY.md) for how to report vulnerabilities.

## License

All BTCDecoded projects are licensed under the MIT License.

## Links

- **Website**: [btcdecoded.org](https://btcdecoded.org)
- **Documentation**: [docs.btcdecoded.org](https://docs.btcdecoded.org)
- **Discussions**: [GitHub Discussions](https://github.com/BTCDecoded/.github/discussions)
- **Issues**: [GitHub Issues](https://github.com/BTCDecoded/.github/issues)

