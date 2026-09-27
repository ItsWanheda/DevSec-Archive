# 🛡️ DevSec-Archive

> **Building secure foundations for the modern web.**

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Security](https://img.shields.io/badge/Focus-DevSecOps-red.svg)](#-mission)
[![Backend](https://img.shields.io/badge/Focus-Backend-6f42c1.svg)](#-backend-engineering)
[![Networking](https://img.shields.io/badge/Focus-Networking-0ea5e9.svg)](#-networking)
[![Knowledge Base](https://img.shields.io/badge/Type-Knowledge%20Base-111827.svg)](#-repository-structure)

**DevSec-Archive** is a technical knowledge base focused on the intersection of **Backend Engineering, Cybersecurity, Networking, Databases, and DevSecOps**.

The goal is simple:

> **Don't just make it work. Make it secure, scalable, observable, and maintainable.**

This repository collects practical concepts, architectural patterns, security principles, checklists, references, and engineering notes for building and defending modern web infrastructure.

---

## 🎯 Mission

Modern backend systems are more than APIs and databases. They are interconnected systems exposed to constantly evolving threats.

**DevSec-Archive** exists to bridge the gap between:

```text
┌──────────────────┐
│  Make It Work    │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Make It Secure  │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Make It Scalable │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│ Make It Reliable │
└──────────────────┘
```

The archive explores how to design, implement, secure, test, and operate backend systems in real-world environments.

---

## 📂 Repository Structure

### ⚙️ Backend Engineering

Foundations and architectural patterns for building modern server-side applications.

* [HTTP Fundamentals](Backend%20Engineering/HTTP%20Fundamentals/README.md) — comprehensive protocol and backend foundation
* [Clean Architecture](Backend%20Engineering/Clean%20Architecture/README.md) — production-oriented architecture and dependency design
* [Database Transactions](Backend%20Engineering/Database%20transactions/database-transactions.md) — transactions, isolation, concurrency, and consistency
* REST API Design
* GraphQL
* WebSockets
* Authentication & Authorization
* Session Management
* API Versioning
* Caching Strategies
* File Uploads
* Background Jobs
* Event-Driven Architecture
* Microservices
* API Gateways
* Backend Design Patterns
* Domain-Driven Design (DDD)

---

### 🛡️ Cybersecurity

Security concepts for protecting applications, APIs, infrastructure, and sensitive data.

* Web Application Security
* Authentication Security
* API Security
* [OWASP Top 10](Cybersecurity/Owasp%20top%2010/README.md)
* [OWASP API Security Top 10](Cybersecurity/OWASP%20API%20Security%20Top%2010/README.md)
* [Cryptography → Hash Functions](Cryptography/Hash%20Functions/README.md)
* Password Security
* [JWT Security](Cybersecurity/JSON-Web-Token/readme.md)
* [OAuth 2.0 & OpenID Connect](Cybersecurity/OAuth%202.0%20and%20OIDC/README.md)
* Secure Coding Practices
* Security Headers
* CORS
* CSRF
* XSS
* [SQL Injection](Cybersecurity/SQL%20Injection/README.md)
* SSRF
* Command Injection
* File Upload Security
* Secrets Management
* Zero Trust Architecture
* Threat Modeling

---

### 🌐 Networking

The protocols and infrastructure that connect modern applications.

* [Computer Networks](Networking/Computer%20Networks/README.md)
* OSI Model
* [TCP/IP Protocol Suite](Networking/TCP-IP%20Protocol%20Suite/README.md)
* HTTP / HTTPS
* DNS
* TLS / SSL
* Reverse Proxies
* Load Balancers
* Firewalls
* VPN
* CDN
* WebSockets
* HTTP/2 & HTTP/3
* gRPC
* SSH
* SMTP
* DNS Security

---

### 💾 Databases

Database fundamentals, performance, architecture, and security.

* SQL
* PostgreSQL
* MySQL
* MongoDB
* Redis
* Database Indexing
* [Transactions](Backend%20Engineering/Database%20transactions/database-transactions.md)
* ACID
* Isolation Levels
* Query Optimization
* Database Security
* Connection Pooling
* Backup & Recovery

---

### 🔐 Cryptography

Core cryptographic concepts used throughout secure systems.

* [Hash Functions](Cryptography/Hash%20Functions/README.md)
* Password Hashing
* Digital Signatures
* Public Key Infrastructure (PKI)
* Digital Certificates
* AES
* RSA
* ECC
* HMAC
* JWT
* JWS
* JWE
* JWK
* TLS Handshake

> **Note:** Cryptography should rarely be implemented from scratch. Prefer well-maintained, audited libraries and established standards.

---

### 📋 Best Practices

Practical checklists and engineering guidance for production systems.

* Secure API Checklist
* Backend Engineering Checklist
* Security Checklist
* Production Readiness
* Performance Optimization
* Logging Best Practices
* Error Handling
* Code Review Guide
* Dependency Management
* Secrets Management
* Monitoring & Observability
* Incident Response

> 🚧 Best-practice articles are being expanded alongside the main technical curriculum.

---

### 📚 Reference

A curated collection of useful technical resources.

* RFC Collection
* Security Cheat Sheets
* Books
* Whitepapers
* Tools
* Glossary
* Interview Questions
* Security Resources
* Engineering References

---

## 🧭 Learning Path

The archive is designed to support a progression from fundamentals to advanced system security.

### 🌱 Recommended Starting Sequence

1. [Computer Networks](Networking/Computer%20Networks/README.md)
2. [TCP/IP Protocol Suite](Networking/TCP-IP%20Protocol%20Suite/README.md)
3. [HTTP Fundamentals](Backend%20Engineering/HTTP%20Fundamentals/README.md)
4. [Database Transactions](Backend%20Engineering/Database%20transactions/database-transactions.md)
5. [Clean Architecture](Backend%20Engineering/Clean%20Architecture/README.md)
6. [OWASP Top 10](Cybersecurity/Owasp%20top%2010/README.md)
7. [OWASP API Security Top 10](Cybersecurity/OWASP%20API%20Security%20Top%2010/README.md)
8. [SQL Injection](Cybersecurity/SQL%20Injection/README.md)
9. [JSON Web Token](Cybersecurity/JSON-Web-Token/readme.md)
10. [OAuth 2.0 & OpenID Connect](Cybersecurity/OAuth%202.0%20and%20OIDC/README.md)
11. [Hash Functions](Cryptography/Hash%20Functions/README.md)

```text
Networking
     │
     ▼
HTTP & Web Fundamentals
     │
     ▼
Backend Engineering
     │
     ├──────────────┐
     ▼              ▼
Databases      API Design
     │              │
     └──────┬───────┘
            ▼
     Authentication
            │
            ▼
      API Security
            │
            ▼
     Secure Coding
            │
            ▼
    System Architecture
            │
            ▼
      DevSecOps / Zero Trust
```



---

## 🗂️ Published Article Index

> **Every link below points to an educational article that currently exists in the repository.**
>
> Topics without a link remain part of the planned curriculum and are not presented as completed material.

### ⚙️ Backend Engineering

| Article | Coverage |
| --- | --- |
| [HTTP Fundamentals](Backend%20Engineering/HTTP%20Fundamentals/README.md) | HTTP protocol, semantics, messages, state, caching, security, and backend foundations |
| [Clean Architecture](Backend%20Engineering/Clean%20Architecture/README.md) | Architecture boundaries, dependency direction, domain design, testing, and production structure |
| [Database Transactions](Backend%20Engineering/Database%20transactions/database-transactions.md) | Transactions, ACID, isolation, concurrency, locking, and database consistency |

### 🛡️ Cybersecurity

| Article | Coverage |
| --- | --- |
| [OWASP API Security Top 10](Cybersecurity/OWASP%20API%20Security%20Top%2010/README.md) | API-specific authorization, authentication, resource consumption, business-flow abuse, SSRF, configuration, inventory, third-party integrations, testing, and defensive engineering |\n| [OWASP Top 10](Cybersecurity/Owasp%20top%2010/README.md) | Major web application risks, root causes, defensive controls, and security engineering |
| [JSON Web Token](Cybersecurity/JSON-Web-Token/readme.md) | JWT structure, authentication, authorization, lifecycle, refresh flows, and security pitfalls |
| [OAuth 2.0 & OpenID Connect](Cybersecurity/OAuth%202.0%20and%20OIDC/README.md) | Authorization, identity, PKCE, SSO, flows, threats, and secure identity architecture |
| [SQL Injection](Cybersecurity/SQL%20Injection/README.md) | Injection mechanics, parameterized queries, mitigation, database security, and secure development |

### 🌐 Networking

| Article | Coverage |
| --- | --- |
| [Computer Networks](Networking/Computer%20Networks/README.md) | Networking foundations, OSI/TCP-IP, switching, routing, DNS, enterprise networks, SDN, VPN, and infrastructure |
| [TCP/IP Protocol Suite](Networking/TCP-IP%20Protocol%20Suite/README.md) | TCP/IP architecture, transport, addressing, routing, and protocol-level reasoning |

### 🔐 Cryptography

| Article | Coverage |
| --- | --- |
| [Hash Functions](Cryptography/Hash%20Functions/README.md) | Hash properties, SHA-2, SHA-3, SHAKE, HMAC, HKDF, password hashing, Merkle trees, signatures, and cryptographic engineering |

### 🎨 Web Design

| Article | Coverage |
| --- | --- |
| [CSS Mastery](Web%20Design/CSS%20Mastery/README.md) | CSS fundamentals, layout, responsive design, modern CSS, and maintainable UI engineering |

### 📄 Repository Documentation

- [Contributing Guide](CONTRIBUTING.md)
- [Security Policy](SECURITY.md)
- [Code of Conduct](CODE_OF_CONDUCT.md)
- [MIT License](LICENSE)

---

## 🚀 Built For

### 👨‍💻 Backend Engineers

Developers who want to build backend systems that are **secure, maintainable, scalable, and production-ready**.

### 🛡️ Security Enthusiasts

Learners exploring application security, API attack surfaces, authentication, infrastructure security, and defensive engineering.

### 🔧 DevSecOps Practitioners

Engineers interested in integrating security into development, deployment, infrastructure, and operational workflows.

### 🎓 Students & Self-Learners

Anyone building a strong foundation in backend engineering and cybersecurity through structured technical notes and references.

---

## 🛠️ Technology Focus

The concepts are intentionally **language-agnostic**, but examples and implementations primarily focus on:

### Languages

* 🐍 **Python**
* 🐹 **Go**
* 🟦 **TypeScript**
* 🟨 **JavaScript**
* ➕ More languages will be added over time

### Infrastructure

* 🐳 **Docker**
* 🐧 **Linux**
* 🔐 **Security-focused Linux environments**

### Architecture

* Microservices
* API Gateways
* Reverse Proxies
* Event-Driven Systems
* Distributed Systems
* Zero Trust Architecture
* Defense in Depth

---

## 🔒 Security Philosophy

DevSec-Archive follows a few core principles:

| Principle              | Goal                                                            |
| ---------------------- | --------------------------------------------------------------- |
| **Secure by Design**   | Security should start at architecture, not after deployment.    |
| **Least Privilege**    | Give systems and users only the access they actually need.      |
| **Defense in Depth**   | Never rely on a single security control.                        |
| **Fail Securely**      | Errors should not create security vulnerabilities.              |
| **Zero Trust**         | Never automatically trust a request, user, or network boundary. |
| **Assume Breach**      | Design systems with the possibility of compromise in mind.      |
| **Keep It Observable** | Logs, metrics, and monitoring are part of security.             |

---

## 🤝 Contributing

Security is a community effort.

Contributions are welcome — whether you want to:

* 📖 Add a new technical article
* 🐛 Fix an incorrect explanation
* 🔐 Improve a security recommendation
* 🧩 Add an architectural pattern
* 🔗 Add a valuable reference
* ✍️ Improve documentation
* 💡 Suggest a new topic

### Contribution Flow

```bash
# Fork the repository

# Create a branch
git checkout -b docs/improve-api-security

# Make your changes

# Commit
git commit -m "docs: improve API security notes"

# Push
git push origin docs/improve-api-security

# Open a Pull Request
```

Please review **CONTRIBUTING.md** before submitting a contribution.

---

## ⚠️ Security & Responsible Use

The security material in this repository is intended for **education, defensive engineering, secure development, and authorized security testing**.

Always test security techniques only against systems you own or have explicit permission to assess.

> **Learn how attacks work so you can build systems that resist them.**

---

## 📜 License

This project is licensed under the **MIT License**.

See [`LICENSE`](LICENSE) for the complete license text.

---

## 👤 Maintainer

Built and maintained with curiosity by **[ItsWanheda](https://github.com/ItsWanheda)**.

> *Keep learning. Keep building. Keep securing.*

---

<div align="center">

**DevSec-Archive**

*Backend • Security • Networking • Cryptography • Infrastructure*

⭐ If this archive helps you learn, consider giving it a star.

</div>
