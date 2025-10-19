# Security Policy

## Supported Versions

We provide security updates for the following versions:

| Version | Supported          |
| ------- | ------------------ |
| 0.1.x   | :white_check_mark: |

## Reporting a Vulnerability

We take security seriously. If you discover a security vulnerability, please follow these steps:

### 1. Do NOT create a public issue

Security vulnerabilities should be reported privately to avoid exposing users to potential risks.

### 2. Report privately

Send an email to: **security@btcdecoded.org**

Include the following information:
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if any)
- Your contact information

### 3. Response timeline

- **Acknowledgment**: Within 48 hours
- **Initial assessment**: Within 1 week
- **Resolution**: Depends on severity and complexity

### 4. Disclosure process

- We will work with you to understand and resolve the issue
- We will notify you when the fix is ready
- We will coordinate public disclosure timing
- We will credit you in security advisories (unless you prefer anonymity)

## Security Boundaries

### consensus-proof

**Security Scope**: Mathematical consensus rule implementation
- **In Scope**: Consensus rule correctness, mathematical accuracy
- **Out of Scope**: Network security, key management, wallet security

**Critical Dependencies**: All consensus-critical dependencies are pinned to exact versions

### protocol-engine

**Security Scope**: Protocol abstraction and variant support
- **In Scope**: Protocol parameter validation, variant isolation
- **Out of Scope**: Network security, consensus rule implementation

### reference-node

**Security Scope**: Bitcoin node implementation
- **In Scope**: Node security, RPC security, storage security
- **Out of Scope**: Wallet security, key management

**Important**: This implementation is designed for pre-production testing and development. Additional hardening is required for production mainnet use.

## Security Considerations

### Production Use

- **consensus-proof**: Production-ready for consensus validation
- **protocol-engine**: Production-ready for protocol abstraction
- **reference-node**: Pre-production testing only (see SECURITY.md in repository)

### Development Use

- All components are safe for development and testing
- Regtest mode recommended for development
- Isolated testing environments preferred

## Security Best Practices

### For Developers

1. **Dependency Management**: Use exact version pinning for consensus-critical dependencies
2. **Testing**: Run comprehensive test suites before deployment
3. **Code Review**: All consensus-critical changes require review
4. **Documentation**: Document security assumptions and boundaries

### For Users

1. **Version Pinning**: Pin to exact versions in production
2. **Testing**: Thoroughly test in isolated environments
3. **Monitoring**: Monitor for security updates
4. **Reporting**: Report any security concerns immediately

## Security Updates

Security updates will be released as:
- **Patch releases**: For security fixes
- **Minor releases**: For security improvements
- **Major releases**: For breaking security changes

## Security Audit

We are committed to regular security audits:
- **External audits**: Planned for production releases
- **Internal reviews**: Ongoing code review process
- **Community feedback**: Open to security community input

## Contact

For security-related questions or concerns:
- **Email**: security@btcdecoded.org
- **PGP Key**: [Available on request]
- **Response Time**: Within 48 hours

## Acknowledgments

We thank the security community for their contributions and responsible disclosure practices.

