Perform a comprehensive security review of all pending changes in this repository.

## Scope

Review all staged and unstaged changes (`git diff` and `git diff --cached`). If no pending changes, review last commit (`git diff HEAD~1`).

## What to Look For

### Injection Attacks
- SQL injection (raw queries, unsanitized input in whereRaw, `DB::raw`)
- Command injection (`exec`, shell_exec, system, passthru, `proc_open`)
- XSS (unescaped {!! !!} with user input, v-html with user data)

### Authentication & Authorization
- Missing authorization checks, privilege escalation, CSRF, session issues

### Data Exposure
- Hardcoded secrets/keys, sensitive data in logs, mass assignment (missing `$fillable`/`$guarded`), sensitive data in API responses

### Laravel-Specific
- Missing validation, insecure file uploads, unsafe deserialization, missing rate limiting, debug routes in production

### Frontend-Specific
- v-html with user content, sensitive data in localStorage, exposed API keys

### Cryptographic Issues
- Weak hashing (MD5/SHA1 for passwords), hardcoded encryption keys

### Dependencies
- Known vulnerabilities in composer.lock, package-lock.json

## Output Format

For each finding: Severity (CRITICAL/HIGH/MEDIUM/LOW/INFO), File:line, Description, Impact, Remediation with code example.

## False Positive Filtering

- Verify data actually comes from user input.
- Check if middleware/validation already mitigates.
- Consider threat model (admin tools vs public APIs).
