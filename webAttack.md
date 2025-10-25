# Web security attacks — quick note (easy language)

Below is a **clear, concise Markdown table** you can share. Each row shows **what the attack is**, **how attackers do it**, and **practical ways to secure** against it.

```md
| Attack | What it is (simple) | How attackers do it | How to secure (practical steps) |
|-------|----------------------|---------------------|----------------------------------|
| XSS (Cross-Site Scripting) | Attacker injects malicious JavaScript into pages viewed by other users. | They submit script in forms, comments or URL parameters; the browser executes it in victims' pages. | Escape/encode output, use Content-Security-Policy (CSP), validate inputs, use templating that auto-escapes, set HttpOnly cookies. |
| SQL Injection (SQLi) | Attacker injects SQL to read/modify your database. | They send crafted input that gets concatenated into SQL (e.g. `'; DROP TABLE users; --`). | Use parameterized queries/prepared statements, ORMs, validate inputs, least-privilege DB user, WAF. |
| CSRF (Cross-Site Request Forgery) | Attacker tricks a logged-in user's browser to perform unwanted actions. | Victim visits a malicious page that causes the browser to submit a request to your site (using cookies). | Use CSRF tokens, require same-site cookies (`SameSite`), require Authorization headers, validate origin/referer. |
| IDOR (Insecure Direct Object Reference) / Broken Access Control | Users access objects (orders, profiles) they shouldn't. | Attacker changes IDs in URLs or requests (e.g. `/order/123` → `/order/122`). | Enforce server-side authorization checks for every request, don't rely on obscurity, use per-user ACLs. |
| SSRF (Server-Side Request Forgery) | Attacker makes your server send requests to internal systems. | They supply a URL that your server fetches (e.g. image or webhook) and point it to internal IPs. | Validate/whitelist outbound URLs, block private IP ranges, restrict metadata service access, use egress proxies. |
| RCE (Remote Code Execution) | Attacker runs arbitrary code on your server. | Via unsafe deserialization, eval of user input, vulnerable native modules. | Avoid `eval`, sandbox untrusted code, keep dependencies updated, use principle of least privilege, run processes in containers. |
| XXE (XML External Entity) | Malicious XML triggers the parser to fetch local/external files. | Attacker supplies XML with external entity pointing to local files or URLs. | Disable external entity resolution in XML parsers, prefer JSON, validate inputs. |
| Clickjacking | Attacker tricks a user into clicking a hidden UI inside a frame. | Page is embedded in an invisible `<iframe>` and user clicks something unknowingly. | Set `X-Frame-Options: DENY` or CSP `frame-ancestors`, use frame busting when needed. |
| Broken Authentication (including session fixation) | Weak auth lets attackers impersonate users. | Predictable session IDs, long-lived tokens, stolen credentials, reuse of sessions after login. | Use strong password policies, MFA, short session expiry, rotate tokens, secure cookies (`HttpOnly`, `Secure`, `SameSite`), revoke on logout. |
| Man-in-the-Middle (MitM) | Attacker intercepts traffic between client and server. | On insecure networks or via DNS hijack, attacker reads/modifies requests. | Enforce HTTPS/TLS, use HSTS, certificate pinning (where applicable), secure DNS (DNSSEC), use modern TLS configs. |
| Directory Traversal | Attacker reads files outside allowed directories (e.g. `/etc/passwd`). | They use `../` or encoded variants in file path parameters. | Normalize and validate path inputs, restrict file access to safe directories, use safe APIs. |
| Dependency / Supply-chain Attack | Malicious or compromised library added to your project. | Attacker publishes malicious package or compromises maintainer account. | Pin versions, review packages, run `npm audit`/Snyk, use lockfiles, install from trusted registries, CI SAST. |
| Denial of Service (DoS) / Resource Exhaustion | Attack overloads your app causing downtime. | Flood of requests, CPU/memory heavy requests, or expensive queries. | Rate limiting, WAF, auto-scaling, timeouts, circuit breakers, resource quotas, validate inputs. |
| Insecure Deserialization | Untrusted serialized data causes code execution or logic flaws. | Attacker crafts serialized payloads which cause application to instantiate harmful objects. | Avoid deserializing untrusted input, use safe formats (JSON), validate content, use signed payloads. |
| MIME sniffing / Content Type attacks | Browser treats content as a different type and executes it. | Serving user content without correct headers, browser guesses type and runs scripts. | Set `X-Content-Type-Options: nosniff`, set correct `Content-Type` header, sanitize uploads. |
| API abuse / Broken Object Level Auth | Unauthorized access via APIs (excess privileges). | Missing rate limits, missing per-user scoping, excessive data returned. | Enforce per-endpoint auth checks, rate limiting, pagination, field-level filtering/whitelisting. |
```

---


Which would you like?
