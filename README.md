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
> **Do not deploy in production until Phase 2 activation.** See [System Status](./SYSTEM_STATUS.md) for details.

Welcome to the Bitcoin Commons project - the next generation of Bitcoin governance infrastructure built on the BLLVM technology stack.

**🌐 [Learn more at thebitcoincommons.org](https://thebitcoincommons.org)** - Understand the governance system, why it's designed this way, and the principles behind it.

**📊 [System Status](./SYSTEM_STATUS.md)** - Verified implementation status and component details.

### Current Status: Phase 1 (Infrastructure Building)

- ✅ **Infrastructure Complete**: All core components implemented
- ⚠️ **Not Yet Activated**: Governance rules are not enforced
- 🔧 **Test Keys Only**: No real cryptographic enforcement
- 📋 **Development Phase**: System is in rapid AI-assisted development

**What This Means:**
- **Production Quality**: The codebase is production-quality in many respects
- **Not Battle-Tested**: Has not been tested in real-world scenarios
- **Expect Changes**: Rapid development means frequent updates
- **Use at Your Own Risk**: This is experimental software

### Timeline

- **Phase 2 Activation**: 3-6 months (governance enforcement begins)
- **Phase 3 Full Operation**: 12+ months (mature, stable system)
- **Current Phase**: Infrastructure building and testing

### For Developers

If you're working on this system:

1. **Test Environment Only**: Never use production keys
2. **Expect Breaking Changes**: APIs and interfaces may change
3. **Follow Development Guidelines**: See individual repository READMEs
4. **Report Issues**: Use GitHub issues for bugs and feature requests

### For Organizations Considering Adoption

- **Wait for Phase 2**: Do not deploy in production yet
- **Monitor Development**: Follow our progress and updates
- **Provide Feedback**: Your input shapes the final system
- **Stay Informed**: Subscribe to updates and announcements

## Architecture

![BLLVM Stack Architecture](../btcdecoded-book/images7/stack.png)
*Figure: BLLVM architecture showing bllvm-spec (Orange Paper) as the foundation, bllvm-consensus as the core implementation with verification paths (Kani proofs, spec drift detection, hash verification), and dependent components (bllvm-protocol, bllvm-node, bllvm-sdk) building on the verified consensus layer.*

## Repositories

### Core Infrastructure
- [`governance-app`](../governance-app/) - GitHub App for governance enforcement
- [`bllvm-sdk`](../developer-sdk/) - Cryptographic primitives and CLI tools (package: bllvm-sdk, directory: developer-sdk/)
- [`governance`](../governance/) - Governance configuration and documentation

### Documentation
- **[SYSTEM_STATUS.md](./SYSTEM_STATUS.md)** - ⭐ **Master status document** with verified implementation status
- **[thebitcoincommons.org](https://thebitcoincommons.org)** - Learn about the governance framework, principles, and why it's designed this way
- [`governance/GOVERNANCE.md`](../governance/GOVERNANCE.md) - Technical governance process documentation
- [`governance-system-review.plan.md`](../governance-system-review.plan.md) - Development roadmap

## Getting Started

1. **Learn About the Governance System**: Visit [thebitcoincommons.org](https://thebitcoincommons.org) to understand why Bitcoin Commons exists, how the governance model works, and the principles behind it
2. **Review the Architecture**: See [SYSTEM_OVERVIEW.md](../SYSTEM_OVERVIEW.md) for technical architecture details
3. **Check Implementation Status**: See [SYSTEM_STATUS.md](./SYSTEM_STATUS.md) for verified component status
4. **Explore the Code**: Browse individual repository READMEs for component-specific documentation
5. **Follow Development**: Monitor progress and updates
6. **Wait for Phase 2**: Do not deploy until officially released

## Learn More

- **🌐 Website**: [thebitcoincommons.org](https://thebitcoincommons.org) - Learn about the governance framework and principles
- **📊 System Status**: [SYSTEM_STATUS.md](./SYSTEM_STATUS.md) - Verified implementation status
- **📖 Documentation**: Individual repository READMEs for component-specific details

## Support

- **Issues**: Use GitHub issues for bug reports and feature requests
- **Discussions**: Use GitHub discussions for questions and feedback
- **Security**: Report security issues privately to maintainers

## License

This project is licensed under the MIT License - see individual repository LICENSE files for details.

---

**Remember**: This is experimental software in active development. Use at your own risk and do not deploy in production until Phase 2 activation.