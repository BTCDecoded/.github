# Contributing to BTCDecoded

Thank you for your interest in contributing to BTCDecoded! This guide covers how to contribute to any repository in the BTCDecoded organization.

## Code of Conduct

This project and everyone participating in it is governed by our [Code of Conduct](CODE_OF_CONDUCT.md). By participating, you are expected to uphold this code.

## How to Contribute

### Reporting Issues

- Use the GitHub issue tracker for the specific repository
- Provide clear, detailed descriptions
- Include steps to reproduce for bugs
- Use appropriate labels

### Suggesting Enhancements

- Open a discussion or issue to discuss the enhancement
- Provide clear use cases and benefits
- Consider the impact on the 5-tier architecture

### Submitting Code

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/amazing-feature`
3. **Make your changes**: Follow our coding standards
4. **Add tests**: Ensure your changes are tested
5. **Run the test suite**: `cargo test`
6. **Commit your changes**: Use conventional commit messages
7. **Push to your fork**: `git push origin feature/amazing-feature`
8. **Open a Pull Request**: Provide a clear description

## Development Standards

### Code Style

- Follow Rust conventions and idioms
- Use `cargo fmt` to format code
- Use `cargo clippy` to check for improvements
- Write clear, self-documenting code

### Testing

- Write tests for all new functionality
- Ensure existing tests continue to pass
- Add integration tests for complex features
- Aim for high test coverage

### Documentation

- Document all public APIs
- Update README files when adding features
- Include code examples in documentation
- Follow Rust documentation conventions

### Commit Messages

Use conventional commit format:

```
type(scope): description

[optional body]

[optional footer]
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

Examples:
- `feat(bllvm-consensus): add new validation function`
- `fix(bllvm-node): resolve connection timeout issue`
- `docs(readme): update installation instructions`

## Repository-Specific Guidelines

### bllvm-consensus

- **Exact Version Pinning**: All consensus-critical dependencies must be pinned to exact versions
- **Mathematical Accuracy**: Changes must maintain mathematical correctness
- **Pure Functions**: All functions must remain side-effect-free
- **Comprehensive Testing**: All mathematical functions must be thoroughly tested

### bllvm-protocol

- **Protocol Abstraction**: Changes must maintain clean abstraction
- **Variant Support**: Ensure all Bitcoin variants continue to work
- **Backward Compatibility**: Avoid breaking changes to protocol interfaces

### bllvm-node

- **Consensus Integrity**: Never modify consensus rules
- **Production Readiness**: Consider production deployment implications
- **Performance**: Maintain reasonable performance characteristics

## Review Process

### Pull Request Requirements

- [ ] All tests pass
- [ ] Code follows style guidelines
- [ ] Documentation is updated
- [ ] Commit messages follow conventions
- [ ] Changes are focused and atomic

### Review Criteria

- **Correctness**: Does the code work as intended?
- **Performance**: Are there any performance implications?
- **Security**: Are there any security concerns?
- **Maintainability**: Is the code maintainable?
- **Documentation**: Is the code well-documented?

## Getting Help

- **Discussions**: Use GitHub Discussions for questions
- **Issues**: Use GitHub Issues for bugs and feature requests
- **Security**: Use private channels for security issues (see SECURITY.md)

## Recognition

Contributors will be recognized in:
- Repository CONTRIBUTORS.md files
- Release notes for significant contributions
- Organization acknowledgments

## Questions?

If you have questions about contributing, please:
1. Check existing discussions and issues
2. Open a new discussion
3. Contact maintainers privately for sensitive matters

Thank you for contributing to BTCDecoded!

