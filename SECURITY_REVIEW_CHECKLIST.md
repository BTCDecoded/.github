# Security Review Checklist

Use this checklist when reviewing pull requests that affect security-sensitive code.

## Pre-Review

- [ ] Identify affected security controls (check `security-control-mapping.yml`)
- [ ] Determine security tier (P0/P1/P2/P3) based on affected controls
- [ ] Check if PR requires security audit, formal verification, or cryptography expert
- [ ] Verify governance tier requirements (signatures, review period)

## Code Review

### Input Validation

- [ ] All user inputs are validated and sanitized
- [ ] No SQL injection vulnerabilities (parameterized queries)
- [ ] No command injection vulnerabilities (no shell execution)
- [ ] No path traversal vulnerabilities (path sanitization)
- [ ] No buffer overflows (Rust memory safety, bounds checking)
- [ ] Integer overflow/underflow handled correctly

### Authentication & Authorization

- [ ] Authentication mechanisms are secure
- [ ] Authorization checks are present and correct
- [ ] No privilege escalation vulnerabilities
- [ ] Session management is secure (if applicable)
- [ ] Key management follows best practices

### Cryptographic Operations

- [ ] Cryptographic primitives used correctly (secp256k1, SHA-256)
- [ ] No hardcoded keys or secrets
- [ ] Random number generation is cryptographically secure
- [ ] Key derivation follows standards (BIP32, BIP39)
- [ ] Signature verification is complete and correct
- [ ] No timing attacks (constant-time operations where needed)

### Consensus & Protocol

- [ ] Consensus rules are correctly implemented
- [ ] No consensus bypass vulnerabilities
- [ ] Protocol message validation is complete
- [ ] Network message handling is secure
- [ ] No DoS vulnerabilities in network handling

### Data Protection

- [ ] Sensitive data is encrypted at rest (if applicable)
- [ ] Sensitive data is encrypted in transit (TLS)
- [ ] No sensitive data in logs
- [ ] No sensitive data in error messages
- [ ] Proper data retention and deletion

### Error Handling

- [ ] Errors don't leak sensitive information
- [ ] Error handling is comprehensive
- [ ] Fail-secure defaults are used
- [ ] No information disclosure through errors

### Memory Safety

- [ ] No unsafe Rust code (or unsafe is justified and documented)
- [ ] No memory leaks
- [ ] No use-after-free vulnerabilities
- [ ] Proper resource cleanup

### Dependencies

- [ ] Dependencies are up-to-date and secure
- [ ] No known vulnerabilities in dependencies (check advisories)
- [ ] Consensus-critical dependencies are pinned to exact versions
- [ ] Dependency licenses are compatible

## Testing

- [ ] Security tests are included
- [ ] Edge cases are tested
- [ ] Fuzzing is appropriate (for consensus/protocol code)
- [ ] Integration tests cover security scenarios
- [ ] Test coverage is adequate for security-sensitive code

## Documentation

- [ ] Security implications are documented
- [ ] Threat model is updated (if applicable)
- [ ] Security assumptions are documented
- [ ] Configuration security is documented

## Governance Compliance

- [ ] Appropriate governance tier is selected
- [ ] Required signatures are obtained
- [ ] Review period is respected
- [ ] Economic node veto is considered (if Tier 3+)
- [ ] Security control mapping is updated (if new controls added)

## Post-Review

- [ ] Security concerns are documented in PR comments
- [ ] Critical issues block merge
- [ ] Security review is approved (if required)
- [ ] Security control status is updated (if applicable)

## Priority-Specific Checks

### P0 (Critical) Controls

- [ ] Security audit is required and scheduled
- [ ] Formal verification is required and completed
- [ ] Cryptography expert review is obtained (if required)
- [ ] All P0 controls are verified

### P1 (High) Controls

- [ ] Security review is completed
- [ ] Formal verification is considered
- [ ] Cryptography expert review is considered (if applicable)

### P2/P3 (Medium/Low) Controls

- [ ] Standard security review is completed
- [ ] Security best practices are followed

## Notes

- For consensus-critical code, require formal verification (Kani proofs)
- For cryptographic code, require cryptography expert review
- For governance code, require economic node consideration
- When in doubt, escalate to security team or maintainers

## Related Resources

- [Developer Security Checklist](https://github.com/BTCDecoded/bllvm-docs/blob/main/src/security/DEVELOPER_SECURITY_CHECKLIST.md) (in bllvm-docs)
- [Security Architecture Review Template](https://github.com/BTCDecoded/bllvm-docs/blob/main/src/security/ARCHITECTURE_REVIEW_TEMPLATE.md) (in bllvm-docs)
- [Security Testing Template](https://github.com/BTCDecoded/bllvm-docs/blob/main/src/security/SECURITY_TESTING_TEMPLATE.md) (in bllvm-docs)
- [Security Controls System](https://github.com/BTCDecoded/bllvm-docs/blob/main/src/security/security-controls.md) (in bllvm-docs)

