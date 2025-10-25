# Web Security Attacks — Summary

| Attack | What it is (Simple) | How Attackers Do It | How to Secure |
|--------|----------------------|---------------------|----------------|
| **XSS (Cross-Site Scripting)** | Injects malicious JS into web pages seen by others. | Injecting `<script>` tags or HTML via forms, comments, or URLs that browsers execute. | Escape/encode output, use CSP, validate inputs, use HttpOnly cookies. |
| **SQL Injection (SQLi)** | Injects SQL code to access/modify database. | Sending crafted input like `'; DROP TABLE users; --` through API or form. | Use prepared statements, ORM, validate input, least-privilege DB user. |
| **CSRF (Cross-Site Request Forgery)** | Forces logged-in user’s browser to perform unwanted actions. | Malicious page sends a request using victim’s cookies to your app. | Use CSRF tokens, SameSite cookies, validate origin/referer. |
| **IDOR / Broken Access Control** | Users access resources they shouldn’t. | Changing IDs in URL like `/user/2` to `/user/1`. | Always check user permissions on server side. |
| **SSRF (Server-Side Request Forgery)** | Forces your server to make requests to internal systems. | Sending a crafted URL to internal IP (e.g., 169.254.169.254). | Validate and whitelist URLs, block internal IP ranges. |
| **RCE (Remote Code Execution)** | Executes arbitrary code on server. | Via `eval`, unsafe deserialization, or vulnerable dependencies. | Avoid `eval`, sandbox code, update dependencies, use least privilege. |
| **XXE (XML External Entity)** | Exploits XML parsers to read local files or make requests. | Supplying XML with malicious entity definitions. | Disable external entity resolution, use JSON, validate XML. |
| **Clickjacking** | Tricks users into clicking hidden elements. | Embedding page in transparent iframe. | Use `X-Frame-Options: DENY` or `Content-Security-Policy: frame-ancestors`. |
| **Broken Authentication** | Weak login/session control. | Predictable tokens, session reuse, stolen credentials. | Use MFA, secure cookies, token expiry, rotate sessions. |
| **Man-in-the-Middle (MitM)** | Intercepts communication between client and server. | On public Wi-Fi, DNS hijack, or SSL stripping. | Use HTTPS/TLS, enable HSTS, certificate pinning. |
| **Directory Traversal** | Reads files outside web root. | Using `../` in file paths. | Sanitize and validate paths, restrict file access. |
| **Dependency Attack** | Using malicious libraries. | Installing compromised NPM packages. | Use lockfiles, verify dependencies, scan with `npm audit`. |
| **Denial of Service (DoS)** | Makes server unavailable by overload. | Flooding with requests or heavy operations. | Use rate limiting, WAF, caching, scaling, input validation. |
| **Insecure Deserialization** | Executes logic from untrusted serialized data. | Crafting fake serialized objects. | Avoid unsafe deserialization, use JSON, validate input. |
| **MIME Sniffing Attack** | Browser misinterprets file type and runs code. | Uploads or files served without headers. | Set `X-Content-Type-Options: nosniff`, correct `Content-Type`. |
| **API Abuse / BOLA** | Unauthorized access via weak APIs. | Missing access control or rate limits. | Add per-user auth, rate limiting, validate all API inputs. |


## **HSTS** and **Certificate Pinning** are *important web security mechanisms* related to HTTPS.
Let’s break them down clearly and simply 👇

---

## 🛡️ **1. HSTS (HTTP Strict Transport Security)**

| **Aspect**           | **Explanation**                                                                                                                        |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **Full form**        | HTTP Strict Transport Security                                                                                                         |
| **Purpose**          | Forces browsers to always use **HTTPS**, never HTTP.                                                                                   |
| **Why needed**       | Prevents attacks like **SSL stripping** (where attackers downgrade HTTPS → HTTP).                                                      |
| **How it works**     | The server sends a special response header to the browser:                                                                             |
| **Header Example**   | `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`                                                              |
| **Browser behavior** | After seeing this header once, the browser will **automatically use HTTPS** for all future visits (for the specified `max-age`).       |
| **Benefits**         | 1. Prevents users from accidentally visiting `http://` versions. <br> 2. Protects against man-in-the-middle attacks.                   |
| **Example**          | - First visit: browser connects via HTTPS, gets HSTS header. <br> - Next time: browser won’t even try HTTP — it forces HTTPS directly. |

---

## 🔒 **2. Certificate Pinning**

| **Aspect**                                                       | **Explanation**                                                                                                                                                                                                                      |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Purpose**                                                      | Ensures your app or browser **only trusts a specific SSL/TLS certificate or public key**.                                                                                                                                            |
| **Why needed**                                                   | Prevents attackers from using **fraudulent certificates** issued by compromised Certificate Authorities (CAs).                                                                                                                       |
| **How it works**                                                 | Your app “pins” a known certificate or public key fingerprint. When connecting to a server, it checks if the certificate matches the pinned one. If not — it **rejects the connection**, even if the certificate is otherwise valid. |
| **Types**                                                        | - **Static Pinning:** Hardcode cert fingerprint in app (common in mobile apps). <br> - **Dynamic Pinning:** Downloaded securely at runtime.                                                                                          |
| **Example (Node.js)**                                            | ```js                                                                                                                                                                                                                                |
| const https = require('https');                                  |                                                                                                                                                                                                                                      |
| const pinnedFingerprint = 'AB:CD:12:34:...';                     |                                                                                                                                                                                                                                      |
| https.get('[https://mybank.com](https://mybank.com)', (res) => { |                                                                                                                                                                                                                                      |
| const cert = res.socket.getPeerCertificate();                    |                                                                                                                                                                                                                                      |
| if (cert.fingerprint !== pinnedFingerprint) {                    |                                                                                                                                                                                                                                      |

```
throw new Error('Untrusted certificate!');
```

}
});

```|
| **Benefits** | Prevents fake certificates or MitM attacks using compromised CAs. |
| **Risk** | If you change your real certificate but forget to update pins → clients will **fail to connect** (so use carefully). |


| Feature | HSTS | Certificate Pinning |
|----------|-------|--------------------|
| **Protects from** | HTTP downgrade attacks (SSL stripping) | Fake or compromised SSL certificates |
| **Used by** | Web browsers | Mobile apps, custom HTTPS clients |
| **Configured via** | HTTP response header | App or server code |
| **Effect** | Forces HTTPS only | Ensures server identity |
| **Failure Impact** | User may see HTTP only warning | Connection fails (can’t bypass) |


```



