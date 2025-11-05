# Security Policy

## Reporting Security Vulnerabilities

If you discover a security vulnerability in this project, please report it responsibly:

1. **DO NOT** open a public issue
2. Send details to the repository maintainer through GitHub's private vulnerability reporting feature
3. Include:
   - Description of the vulnerability
   - Steps to reproduce
   - Potential impact
   - Suggested fix (if any)

## Security Best Practices

### For Contributors

1. **Never commit sensitive information:**
   - API keys, tokens, or credentials
   - Personal email addresses or phone numbers
   - Private keys or certificates
   - Internal IP addresses or network configurations

2. **Review your commits:**
   - Check git diff before committing
   - Use `.gitignore` to exclude sensitive files
   - Never force-push to rewrite history without good reason

3. **Code security:**
   - Validate all inputs
   - Use secure coding practices for embedded systems
   - Be cautious with memory operations (buffer overflows)
   - Follow MISRA-C guidelines where applicable

### For the Codebase

1. **Hardware Security:**
   - Ensure debug interfaces are disabled in production
   - Implement secure boot if applicable
   - Protect firmware from unauthorized modification

2. **Build Security:**
   - Keep build artifacts out of version control
   - Use reproducible builds
   - Verify toolchain integrity

## Supported Versions

This project follows a rolling release model. Only the latest version on the main branch receives security updates.

## Privacy Protection

This repository follows privacy-first principles:
- No personal identifying information (PII) should be committed
- GitHub provides anonymized email addresses (`username@users.noreply.github.com` or `ID+username@users.noreply.github.com`) - use them for commits
- IDE configuration files containing personal paths are gitignored

## Secure Development Checklist

- [ ] No hardcoded credentials
- [ ] No personal information in commits
- [ ] `.gitignore` properly configured
- [ ] Sensitive files excluded from repository
- [ ] Code follows secure coding guidelines
- [ ] Dependencies are kept up to date
- [ ] Debug features disabled in production builds

## Additional Resources

- [OWASP Embedded Application Security](https://owasp.org/www-project-embedded-application-security/)
- [CWE/SANS Top 25 Most Dangerous Software Errors](https://www.sans.org/top25-software-errors/)
- [STM32 Security Guidelines](https://www.st.com/content/st_com/en/campaigns/cybersecurity.html)
