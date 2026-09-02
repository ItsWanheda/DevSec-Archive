# The Complete OWASP Top 10 Guide 🛡️

> A deep-dive, practitioner-oriented reference to the OWASP Top 10 — covering theory, real-world incidents, vulnerable/secure code samples, detection tooling, prevention checklists, and a hands-on practice roadmap for every category.

---

## 📖 Table of Contents

1. [What Is the OWASP Top 10?](#1-what-is-the-owasp-top-10)
2. [A Short History of the Top 10](#2-a-short-history-of-the-top-10)
3. [What Changed in the 2025 Edition](#3-what-changed-in-the-2025-edition)
4. [2025 vs 2021 — Full Comparison](#4-2025-vs-2021--full-comparison)
5. [Methodology — How the List Is Built](#5-methodology--how-the-list-is-built)
6. [How to Use This Guide](#6-how-to-use-this-guide)
7. [A01:2025 — Broken Access Control](#7-a012025--broken-access-control)
8. [A02:2025 — Security Misconfiguration](#8-a022025--security-misconfiguration)
9. [A03:2025 — Software Supply Chain Failures](#9-a032025--software-supply-chain-failures)
10. [A04:2025 — Cryptographic Failures](#10-a042025--cryptographic-failures)
11. [A05:2025 — Injection](#11-a052025--injection)
12. [A06:2025 — Insecure Design](#12-a062025--insecure-design)
13. [A07:2025 — Authentication Failures](#13-a072025--authentication-failures)
14. [A08:2025 — Software or Data Integrity Failures](#14-a082025--software-or-data-integrity-failures)
15. [A09:2025 — Security Logging & Alerting Failures](#15-a092025--security-logging--alerting-failures)
16. [A10:2025 — Mishandling of Exceptional Conditions](#16-a102025--mishandling-of-exceptional-conditions)
17. [Building an AppSec Program Around the Top 10](#17-building-an-appsec-program-around-the-top-10)
18. [Threat Modeling Primer](#18-threat-modeling-primer)
19. [Secure Code Review Checklist](#19-secure-code-review-checklist)
20. [The Security Testing Toolbox](#20-the-security-testing-toolbox)
21. [CI/CD Pipeline Security Gates](#21-cicd-pipeline-security-gates)
22. [Incident Response Basics](#22-incident-response-basics)
23. [Compliance & Framework Mapping](#23-compliance--framework-mapping)
24. [Learning Roadmap & Practice Labs](#24-learning-roadmap--practice-labs)
25. [Master Prevention Checklist](#25-master-prevention-checklist)
26. [Glossary of Terms](#26-glossary-of-terms)
27. [Frequently Asked Questions](#27-frequently-asked-questions)
28. [References](#28-references)

---

## 1. What Is the OWASP Top 10?

The **OWASP Top 10** is a standard awareness document published by the [OWASP Foundation](https://owasp.org), a nonprofit dedicated to improving software security. It represents a broad, data-informed consensus — built from real-world testing data and practitioner surveys — on the most critical risks facing web applications.

It is not:
- A complete list of every possible vulnerability
- A compliance checklist that guarantees a "secure" application if followed
- A replacement for a full application security program

It is:
- A prioritization tool — telling teams where to focus limited security budget and attention first
- A shared vocabulary — so a developer in Tokyo and a pentester in Toronto mean the same thing when they say "A01" or "Broken Access Control"
- A training and awareness baseline — the foundation for countless certifications, scanners, and secure-coding courses
- A living document — revised periodically as the threat landscape, tooling, and industry practices evolve

The document exists because most real-world breaches don't come from exotic zero-days — they come from a small, recurring set of well-understood mistakes, repeated across millions of applications. The Top 10 exists to break that cycle.

### Who publishes it, and why does it matter?

OWASP is entirely volunteer-driven and vendor-neutral. Unlike a single vendor's "top vulnerabilities" report — which might be shaped by what that vendor's tools happen to detect well — the OWASP Top 10 pools **contributed testing data from dozens of independent organizations** (consultancies, scanner vendors, bug bounty platforms) covering millions of applications, and combines it with a **survey of security practitioners** to catch risks the raw data doesn't yet reflect. That combination of hard data and grounded practitioner judgment is what gives the list its credibility.

---

## 2. A Short History of the Top 10

| Year | Edition | Notable facts |
|---|---|---|
| 2003 | 1st edition | The original list; introduced the concept broadly |
| 2004 | 2nd edition | Minor refinements |
| 2007 | 3rd edition | Risk-rating methodology introduced |
| 2010 | 4th edition | Shifted toward risk-based prioritization over pure frequency |
| 2013 | 5th edition | "Using Components with Known Vulnerabilities" first appears — the ancestor of today's A03:2025 |
| 2017 | 6th edition | Controversial cycle; introduced XXE as its own category |
| 2021 | 7th edition | Introduced "Insecure Design" and "Software and Data Integrity Failures" as new categories; broadened CWE mapping methodology significantly |
| 2025 | **8th edition (current)** | Announced Nov 2025 at Global AppSec Washington D.C.; finalized Jan 2026. Introduces Software Supply Chain Failures and Mishandling of Exceptional Conditions; folds SSRF into Broken Access Control |

Tracking this history matters practically: a huge amount of existing tooling, training material, certifications (like many OSCP/eJPT-adjacent courses), and compliance frameworks still reference **OWASP Top 10:2021** by name. As a learner or professional, you'll need fluency in both for at least the next few years, since organizations migrate to the new list at different speeds.

---

## 3. What Changed in the 2025 Edition

The 2025 list introduces **two new categories**, **retires one standalone category by merging it into another**, and **renames two categories** for precision — while holding to the project's long-standing philosophy of ranking *root causes* over *symptoms* wherever practical.

### 🆕 New Categories

**A03:2025 – Software Supply Chain Failures**
An expansion of 2021's "Vulnerable and Outdated Components" (A06:2021). It's no longer just about patching outdated libraries — it now covers the *entire* software supply chain: compromised or malicious dependencies, tampered build pipelines, insecure CI/CD infrastructure, and unverified distribution/update mechanisms. This category was the community survey's overwhelming top concern, with exactly half of respondents ranking it #1 despite it having the thinnest hard testing data of any category (supply-chain compromises are notoriously hard to detect through conventional scanning).

**A10:2025 – Mishandling of Exceptional Conditions**
A completely new category covering what happens when software encounters abnormal, unexpected, or edge-case conditions and handles them insecurely — verbose error messages leaking internals, "fail open" logic that grants access by default when a check errors out, uncaught exceptions crashing services, and logic flaws only reachable via unusual state transitions. OWASP notes some of these weaknesses were previously bucketed under vague "poor code quality" labels; giving them their own category is meant to sharpen both detection tooling and developer guidance.

### 🔀 Consolidations

**Server-Side Request Forgery (SSRF)**, which stood alone as A10:2021, has been folded into **A01:2025 – Broken Access Control**, since SSRF is fundamentally an access-control failure — the server is tricked into accessing resources (often internal ones) that the requester shouldn't be able to reach directly.

### ✏️ Renames

- *Identification and Authentication Failures* (A07:2021) → **Authentication Failures** (A07:2025) — shortened for precision, same 36-CWE scope, largely unchanged ranking behavior.
- *Security Logging and Monitoring Failures* (A09:2021) → **Security Logging & Alerting Failures** (A09:2025) — the word swap from "Monitoring" to "Alerting" is deliberate: OWASP explicitly notes that comprehensive logs with no alerting mechanism provide minimal real-world security value, since nobody is watching them in time to act.

### 📊 By the Numbers

- **8th installment** of the Top 10 overall
- Announced **November 2025** at the OWASP Global AppSec Conference in Washington, D.C.
- Finalized **January 2026**
- Built from **2.8+ million tested applications** — the largest dataset in the project's history
- **589 CWEs** analyzed in total (up from ~400 in 2021, ~30 in 2017)
- **248–249 CWEs** distributed across the ten final categories (sources vary slightly on the exact total — official per-category counts are listed in each section below)
- **13 data-contributing organizations**, including Veracode, Contrast Security, Semgrep, Sonar, Bugcrowd, Orca Security, Probely, Wallarm, and others (plus anonymous contributors)
- **5 lead authors**: Andrew van der Stock, Brian Glas, Neil Smithline, Tanya Janca, and Torsten Gigler

---

## 4. 2025 vs 2021 — Full Comparison

| Rank 2025 | Category (2025) | CWEs | Prevalence | Rank 2021 | Category (2021) | Change |
|:---:|---|:---:|:---:|:---:|---|---|
| A01 | Broken Access Control | 40 | 3.73% (100% of apps had ≥1 form) | A01 | Broken Access Control | ➡️ Holds #1, absorbs SSRF |
| A02 | Security Misconfiguration | 16 | 3.00% (100% of apps) | A05 | Security Misconfiguration | ⬆️ Up 3 spots |
| A03 | Software Supply Chain Failures | 5–6 | 5.19% (when tested) | A06 | Vulnerable & Outdated Components | 🆕 Expanded & renamed |
| A04 | Cryptographic Failures | 32 | 3.80% | A02 | Cryptographic Failures | ⬇️ Down 2 spots |
| A05 | Injection | 37–38 | High (most-tested category) | A03 | Injection | ⬇️ Down 2 spots |
| A06 | Insecure Design | 39 | — | A04 | Insecure Design | ⬇️ Down 2 spots |
| A07 | Authentication Failures | 36 | — | A07 | Identification & Authentication Failures | ➡️ Holds #7, renamed |
| A08 | Software or Data Integrity Failures | 14 | — | A08 | Software and Data Integrity Failures | ➡️ Holds #8, minor rename |
| A09 | Security Logging & Alerting Failures | 5 | — | A09 | Security Logging and Monitoring Failures | ➡️ Holds #9, renamed |
| A10 | Mishandling of Exceptional Conditions | 24 | 2.95%, 769,581 occurrences, 3,416 CVEs | — | *(new)* | 🆕 Brand new |
| — | *(retired as standalone)* | — | — | A10 | Server-Side Request Forgery (SSRF) | 🔀 Merged into A01 |

**Reading this table correctly:** a category moving "down" in rank (like Cryptographic Failures or Injection) does **not** mean it became less dangerous in absolute terms — often it reflects genuine industry improvement (wider TLS adoption, better default cipher suites, more mature ORMs) *relative to* categories like misconfiguration and supply chain, which grew faster. Don't deprioritize a "falling" category just because its rank number went up.

---

## 5. Methodology — How the List Is Built

Understanding *how* OWASP builds this list will make you a better consumer of it — you'll know when to trust the ranking at face value and when to apply your own judgment.

### Step 1 — Data collection
OWASP asked contributing organizations for, per CWE, per year (starting from 2021): the **number of applications tested** and the **number of applications with at least one instance of that CWE**. Critically, they ignore *frequency* — an app with 4 instances of a SQLi bug and an app with 4,000 instances both just count as "1 app with the weakness present." This avoids skew from automated scanners that report every instance as unique versus manual testers who log a class of bug once.

### Step 2 — Risk scoring
For each CWE, OWASP pulled associated CVEs from the National Vulnerability Database (via OWASP Dependency-Check) and calculated **Exploitability** and **Technical Impact** scores from CVSS data — blending CVSSv2 and CVSSv3 scores (weighted by population, since not all CVEs have both) because CVSSv4's scoring model doesn't cleanly decompose into the same two axes.

### Step 3 — Category construction
With **589 CWEs** to organize (up from ~400 in 2021), OWASP spent months grouping them, deliberately favoring **root-cause categories** (like "Cryptographic Failure" or "Misconfiguration") over **symptom categories** (like "Sensitive Data Exposure" or "Denial of Service") wherever the distinction was practical — because root-cause framing gives clearer remediation guidance. Category size was capped at **40 CWEs** to keep each one trainable; the smallest category (A09, Logging & Alerting) has just 5.

### Step 4 — Ranking: 8 from data, 2 from survey
Here's the part most people don't realize: **only 8 of the 10 slots are chosen directly from the contributed testing data.** The remaining 2 are filled by the **community practitioner survey** — because raw testing data is inherently backward-looking. It takes years for a new vulnerability class to go from "researchers find it" → "tooling can detect it at scale" → "enough organizations have run that tooling long enough to contribute meaningful data." Supply-chain compromise and logging/alerting gaps are exactly this kind of risk: everyone on the front lines knows they matter, but conventional scanners struggle to quantify them. The survey is how OWASP keeps the list from being permanently a few years behind the actual threat landscape.

### Why not just list the individual CWEs?
OWASP has explicitly considered (and rejected) switching to a flat "Top 10/25 CWEs" model like MITRE's CWE Top 25. Two reasons given:
1. **Not all CWEs apply to all stacks.** A CWE specific to memory-unsafe languages (buffer overflows) is meaningless to a pure Python/JS shop; a flat CWE list would waste part of every organization's attention budget.
2. **Multiple CWEs often describe the same practical vulnerability class.** There are separate CWEs for general injection, command injection, XSS, hardcoded passwords, and more — bucketing them into categories raises the shared baseline awareness rather than fragmenting it.

---

## 6. How to Use This Guide

Each category section from here forward follows the same structure so you can jump straight to what you need:

- **Overview & Stats** — rank, CWE count, prevalence data
- **Root Causes** — the underlying engineering/process failures that produce this risk
- **Real-World Incidents** — named breaches or documented CVEs illustrating the category in production
- **Vulnerable Code** — deliberately insecure snippets across common languages/frameworks
- **Secure Code** — the corrected version, with inline notes on *why* it's safer
- **Notable CWEs** — a representative (not exhaustive) subset of the official CWE mapping
- **Detection & Testing** — tools and techniques to find this class of bug
- **Prevention Checklist** — actionable, copy-into-your-own-checklist items
- **Practice & Labs** — where to drill the skill hands-on

> ⚠️ **Ethical note:** every vulnerable snippet below is for educational/defensive purposes — to recognize the pattern in code review, not to attack systems you don't own or have explicit authorization to test. Practice offensive techniques only on your own infrastructure or authorized platforms (TryHackMe, HackTheBox, PortSwigger Academy, deliberately vulnerable apps like DVWA/Juice Shop, or a sanctioned bug bounty scope).

---

## 7. A01:2025 — Broken Access Control

### Overview & Stats
- **Rank:** #1 (4th consecutive cycle at the top)
- **CWEs mapped:** 40 — the largest category in the list
- **Prevalence:** ~100% of tested applications showed *some* form of broken access control; average incidence rate 3.73% per-CWE
- **Notable CWEs referenced by OWASP:** CWE-200 (Exposure of Sensitive Information), CWE-201 (Exposure Through Sent Data), CWE-352 (CSRF), CWE-918 (SSRF — newly absorbed here)
- **2025 change:** absorbs Server-Side Request Forgery, which was its own standalone category in 2021

### Root Causes
Access control enforces that authenticated users can only perform the actions and view the data their role permits. It fails when:
- Checks are implemented client-side only (a hidden button isn't a security boundary)
- Access-control logic is duplicated across dozens of endpoints instead of centralized, so some get missed
- Object references (IDs) are exposed directly and trusted without ownership verification
- The system defaults to *allow* instead of *deny* for anything not explicitly restricted
- CORS policies are configured too permissively
- The server can be tricked into making requests to internal-only resources on a user's behalf (SSRF)

### Real-World Incidents
Broken access control is consistently the highest-occurrence category in OWASP's contributed data — security researchers and reporting outlets have documented very high real-world prevalence, largely because authorization logic is application-specific and can't be solved by a generic library the way authentication mostly can. Two of the most common real-world patterns:
- **IDOR-driven mass data exposure** — an attacker increments a numeric ID in a URL or API call (`/api/orders/1001` → `/api/orders/1002`) and retrieves another customer's data because the backend checks "is this ID valid?" but never checks "does this ID belong to *this* user?"
- **SSRF against cloud metadata endpoints** — a server-side feature that fetches a user-supplied URL (image proxy, webhook validator, PDF renderer) is pointed at an internal address like `http://169.254.169.254/latest/meta-data/` to steal cloud credentials, a technique implicated in several major cloud breaches over the past decade.

### Vulnerable Code

**IDOR — Node.js/Express:**
```javascript
// VULNERABLE: no ownership check
app.get('/api/invoices/:id', authenticate, (req, res) => {
  const invoice = db.invoices.findById(req.params.id);
  res.json(invoice); // any authenticated user can read any invoice
});
```

**IDOR — Python/Flask:**
```python
# VULNERABLE: trusts the client-supplied user_id
@app.route('/api/profile/<user_id>')
@login_required
def get_profile(user_id):
    profile = db.query(f"SELECT * FROM profiles WHERE id = {user_id}")
    return jsonify(profile)
```

**SSRF — Python:**
```python
# VULNERABLE: fetches whatever URL the user provides, no restriction
@app.route('/preview')
def preview_url():
    target = request.args.get('url')
    response = requests.get(target)  # could be http://169.254.169.254/...
    return response.content
```

### Secure Code

**IDOR fixed — Node.js/Express:**
```javascript
// SECURE: verifies the resource belongs to the requesting user
app.get('/api/invoices/:id', authenticate, async (req, res) => {
  const invoice = await db.invoices.findOne({
    id: req.params.id,
    ownerId: req.user.id   // enforce record ownership
  });
  if (!invoice) return res.status(404).json({ error: 'Not found' });
  res.json(invoice);
});
```

**IDOR fixed — Python/Flask:**
```python
# SECURE: uses the authenticated session's own ID, parameterized query
@app.route('/api/profile')
@login_required
def get_profile():
    profile = db.execute(
        "SELECT * FROM profiles WHERE id = ?", (current_user.id,)
    )
    return jsonify(profile)
```

**SSRF fixed — Python:**
```python
import ipaddress
from urllib.parse import urlparse

ALLOWED_HOSTS = {"trusted-cdn.example.com"}

def is_safe_url(url: str) -> bool:
    parsed = urlparse(url)
    if parsed.hostname not in ALLOWED_HOSTS:
        return False
    try:
        ip = ipaddress.ip_address(socket.gethostbyname(parsed.hostname))
        if ip.is_private or ip.is_loopback or ip.is_link_local:
            return False
    except Exception:
        return False
    return True

@app.route('/preview')
def preview_url():
    target = request.args.get('url')
    if not is_safe_url(target):
        return jsonify({"error": "URL not allowed"}), 400
    response = requests.get(target, timeout=3)
    return response.content
```

### Notable CWEs (representative subset of 40)
| CWE | Name |
|---|---|
| CWE-22 | Path Traversal |
| CWE-200 | Exposure of Sensitive Information to an Unauthorized Actor |
| CWE-352 | Cross-Site Request Forgery (CSRF) |
| CWE-425 | Direct Request ("Forced Browsing") |
| CWE-441 | Unintended Proxy or Intermediary ("Confused Deputy") |
| CWE-538 | Insertion of Sensitive Info into Externally-Accessible File/Directory |
| CWE-548 | Exposure of Information Through Directory Listing |
| CWE-918 | Server-Side Request Forgery (SSRF) |

### Detection & Testing
- **Manual testing:** log in as two different low-privilege users; systematically swap IDs/tokens between their sessions ("differential testing")
- **Automated tools:** Burp Suite (Autorize / Auth Analyzer extensions), OWASP ZAP access-control scan rules
- **Static analysis:** SAST tools flagging missing authorization checks on routes/controllers
- **SSRF-specific:** Burp Collaborator / interactsh for out-of-band confirmation of blind SSRF

### Prevention Checklist
- [ ] Deny by default; grant access only for explicitly modeled roles/capabilities
- [ ] Enforce authorization **server-side**, on every request, never trusting client state
- [ ] Centralize access-control logic in one reusable mechanism/middleware
- [ ] Model access control around **record ownership**, not just "is the ID valid"
- [ ] Disable directory listing; ensure `.git`, backup files, and metadata aren't in web roots
- [ ] Rate-limit sensitive endpoints to blunt automated enumeration
- [ ] Invalidate session identifiers server-side on logout; keep JWTs short-lived
- [ ] Log and alert on repeated access-control failures
- [ ] For any server-side URL fetch: allow-list destinations, block private/loopback/link-local IP ranges, disable redirects to unchecked hosts

### Practice & Labs
- **PortSwigger Web Security Academy** — Access Control and SSRF learning paths (free, browser-based)
- **TryHackMe** — "OWASP Top 10" room, "IDOR" room
- **HackTheBox** — Starting Point track, many easy boxes feature IDOR/access-control bugs
- **DVWA / OWASP Juice Shop** — self-hosted practice targets with dedicated access-control challenges

---

## 8. A02:2025 — Security Misconfiguration

### Overview & Stats
- **Rank:** #2 (up from #5 in 2021 — the largest upward jump in the list)
- **CWEs mapped:** 16
- **Prevalence:** ~100% of tested applications had some form of misconfiguration; average incidence rate 3.00%; over **719,000 occurrences** recorded in the contributed data
- **Notable CWEs referenced by OWASP:** CWE-16 (Configuration), CWE-611 (XXE)

### Root Causes
As software increasingly runs on configuration rather than hardcoded behavior (cloud IAM policies, Kubernetes manifests, feature flags, environment variables, Infrastructure-as-Code), the *surface area* for a single wrong setting to cause a breach has grown enormously. Root causes include:
- Missing security hardening anywhere across the stack (OS, web/app server, database, framework, cloud service)
- Unnecessary features, ports, services, sample apps, or default accounts left enabled
- Verbose error handling exposing stack traces to end users
- Out-of-date software with security features not enabled after an upgrade
- Cloud storage (S3 buckets, blob storage) left with public/overly broad permissions

### Real-World Incidents
- **Scenario — directory listing left enabled:** an attacker discovers directory listing isn't disabled on a production server, browses to a `/classes/` folder, downloads compiled application bytecode, decompiles it, and finds a hardcoded access-control flaw — turning a "minor" misconfiguration into full application compromise.
- **Scenario — verbose error pages:** a production app returns full stack traces on unhandled exceptions, revealing the exact framework version in use (which has a known public CVE) and internal file paths — handing the attacker a roadmap for their next move.
- **Public cloud storage exposure** remains one of the most common real-world misconfiguration incidents reported across the industry — a storage bucket created for internal use gets set to public-read (or the default permission is never tightened) and is later indexed or discovered by automated internet-wide scanners.

### Vulnerable Code

**Debug mode left on in production — Django:**
```python
# settings.py — VULNERABLE
DEBUG = True
ALLOWED_HOSTS = ['*']
SECRET_KEY = 'django-insecure-hardcoded-key-123'
```

**Verbose error handler — Express.js:**
```javascript
// VULNERABLE: leaks stack trace to the client
app.use((err, req, res, next) => {
  res.status(500).send(err.stack);
});
```

**Overly permissive CORS:**
```javascript
// VULNERABLE
app.use(cors({ origin: '*', credentials: true }));
```

### Secure Code

**Django settings hardened:**
```python
# settings.py — SECURE
import os

DEBUG = False
ALLOWED_HOSTS = ['app.example.com']
SECRET_KEY = os.environ['DJANGO_SECRET_KEY']  # from a secrets manager
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
X_FRAME_OPTIONS = 'DENY'
```

**Error handler hardened — Express.js:**
```javascript
// SECURE: generic message to client, full detail to server-side logs only
app.use((err, req, res, next) => {
  logger.error(err.stack); // internal logging system, not the HTTP response
  res.status(500).json({ error: 'An unexpected error occurred.' });
});
```

**CORS locked down:**
```javascript
// SECURE
app.use(cors({
  origin: ['https://app.example.com'],
  credentials: true
}));
```

### Notable CWEs (representative subset of 16)
| CWE | Name |
|---|---|
| CWE-16 | Configuration |
| CWE-11 | ASP.NET Misconfiguration: Creating Debug Binary |
| CWE-260 | Password in Configuration File |
| CWE-315 | Cleartext Storage of Sensitive Information in a Cookie |
| CWE-489 | Active Debug Code |
| CWE-611 | Improper Restriction of XML External Entity Reference (XXE) |
| CWE-614 | Sensitive Cookie in HTTPS Session Without 'Secure' Attribute |
| CWE-942 | Permissive Cross-domain Policy with Untrusted Domains |
| CWE-1004 | Sensitive Cookie Without 'HttpOnly' Flag |

### Detection & Testing
- **Cloud posture:** cloud-native security posture management (CSPM) tools to catch public buckets/overly broad IAM
- **Header scanning:** `securityheaders.com`, Mozilla Observatory, or `nmap`/`nikto` for quick server-config sweeps
- **Config-as-code review:** treat Terraform/Kubernetes manifests as code — run them through SAST too
- **DAST:** OWASP ZAP and Burp both flag missing headers, verbose errors, directory listing automatically

### Prevention Checklist
- [ ] Automate hardening with a repeatable process for every environment (dev/staging/prod identical baselines)
- [ ] Remove unused features, frameworks, sample apps, and default accounts before shipping
- [ ] Centralize error handling; never return stack traces to end users
- [ ] Review and update configuration as part of routine patch management (see A03)
- [ ] Audit cloud storage and IAM permissions on a recurring schedule, not just at creation time
- [ ] Set all recommended security headers (CSP, HSTS, X-Content-Type-Options, X-Frame-Options)
- [ ] Mark cookies `Secure`, `HttpOnly`, and `SameSite` appropriately
- [ ] Disable XML external entity processing by default in all XML parsers

### Practice & Labs
- **TryHackMe** — "Linux Server Hardening," cloud misconfiguration rooms
- **HackTheBox** — easy/medium boxes frequently hinge on a single misconfigured service
- **Cloud CTFs** — flaws.cloud, CloudGoat (deliberately misconfigured AWS environments)

---

## 9. A03:2025 — Software Supply Chain Failures

### Overview & Stats
- **Rank:** #3 (new expanded scope; ancestor category first appeared in the 2013 list as "Using Components with Known Vulnerabilities")
- **CWEs mapped:** 5–6 (smallest category alongside A09)
- **Community survey:** ranked #1 by exactly 50% of respondents — the strongest single mandate of any category in this edition
- **Data challenge:** only ~11 CVEs directly map to this category's core CWEs, yet when tested it shows the **highest average incidence rate of any category at 5.19%** — a category that's rare to catch but severe when found

### Root Causes
This category widens the old "outdated components" idea into the full chain of trust behind your software:
- Not tracking versions of every component you depend on, client-side and server-side
- Pulling components from untrusted or unofficial sources
- Delaying patches for known-vulnerable dependencies due to change-control friction
- Not testing compatibility after upgrading/patching a library
- Unverified, unsigned build and distribution pipelines
- No human oversight on the path from "code written" to "code in production"

### Real-World Incidents

**SolarWinds Orion (2020)** — attackers compromised the vendor's *build environment* and inserted a backdoor into digitally signed Orion software updates. Roughly 18,000 customers downloaded the trojanized update as routine maintenance, trusting the vendor's signature — a textbook case of the build pipeline itself being the attack surface, not any individual line of application code.

**Log4Shell / CVE-2021-44228 (2021)** — a critical remote-code-execution flaw in the ubiquitous Java logging library Log4j 2. Because the library was embedded — often transitively, several dependencies deep — in an enormous share of enterprise Java software, organizations worldwide had to scramble just to *find every place it was used* before they could even begin patching.

**XZ Utils backdoor / CVE-2024-3094 (2024)** — a patient, multi-year social-engineering campaign got a malicious actor added as a co-maintainer of the widely used `xz`/`liblzma` compression library, who then inserted a stealthy backdoor into official release tarballs (versions 5.6.0–5.6.1). It was discovered almost by chance when a developer noticed anomalous SSH login latency on Debian systems.

**3CX DesktopApp (2023)** — a popular VoIP desktop client shipped trojanized installers for both Windows and macOS after attackers compromised the vendor's build/distribution infrastructure — demonstrating that poisoning a release channel creates instant enterprise-wide exposure the moment users update.

**Shai-Hulud npm worm (2025)** — the first successful self-propagating worm targeting the npm ecosystem. It seeded malicious versions of popular packages with a post-install script that harvested and exfiltrated developer secrets to public GitHub repositories, then automatically used any npm publish tokens it found to push malicious versions of every other package the compromised developer had access to — reaching over 500 affected package versions before npm disrupted it. It's a sharp reminder that in modern supply-chain attacks, **developer workstations themselves are now a prime target**, not just production infrastructure.

**Bybit theft, ~$1.5 billion (2025)** — one of the largest cryptocurrency thefts on record was traced to a supply-chain compromise of wallet-management software that only activated its malicious behavior under a very specific operational condition — illustrating how a supply-chain implant can lie dormant and undetected until the exact moment of highest-value opportunity.

### Vulnerable Code

**Unpinned, unverified dependency installation:**
```bash
# VULNERABLE: no version pin, no integrity verification
npm install left-pad
pip install requests
```

**Blind trust of an auto-update mechanism (see also A08):**
```python
# VULNERABLE: downloads and runs an update with no integrity check at all
import requests

def download_update(url):
    response = requests.get(url)
    with open('update.zip', 'wb') as f:
        f.write(response.content)
    # no signature or checksum verification before execution
```

### Secure Code

**Pinned, integrity-verified dependencies:**
```json
// package.json — pin exact versions, commit the lockfile
{
  "dependencies": {
    "left-pad": "1.3.0"
  }
}
```
```bash
# Verify lockfile integrity in CI before every install
npm ci --ignore-scripts   # skip arbitrary postinstall scripts by default
pip install --require-hashes -r requirements.txt
```

**Signature-verified update flow:**
```python
import hashlib
import hmac

TRUSTED_PUBLIC_KEY = load_trusted_key()

def download_update(url: str, expected_signature: bytes) -> bool:
    response = requests.get(url, timeout=10)
    content = response.content
    if not verify_signature(content, expected_signature, TRUSTED_PUBLIC_KEY):
        raise SecurityError("Update signature verification failed — aborting")
    with open('update.zip', 'wb') as f:
        f.write(content)
    return True
```

### Notable CWEs (representative subset)
| CWE | Name |
|---|---|
| CWE-477 | Use of Obsolete Function |
| CWE-1035 | Using Components with Known Vulnerabilities |
| CWE-1104 | Use of Unmaintained Third-Party Components |
| CWE-1329 | Reliance on Component That Is Not Updateable |
| CWE-1357 | Reliance on Insufficiently Trustworthy Component |
| CWE-1395 | Dependency on Vulnerable Third-Party Component |

### Detection & Testing
- **SCA (Software Composition Analysis):** Dependabot, Snyk, OWASP Dependency-Check, Trivy — continuously scan dependency trees against vulnerability databases
- **SBOM generation & diffing:** CycloneDX, SPDX tooling to know exactly what's in every build
- **Provenance verification:** Sigstore/cosign for signed container images and artifacts; `npm provenance`
- **Pipeline hardening review:** audit who can modify build scripts, secrets, and CI configuration

### Prevention Checklist
- [ ] Maintain a live Software Bill of Materials (SBOM) for every application
- [ ] Pin exact dependency versions; commit lockfiles; verify checksums/signatures on install
- [ ] Only pull packages from trusted, official registries; audit new third-party packages before adopting them
- [ ] Patch known-vulnerable components on a risk-based schedule, not just quarterly change windows
- [ ] Test compatibility before rolling out upgraded/patched libraries
- [ ] Require human code review before anything reaches production — no single person ships unreviewed
- [ ] Harden CI/CD: restrict who can edit pipeline configs, rotate build secrets, isolate build environments
- [ ] Disable arbitrary postinstall/lifecycle scripts in package managers where feasible

### Practice & Labs
- **Hands-on tools:** run `npm audit`, `pip-audit`, or `trivy fs .` against a real project and read every finding
- **TryHackMe / HackTheBox:** dependency-confusion and CI/CD pipeline attack rooms
- **OWASP Dependency-Track:** stand up a local instance and feed it your own SBOMs

---

## 10. A04:2025 — Cryptographic Failures

### Overview & Stats
- **Rank:** #4 (down from #2 in 2021 — reflecting broader industry TLS adoption and stronger default cipher suites)
- **CWEs mapped:** 32
- **Prevalence:** average incidence rate 3.80%
- Still one of the most consequential categories: failures here routinely lead directly to full sensitive-data exposure or account/session compromise

### Root Causes
This category is about failures *related to* cryptography — including simply not using it where it's needed — not about breaking the math of a well-implemented algorithm (that's exceptionally rare in practice).
- Transmitting sensitive data in cleartext (plain HTTP, outdated TLS versions)
- Using deprecated, weak, or home-grown cryptographic algorithms
- Hardcoded encryption keys or secrets committed to source control
- Storing passwords with weak or no hashing (unsalted MD5/SHA1, or plaintext)
- Predictable or reused initialization vectors (IVs) and insufficient randomness/entropy sources

### Real-World Incidents
Cryptographic failures underpin an enormous share of historical large-scale data breaches — not because attackers broke AES or RSA, but because organizations either didn't encrypt sensitive data at rest at all, stored password hashes with outdated algorithms that are now trivially crackable at scale, or exposed private keys through misconfiguration. This pattern — the *absence or misuse* of cryptography rather than its mathematical defeat — is consistent across the vast majority of publicly disclosed "crypto failure" incidents in the industry.

### Vulnerable Code

**Weak password hashing — Python:**
```python
import hashlib

# VULNERABLE: fast, unsalted hash — crackable at billions of guesses/sec on GPUs
def hash_password(password):
    return hashlib.md5(password.encode()).hexdigest()
```

**Hardcoded secret key — Node.js:**
```javascript
// VULNERABLE: secret committed straight into source control
const JWT_SECRET = "my-super-secret-key-2024";
jwt.sign(payload, JWT_SECRET);
```

**Predictable IV reuse — Python:**
```python
# VULNERABLE: same IV reused for every encryption call
from Crypto.Cipher import AES

IV = b'0000000000000000'  # constant, predictable

def encrypt(data, key):
    cipher = AES.new(key, AES.MODE_CBC, IV)
    return cipher.encrypt(pad(data))
```

### Secure Code

**Strong password hashing — Python:**
```python
import bcrypt

# SECURE: adaptive, salted hashing function designed for passwords
def hash_password(password: str) -> bytes:
    return bcrypt.hashpw(password.encode(), bcrypt.gensalt(rounds=12))

def verify_password(password: str, hashed: bytes) -> bool:
    return bcrypt.checkpw(password.encode(), hashed)
```

**Secret from a secrets manager — Node.js:**
```javascript
// SECURE: pulled from a secrets manager / environment at runtime, never committed
const JWT_SECRET = process.env.JWT_SECRET; // sourced from AWS Secrets Manager, Vault, etc.
if (!JWT_SECRET) throw new Error('JWT_SECRET not configured');
jwt.sign(payload, JWT_SECRET, { algorithm: 'HS256', expiresIn: '15m' });
```

**Fresh random IV per encryption — Python:**
```python
import os
from Crypto.Cipher import AES

# SECURE: cryptographically random IV generated fresh every call
def encrypt(data: bytes, key: bytes) -> bytes:
    iv = os.urandom(16)
    cipher = AES.new(key, AES.MODE_GCM, nonce=iv)
    ciphertext, tag = cipher.encrypt_and_digest(data)
    return iv + tag + ciphertext  # prepend IV/tag for use during decryption
```

### Notable CWEs (representative subset of 32)
| CWE | Name |
|---|---|
| CWE-261 | Weak Encoding for Password |
| CWE-295 | Improper Certificate Validation |
| CWE-321 | Use of Hard-coded Cryptographic Key |
| CWE-326 | Inadequate Encryption Strength |
| CWE-327 | Use of a Broken or Risky Cryptographic Algorithm |
| CWE-329 | Not Using a Random IV with CBC Mode |
| CWE-330 | Use of Insufficiently Random Values |
| CWE-338 | Use of Cryptographically Weak PRNG |
| CWE-759 | Use of a One-Way Hash Without a Salt |

### Detection & Testing
- **TLS/certificate checks:** `testssl.sh`, Qualys SSL Labs
- **Secret scanning:** TruffleHog, GitLeaks, GitHub secret scanning — run in CI on every commit
- **SAST:** most modern SAST tools flag weak algorithms (MD5/SHA1 for passwords), hardcoded keys, insecure randomness

### Prevention Checklist
- [ ] Classify data by sensitivity; encrypt sensitive data at rest and in transit as policy, not exception
- [ ] Use current, vetted cryptographic libraries — never implement your own primitives
- [ ] Enforce TLS 1.2+ everywhere; disable legacy protocol versions and weak cipher suites
- [ ] Hash passwords with a modern adaptive function: Argon2id, bcrypt, or scrypt — never MD5/SHA1/SHA256 alone
- [ ] Generate fresh, cryptographically random IVs/nonces for every encryption operation
- [ ] Store keys and secrets in a dedicated secrets manager, never in source control or config files
- [ ] Rotate keys and credentials on a defined schedule and immediately after any suspected exposure

### Practice & Labs
- **CryptoHack** — gamified cryptography challenges from fundamentals to applied attacks
- **PortSwigger Academy** — weak-JWT and weak-TLS-configuration labs
- **TryHackMe** — "Crypto" and "JWT Security" rooms

---

## 11. A05:2025 — Injection

### Overview & Stats
- **Rank:** #5 (down from #3, holding its relative position vs. Cryptographic Failures and Insecure Design)
- **CWEs mapped:** 37–38
- **Testing coverage:** the most-tested category of the ten, and the one with the greatest number of associated CVEs — spanning high-frequency/lower-impact bugs (reflected XSS) to lower-frequency/very-high-impact bugs (SQL injection)

### Root Causes
Injection happens when untrusted input is passed to an interpreter — SQL engine, shell, template engine, LDAP directory, XML parser — as part of a command or query, tricking it into executing something the developer never intended.
- String concatenation of user input directly into queries/commands
- Missing or purely client-side input validation
- Output rendered without context-aware escaping (HTML vs. JS vs. URL vs. SQL each need different escaping)
- Trusting "internal" input sources (message queues, other microservices) as if they were inherently safe

### Real-World Incidents
Injection — and specifically SQL injection — has been directly implicated in some of the largest data breaches in history, precisely because a single successful injection point can often be leveraged to dump an entire database in one automated run. Cross-Site Scripting remains, by volume, one of the most commonly reported vulnerability classes in bug bounty programs worldwide, because it's easy to introduce (one unescaped template variable) and the attack surface (every place user content is rendered back to other users) is enormous in any social or content-sharing platform.

### Vulnerable Code

**SQL Injection — PHP:**
```php
// VULNERABLE: string concatenation directly into the query
$username = $_POST['username'];
$query = "SELECT * FROM users WHERE username = '" . $username . "'";
$result = mysqli_query($conn, $query);
// input: admin' OR '1'='1  -->  bypasses authentication entirely
```

**Command Injection — Python:**
```python
# VULNERABLE: user input passed straight to a shell
import os
def ping_host(host):
    os.system(f"ping -c 4 {host}")
# input: "8.8.8.8; rm -rf /" --> arbitrary command execution
```

**Stored XSS — JavaScript/React (dangerouslySetInnerHTML misuse):**
```jsx
// VULNERABLE: renders raw, unescaped user content
function Comment({ text }) {
  return <div dangerouslySetInnerHTML={{ __html: text }} />;
}
```

### Secure Code

**SQL Injection fixed — PHP (prepared statements):**
```php
// SECURE: parameterized query, user input never touches the query string
$stmt = $conn->prepare("SELECT * FROM users WHERE username = ?");
$stmt->bind_param("s", $_POST['username']);
$stmt->execute();
$result = $stmt->get_result();
```

**Command Injection fixed — Python:**
```python
# SECURE: no shell involved, argument list avoids shell metacharacter parsing
import subprocess
import ipaddress

def ping_host(host: str):
    ipaddress.ip_address(host)  # validate it's a real IP first — allow-list, not deny-list
    subprocess.run(["ping", "-c", "4", host], check=True)
```

**Stored XSS fixed — React:**
```jsx
// SECURE: React escapes text content by default when NOT using dangerouslySetInnerHTML
function Comment({ text }) {
  return <div>{text}</div>;
}
// If raw HTML rendering is genuinely required, sanitize first:
import DOMPurify from 'dompurify';
function RichComment({ html }) {
  return <div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(html) }} />;
}
```

### Notable CWEs (representative subset)
| CWE | Name |
|---|---|
| CWE-20 | Improper Input Validation |
| CWE-74 | Improper Neutralization of Special Elements ("Injection") |
| CWE-77 | Command Injection |
| CWE-78 | OS Command Injection |
| CWE-79 | Cross-Site Scripting (XSS) |
| CWE-89 | SQL Injection |
| CWE-90 | LDAP Injection |
| CWE-91 | XML Injection |
| CWE-643 | XPath Injection |

### Detection & Testing
- **DAST:** OWASP ZAP, Burp Suite active scanning
- **SAST:** Semgrep, CodeQL, SonarQube — catches string-concatenated queries and unsafe sinks
- **Fuzzing:** sqlmap for automated SQLi discovery/exploitation (authorized testing only)
- **Manual testing:** classic payloads (`' OR '1'='1`, `<script>alert(1)</script>`) as a first-pass smoke test, followed by context-specific payloads

### Prevention Checklist
- [ ] Use parameterized queries / prepared statements everywhere — never concatenate input into queries
- [ ] Use an ORM with built-in escaping where practical, and still review its raw-query escape hatches
- [ ] Apply positive (allow-list) server-side input validation, not just client-side
- [ ] Escape output based on context: HTML, JS, CSS, URL, and SQL contexts each need different escaping
- [ ] Use auto-escaping templating engines by default (Jinja2, React, most modern frameworks)
- [ ] Set a strong Content-Security-Policy as defense-in-depth against XSS
- [ ] Avoid shelling out to the OS entirely where possible; if unavoidable, use argument lists, never string-built shell commands

### Practice & Labs
- **PortSwigger Web Security Academy** — SQLi and XSS learning paths are the gold standard, free and extremely thorough
- **DVWA / OWASP Juice Shop** — self-hosted, dozens of injection challenges at varying difficulty
- **TryHackMe / HackTheBox** — "SQL Injection" rooms, most "starting point" boxes feature an injection foothold
- **sqlmap** — practice against your own DVWA instance to understand what automated exploitation looks like

---

## 12. A06:2025 — Insecure Design

### Overview & Stats
- **Rank:** #6 (down from #4 — as Security Misconfiguration and Software Supply Chain Failures overtook it)
- **CWEs mapped:** 39
- Introduced as a category in 2021; OWASP notes measurable industry improvement in threat modeling adoption and secure-design emphasis since then, which is part of why its relative rank has eased

### Root Causes
This is fundamentally different from the other categories — it's not about an *implementation bug* in otherwise-sound architecture, it's about a flaw in the architecture and business logic itself. No amount of perfect coding fixes a design that was never secure to begin with.
- No threat modeling performed during the design phase
- Business logic flaws — no rate limit on password-reset requests, ability to submit negative quantities in a purchase, discount codes that stack infinitely
- Trusting the client to enforce rules that only the server can actually guarantee
- Missing segmentation, so a single compromised component has blast radius across the whole system

### Real-World Incidents
Business-logic flaws are, almost by definition, unique to each application — they can't be caught by a generic scanner the way a missing input filter can, because the "bug" is entirely in what the *business rules allow*, not in any single line of vulnerable code. Classic, widely documented examples across the industry include e-commerce platforms allowing coupon codes to be applied multiple times in a single checkout, loyalty/rewards systems allowing point balances to go negative and then be redeemed anyway, and multi-step checkout flows that can be replayed or reordered to skip a payment step entirely.

### Vulnerable Code

**Missing rate limit on password reset — conceptual (Python/Flask):**
```python
# VULNERABLE: no limit on reset attempts, no lockout
@app.route('/reset-password', methods=['POST'])
def reset_password():
    email = request.form['email']
    code = request.form['code']
    if code == get_stored_reset_code(email):  # 6-digit code, ~1M possibilities
        return set_new_password(email, request.form['new_password'])
    return "Invalid code", 400
```

**Negative-quantity business logic flaw:**
```python
# VULNERABLE: no validation that quantity is positive
@app.route('/cart/add', methods=['POST'])
def add_to_cart():
    product_id = request.form['product_id']
    quantity = int(request.form['quantity'])  # attacker sends -5
    price = get_price(product_id)
    total = price * quantity  # negative total "credits" the attacker's account
    cart.add(product_id, quantity, total)
```

### Secure Code

**Rate-limited, time-boxed reset flow:**
```python
from flask_limiter import Limiter

limiter = Limiter(app, key_func=lambda: request.form.get('email', ''))

# SECURE: rate limited per-email, codes expire, attempts are counted and locked out
@app.route('/reset-password', methods=['POST'])
@limiter.limit("5 per hour")
def reset_password():
    email = request.form['email']
    code = request.form['code']
    stored = get_stored_reset_code(email)
    if stored is None or stored.is_expired() or stored.attempts >= 5:
        return "Reset unavailable, request a new code", 400
    stored.attempts += 1
    if not hmac.compare_digest(code, stored.value):
        return "Invalid code", 400
    return set_new_password(email, request.form['new_password'])
```

**Business-logic-validated cart:**
```python
# SECURE: quantity validated against sane, explicit business bounds
@app.route('/cart/add', methods=['POST'])
def add_to_cart():
    product_id = request.form['product_id']
    quantity = int(request.form['quantity'])
    if not (1 <= quantity <= 50):
        return "Invalid quantity", 400
    price = get_price(product_id)
    total = price * quantity
    cart.add(product_id, quantity, total)
```

### Notable CWEs (representative subset of 39)
| CWE | Name |
|---|---|
| CWE-73 | External Control of File Name or Path |
| CWE-183 | Permissive List of Allowed Inputs |
| CWE-256 | Unprotected Storage of Credentials |
| CWE-266 | Incorrect Privilege Assignment |
| CWE-269 | Improper Privilege Management |
| CWE-602 | Client-Side Enforcement of Server-Side Security |
| CWE-841 | Improper Enforcement of Behavioral Workflow |

### Detection & Testing
- **Threat modeling workshops** (see [Section 18](#18-threat-modeling-primer)) — the primary detection mechanism, since automated tools largely can't find these
- **Business-logic-focused manual pentesting** — walking through multi-step workflows looking for skippable/replayable steps
- **Abuse-case testing** — writing test cases for "what happens if a user does X in an order the designer didn't expect"

### Prevention Checklist
- [ ] Integrate threat modeling into the design phase — before code is written, not after
- [ ] Establish and reuse secure design patterns and a vetted component library across teams
- [ ] Write tests that specifically validate business logic resists abuse (negative quantities, replay, race conditions)
- [ ] Apply least privilege throughout the architecture, not just at the perimeter
- [ ] Segment tenants, environments, and trust zones at the design level
- [ ] Enforce all critical rules server-side; treat client-side checks as UX convenience only

### Practice & Labs
- **PortSwigger Academy** — "Business logic vulnerabilities" learning path
- **OWASP Threat Dragon** — free, open-source threat-modeling tool, practice on your own project designs
- **HackTheBox / TryHackMe** — look specifically for boxes/rooms tagged "logic flaw" or "race condition"

---

## 13. A07:2025 — Authentication Failures

### Overview & Stats
- **Rank:** #7 (holds position, renamed from "Identification and Authentication Failures")
- **CWEs mapped:** 36
- Increased industry adoption of standardized authentication frameworks (OAuth 2.0/OIDC, well-vetted auth libraries) appears to be reducing real-world occurrence of the most basic failures, even as the category retains its rank

### Root Causes
Confirming a user's identity and safely managing their session afterward is one of the highest-stakes pieces of any application — get it wrong and every other control built "behind" authentication is moot.
- Permitting weak, default, or known-breached passwords
- Missing or improperly implemented multi-factor authentication (MFA)
- Session identifiers exposed in URLs, or not rotated after login (session fixation)
- Weak account-recovery ("forgot password") flows
- No brute-force/credential-stuffing protection

### Real-World Incidents
Credential-stuffing attacks — where attackers replay username/password pairs leaked from *other* breaches against a target's login page, banking on password reuse — are one of the most consistently reported attack patterns across the industry, precisely because they require no vulnerability in the target application at all, only the absence of rate-limiting or anomaly detection on the login endpoint. Session-fixation and token-leakage bugs (session IDs appearing in server access logs because they were passed as URL parameters instead of cookies/headers) are a recurring, well-documented pattern in web application penetration test findings.

### Vulnerable Code

**No brute-force protection — Node.js/Express:**
```javascript
// VULNERABLE: unlimited login attempts, no lockout, no delay
app.post('/login', async (req, res) => {
  const user = await db.users.findByUsername(req.body.username);
  if (user && user.password === req.body.password) { // also: plaintext comparison!
    return res.json({ token: generateToken(user) });
  }
  res.status(401).json({ error: 'Invalid credentials' });
});
```

**Session ID in the URL:**
```
VULNERABLE:
GET /dashboard?session_id=a1b2c3d4e5f6 HTTP/1.1
```
This leaks the session token into server access logs, browser history, and the `Referer` header of any outbound link.

### Secure Code

**Rate-limited, secure comparison, lockout — Node.js/Express:**
```javascript
const bcrypt = require('bcrypt');
const rateLimit = require('express-rate-limit');

const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5,
  message: 'Too many login attempts, try again later.'
});

// SECURE: rate limited, constant-time hash comparison, generic error message
app.post('/login', loginLimiter, async (req, res) => {
  const user = await db.users.findByUsername(req.body.username);
  const validPassword = user
    ? await bcrypt.compare(req.body.password, user.passwordHash)
    : await bcrypt.compare(req.body.password, DUMMY_HASH); // constant-time even for unknown users
  if (!user || !validPassword) {
    return res.status(401).json({ error: 'Invalid credentials' }); // don't reveal which field was wrong
  }
  req.session.regenerate(() => {   // rotate session ID on login
    req.session.userId = user.id;
    res.json({ success: true });
  });
});
```

**Session ID via secure cookie, never the URL:**
```javascript
// SECURE
res.cookie('session_id', sessionToken, {
  httpOnly: true,
  secure: true,
  sameSite: 'strict',
  maxAge: 15 * 60 * 1000
});
```

### Notable CWEs (representative subset of 36)
| CWE | Name |
|---|---|
| CWE-287 | Improper Authentication |
| CWE-288 | Authentication Bypass Using an Alternate Path |
| CWE-307 | Improper Restriction of Excessive Authentication Attempts |
| CWE-384 | Session Fixation |
| CWE-521 | Weak Password Requirements |
| CWE-613 | Insufficient Session Expiration |
| CWE-620 | Unverified Password Change |
| CWE-798 | Use of Hard-coded Credentials |

### Detection & Testing
- **Automated:** Burp Suite Intruder for credential-stuffing/brute-force simulation (authorized targets only)
- **MFA bypass testing:** manual review of "remember this device" and recovery-code flows
- **Session analysis:** confirm session tokens rotate on privilege change and never appear in logs/URLs

### Prevention Checklist
- [ ] Implement multi-factor authentication wherever feasible, and make it easy to enroll
- [ ] Never ship default credentials, especially for admin/service accounts
- [ ] Enforce strong password policies; check new passwords against known-breach corpora (e.g., Have I Been Pwned's API)
- [ ] Use secure, server-managed sessions — rotate IDs on login/privilege change, `HttpOnly`/`Secure`/`SameSite` cookies
- [ ] Rate-limit and monitor login attempts; add progressive delays/lockouts on repeated failures
- [ ] Use constant-time comparisons for credentials to avoid timing side-channels
- [ ] Return generic error messages that don't reveal whether the username or password was wrong

### Practice & Labs
- **PortSwigger Academy** — Authentication learning path (including 2FA bypass labs)
- **TryHackMe** — "Authentication Bypass," "JWT Security" rooms
- **HackTheBox** — many machines feature weak-credential or session-management footholds

---

## 14. A08:2025 — Software or Data Integrity Failures

### Overview & Stats
- **Rank:** #8 (holds position from 2021, minor rename)
- **CWEs mapped:** 14
- Distinguished from A03 (Supply Chain) by operating at a **lower, more granular level** — this category is about verifying integrity of specific artifacts, updates, and pipelines, whereas A03 is about the broader ecosystem of trust

### Root Causes
- Auto-update mechanisms that apply updates without verifying a cryptographic signature
- Insecure deserialization of data from untrusted sources
- CI/CD pipelines lacking proper segregation of duties or integrity checks on artifacts
- Blindly trusting plugins, libraries, or modules pulled from untrusted sources without verification

### Real-World Incidents
The SolarWinds Orion incident (also referenced under A03 above) is frequently cited under this category too, since it's simultaneously a supply-chain failure *and*, at the artifact level, an integrity failure — the digitally signed update itself lost its integrity guarantee because the signing process happened inside a compromised build environment. Insecure deserialization vulnerabilities have historically enabled remote code execution in numerous widely used Java and .NET frameworks, once researchers demonstrated that deserializing attacker-controlled data can be coerced into instantiating arbitrary objects and triggering unintended code paths.

### Vulnerable Code

**Insecure deserialization — Python (pickle):**
```python
import pickle

# VULNERABLE: pickle.loads() on untrusted input can execute arbitrary code
def load_session(data):
    return pickle.loads(data)  # attacker-controlled bytes -> arbitrary code execution
```

**Update with no integrity check (repeated from A03 for this specific angle):**
```python
# VULNERABLE
def download_update(url):
    response = requests.get(url)
    with open('update.zip', 'wb') as f:
        f.write(response.content)
    run_installer('update.zip')  # executed without any verification
```

### Secure Code

**Safe serialization — Python (JSON instead of pickle):**
```python
import json

# SECURE: JSON cannot execute code during deserialization
def load_session(data: str):
    return json.loads(data)  # only produces plain data structures, never code
```

**Signature-verified, sandboxed update:**
```python
def download_update(url: str, expected_sig: bytes):
    response = requests.get(url, timeout=10)
    if not verify_signature(response.content, expected_sig, TRUSTED_KEY):
        raise SecurityError("Signature mismatch — refusing to install")
    with open('update.zip', 'wb') as f:
        f.write(response.content)
    run_installer_in_sandbox('update.zip')  # least-privilege execution
```

### Notable CWEs (representative subset of 14)
| CWE | Name |
|---|---|
| CWE-345 | Insufficient Verification of Data Authenticity |
| CWE-346 | Origin Validation Error |
| CWE-353 | Missing Support for Integrity Check |
| CWE-494 | Download of Code Without Integrity Check |
| CWE-502 | Deserialization of Untrusted Data |
| CWE-829 | Inclusion of Functionality from Untrusted Control Sphere |

### Detection & Testing
- **SAST:** flag use of unsafe deserialization functions (`pickle.loads`, Java `ObjectInputStream`, PHP `unserialize`)
- **Pipeline audits:** verify every CI/CD stage checks artifact signatures before promotion
- **Fuzzing deserialization endpoints** with tools like `ysoserial` (Java) in authorized test environments only

### Prevention Checklist
- [ ] Use digital signatures to verify software/data hasn't been altered before trusting it
- [ ] Avoid deserializing data from untrusted sources; prefer data-only formats (JSON) over code-capable ones (pickle, Java serialization)
- [ ] Ensure CI/CD pipeline configs and Infrastructure-as-Code have strong access controls and audit trails
- [ ] Never send unsigned or unencrypted serialized objects to untrusted clients
- [ ] Review dependency/library changes for unexpected or unauthorized modifications before merging

### Practice & Labs
- **PortSwigger Academy** — insecure deserialization labs
- **HackTheBox** — several boxes built specifically around Java/.NET deserialization chains
- **ysoserial** — study payload generation for Java deserialization (defensive study, authorized targets only)

---

## 15. A09:2025 — Security Logging & Alerting Failures

### Overview & Stats
- **Rank:** #9 (holds position, renamed from "Security Logging and Monitoring Failures")
- **CWEs mapped:** 5 (tied smallest with A03)
- Chronically underrepresented in raw testing data — like A03, this category earns its spot largely through the community practitioner survey rather than automated scan data, because "we failed to log/alert on an incident" isn't something a scanner discovers by probing an app from outside

### Root Causes
- Auditable events (logins, failed logins, access-control failures, high-value transactions) not logged at all
- Logs stored only locally on the host, with no tamper resistance or centralized aggregation
- No alerting thresholds configured — incidents sit in logs unnoticed until real damage is already done
- Logs lacking sufficient context (no user ID, source IP, timestamp precision) to support a forensic investigation later

### Real-World Incidents
A recurring pattern across major publicly disclosed breaches is a significant gap — often measured in weeks or months — between initial compromise and detection, precisely because logging existed but nobody was alerted to the anomalous activity in time to intervene. OWASP explicitly frames this as the core problem the 2025 rename tries to address: "great logging with no alerting is of minimal value in identifying security incidents." A system can generate perfectly detailed logs and still fail catastrophically if there's no one — human or automated — watching them.

### Vulnerable Code

**No logging on authentication failure — Python/Flask:**
```python
# VULNERABLE: silent failure, nothing recorded, no way to detect a brute-force attempt later
@app.route('/login', methods=['POST'])
def login():
    user = authenticate(request.form['username'], request.form['password'])
    if not user:
        return "Invalid credentials", 401  # nothing logged at all
    return create_session(user)
```

### Secure Code

**Structured, alertable logging — Python/Flask:**
```python
import logging
import structlog

logger = structlog.get_logger()

# SECURE: structured log entry with context, feeding a SIEM/alerting pipeline
@app.route('/login', methods=['POST'])
def login():
    username = request.form['username']
    user = authenticate(username, request.form['password'])
    if not user:
        logger.warning(
            "auth_failure",
            username=username,
            source_ip=request.remote_addr,
            user_agent=request.headers.get('User-Agent'),
            timestamp=datetime.utcnow().isoformat()
        )
        record_failed_attempt(username, request.remote_addr)  # feeds rate-limit/alerting logic
        return "Invalid credentials", 401
    logger.info("auth_success", user_id=user.id, source_ip=request.remote_addr)
    return create_session(user)
```

**Alerting threshold example (pseudocode for a SIEM rule):**
```yaml
# SECURE: alert rule, not just a log line
rule: repeated_auth_failures
condition: count(event="auth_failure", group_by="source_ip") > 10 within 5m
action:
  - notify: security-oncall
  - auto_block: source_ip for 30m
```

### Notable CWEs (all 5)
| CWE | Name |
|---|---|
| CWE-117 | Improper Output Neutralization for Logs |
| CWE-223 | Omission of Security-relevant Information |
| CWE-532 | Insertion of Sensitive Information into Log File |
| CWE-778 | Insufficient Logging |
| CWE-779 | Logging of Excessive Data |

### Detection & Testing
- **Review, don't just scan:** logging gaps are found by tabletop exercises and log-coverage audits, not automated scanners
- **SIEM integration testing:** simulate an attack (safely, in staging) and confirm an alert actually fires and reaches the right people
- **Log-injection testing:** confirm user input can't forge log entries (CWE-117) — e.g., newline injection to fake a fraudulent "successful login" line

### Prevention Checklist
- [ ] Log all authentication, access-control, and server-side validation failures with sufficient context
- [ ] Generate logs in a format easily consumed by centralized log management / SIEM tooling
- [ ] Establish real alerting thresholds — logs alone are not a detection control
- [ ] Sanitize any user input that ends up in a log line to prevent log injection/forging
- [ ] Never log sensitive data itself (passwords, full card numbers, tokens) — log that an event happened, not the secret
- [ ] Build and periodically test an incident-response plan against these alerts (see [Section 22](#22-incident-response-basics))

### Practice & Labs
- **LetsDefend / Blue Team Labs Online** — blue-team-focused platforms simulating real SOC alert triage
- **TryHackMe** — "SOC Level 1" learning path, log-analysis rooms
- **Self-hosted:** stand up an ELK/Grafana Loki stack against a Juice Shop instance and build your own alert rules

---

## 16. A10:2025 — Mishandling of Exceptional Conditions

### Overview & Stats
- **Rank:** #10 — brand-new category for 2025
- **CWEs mapped:** 24
- **Stats:** average incidence rate 2.95%, **769,581 total occurrences** recorded, **3,416 associated CVEs** — a substantial footprint for a newly formalized category
- **Notable CWEs referenced by OWASP:** CWE-209 (sensitive error messages), CWE-234 (missing parameter handling), CWE-274 (insufficient privilege handling), CWE-476 (null pointer dereference), CWE-636 (not failing securely / "failing open")

### Root Causes
Attackers don't only attack the "happy path" — they deliberately target timeouts, malformed requests, missing parameters, race conditions, and partial transaction failures, because these are exactly the moments where an application's assumptions about "normal" behavior break down.
- Uncaught exceptions crashing threads/processes (a denial-of-service vector)
- Error messages that leak internal state (stack traces, SQL errors, file paths) back to the user
- "Fail open" logic — a security check that grants access by default when it errors instead of denying it
- Improper handling of missing or extra parameters, leading to undefined behavior
- Multi-step transactions with no rollback when a later step fails

### Real-World Incidents

**gdm3 fail-open bug (2020)** — a flaw in the GNOME Display Manager where a specific error condition during the login process could cause it to fail open, granting unintended access — cataloged as a textbook "not failing securely" (CWE-636) case.

**Cisco firewall crash — CVE-2021-34781** — an unhandled error condition in Cisco firewall software could be triggered to crash the device, a denial-of-service outcome stemming directly from improper exception handling in a security-critical component.

**Squid proxy credential leak — CVE-2025-62168** — a documented case where error-handling logic in the Squid proxy generated an error message that leaked credentials — a direct, real-world instance of CWE-209 (Generation of Error Message Containing Sensitive Information).

### Vulnerable Code

**Fail-open authorization check — Python:**
```python
# VULNERABLE: an error in the permission check grants access instead of denying it
def is_authorized(user, resource):
    try:
        return check_permissions(user, resource)
    except Exception:
        return True  # wrong: any error at all opens the door
```

**Swallowed exception hiding a security event — Java:**
```java
// VULNERABLE: authentication error is silently swallowed
try {
    authService.authenticate(user, password);
} catch (AuthenticationException e) {
    // empty catch block — the failure vanishes with no record and no denial
}
proceedToDashboard(user); // executes regardless of whether auth actually succeeded
```

**Resource exhaustion via unreleased resources on error:**
```python
# VULNERABLE: file handles/locks never released if an exception occurs mid-upload
def handle_upload(file):
    handle = acquire_upload_slot()
    process_file(file)  # if this throws, handle is never released
    release_upload_slot(handle)
```

### Secure Code

**Fail-closed authorization check — Python:**
```python
# SECURE: any error denies access by default; the error is also logged for investigation
def is_authorized(user, resource) -> bool:
    try:
        return check_permissions(user, resource)
    except Exception as e:
        logger.error("authorization_check_failed", user=user.id, resource=resource, error=str(e))
        return False  # fail closed
```

**Exception handled and denied explicitly — Java:**
```java
// SECURE: failure is logged, and access is explicitly denied
try {
    authService.authenticate(user, password);
} catch (AuthenticationException e) {
    logger.warn("Authentication failed for user {}: {}", user.getId(), e.getMessage());
    throw new AccessDeniedException("Authentication failed");
}
proceedToDashboard(user); // only reached if authentication genuinely succeeded
```

**Guaranteed resource cleanup:**
```python
# SECURE: resource is always released, whether processing succeeds or fails
def handle_upload(file):
    handle = acquire_upload_slot()
    try:
        process_file(file)
    finally:
        release_upload_slot(handle)  # runs no matter what
```

**Transactional rollback for multi-step operations:**
```python
# SECURE: the whole transaction fails closed if any step errors, no partial state
def transfer_funds(source, destination, amount):
    with db.transaction() as tx:
        try:
            tx.debit(source, amount)
            tx.credit(destination, amount)
            tx.log_transaction(source, destination, amount)
        except Exception as e:
            tx.rollback()
            logger.error("transfer_failed", error=str(e))
            raise
```

### Notable CWEs (representative subset of 24)
| CWE | Name |
|---|---|
| CWE-209 | Generation of Error Message Containing Sensitive Information |
| CWE-234 | Failure to Handle Missing Parameter |
| CWE-248 | Uncaught Exception |
| CWE-252 | Unchecked Return Value |
| CWE-274 | Improper Handling of Insufficient Privileges |
| CWE-369 | Divide By Zero |
| CWE-396 | Declaration of Catch for Generic Exception |
| CWE-476 | NULL Pointer Dereference |
| CWE-636 | Not Failing Securely ("Failing Open") |

### Detection & Testing
- **Fuzzing:** malformed input, missing parameters, unexpected types — tools like `ffuf`, Burp Intruder with negative-test payload lists
- **Static analysis:** flag empty catch blocks, catches of overly generic `Exception`/`Throwable`, unchecked return values
- **Chaos/fault injection testing:** deliberately kill dependencies mid-request (database, downstream API) and confirm the system fails closed, not open
- **Code review focus:** search the codebase for `except: pass`, `catch (Exception e) {}`, and similar swallow-everything patterns

### Prevention Checklist
- [ ] Fail securely — default to denying access or aborting the operation when an error occurs
- [ ] Return generic error messages to users; log full detail server-side only
- [ ] Explicitly handle every exceptional/edge-case condition rather than relying on default/uncaught behavior
- [ ] Wrap multi-step operations in transactions with guaranteed rollback on failure
- [ ] Use `finally`/`try-with-resources`/context managers to guarantee cleanup regardless of success or failure
- [ ] Include negative and abnormal-condition test cases in your test suite, not just the happy path
- [ ] Rate-limit and aggregate identical repeated errors rather than letting them silently cascade into resource exhaustion

### Practice & Labs
- **Fuzzing practice:** run `ffuf`/`wfuzz` against a local Juice Shop instance targeting parameter omission and malformed input
- **TryHackMe / HackTheBox:** look for rooms/boxes tagged "race condition" or "logic flaw" — many A10-style bugs surface as race conditions in multi-step flows
- **Code review drills:** grep open-source projects for empty catch blocks and generic exception handling as a pattern-recognition exercise

---

## 17. Building an AppSec Program Around the Top 10

The Top 10 is a prioritization tool, not a finish line. A mature program layers it into the full software lifecycle:

### Phase 1 — Design
- Threat modeling for every new feature touching sensitive data or access control (see [Section 18](#18-threat-modeling-primer))
- Security requirements written alongside functional requirements, not bolted on afterward
- Reference architectures and secure design patterns available to every team, not reinvented per-project

### Phase 2 — Build
- Secure coding standards mapped to Top 10 categories, enforced via linting/SAST in the IDE and pre-commit hooks
- Dependency policies (A03) enforced automatically — no merge without a passing SCA scan
- Peer code review with a security-focused checklist (see [Section 19](#19-secure-code-review-checklist))

### Phase 3 — Test
- SAST + SCA + secret scanning on every commit
- DAST against every staging deployment
- Periodic manual penetration testing — automated tools alone reliably miss A06 (Insecure Design) and A09 (Logging & Alerting) classes of issues
- Bug bounty or coordinated disclosure program for continuous external testing

### Phase 4 — Deploy & Operate
- Infrastructure-as-Code security scanning (A02)
- Runtime monitoring, logging, and alerting wired to an actual on-call rotation (A09)
- Incident response plan tested via tabletop exercises at least twice a year

### Phase 5 — Measure & Improve
- Track findings by Top 10 category over time — which categories keep recurring tells you where training or tooling investment will pay off most
- Feed real incidents back into the threat-modeling and design phase for the next cycle

---

## 18. Threat Modeling Primer

Threat modeling is the single highest-leverage practice for catching A06 (Insecure Design) issues before they're ever coded — because it happens *before* code exists.

### The STRIDE framework (a common starting point)
| Letter | Threat | Maps loosely to |
|---|---|---|
| **S** | Spoofing | A07 Authentication Failures |
| **T** | Tampering | A08 Software/Data Integrity Failures |
| **R** | Repudiation | A09 Logging & Alerting Failures |
| **I** | Information Disclosure | A04 Cryptographic Failures, A02 Misconfiguration |
| **D** | Denial of Service | A10 Mishandling of Exceptional Conditions |
| **E** | Elevation of Privilege | A01 Broken Access Control |

### A lightweight 4-question process (from the "Four Question Framework")
1. **What are we building?** — draw a data-flow diagram: components, trust boundaries, data stores
2. **What can go wrong?** — walk each trust boundary crossing with STRIDE
3. **What are we going to do about it?** — assign mitigations to specific threats, prioritized by risk
4. **Did we do a good job?** — review the model again after implementation; threat models are living documents

### Tools
- **OWASP Threat Dragon** — free, open-source, diagram-based threat modeling tool
- **Microsoft Threat Modeling Tool** — STRIDE-based, Windows-focused but widely used regardless of stack
- **pytm / threagile** — "threat-model-as-code" tools for teams that want models in version control alongside the codebase they describe

---

## 19. Secure Code Review Checklist

A practical checklist to run during pull-request review, organized by Top 10 category:

**Access Control (A01)**
- [ ] Does every new endpoint enforce authorization, not just authentication?
- [ ] Are object references checked against the requesting user's ownership?
- [ ] Any new server-side URL fetches — are destinations allow-listed?

**Configuration (A02)**
- [ ] Any new config values that should come from a secrets manager instead of code/env files checked into git?
- [ ] New third-party integration — is its default configuration reviewed, not just accepted?

**Supply Chain (A03)**
- [ ] New dependency added — is it from a trusted source, pinned, and does it pass SCA scanning?
- [ ] Does the PR modify CI/CD pipeline configuration? If so, does it need extra reviewer scrutiny?

**Cryptography (A04)**
- [ ] Any new hashing/encryption code — does it use vetted libraries and current algorithms?
- [ ] Any new secret/key material — confirmed it's not hardcoded?

**Injection (A05)**
- [ ] Any new database query — parameterized, not string-concatenated?
- [ ] Any new user-generated content rendered — properly escaped for its output context?
- [ ] Any new shell/OS command execution — avoided, or using argument lists instead of string interpolation?

**Design (A06)**
- [ ] Does this change introduce a new multi-step business process? Has it been threat-modeled?
- [ ] Are there rate limits or bounds on anything user-controllable that has a cost (money, resources, sends)?

**Authentication (A07)**
- [ ] Any new login/session-related code — rate limited, using constant-time comparison, rotating session IDs appropriately?

**Integrity (A08)**
- [ ] Any new deserialization of external data — using a safe format, not a code-executing one?
- [ ] Any new auto-update or plugin-loading logic — signature-verified?

**Logging (A09)**
- [ ] Does this change touch a security-relevant action? Is it logged with enough context?
- [ ] Does any log statement risk logging a secret (password, token, full card number)?

**Exceptional Conditions (A10)**
- [ ] Any new `try/catch` — does the catch block do something other than silently pass?
- [ ] Does error handling here fail open or fail closed? Should it be the other way?
- [ ] Are all resources (files, locks, connections) guaranteed to be released via `finally`/context managers?

---

## 20. The Security Testing Toolbox

| Tool Category | What It Finds | Example Tools |
|---|---|---|
| **SAST** (Static App Security Testing) | Insecure code patterns before runtime | Semgrep, CodeQL, SonarQube, Checkmarx |
| **DAST** (Dynamic App Security Testing) | Runtime vulnerabilities via black-box probing | OWASP ZAP, Burp Suite |
| **SCA** (Software Composition Analysis) | Vulnerable/outdated dependencies | Dependabot, Snyk, OWASP Dependency-Check, Trivy |
| **IAST** (Interactive App Security Testing) | Runtime issues with code-level context | Contrast Security, Seeker |
| **Secret Scanning** | Hardcoded credentials/keys in source | GitLeaks, TruffleHog, GitHub secret scanning |
| **Container/IaC Scanning** | Misconfigurations in Docker/K8s/Terraform | Trivy, Checkov, tfsec |
| **Fuzzing** | Crashes and edge cases from malformed input | ffuf, AFL++, wfuzz |
| **Manual Pentesting** | Business-logic flaws, chained exploits automated tools miss | Human testers, Burp Suite Pro |

**A note on tool coverage:** no single tool category catches everything. SAST is blind to runtime configuration; DAST is blind to code it can't reach through the UI/API surface; SCA only knows about *known* vulnerabilities in *named* dependencies. A06 (Insecure Design) and A09 (Logging & Alerting) in particular are categories where tooling coverage is weakest and human review matters most — which is exactly why OWASP's own methodology leans on a practitioner survey to keep those categories represented.

---

## 21. CI/CD Pipeline Security Gates

A reasonable default pipeline, roughly in order of execution:

```
1. Pre-commit hooks       → secret scanning (GitLeaks), lint
2. On every push           → SAST (Semgrep/CodeQL), SCA (Dependabot/Snyk)
3. On pull request         → require passing SAST/SCA + human code review
4. Build stage              → SBOM generation, artifact signing (Sigstore/cosign)
5. Pre-deploy to staging   → IaC scanning (Checkov/tfsec), container scanning (Trivy)
6. Staging environment     → DAST scan (OWASP ZAP baseline scan)
7. Pre-production gate     → verify artifact signature matches build stage output
8. Production              → runtime monitoring + alerting (A09) wired to on-call
```

**Key principle:** gates should get *stricter*, not looser, the closer you get to production — a finding that's a warning in a feature branch should be a hard block before a production deploy.

---

## 22. Incident Response Basics

Even a perfect prevention program will eventually face an incident. A minimal, practical IR structure:

1. **Preparation** — have a written plan, a defined on-call rotation, and pre-approved communication templates *before* you need them
2. **Detection & Analysis** — this is where A09 (Logging & Alerting) pays off directly; you cannot respond to what you never detected
3. **Containment** — short-term (isolate the affected system) and long-term (patch/rotate credentials) containment steps
4. **Eradication** — remove the root cause, not just the symptom (e.g., don't just delete a webshell — find and close the access-control gap that let it be uploaded)
5. **Recovery** — restore service, with heightened monitoring during the recovery window
6. **Lessons Learned** — blameless post-incident review, feeding findings back into [Section 17's](#17-building-an-appsec-program-around-the-top-10) design phase

Practicing this via **tabletop exercises** — a facilitated walkthrough of a hypothetical incident with no actual systems touched — is one of the highest-value, lowest-cost activities a security team can run regularly.

---

## 23. Compliance & Framework Mapping

The Top 10 doesn't exist in isolation — it's commonly referenced by or mapped into other standards:

| Framework | Relationship to OWASP Top 10 |
|---|---|
| **OWASP ASVS** (Application Security Verification Standard) | A much more granular, testable checklist; Top 10 categories map to specific ASVS requirements |
| **PCI DSS** | Requirement 6.2 explicitly references addressing "common coding vulnerabilities," historically pointing to the OWASP Top 10 by name |
| **NIST SSDF / SP 800-218** | Secure software development practices that align conceptually with Top 10 prevention guidance |
| **ISO/IEC 27001** | Broader information-security management; application security controls often cite OWASP guidance as implementation detail |
| **CWE / CVE (MITRE)** | The Top 10's underlying data model — every OWASP category is built from CWE mappings |

If your organization needs to demonstrate compliance, the Top 10 is a good **starting vocabulary**, but auditors will typically expect the more granular **ASVS** for anything formal.

---

## 24. Learning Roadmap & Practice Labs

### Suggested order for a learner building offensive-security skills
1. **Foundations first:** HTTP fundamentals, how cookies/sessions work, basic SQL — before touching any vulnerability class
2. **Injection (A05)** — the most "classic" and well-documented category; start here for the best learning-resource density
3. **Broken Access Control (A01)** — IDOR is often the easiest bug class to *find* once you understand the concept, making it a confidence-building second stop
4. **Authentication Failures (A07)** — pairs naturally with access control since both concern identity and session handling
5. **Cryptographic Failures (A04)** — requires a bit more foundational math/concepts; CryptoHack is purpose-built for this
6. **Security Misconfiguration (A02)** — often discovered opportunistically while doing recon on other categories; good to layer in throughout
7. **Insecure Design / Business Logic (A06)** — the hardest to "practice" via automated labs; requires deliberate manual-testing mindset
8. **Supply Chain (A03), Integrity Failures (A08), Logging Failures (A09), Exceptional Conditions (A10)** — increasingly relevant for more senior/blue-team-adjacent roles; good targets once the fundamentals above are solid

### Platform-by-platform guide
| Platform | Best for |
|---|---|
| **PortSwigger Web Security Academy** | Free, extremely thorough, the closest thing to a canonical web-vuln curriculum; start here for A01, A05, A06, A07 |
| **TryHackMe** | Guided, beginner-friendly rooms; good "OWASP Top 10" room exists as a direct companion to this guide |
| **HackTheBox** | Less guided, more realistic; Starting Point track is the right on-ramp before general boxes |
| **DVWA (Damn Vulnerable Web App)** | Self-hosted, adjustable difficulty, great for repeatable local practice |
| **OWASP Juice Shop** | Self-hosted, modern JS stack, gamified scoreboard covering nearly every category in this guide |
| **CryptoHack** | Dedicated to A04-style cryptography challenges |
| **LetsDefend / Blue Team Labs Online** | For A09-style detection/alerting practice from the defender's side |
| **flaws.cloud / CloudGoat** | A02-style cloud misconfiguration practice |

---

## 25. Master Prevention Checklist

A single compiled checklist across all ten categories — useful as a final pre-release gate:

- [ ] **A01** — Deny by default; server-side ownership checks on every object reference; SSRF destinations allow-listed
- [ ] **A02** — No debug mode, no default credentials, no verbose errors, security headers set, cloud storage audited
- [ ] **A03** — SBOM maintained; dependencies pinned and verified; CI/CD pipeline access restricted
- [ ] **A04** — Sensitive data encrypted at rest and in transit; modern password hashing; no hardcoded secrets
- [ ] **A05** — Parameterized queries everywhere; context-aware output escaping; no raw shell command building
- [ ] **A06** — Threat model exists for sensitive features; business logic has explicit bounds and rate limits
- [ ] **A07** — MFA available; rate-limited login; secure session management with rotation
- [ ] **A08** — Safe deserialization formats only; signature verification on updates/plugins
- [ ] **A09** — Security events logged with context; real alerting thresholds configured and tested
- [ ] **A10** — All error handling fails closed; no empty catch blocks; resources always released; generic user-facing errors

---

## 26. Glossary of Terms

- **ASVS** — OWASP Application Security Verification Standard; a granular, testable security-requirements checklist
- **CVE** — Common Vulnerabilities and Exposures; a public identifier for a specific, disclosed vulnerability instance
- **CVSS** — Common Vulnerability Scoring System; a numeric severity score for a vulnerability
- **CWE** — Common Weakness Enumeration; a taxonomy of *types* of software weaknesses (broader than a single CVE)
- **CSRF** — Cross-Site Request Forgery; tricking a victim's browser into submitting an unwanted authenticated request
- **DAST** — Dynamic Application Security Testing; black-box scanning of a running application
- **Fail Open / Fail Closed** — whether a system defaults to allow (open) or deny (closed) access when an error occurs; security-critical checks should always fail closed
- **IDOR** — Insecure Direct Object Reference; accessing a resource by manipulating an identifier without proper ownership checks
- **IAST** — Interactive Application Security Testing; combines static and dynamic analysis at runtime
- **JWT** — JSON Web Token; a compact, signed token format commonly used for stateless authentication
- **MFA** — Multi-Factor Authentication; requiring more than one proof of identity to log in
- **PoC** — Proof of Concept; a minimal working demonstration of a vulnerability
- **RCE** — Remote Code Execution; the ability to run arbitrary code on a target system remotely
- **SAST** — Static Application Security Testing; analyzing source code without executing it
- **SBOM** — Software Bill of Materials; a complete inventory of components in a piece of software
- **SCA** — Software Composition Analysis; scanning dependencies for known vulnerabilities
- **SIEM** — Security Information and Event Management; a platform that aggregates and analyzes logs for security alerting
- **SSRF** — Server-Side Request Forgery; tricking a server into making unintended requests, often to internal resources
- **STRIDE** — a threat-modeling mnemonic: Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, Elevation of privilege
- **XXE** — XML External Entity injection; exploiting XML parsers that resolve external entities to read files or make requests
- **XSS** — Cross-Site Scripting; injecting malicious script content that executes in another user's browser
- **Zero Trust** — an architectural principle of never implicitly trusting any request based on network location alone; verify explicitly, every time

---

## 27. Frequently Asked Questions

**Is the 2021 list still relevant to learn?**
Yes — a huge amount of existing tooling, certifications, and organizational documentation still references it by name, and the underlying vulnerability classes haven't gone anywhere even where categories were renamed or merged.

**Do I need to memorize every CWE number?**
No. Understanding the *pattern* each category describes matters far more than memorizing IDs. CWE numbers are useful for precise bug reports and cross-referencing tooling output, not for flashcard-style memorization.

**Which category should a beginner focus on first?**
Injection (A05) and Broken Access Control (A01) have the richest set of free, structured learning resources (especially PortSwigger's Academy) and the clearest "aha" moment when you find your first real bug — making them the best on-ramp.

**Is passing an automated scanner enough to be "secure"?**
No. Automated tools are strong on injection, misconfiguration, and known-vulnerable dependencies, but structurally weak on Insecure Design (A06) and Logging & Alerting Failures (A09) — both require human judgment about what the application is *supposed* to do before you can say what's wrong.

**How often does OWASP update the Top 10?**
Historically roughly every 3–4 years, though the gap between the 2021 and 2025 editions was the longest in the project's history, reflecting the scale of the underlying methodology overhaul (589 CWEs analyzed vs. ~400 previously).

---

## 28. References

- Official OWASP Top 10:2025 site: [owasp.org/Top10/2025](https://owasp.org/Top10/2025/)
- Official GitHub repository: [github.com/OWASP/Top10](https://github.com/OWASP/Top10)
- Full introduction & methodology: [owasp.org/Top10/2025/0x00_2025-Introduction](https://owasp.org/Top10/2025/0x00_2025-Introduction/)
- Individual category pages: `owasp.org/Top10/2025/A0{1-9}_2025-<Category_Name>/` and `A10_2025-Mishandling_of_Exceptional_Conditions`
- CWE master database: [cwe.mitre.org](https://cwe.mitre.org)
- OWASP Application Security Verification Standard (ASVS): [owasp.org/www-project-application-security-verification-standard](https://owasp.org/www-project-application-security-verification-standard/)
- OWASP Web Security Testing Guide: [owasp.org/www-project-web-security-testing-guide](https://owasp.org/www-project-web-security-testing-guide/)
- PortSwigger Web Security Academy: [portswigger.net/web-security](https://portswigger.net/web-security)
- OWASP Juice Shop: [owasp.org/www-project-juice-shop](https://owasp.org/www-project-juice-shop/)
- OWASP Threat Dragon: [owasp.org/www-project-threat-dragon](https://owasp.org/www-project-threat-dragon/)

---

*This guide compiles official OWASP Top 10:2025 source material with original explanations, code samples, and practice guidance for study and internal reference purposes. Verify against the official OWASP source for anything mission-critical, and note that documented real-world incidents are summarized from public reporting — consult primary sources for authoritative detail on any specific breach.*