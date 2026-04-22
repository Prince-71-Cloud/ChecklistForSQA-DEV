

Input Validation Checklist (Developer & Tester Reference)

---

### General Principles

- [ ] Validate all input on both frontend and backend
- [ ] Enforce minimum and maximum length for all fields
- [ ] Normalize Unicode input and reject suspicious homoglyphs
- [ ] Reject unexpected input types (arrays, objects, binary data)
- [ ] Provide clear, actionable error messages to users
- [ ] Log repeated invalid input attempts and alert if thresholds are exceeded
- [ ] Apply rate limiting to sensitive endpoints

---

### Field-Type Specific Validation

#### Text Fields (Name, Username, City, etc.)

- [ ] Allow only characters required by business rules (e.g., letters, spaces, hyphens, apostrophes)
- [ ] Reject input containing special characters not explicitly allowed: `< > { } [ ] ( ) ; : \ / | ~ ! @ # $ % ^ & * = +`
- [ ] Reject emojis and non-standard Unicode symbols (unless business rules allow)
- [ ] Reject script tags, SQL keywords, or suspicious payloads (see Security Payloads)
- [ ] Enforce length limits (e.g., 2–50 characters)
- [ ] Reject leading, trailing, or consecutive whitespace
- [ ] For usernames: allow only letters, numbers, underscores, and dots
- [ ] For usernames: reject spaces, emojis, and risky symbols

#### Numeric Fields (Phone, Zip, Amount, etc.)

- [ ] Allow only digits (0–9) unless otherwise specified
- [ ] Reject alphabets, emojis, and special symbols (`+ - . , / \ e E < > =`)
- [ ] Reject spaces and invisible characters
- [ ] Reject scientific notation unless explicitly required
- [ ] Enforce valid range and length (e.g., phone numbers must be 10–15 digits)

#### Email Fields

- [ ] Validate format per RFC 5322 (use robust libraries)
- [ ] Reject spaces, multiple `@`, leading/trailing/consecutive dots
- [ ] Reject special characters: `< > ( ) , ; : \ " '` 
- [ ] Reject emojis and non-standard Unicode
- [ ] Enforce domain allow/block lists if required

#### URL Fields

- [ ] Validate scheme (`http`, `https`) and domain if required
- [ ] Reject spaces, `< > " ' { } | \ ^`
- [ ] Reject `javascript:`, `data:`, or other executable schemes
- [ ] Reject emojis and malformed URLs
- [ ] Enforce length and allowed protocols

#### File Uploads

- [ ] Restrict allowed file extensions (block dangerous types: `.php`, `.exe`, `.js`, etc.)
- [ ] Validate file content type (MIME) matches extension
- [ ] Reject double extensions (e.g., `file.jpg.php`)
- [ ] Reject null byte attempts, path traversal, hidden/mixed-case executable extensions
- [ ] Enforce file size limits
- [ ] Sanitize uploaded filenames

---

### Special Cases

- [ ] Test with common emojis (smileys, hearts, hand gestures, flags, objects, ZWJ sequences)
- [ ] Test with zero-width/invisible Unicode characters
- [ ] Reject or sanitize emojis and invisible characters as per business requirements
- [ ] Reject leading/trailing/multiple consecutive spaces, tabs, line breaks, non-breaking spaces, zero-width spaces
- [ ] Normalize whitespace where appropriate
- [ ] Enforce minimum and maximum input lengths for all fields
- [ ] Apply rate limiting to prevent brute-force or DoS attacks

---

### Security Payloads

- [ ] Reject input containing SQL injection patterns: `--`, `/*`, `*/`, `@@`, `xp_`, `OR '1'='1'`, `UNION`, `SELECT`, `DROP`, `INSERT`, `DELETE`, `UPDATE`
- [ ] Reject input containing XSS patterns: `<script>`, `</script>`, `javascript:`, `onerror=`, `onload=`, `alert(`
- [ ] Reject input containing path traversal: `../`, `..\`, `/`, `\`, `:`, `*`, `?`, `%00`
- [ ] Use allow-lists over block-lists where possible

---

### Error Handling & Logging

- [ ] Provide user-friendly, accessible error messages
- [ ] Log all validation failures with context (field, value, user/session)
- [ ] Alert on repeated or suspicious invalid input attempts

---

### Final QA Checklist

- [ ] All input fields validated on both frontend and backend
- [ ] Length, type, and format checks enforced
- [ ] Special characters, emojis, and invisible characters handled per requirements
- [ ] Security payloads and attack vectors tested and blocked
- [ ] File uploads validated for extension, content type, and size
- [ ] Error messages are clear and accessible
- [ ] Logging and alerting in place for validation failures
- [ ] Rate limiting implemented on sensitive endpoints
- [ ] Validation logic reviewed for consistency and completeness
