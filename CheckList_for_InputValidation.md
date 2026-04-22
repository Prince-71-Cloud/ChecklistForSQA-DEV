
Input Validation Checklist (Refined for Developer & Tester Teams)

---

## General Principles
- Validate all input on both frontend and backend.
- Enforce minimum and maximum length for all fields.
- Normalize Unicode input and reject suspicious homoglyphs.
- Reject unexpected input types (e.g., arrays, objects, binary data).
- Provide clear, actionable error messages to users.
- Log repeated invalid input attempts and alert if thresholds are exceeded.
- Apply rate limiting to sensitive endpoints.

---

## Field-Type Specific Validation

### 1. Text Fields (Name, Username, City, etc.)
- Allow only characters required by business rules (e.g., letters, spaces, hyphens, apostrophes).
- Reject input containing:
  - Special characters not explicitly allowed: `< > { } [ ] ( ) ; : \ / | ~ ! @ # $ % ^ & * = +`
  - Emojis and non-standard Unicode symbols (unless business rules allow).
  - Script tags, SQL keywords, or suspicious payloads (see Security Payloads).
- Enforce length limits (e.g., 2–50 characters).
- Reject leading, trailing, or consecutive whitespace.
- For usernames:
  - Allow letters, numbers, underscores, and dots.
  - Reject spaces, emojis, and risky symbols.

### 2. Numeric Fields (Phone, Zip, Amount, etc.)
- Allow only digits (0–9) unless otherwise specified.
- Reject:
  - Alphabets, emojis, and special symbols (`+ - . , / \ e E < > =`)
  - Spaces and invisible characters.
  - Scientific notation unless explicitly required.
- Enforce valid range and length (e.g., phone numbers must be 10–15 digits).

### 3. Email Fields
- Validate format per RFC 5322 (use robust libraries).
- Reject:
  - Spaces, multiple `@`, leading/trailing/consecutive dots.
  - Special characters: `< > ( ) , ; : \ " '` 
  - Emojis and non-standard Unicode.
- Enforce domain allow/block lists if required.

### 4. URL Fields
- Validate scheme (`http`, `https`) and domain if required.
- Reject:
  - Spaces, `< > " ' { } | \ ^`
  - `javascript:`, `data:`, or other executable schemes.
  - Emojis and malformed URLs.
- Enforce length and allowed protocols.

### 5. File Uploads
- Restrict allowed file extensions (block dangerous types: `.php`, `.exe`, `.js`, etc.).
- Validate file content type (MIME) matches extension.
- Reject:
  - Double extensions (e.g., `file.jpg.php`)
  - Null byte attempts, path traversal, hidden/mixed-case executable extensions.
- Enforce file size limits.
- Sanitize uploaded filenames.

---

## Special Cases

### Emojis & Unicode
- Test with:
  - Common emojis (smileys, hearts, hand gestures, flags, objects, ZWJ sequences).
  - Zero-width/invisible Unicode characters.
- Reject or sanitize as per business requirements.

### Whitespace & Invisible Characters
- Reject:
  - Leading/trailing/multiple consecutive spaces.
  - Tabs, line breaks, non-breaking spaces, zero-width spaces.
- Normalize whitespace where appropriate.

### Length & Rate Limiting
- Enforce minimum and maximum input lengths for all fields.
- Apply rate limiting to prevent brute-force or DoS attacks.

---

## Security Payloads

- Reject input containing:
  - SQL injection patterns: `--`, `/*`, `*/`, `@@`, `xp_`, `OR '1'='1'`, `UNION`, `SELECT`, `DROP`, `INSERT`, `DELETE`, `UPDATE`
  - XSS patterns: `<script>`, `</script>`, `javascript:`, `onerror=`, `onload=`, `alert(`
  - Path traversal: `../`, `..\`, `/`, `\`, `:`, `*`, `?`, `%00`
- Use allow-lists over block-lists where possible.

---

## Error Handling & Logging

- Provide user-friendly, accessible error messages.
- Log all validation failures with context (field, value, user/session).
- Alert on repeated or suspicious invalid input attempts.

---

## Final QA Checklist

- [ ] All input fields validated on both frontend and backend.
- [ ] Length, type, and format checks enforced.
- [ ] Special characters, emojis, and invisible characters handled per requirements.
- [ ] Security payloads and attack vectors tested and blocked.
- [ ] File uploads validated for extension, content type, and size.
- [ ] Error messages are clear and accessible.
- [ ] Logging and alerting in place for validation failures.
- [ ] Rate limiting implemented on sensitive endpoints.
- [ ] Validation logic reviewed for consistency and completeness.


