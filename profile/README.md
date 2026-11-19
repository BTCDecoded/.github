# BTCDecoded

Don't take our word for it.

Cited Bitcoin reference and intelligence. Site: [btcdecoded.org](https://btcdecoded.org)

This organization also hosts two related, separate products:

- **Bitcoin Commons** is the open consensus specification and coordination for independent implementation. Bitcoin belongs to no one. Site: [thebitcoincommons.org](https://thebitcoincommons.org)
- **BLVM** is one implementation of those written rules, not the rules themselves. Docs: [docs.btcdecoded.org/blvm](https://docs.btcdecoded.org/blvm/)

The governance system hosted here is unreleased and untested in production. Governance rules are not enforced. The keys are test keys. Do not deploy it in production. See [System Status](https://github.com/BTCDecoded/.github/blob/main/SYSTEM_STATUS.md) for what is verified today.

## Bitcoin Commons

Primary action: review a specified rule or test and submit informed technical feedback.

- [`commons-website`](https://github.com/BTCDecoded/commons-website) - Source for [thebitcoincommons.org](https://thebitcoincommons.org)
- [`governance`](https://github.com/BTCDecoded/governance) - Governance configuration and the [Bitcoin Commons Compact](https://github.com/BTCDecoded/governance/blob/main/COMPACT.md)
- [`docs-commons-redirect`](https://github.com/BTCDecoded/docs-commons-redirect) - Redirects `docs.thebitcoincommons.org` to [docs.btcdecoded.org/blvm](https://docs.btcdecoded.org/blvm/)

## BLVM

BLVM is a Bitcoin-compatible node stack. It is an implementation, not the written rule.

- [`blvm-spec`](https://github.com/BTCDecoded/blvm-spec) - Orange Paper (specification text used by the implementation)
- [`blvm-spec-lock`](https://github.com/BTCDecoded/blvm-spec-lock) - Formal verification tooling
- [`blvm-consensus`](https://github.com/BTCDecoded/blvm-consensus) - Consensus implementation
- [`blvm-protocol`](https://github.com/BTCDecoded/blvm-protocol) - Protocol abstraction
- [`blvm-node`](https://github.com/BTCDecoded/blvm-node) - Node library
- [`blvm`](https://github.com/BTCDecoded/blvm) - Main `blvm` binary
- [`blvm-sdk`](https://github.com/BTCDecoded/blvm-sdk) - Developer toolkit
- [`blvm-docs`](https://github.com/BTCDecoded/blvm-docs) - Unified documentation, published at [docs.btcdecoded.org/blvm](https://docs.btcdecoded.org/blvm/)

Other `blvm-*` modules live in this same organization. Check each repository README and [System Status](https://github.com/BTCDecoded/.github/blob/main/SYSTEM_STATUS.md) for what has shipped.

## BTCDecoded

- [`website`](https://github.com/BTCDecoded/website) - Source for [btcdecoded.org](https://btcdecoded.org)

## Getting started

1. Bitcoin Commons: [thebitcoincommons.org](https://thebitcoincommons.org)
2. BLVM docs and installs: [docs.btcdecoded.org/blvm](https://docs.btcdecoded.org/blvm/) and [btcdecoded.org/install](https://btcdecoded.org/install)
3. Pre-built `blvm` binaries: [GitHub Releases (blvm)](https://github.com/BTCDecoded/blvm/releases)
4. Status: [SYSTEM_STATUS.md](https://github.com/BTCDecoded/.github/blob/main/SYSTEM_STATUS.md)

## License

MIT License. See individual repository LICENSE files for details.
