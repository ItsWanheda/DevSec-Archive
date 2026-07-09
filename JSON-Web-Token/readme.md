# JSON Web Token (JWT): The Complete Developer Guide
> A comprehensive, production-grade guide to understanding, implementing, and securing JSON Web Tokens. From first principles to enterprise architecture.

[![Introduction](...)](...)
[![Authentication Basics](...)](...)
[![History of JWT](...)](...)
[![JWT Structure](...)](...)
[![JWT Claims](...)](...)
[![JWT Signing Algorithms](...)](...)
[![How JWT Authentication Works](...)](...)
[![Access Token](...)](...)
[![Refresh Token](...)](...)
[![Token Storage](...)](...)
[![Security Risks](...)](...)
[![Best Practices](...)](...)
[![Backend Implementation](...)](...)
[![Frontend Implementation](...)](...)
[![Building Production Authentication](...)](...)
[![Common Mistakes](...)](...)
[![Frequently Asked Questions](...)](...)
[![Interview Questions](...)](...)
[![Cheat Sheet](...)](...)
[![Resources](...)](...)

---

## Introduction
### **What is JWT?**
**JSON Web Token (JWT)** is a compact, URL-safe, self-contained token format defined by RFC 7519. It is used to securely transmit information between two parties as a JSON object. The information inside a JWT is digitally signed using a secret (HMAC) or a public/private key pair (RSA, ECDSA, EdDSA), which guarantees that the data has not been tampered with and that the sender is who they claim to be.

A **JWT** looks like this:
```bash
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
Three base64url-encoded strings separated by two dots. Simple on the outside, powerful on the inside.
> 💡 **Note**: JWT is a token format, not an authentication protocol. It is the container used inside many authentication and authorization systems. 

### **Why JWT Was Created**
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

### **Problems JWT Solves**
| **Problem** | **How JWT Solves It** |
|:------------|:----------------------|
| **Distributed authentication across microservices** | Tokens are self-contained and can be verified by any service using the same secret key (HS256) or a public key (RS256/ES256), eliminating the need for centralized session storage. |
| **Mobile app authentication** | JWTs are compact, lightweight, and easily transmitted in the `Authorization: Bearer <token>` header, making them ideal for mobile applications. |
| **Federated identity** | JWT claims securely carry user identity and attributes, allowing services to trust authenticated user information without repeatedly querying the identity provider. |
| **Single Sign-On (SSO)** | A single JWT issued after authentication can be accepted by multiple trusted applications, enabling seamless access across services. |
| **API authorization** | Each request includes a signed JWT containing user roles, permissions, and scopes, allowing APIs to authorize access without maintaining server-side sessions. |
| **Offline verification** | Since JWTs are digitally signed, their integrity and authenticity can be verified locally without requiring a database lookup or communication with the authentication server. |

### **When NOT to Use JWT**
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

## Authentication Basics
Before diving into JWT, you need a solid understanding of the concepts around it.

### Authentication
**Authentication** is the process of proving who you are. When you log in with a username and password, you are authenticating.
<p align="center">
  <img src="./src/image/Authentication.svg" alt="Authentication diagram" width="500">
</p>

### Authorization
**Authorization** is the process of determining what you are allowed to do. After authentication, the system decides which resources you can access.
<p align="center">
  <img src="./src/image/Authorization.svg" alt="Authorization diagram" width="500">
</p>

> 💡 Analogy: Authentication is showing your ID at the airport. Authorization is the boarding pass telling you which gate you can enter. 

### Identity
**Identity** is the set of attributes that uniquely describes a user — username, email, ID, roles, etc.

### Sessions
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

### Stateless Authentication
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

## History of JWT
### RFC 7519
JWT was formalized in RFC 7519, published in May 2015 by the IETF. The RFC defines:
- The token structure
- Standard claims ( iss, sub, aud, exp, nbf, iat, jti)
- Validation rules
- Use cases
### JOSE
JOSE stands for **Javascript Object Signing and Encryption**. It is the family of specifications that defines how JSON-based security tokens work.

| **Specification** | **Purpose** |
|:------------------|:------------|
| **JWS (RFC 7515)** | **JSON Web Signature** — Defines how JSON-based payloads are digitally signed to guarantee authenticity and integrity. |
| **JWE (RFC 7516)** | **JSON Web Encryption** — Defines how JSON-based payloads are encrypted to ensure confidentiality. |
| **JWK (RFC 7517)** | **JSON Web Key** — Specifies a standardized JSON format for representing cryptographic keys used for signing and encryption. |
| **JWA (RFC 7518)** | **JSON Web Algorithms** — Defines the registry of cryptographic algorithms used for signing, encryption, hashing, and key management. |
| **JWT (RFC 7519)** | **JSON Web Token** — Defines the compact, URL-safe token format used to securely transmit claims between parties. |

### Why JWT Became Popular
1. **Microservices boom** — distributed services needed a way to trust each other without central session storage.
2. **Mobile revolution** — native apps need header-based auth, not browser cookies.
3. **OAuth 2.0 and OIDC adoption** — both rely on JWT for ID tokens and access tokens.
4. **Lightweight** — easy to parse, easy to debug, easy to generate.
5. **Libraries everywhere** — every language has mature JWT libraries.
**Key Takeaways**
* JWT is a 2015 IETF standard, backed by the broader JOSE family.
* It became the de-facto token format for OAuth 2.0, OIDC, and modern APIs.

---

## JWT Structure
A JWT consists of three parts, separated by dots:
```bash
HEADER.PAYLOAD.SIGNATURE
```
Each part is **Base64URL** encoded (not Base64, not encrypted).

### The Three Parts Explained
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

### Base64URL Encoding
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

### Why JWT Is NOT Encrypted
A signed JWT only guarantees **integrity and authenticity** — that the token was created by a trusted party and hasn't been tampered with. **Anyone** who has the token can decode the payload and read it.
> ⚠️ **Warning**: Never put sensitive data (passwords, credit cards, SSNs, PII) in a JWT payload. It is base64, not encryption.
If you need confidentiality, use **JWE** (JSON Web Encryption) instead, or keep the data server-side and store only the ID in the JWT.

### Decode a Real JWT Manually
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

