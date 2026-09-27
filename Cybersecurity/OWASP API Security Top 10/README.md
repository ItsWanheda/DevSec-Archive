# 🛡️ OWASP API Security Top 10 — Complete Educational Guide

> **A practical, defensive curriculum for designing, reviewing, testing, and operating secure APIs.**
> **Edition:** OWASP API Security Top 10 — 2023
> **Last reviewed:** 2026-09-27

[![OWASP](https://img.shields.io/badge/OWASP-API%20Security-red)](https://owasp.org/www-project-api-security/) [![Edition](https://img.shields.io/badge/Edition-2023-blue)](https://api-security.owasp.org/editions/2023/en/0x11-t10/)

> 🌱 **Learning principle:** security is a set of explicit server-side decisions that must be designed, tested, observed, and maintained.

---

## 🧭 Quick Navigation
- [Scope](#scope)
- [2023 Risk List](#2023-risk-list)
- [Methodology and Data](#methodology-and-data)
- [2019 to 2023 Changes](#2019-to-2023-changes)
- [Mental Model](#mental-model)
- [API1](#api1)
- [API2](#api2)
- [API3](#api3)
- [API4](#api4)
- [API5](#api5)
- [API6](#api6)
- [API7](#api7)
- [API8](#api8)
- [API9](#api9)
- [API10](#api10)
- [Authentication](#authentication)
- [Authorization](#authorization)
- [Validation](#validation)
- [Resource Controls](#resource-controls)
- [Business Flows](#business-flows)
- [SSRF](#ssrf)
- [Configuration](#configuration)
- [Inventory](#inventory)
- [Third-Party APIs](#thirdparty-apis)
- [Testing](#testing)
- [CI/CD](#cicd)
- [Threat Modeling](#threat-modeling)
- [Logging](#logging)
- [Incident Response](#incident-response)
- [Checklist](#checklist)
- [Labs](#labs)
- [Glossary](#glossary)
- [References](#references)

---

## 🎯 Scope
- The OWASP API Security Top 10 is an awareness document focused on risks that are particularly important to APIs.
- The 2023 edition is the current edition covered by this article.
- It is not a complete application security program.
- It does not replace secure coding, identity security, cryptography, network security, threat modeling, or operational security.
- Generic vulnerabilities such as injection still apply to APIs even when they are not separate API Top 10 categories.
- The correct priority for a real system depends on assets, exposure, architecture, threat model, and business impact.
- This guide uses original educational examples and links readers to authoritative sources.

## 📋 2023 Risk List
| ID | Risk | Core boundary |
| --- | --- | --- |
| API1:2023 | Broken Object Level Authorization | object |
| API2:2023 | Broken Authentication | identity |
| API3:2023 | Broken Object Property Level Authorization | property |
| API4:2023 | Unrestricted Resource Consumption | resource |
| API5:2023 | Broken Function Level Authorization | function |
| API6:2023 | Unrestricted Access to Sensitive Business Flows | workflow |
| API7:2023 | Server Side Request Forgery | network |
| API8:2023 | Security Misconfiguration | configuration |
| API9:2023 | Improper Inventory Management | inventory |
| API10:2023 | Unsafe Consumption of APIs | dependency |

## 📊 Methodology and Data
OWASP states that the 2023 update used publicly available API security incident information from 2019–2022.
A three-month public call for data was conducted.
OWASP states that the call did not provide enough contributed data for a relevant statistical analysis.
The project therefore used specialist review, community feedback, public incident information, and OWASP risk-rating methodology.
Prevalence in the 2023 material is not a universal measurement of every deployed API.
Do not turn the category order into a claim that one category is always more severe than another in every application.

## 🔄 2019 to 2023 Changes
- API1 Broken Object Level Authorization remained.
- API2 Broken Authentication remained.
- Excessive Data Exposure and Mass Assignment were combined into Broken Object Property Level Authorization.
- Resource controls expanded into Unrestricted Resource Consumption.
- Broken Function Level Authorization remained.
- Unrestricted Access to Sensitive Business Flows was introduced.
- Server Side Request Forgery was included as an API-specific category.
- Security Misconfiguration remained.
- Improper Inventory Management remained.
- Unsafe Consumption of APIs was introduced.

---
## 🛡️ API1:2023 — Broken Object Level Authorization

> **Core rule:** Verify that the authenticated principal is allowed to access the exact object identified by the request.

### 🔎 What the risk means
This category concerns the object boundary of an API.
The client is not a trusted authority for security decisions.
The server must establish trusted security context before making the decision.
The same technical flaw can have different business impact depending on the affected data and operation.

### 🧩 Common attack surface
- path identifiers: verify the object policy is enforced when this input reaches the operation (1).
- query parameters: verify the object policy is enforced when this input reaches the operation (2).
- JSON fields: verify the object policy is enforced when this input reaches the operation (3).
- HTTP headers: verify the object policy is enforced when this input reaches the operation (4).
- cookies: verify the object policy is enforced when this input reaches the operation (5).
- multipart data: verify the object policy is enforced when this input reaches the operation (6).
- GraphQL arguments: verify the object policy is enforced when this input reaches the operation (7).
- gRPC metadata: verify the object policy is enforced when this input reaches the operation (8).
- webhooks: verify the object policy is enforced when this input reaches the operation (9).
- bulk endpoints: verify the object policy is enforced when this input reaches the operation (10).
- exports: verify the object policy is enforced when this input reaches the operation (11).
- search: verify the object policy is enforced when this input reaches the operation (12).
- background jobs: verify the object policy is enforced when this input reaches the operation (13).
- scheduled jobs: verify the object policy is enforced when this input reaches the operation (14).
- mobile clients: verify the object policy is enforced when this input reaches the operation (15).

### 🧱 Root causes
- implicit trust
- missing server-side policy
- overly broad permissions
- inconsistent middleware
- copy-pasted authorization
- missing tenant context
- unsafe defaults
- ambiguous contracts
- unbounded work
- stale configuration
- missing inventory
- weak dependency boundaries

### 🛠️ Defensive controls
- authenticate before privileged work
- authorize every sensitive operation
- apply least privilege
- deny by default
- bind tenant context to trusted identity
- use explicit object policies
- use explicit property allowlists
- protect privileged functions
- bound expensive work
- use timeouts
- validate external data
- restrict outbound destinations when applicable
- maintain API inventory
- emit security telemetry
- write regression tests

### 🧪 Security tests
| 1 | anonymous request | Define and assert the expected secure result. |
| 2 | valid request | Define and assert the expected secure result. |
| 3 | wrong user | Define and assert the expected secure result. |
| 4 | wrong tenant | Define and assert the expected secure result. |
| 5 | wrong role | Define and assert the expected secure result. |
| 6 | protected property | Define and assert the expected secure result. |
| 7 | privileged function | Define and assert the expected secure result. |
| 8 | malformed input | Define and assert the expected secure result. |
| 9 | oversized input | Define and assert the expected secure result. |
| 10 | maximum page | Define and assert the expected secure result. |
| 11 | excessive page | Define and assert the expected secure result. |
| 12 | repeated workflow | Define and assert the expected secure result. |
| 13 | expired credential | Define and assert the expected secure result. |
| 14 | invalid signature | Define and assert the expected secure result. |
| 15 | wrong audience | Define and assert the expected secure result. |
| 16 | dependency timeout | Define and assert the expected secure result. |
| 17 | malformed dependency response | Define and assert the expected secure result. |
| 18 | forbidden destination | Define and assert the expected secure result. |
| 19 | deprecated endpoint | Define and assert the expected secure result. |
| 20 | unexpected API version | Define and assert the expected secure result. |
| # | Test | Expected |
| --- | --- | --- |
| 1 | owner context | allow when policy permits |
| 2 | other user context | deny unless an explicit policy permits |
| 3 | other tenant context | deny unless an explicit policy permits |
| 4 | administrator context | deny unless an explicit policy permits |
| 5 | ordinary role context | deny unless an explicit policy permits |
| 6 | anonymous context | deny unless an explicit policy permits |

### 💻 Defensive pattern
```python
principal = authenticate(request)
validate_schema(request)
authorize_function(principal, request.operation)
resource = load_resource(request.id)
authorize_object(principal, resource)
authorize_properties(principal, request.body)
enforce_limits(request)
result = execute(principal, resource)
return serialize_allowed(principal, result)
```

### ❌ Dangerous pattern
```python
resource = database.get(request.id)
return resource
```
This pattern demonstrates why an identifier cannot be treated as proof of permission.

### 🔍 Review questions
- Who is calling?
- What exact object is involved?
- Which tenant owns it?
- Which function is being called?
- Which fields are readable?
- Which fields are writable?
- What is the resource budget?
- What happens on timeout?
- What happens when a dependency lies or fails?
- What test proves the boundary?
- What telemetry detects repeated violations?
- Who owns this endpoint?
- When is it retired?

---
## 🛡️ API2:2023 — Broken Authentication

> **Core rule:** Verify credentials, tokens, signatures, issuer, audience, expiration, lifecycle, and recovery controls.

### 🔎 What the risk means
This category concerns the identity boundary of an API.
The client is not a trusted authority for security decisions.
The server must establish trusted security context before making the decision.
The same technical flaw can have different business impact depending on the affected data and operation.

### 🧩 Common attack surface
- path identifiers: verify the identity policy is enforced when this input reaches the operation (1).
- query parameters: verify the identity policy is enforced when this input reaches the operation (2).
- JSON fields: verify the identity policy is enforced when this input reaches the operation (3).
- HTTP headers: verify the identity policy is enforced when this input reaches the operation (4).
- cookies: verify the identity policy is enforced when this input reaches the operation (5).
- multipart data: verify the identity policy is enforced when this input reaches the operation (6).
- GraphQL arguments: verify the identity policy is enforced when this input reaches the operation (7).
- gRPC metadata: verify the identity policy is enforced when this input reaches the operation (8).
- webhooks: verify the identity policy is enforced when this input reaches the operation (9).
- bulk endpoints: verify the identity policy is enforced when this input reaches the operation (10).
- exports: verify the identity policy is enforced when this input reaches the operation (11).
- search: verify the identity policy is enforced when this input reaches the operation (12).
- background jobs: verify the identity policy is enforced when this input reaches the operation (13).
- scheduled jobs: verify the identity policy is enforced when this input reaches the operation (14).
- mobile clients: verify the identity policy is enforced when this input reaches the operation (15).

### 🧱 Root causes
- implicit trust
- missing server-side policy
- overly broad permissions
- inconsistent middleware
- copy-pasted authorization
- missing tenant context
- unsafe defaults
- ambiguous contracts
- unbounded work
- stale configuration
- missing inventory
- weak dependency boundaries

### 🛠️ Defensive controls
- authenticate before privileged work
- authorize every sensitive operation
- apply least privilege
- deny by default
- bind tenant context to trusted identity
- use explicit object policies
- use explicit property allowlists
- protect privileged functions
- bound expensive work
- use timeouts
- validate external data
- restrict outbound destinations when applicable
- maintain API inventory
- emit security telemetry
- write regression tests

### 🧪 Security tests
| 1 | anonymous request | Define and assert the expected secure result. |
| 2 | valid request | Define and assert the expected secure result. |
| 3 | wrong user | Define and assert the expected secure result. |
| 4 | wrong tenant | Define and assert the expected secure result. |
| 5 | wrong role | Define and assert the expected secure result. |
| 6 | protected property | Define and assert the expected secure result. |
| 7 | privileged function | Define and assert the expected secure result. |
| 8 | malformed input | Define and assert the expected secure result. |
| 9 | oversized input | Define and assert the expected secure result. |
| 10 | maximum page | Define and assert the expected secure result. |
| 11 | excessive page | Define and assert the expected secure result. |
| 12 | repeated workflow | Define and assert the expected secure result. |
| 13 | expired credential | Define and assert the expected secure result. |
| 14 | invalid signature | Define and assert the expected secure result. |
| 15 | wrong audience | Define and assert the expected secure result. |
| 16 | dependency timeout | Define and assert the expected secure result. |
| 17 | malformed dependency response | Define and assert the expected secure result. |
| 18 | forbidden destination | Define and assert the expected secure result. |
| 19 | deprecated endpoint | Define and assert the expected secure result. |
| 20 | unexpected API version | Define and assert the expected secure result. |
| # | Test | Expected |
| --- | --- | --- |
| 1 | owner context | allow when policy permits |
| 2 | other user context | deny unless an explicit policy permits |
| 3 | other tenant context | deny unless an explicit policy permits |
| 4 | administrator context | deny unless an explicit policy permits |
| 5 | ordinary role context | deny unless an explicit policy permits |
| 6 | anonymous context | deny unless an explicit policy permits |

### 💻 Defensive pattern
```python
principal = authenticate(request)
validate_schema(request)
authorize_function(principal, request.operation)
resource = load_resource(request.id)
authorize_object(principal, resource)
authorize_properties(principal, request.body)
enforce_limits(request)
result = execute(principal, resource)
return serialize_allowed(principal, result)
```

### ❌ Dangerous pattern
```python
resource = database.get(request.id)
return resource
```
This pattern demonstrates why an identifier cannot be treated as proof of permission.

### 🔍 Review questions
- Who is calling?
- What exact object is involved?
- Which tenant owns it?
- Which function is being called?
- Which fields are readable?
- Which fields are writable?
- What is the resource budget?
- What happens on timeout?
- What happens when a dependency lies or fails?
- What test proves the boundary?
- What telemetry detects repeated violations?
- Who owns this endpoint?
- When is it retired?

---
## 🛡️ API3:2023 — Broken Object Property Level Authorization

> **Core rule:** Define readable and writable properties explicitly and protect privileged fields.

### 🔎 What the risk means
This category concerns the property boundary of an API.
The client is not a trusted authority for security decisions.
The server must establish trusted security context before making the decision.
The same technical flaw can have different business impact depending on the affected data and operation.

### 🧩 Common attack surface
- path identifiers: verify the property policy is enforced when this input reaches the operation (1).
- query parameters: verify the property policy is enforced when this input reaches the operation (2).
- JSON fields: verify the property policy is enforced when this input reaches the operation (3).
- HTTP headers: verify the property policy is enforced when this input reaches the operation (4).
- cookies: verify the property policy is enforced when this input reaches the operation (5).
- multipart data: verify the property policy is enforced when this input reaches the operation (6).
- GraphQL arguments: verify the property policy is enforced when this input reaches the operation (7).
- gRPC metadata: verify the property policy is enforced when this input reaches the operation (8).
- webhooks: verify the property policy is enforced when this input reaches the operation (9).
- bulk endpoints: verify the property policy is enforced when this input reaches the operation (10).
- exports: verify the property policy is enforced when this input reaches the operation (11).
- search: verify the property policy is enforced when this input reaches the operation (12).
- background jobs: verify the property policy is enforced when this input reaches the operation (13).
- scheduled jobs: verify the property policy is enforced when this input reaches the operation (14).
- mobile clients: verify the property policy is enforced when this input reaches the operation (15).

### 🧱 Root causes
- implicit trust
- missing server-side policy
- overly broad permissions
- inconsistent middleware
- copy-pasted authorization
- missing tenant context
- unsafe defaults
- ambiguous contracts
- unbounded work
- stale configuration
- missing inventory
- weak dependency boundaries

### 🛠️ Defensive controls
- authenticate before privileged work
- authorize every sensitive operation
- apply least privilege
- deny by default
- bind tenant context to trusted identity
- use explicit object policies
- use explicit property allowlists
- protect privileged functions
- bound expensive work
- use timeouts
- validate external data
- restrict outbound destinations when applicable
- maintain API inventory
- emit security telemetry
- write regression tests

### 🧪 Security tests
| 1 | anonymous request | Define and assert the expected secure result. |
| 2 | valid request | Define and assert the expected secure result. |
| 3 | wrong user | Define and assert the expected secure result. |
| 4 | wrong tenant | Define and assert the expected secure result. |
| 5 | wrong role | Define and assert the expected secure result. |
| 6 | protected property | Define and assert the expected secure result. |
| 7 | privileged function | Define and assert the expected secure result. |
| 8 | malformed input | Define and assert the expected secure result. |
| 9 | oversized input | Define and assert the expected secure result. |
| 10 | maximum page | Define and assert the expected secure result. |
| 11 | excessive page | Define and assert the expected secure result. |
| 12 | repeated workflow | Define and assert the expected secure result. |
| 13 | expired credential | Define and assert the expected secure result. |
| 14 | invalid signature | Define and assert the expected secure result. |
| 15 | wrong audience | Define and assert the expected secure result. |
| 16 | dependency timeout | Define and assert the expected secure result. |
| 17 | malformed dependency response | Define and assert the expected secure result. |
| 18 | forbidden destination | Define and assert the expected secure result. |
| 19 | deprecated endpoint | Define and assert the expected secure result. |
| 20 | unexpected API version | Define and assert the expected secure result. |
| # | Test | Expected |
| --- | --- | --- |
| 1 | owner context | allow when policy permits |
| 2 | other user context | deny unless an explicit policy permits |
| 3 | other tenant context | deny unless an explicit policy permits |
| 4 | administrator context | deny unless an explicit policy permits |
| 5 | ordinary role context | deny unless an explicit policy permits |
| 6 | anonymous context | deny unless an explicit policy permits |

### 💻 Defensive pattern
```python
principal = authenticate(request)
validate_schema(request)
authorize_function(principal, request.operation)
resource = load_resource(request.id)
authorize_object(principal, resource)
authorize_properties(principal, request.body)
enforce_limits(request)
result = execute(principal, resource)
return serialize_allowed(principal, result)
```

### ❌ Dangerous pattern
```python
resource = database.get(request.id)
return resource
```
This pattern demonstrates why an identifier cannot be treated as proof of permission.

### 🔍 Review questions
- Who is calling?
- What exact object is involved?
- Which tenant owns it?
- Which function is being called?
- Which fields are readable?
- Which fields are writable?
- What is the resource budget?
- What happens on timeout?
- What happens when a dependency lies or fails?
- What test proves the boundary?
- What telemetry detects repeated violations?
- Who owns this endpoint?
- When is it retired?

---
## 🛡️ API4:2023 — Unrestricted Resource Consumption

> **Core rule:** Bound request size, concurrency, pagination, compute, retries, storage, and downstream cost.

### 🔎 What the risk means
This category concerns the resource boundary of an API.
The client is not a trusted authority for security decisions.
The server must establish trusted security context before making the decision.
The same technical flaw can have different business impact depending on the affected data and operation.

### 🧩 Common attack surface
- path identifiers: verify the resource policy is enforced when this input reaches the operation (1).
- query parameters: verify the resource policy is enforced when this input reaches the operation (2).
- JSON fields: verify the resource policy is enforced when this input reaches the operation (3).
- HTTP headers: verify the resource policy is enforced when this input reaches the operation (4).
- cookies: verify the resource policy is enforced when this input reaches the operation (5).
- multipart data: verify the resource policy is enforced when this input reaches the operation (6).
- GraphQL arguments: verify the resource policy is enforced when this input reaches the operation (7).
- gRPC metadata: verify the resource policy is enforced when this input reaches the operation (8).
- webhooks: verify the resource policy is enforced when this input reaches the operation (9).
- bulk endpoints: verify the resource policy is enforced when this input reaches the operation (10).
- exports: verify the resource policy is enforced when this input reaches the operation (11).
- search: verify the resource policy is enforced when this input reaches the operation (12).
- background jobs: verify the resource policy is enforced when this input reaches the operation (13).
- scheduled jobs: verify the resource policy is enforced when this input reaches the operation (14).
- mobile clients: verify the resource policy is enforced when this input reaches the operation (15).

### 🧱 Root causes
- implicit trust
- missing server-side policy
- overly broad permissions
- inconsistent middleware
- copy-pasted authorization
- missing tenant context
- unsafe defaults
- ambiguous contracts
- unbounded work
- stale configuration
- missing inventory
- weak dependency boundaries

### 🛠️ Defensive controls
- authenticate before privileged work
- authorize every sensitive operation
- apply least privilege
- deny by default
- bind tenant context to trusted identity
- use explicit object policies
- use explicit property allowlists
- protect privileged functions
- bound expensive work
- use timeouts
- validate external data
- restrict outbound destinations when applicable
- maintain API inventory
- emit security telemetry
- write regression tests

### 🧪 Security tests
| 1 | anonymous request | Define and assert the expected secure result. |
| 2 | valid request | Define and assert the expected secure result. |
| 3 | wrong user | Define and assert the expected secure result. |
| 4 | wrong tenant | Define and assert the expected secure result. |
| 5 | wrong role | Define and assert the expected secure result. |
| 6 | protected property | Define and assert the expected secure result. |
| 7 | privileged function | Define and assert the expected secure result. |
| 8 | malformed input | Define and assert the expected secure result. |
| 9 | oversized input | Define and assert the expected secure result. |
| 10 | maximum page | Define and assert the expected secure result. |
| 11 | excessive page | Define and assert the expected secure result. |
| 12 | repeated workflow | Define and assert the expected secure result. |
| 13 | expired credential | Define and assert the expected secure result. |
| 14 | invalid signature | Define and assert the expected secure result. |
| 15 | wrong audience | Define and assert the expected secure result. |
| 16 | dependency timeout | Define and assert the expected secure result. |
| 17 | malformed dependency response | Define and assert the expected secure result. |
| 18 | forbidden destination | Define and assert the expected secure result. |
| 19 | deprecated endpoint | Define and assert the expected secure result. |
| 20 | unexpected API version | Define and assert the expected secure result. |
| # | Test | Expected |
| --- | --- | --- |
| 1 | owner context | allow when policy permits |
| 2 | other user context | deny unless an explicit policy permits |
| 3 | other tenant context | deny unless an explicit policy permits |
| 4 | administrator context | deny unless an explicit policy permits |
| 5 | ordinary role context | deny unless an explicit policy permits |
| 6 | anonymous context | deny unless an explicit policy permits |

### 💻 Defensive pattern
```python
principal = authenticate(request)
validate_schema(request)
authorize_function(principal, request.operation)
resource = load_resource(request.id)
authorize_object(principal, resource)
authorize_properties(principal, request.body)
enforce_limits(request)
result = execute(principal, resource)
return serialize_allowed(principal, result)
```

### ❌ Dangerous pattern
```python
resource = database.get(request.id)
return resource
```
This pattern demonstrates why an identifier cannot be treated as proof of permission.

### 🔍 Review questions
- Who is calling?
- What exact object is involved?
- Which tenant owns it?
- Which function is being called?
- Which fields are readable?
- Which fields are writable?
- What is the resource budget?
- What happens on timeout?
- What happens when a dependency lies or fails?
- What test proves the boundary?
- What telemetry detects repeated violations?
- Who owns this endpoint?
- When is it retired?

---
## 🛡️ API5:2023 — Broken Function Level Authorization

> **Core rule:** Enforce server-side permissions for every privileged operation and administrative function.

### 🔎 What the risk means
This category concerns the function boundary of an API.
The client is not a trusted authority for security decisions.
The server must establish trusted security context before making the decision.
The same technical flaw can have different business impact depending on the affected data and operation.

### 🧩 Common attack surface
- path identifiers: verify the function policy is enforced when this input reaches the operation (1).
- query parameters: verify the function policy is enforced when this input reaches the operation (2).
- JSON fields: verify the function policy is enforced when this input reaches the operation (3).
- HTTP headers: verify the function policy is enforced when this input reaches the operation (4).
- cookies: verify the function policy is enforced when this input reaches the operation (5).
- multipart data: verify the function policy is enforced when this input reaches the operation (6).
- GraphQL arguments: verify the function policy is enforced when this input reaches the operation (7).
- gRPC metadata: verify the function policy is enforced when this input reaches the operation (8).
- webhooks: verify the function policy is enforced when this input reaches the operation (9).
- bulk endpoints: verify the function policy is enforced when this input reaches the operation (10).
- exports: verify the function policy is enforced when this input reaches the operation (11).
- search: verify the function policy is enforced when this input reaches the operation (12).
- background jobs: verify the function policy is enforced when this input reaches the operation (13).
- scheduled jobs: verify the function policy is enforced when this input reaches the operation (14).
- mobile clients: verify the function policy is enforced when this input reaches the operation (15).

### 🧱 Root causes
- implicit trust
- missing server-side policy
- overly broad permissions
- inconsistent middleware
- copy-pasted authorization
- missing tenant context
- unsafe defaults
- ambiguous contracts
- unbounded work
- stale configuration
- missing inventory
- weak dependency boundaries

### 🛠️ Defensive controls
- authenticate before privileged work
- authorize every sensitive operation
- apply least privilege
- deny by default
- bind tenant context to trusted identity
- use explicit object policies
- use explicit property allowlists
- protect privileged functions
- bound expensive work
- use timeouts
- validate external data
- restrict outbound destinations when applicable
- maintain API inventory
- emit security telemetry
- write regression tests

### 🧪 Security tests
| 1 | anonymous request | Define and assert the expected secure result. |
| 2 | valid request | Define and assert the expected secure result. |
| 3 | wrong user | Define and assert the expected secure result. |
| 4 | wrong tenant | Define and assert the expected secure result. |
| 5 | wrong role | Define and assert the expected secure result. |
| 6 | protected property | Define and assert the expected secure result. |
| 7 | privileged function | Define and assert the expected secure result. |
| 8 | malformed input | Define and assert the expected secure result. |
| 9 | oversized input | Define and assert the expected secure result. |
| 10 | maximum page | Define and assert the expected secure result. |
| 11 | excessive page | Define and assert the expected secure result. |
| 12 | repeated workflow | Define and assert the expected secure result. |
| 13 | expired credential | Define and assert the expected secure result. |
| 14 | invalid signature | Define and assert the expected secure result. |
| 15 | wrong audience | Define and assert the expected secure result. |
| 16 | dependency timeout | Define and assert the expected secure result. |
| 17 | malformed dependency response | Define and assert the expected secure result. |
| 18 | forbidden destination | Define and assert the expected secure result. |
| 19 | deprecated endpoint | Define and assert the expected secure result. |
| 20 | unexpected API version | Define and assert the expected secure result. |
| # | Test | Expected |
| --- | --- | --- |
| 1 | owner context | allow when policy permits |
| 2 | other user context | deny unless an explicit policy permits |
| 3 | other tenant context | deny unless an explicit policy permits |
| 4 | administrator context | deny unless an explicit policy permits |
| 5 | ordinary role context | deny unless an explicit policy permits |
| 6 | anonymous context | deny unless an explicit policy permits |

### 💻 Defensive pattern
```python
principal = authenticate(request)
validate_schema(request)
authorize_function(principal, request.operation)
resource = load_resource(request.id)
authorize_object(principal, resource)
authorize_properties(principal, request.body)
enforce_limits(request)
result = execute(principal, resource)
return serialize_allowed(principal, result)
```

### ❌ Dangerous pattern
```python
resource = database.get(request.id)
return resource
```
This pattern demonstrates why an identifier cannot be treated as proof of permission.

### 🔍 Review questions
- Who is calling?
- What exact object is involved?
- Which tenant owns it?
- Which function is being called?
- Which fields are readable?
- Which fields are writable?
- What is the resource budget?
- What happens on timeout?
- What happens when a dependency lies or fails?
- What test proves the boundary?
- What telemetry detects repeated violations?
- Who owns this endpoint?
- When is it retired?

---
## 🛡️ API6:2023 — Unrestricted Access to Sensitive Business Flows

> **Core rule:** Identify sensitive workflows and control automation, velocity, state, quotas, and abuse.

### 🔎 What the risk means
This category concerns the workflow boundary of an API.
The client is not a trusted authority for security decisions.
The server must establish trusted security context before making the decision.
The same technical flaw can have different business impact depending on the affected data and operation.

### 🧩 Common attack surface
- path identifiers: verify the workflow policy is enforced when this input reaches the operation (1).
- query parameters: verify the workflow policy is enforced when this input reaches the operation (2).
- JSON fields: verify the workflow policy is enforced when this input reaches the operation (3).
- HTTP headers: verify the workflow policy is enforced when this input reaches the operation (4).
- cookies: verify the workflow policy is enforced when this input reaches the operation (5).
- multipart data: verify the workflow policy is enforced when this input reaches the operation (6).
- GraphQL arguments: verify the workflow policy is enforced when this input reaches the operation (7).
- gRPC metadata: verify the workflow policy is enforced when this input reaches the operation (8).
- webhooks: verify the workflow policy is enforced when this input reaches the operation (9).
- bulk endpoints: verify the workflow policy is enforced when this input reaches the operation (10).
- exports: verify the workflow policy is enforced when this input reaches the operation (11).
- search: verify the workflow policy is enforced when this input reaches the operation (12).
- background jobs: verify the workflow policy is enforced when this input reaches the operation (13).
- scheduled jobs: verify the workflow policy is enforced when this input reaches the operation (14).
- mobile clients: verify the workflow policy is enforced when this input reaches the operation (15).

### 🧱 Root causes
- implicit trust
- missing server-side policy
- overly broad permissions
- inconsistent middleware
- copy-pasted authorization
- missing tenant context
- unsafe defaults
- ambiguous contracts
- unbounded work
- stale configuration
- missing inventory
- weak dependency boundaries

### 🛠️ Defensive controls
- authenticate before privileged work
- authorize every sensitive operation
- apply least privilege
- deny by default
- bind tenant context to trusted identity
- use explicit object policies
- use explicit property allowlists
- protect privileged functions
- bound expensive work
- use timeouts
- validate external data
- restrict outbound destinations when applicable
- maintain API inventory
- emit security telemetry
- write regression tests

### 🧪 Security tests
| 1 | anonymous request | Define and assert the expected secure result. |
| 2 | valid request | Define and assert the expected secure result. |
| 3 | wrong user | Define and assert the expected secure result. |
| 4 | wrong tenant | Define and assert the expected secure result. |
| 5 | wrong role | Define and assert the expected secure result. |
| 6 | protected property | Define and assert the expected secure result. |
| 7 | privileged function | Define and assert the expected secure result. |
| 8 | malformed input | Define and assert the expected secure result. |
| 9 | oversized input | Define and assert the expected secure result. |
| 10 | maximum page | Define and assert the expected secure result. |
| 11 | excessive page | Define and assert the expected secure result. |
| 12 | repeated workflow | Define and assert the expected secure result. |
| 13 | expired credential | Define and assert the expected secure result. |
| 14 | invalid signature | Define and assert the expected secure result. |
| 15 | wrong audience | Define and assert the expected secure result. |
| 16 | dependency timeout | Define and assert the expected secure result. |
| 17 | malformed dependency response | Define and assert the expected secure result. |
| 18 | forbidden destination | Define and assert the expected secure result. |
| 19 | deprecated endpoint | Define and assert the expected secure result. |
| 20 | unexpected API version | Define and assert the expected secure result. |
| # | Test | Expected |
| --- | --- | --- |
| 1 | owner context | allow when policy permits |
| 2 | other user context | deny unless an explicit policy permits |
| 3 | other tenant context | deny unless an explicit policy permits |
| 4 | administrator context | deny unless an explicit policy permits |
| 5 | ordinary role context | deny unless an explicit policy permits |
| 6 | anonymous context | deny unless an explicit policy permits |

### 💻 Defensive pattern
```python
principal = authenticate(request)
validate_schema(request)
authorize_function(principal, request.operation)
resource = load_resource(request.id)
authorize_object(principal, resource)
authorize_properties(principal, request.body)
enforce_limits(request)
result = execute(principal, resource)
return serialize_allowed(principal, result)
```

### ❌ Dangerous pattern
```python
resource = database.get(request.id)
return resource
```
This pattern demonstrates why an identifier cannot be treated as proof of permission.

### 🔍 Review questions
- Who is calling?
- What exact object is involved?
- Which tenant owns it?
- Which function is being called?
- Which fields are readable?
- Which fields are writable?
- What is the resource budget?
- What happens on timeout?
- What happens when a dependency lies or fails?
- What test proves the boundary?
- What telemetry detects repeated violations?
- Who owns this endpoint?
- When is it retired?

---
## 🛡️ API7:2023 — Server Side Request Forgery

> **Core rule:** Constrain server-side destinations, redirects, DNS behavior, egress, timeouts, and response size.

### 🔎 What the risk means
This category concerns the network boundary of an API.
The client is not a trusted authority for security decisions.
The server must establish trusted security context before making the decision.
The same technical flaw can have different business impact depending on the affected data and operation.

### 🧩 Common attack surface
- path identifiers: verify the network policy is enforced when this input reaches the operation (1).
- query parameters: verify the network policy is enforced when this input reaches the operation (2).
- JSON fields: verify the network policy is enforced when this input reaches the operation (3).
- HTTP headers: verify the network policy is enforced when this input reaches the operation (4).
- cookies: verify the network policy is enforced when this input reaches the operation (5).
- multipart data: verify the network policy is enforced when this input reaches the operation (6).
- GraphQL arguments: verify the network policy is enforced when this input reaches the operation (7).
- gRPC metadata: verify the network policy is enforced when this input reaches the operation (8).
- webhooks: verify the network policy is enforced when this input reaches the operation (9).
- bulk endpoints: verify the network policy is enforced when this input reaches the operation (10).
- exports: verify the network policy is enforced when this input reaches the operation (11).
- search: verify the network policy is enforced when this input reaches the operation (12).
- background jobs: verify the network policy is enforced when this input reaches the operation (13).
- scheduled jobs: verify the network policy is enforced when this input reaches the operation (14).
- mobile clients: verify the network policy is enforced when this input reaches the operation (15).

### 🧱 Root causes
- implicit trust
- missing server-side policy
- overly broad permissions
- inconsistent middleware
- copy-pasted authorization
- missing tenant context
- unsafe defaults
- ambiguous contracts
- unbounded work
- stale configuration
- missing inventory
- weak dependency boundaries

### 🛠️ Defensive controls
- authenticate before privileged work
- authorize every sensitive operation
- apply least privilege
- deny by default
- bind tenant context to trusted identity
- use explicit object policies
- use explicit property allowlists
- protect privileged functions
- bound expensive work
- use timeouts
- validate external data
- restrict outbound destinations when applicable
- maintain API inventory
- emit security telemetry
- write regression tests

### 🧪 Security tests
| 1 | anonymous request | Define and assert the expected secure result. |
| 2 | valid request | Define and assert the expected secure result. |
| 3 | wrong user | Define and assert the expected secure result. |
| 4 | wrong tenant | Define and assert the expected secure result. |
| 5 | wrong role | Define and assert the expected secure result. |
| 6 | protected property | Define and assert the expected secure result. |
| 7 | privileged function | Define and assert the expected secure result. |
| 8 | malformed input | Define and assert the expected secure result. |
| 9 | oversized input | Define and assert the expected secure result. |
| 10 | maximum page | Define and assert the expected secure result. |
| 11 | excessive page | Define and assert the expected secure result. |
| 12 | repeated workflow | Define and assert the expected secure result. |
| 13 | expired credential | Define and assert the expected secure result. |
| 14 | invalid signature | Define and assert the expected secure result. |
| 15 | wrong audience | Define and assert the expected secure result. |
| 16 | dependency timeout | Define and assert the expected secure result. |
| 17 | malformed dependency response | Define and assert the expected secure result. |
| 18 | forbidden destination | Define and assert the expected secure result. |
| 19 | deprecated endpoint | Define and assert the expected secure result. |
| 20 | unexpected API version | Define and assert the expected secure result. |
| # | Test | Expected |
| --- | --- | --- |
| 1 | owner context | allow when policy permits |
| 2 | other user context | deny unless an explicit policy permits |
| 3 | other tenant context | deny unless an explicit policy permits |
| 4 | administrator context | deny unless an explicit policy permits |
| 5 | ordinary role context | deny unless an explicit policy permits |
| 6 | anonymous context | deny unless an explicit policy permits |

### 💻 Defensive pattern
```python
principal = authenticate(request)
validate_schema(request)
authorize_function(principal, request.operation)
resource = load_resource(request.id)
authorize_object(principal, resource)
authorize_properties(principal, request.body)
enforce_limits(request)
result = execute(principal, resource)
return serialize_allowed(principal, result)
```

### ❌ Dangerous pattern
```python
resource = database.get(request.id)
return resource
```
This pattern demonstrates why an identifier cannot be treated as proof of permission.

### 🔍 Review questions
- Who is calling?
- What exact object is involved?
- Which tenant owns it?
- Which function is being called?
- Which fields are readable?
- Which fields are writable?
- What is the resource budget?
- What happens on timeout?
- What happens when a dependency lies or fails?
- What test proves the boundary?
- What telemetry detects repeated violations?
- Who owns this endpoint?
- When is it retired?

---
## 🛡️ API8:2023 — Security Misconfiguration

> **Core rule:** Harden defaults, errors, methods, exposure, credentials, CORS, TLS, and diagnostics.

### 🔎 What the risk means
This category concerns the configuration boundary of an API.
The client is not a trusted authority for security decisions.
The server must establish trusted security context before making the decision.
The same technical flaw can have different business impact depending on the affected data and operation.

### 🧩 Common attack surface
- path identifiers: verify the configuration policy is enforced when this input reaches the operation (1).
- query parameters: verify the configuration policy is enforced when this input reaches the operation (2).
- JSON fields: verify the configuration policy is enforced when this input reaches the operation (3).
- HTTP headers: verify the configuration policy is enforced when this input reaches the operation (4).
- cookies: verify the configuration policy is enforced when this input reaches the operation (5).
- multipart data: verify the configuration policy is enforced when this input reaches the operation (6).
- GraphQL arguments: verify the configuration policy is enforced when this input reaches the operation (7).
- gRPC metadata: verify the configuration policy is enforced when this input reaches the operation (8).
- webhooks: verify the configuration policy is enforced when this input reaches the operation (9).
- bulk endpoints: verify the configuration policy is enforced when this input reaches the operation (10).
- exports: verify the configuration policy is enforced when this input reaches the operation (11).
- search: verify the configuration policy is enforced when this input reaches the operation (12).
- background jobs: verify the configuration policy is enforced when this input reaches the operation (13).
- scheduled jobs: verify the configuration policy is enforced when this input reaches the operation (14).
- mobile clients: verify the configuration policy is enforced when this input reaches the operation (15).

### 🧱 Root causes
- implicit trust
- missing server-side policy
- overly broad permissions
- inconsistent middleware
- copy-pasted authorization
- missing tenant context
- unsafe defaults
- ambiguous contracts
- unbounded work
- stale configuration
- missing inventory
- weak dependency boundaries

### 🛠️ Defensive controls
- authenticate before privileged work
- authorize every sensitive operation
- apply least privilege
- deny by default
- bind tenant context to trusted identity
- use explicit object policies
- use explicit property allowlists
- protect privileged functions
- bound expensive work
- use timeouts
- validate external data
- restrict outbound destinations when applicable
- maintain API inventory
- emit security telemetry
- write regression tests

### 🧪 Security tests
| 1 | anonymous request | Define and assert the expected secure result. |
| 2 | valid request | Define and assert the expected secure result. |
| 3 | wrong user | Define and assert the expected secure result. |
| 4 | wrong tenant | Define and assert the expected secure result. |
| 5 | wrong role | Define and assert the expected secure result. |
| 6 | protected property | Define and assert the expected secure result. |
| 7 | privileged function | Define and assert the expected secure result. |
| 8 | malformed input | Define and assert the expected secure result. |
| 9 | oversized input | Define and assert the expected secure result. |
| 10 | maximum page | Define and assert the expected secure result. |
| 11 | excessive page | Define and assert the expected secure result. |
| 12 | repeated workflow | Define and assert the expected secure result. |
| 13 | expired credential | Define and assert the expected secure result. |
| 14 | invalid signature | Define and assert the expected secure result. |
| 15 | wrong audience | Define and assert the expected secure result. |
| 16 | dependency timeout | Define and assert the expected secure result. |
| 17 | malformed dependency response | Define and assert the expected secure result. |
| 18 | forbidden destination | Define and assert the expected secure result. |
| 19 | deprecated endpoint | Define and assert the expected secure result. |
| 20 | unexpected API version | Define and assert the expected secure result. |
| # | Test | Expected |
| --- | --- | --- |
| 1 | owner context | allow when policy permits |
| 2 | other user context | deny unless an explicit policy permits |
| 3 | other tenant context | deny unless an explicit policy permits |
| 4 | administrator context | deny unless an explicit policy permits |
| 5 | ordinary role context | deny unless an explicit policy permits |
| 6 | anonymous context | deny unless an explicit policy permits |

### 💻 Defensive pattern
```python
principal = authenticate(request)
validate_schema(request)
authorize_function(principal, request.operation)
resource = load_resource(request.id)
authorize_object(principal, resource)
authorize_properties(principal, request.body)
enforce_limits(request)
result = execute(principal, resource)
return serialize_allowed(principal, result)
```

### ❌ Dangerous pattern
```python
resource = database.get(request.id)
return resource
```
This pattern demonstrates why an identifier cannot be treated as proof of permission.

### 🔍 Review questions
- Who is calling?
- What exact object is involved?
- Which tenant owns it?
- Which function is being called?
- Which fields are readable?
- Which fields are writable?
- What is the resource budget?
- What happens on timeout?
- What happens when a dependency lies or fails?
- What test proves the boundary?
- What telemetry detects repeated violations?
- Who owns this endpoint?
- When is it retired?

---
## 🛡️ API9:2023 — Improper Inventory Management

> **Core rule:** Maintain an accurate inventory of hosts, versions, routes, environments, owners, and lifecycle.

### 🔎 What the risk means
This category concerns the inventory boundary of an API.
The client is not a trusted authority for security decisions.
The server must establish trusted security context before making the decision.
The same technical flaw can have different business impact depending on the affected data and operation.

### 🧩 Common attack surface
- path identifiers: verify the inventory policy is enforced when this input reaches the operation (1).
- query parameters: verify the inventory policy is enforced when this input reaches the operation (2).
- JSON fields: verify the inventory policy is enforced when this input reaches the operation (3).
- HTTP headers: verify the inventory policy is enforced when this input reaches the operation (4).
- cookies: verify the inventory policy is enforced when this input reaches the operation (5).
- multipart data: verify the inventory policy is enforced when this input reaches the operation (6).
- GraphQL arguments: verify the inventory policy is enforced when this input reaches the operation (7).
- gRPC metadata: verify the inventory policy is enforced when this input reaches the operation (8).
- webhooks: verify the inventory policy is enforced when this input reaches the operation (9).
- bulk endpoints: verify the inventory policy is enforced when this input reaches the operation (10).
- exports: verify the inventory policy is enforced when this input reaches the operation (11).
- search: verify the inventory policy is enforced when this input reaches the operation (12).
- background jobs: verify the inventory policy is enforced when this input reaches the operation (13).
- scheduled jobs: verify the inventory policy is enforced when this input reaches the operation (14).
- mobile clients: verify the inventory policy is enforced when this input reaches the operation (15).

### 🧱 Root causes
- implicit trust
- missing server-side policy
- overly broad permissions
- inconsistent middleware
- copy-pasted authorization
- missing tenant context
- unsafe defaults
- ambiguous contracts
- unbounded work
- stale configuration
- missing inventory
- weak dependency boundaries

### 🛠️ Defensive controls
- authenticate before privileged work
- authorize every sensitive operation
- apply least privilege
- deny by default
- bind tenant context to trusted identity
- use explicit object policies
- use explicit property allowlists
- protect privileged functions
- bound expensive work
- use timeouts
- validate external data
- restrict outbound destinations when applicable
- maintain API inventory
- emit security telemetry
- write regression tests

### 🧪 Security tests
| 1 | anonymous request | Define and assert the expected secure result. |
| 2 | valid request | Define and assert the expected secure result. |
| 3 | wrong user | Define and assert the expected secure result. |
| 4 | wrong tenant | Define and assert the expected secure result. |
| 5 | wrong role | Define and assert the expected secure result. |
| 6 | protected property | Define and assert the expected secure result. |
| 7 | privileged function | Define and assert the expected secure result. |
| 8 | malformed input | Define and assert the expected secure result. |
| 9 | oversized input | Define and assert the expected secure result. |
| 10 | maximum page | Define and assert the expected secure result. |
| 11 | excessive page | Define and assert the expected secure result. |
| 12 | repeated workflow | Define and assert the expected secure result. |
| 13 | expired credential | Define and assert the expected secure result. |
| 14 | invalid signature | Define and assert the expected secure result. |
| 15 | wrong audience | Define and assert the expected secure result. |
| 16 | dependency timeout | Define and assert the expected secure result. |
| 17 | malformed dependency response | Define and assert the expected secure result. |
| 18 | forbidden destination | Define and assert the expected secure result. |
| 19 | deprecated endpoint | Define and assert the expected secure result. |
| 20 | unexpected API version | Define and assert the expected secure result. |
| # | Test | Expected |
| --- | --- | --- |
| 1 | owner context | allow when policy permits |
| 2 | other user context | deny unless an explicit policy permits |
| 3 | other tenant context | deny unless an explicit policy permits |
| 4 | administrator context | deny unless an explicit policy permits |
| 5 | ordinary role context | deny unless an explicit policy permits |
| 6 | anonymous context | deny unless an explicit policy permits |

### 💻 Defensive pattern
```python
principal = authenticate(request)
validate_schema(request)
authorize_function(principal, request.operation)
resource = load_resource(request.id)
authorize_object(principal, resource)
authorize_properties(principal, request.body)
enforce_limits(request)
result = execute(principal, resource)
return serialize_allowed(principal, result)
```

### ❌ Dangerous pattern
```python
resource = database.get(request.id)
return resource
```
This pattern demonstrates why an identifier cannot be treated as proof of permission.

### 🔍 Review questions
- Who is calling?
- What exact object is involved?
- Which tenant owns it?
- Which function is being called?
- Which fields are readable?
- Which fields are writable?
- What is the resource budget?
- What happens on timeout?
- What happens when a dependency lies or fails?
- What test proves the boundary?
- What telemetry detects repeated violations?
- Who owns this endpoint?
- When is it retired?

---
## 🛡️ API10:2023 — Unsafe Consumption of APIs

> **Core rule:** Treat third-party responses as untrusted and validate transport, schema, redirects, retries, and resource use.

### 🔎 What the risk means
This category concerns the dependency boundary of an API.
The client is not a trusted authority for security decisions.
The server must establish trusted security context before making the decision.
The same technical flaw can have different business impact depending on the affected data and operation.

### 🧩 Common attack surface
- path identifiers: verify the dependency policy is enforced when this input reaches the operation (1).
- query parameters: verify the dependency policy is enforced when this input reaches the operation (2).
- JSON fields: verify the dependency policy is enforced when this input reaches the operation (3).
- HTTP headers: verify the dependency policy is enforced when this input reaches the operation (4).
- cookies: verify the dependency policy is enforced when this input reaches the operation (5).
- multipart data: verify the dependency policy is enforced when this input reaches the operation (6).
- GraphQL arguments: verify the dependency policy is enforced when this input reaches the operation (7).
- gRPC metadata: verify the dependency policy is enforced when this input reaches the operation (8).
- webhooks: verify the dependency policy is enforced when this input reaches the operation (9).
- bulk endpoints: verify the dependency policy is enforced when this input reaches the operation (10).
- exports: verify the dependency policy is enforced when this input reaches the operation (11).
- search: verify the dependency policy is enforced when this input reaches the operation (12).
- background jobs: verify the dependency policy is enforced when this input reaches the operation (13).
- scheduled jobs: verify the dependency policy is enforced when this input reaches the operation (14).
- mobile clients: verify the dependency policy is enforced when this input reaches the operation (15).

### 🧱 Root causes
- implicit trust
- missing server-side policy
- overly broad permissions
- inconsistent middleware
- copy-pasted authorization
- missing tenant context
- unsafe defaults
- ambiguous contracts
- unbounded work
- stale configuration
- missing inventory
- weak dependency boundaries

### 🛠️ Defensive controls
- authenticate before privileged work
- authorize every sensitive operation
- apply least privilege
- deny by default
- bind tenant context to trusted identity
- use explicit object policies
- use explicit property allowlists
- protect privileged functions
- bound expensive work
- use timeouts
- validate external data
- restrict outbound destinations when applicable
- maintain API inventory
- emit security telemetry
- write regression tests

### 🧪 Security tests
| 1 | anonymous request | Define and assert the expected secure result. |
| 2 | valid request | Define and assert the expected secure result. |
| 3 | wrong user | Define and assert the expected secure result. |
| 4 | wrong tenant | Define and assert the expected secure result. |
| 5 | wrong role | Define and assert the expected secure result. |
| 6 | protected property | Define and assert the expected secure result. |
| 7 | privileged function | Define and assert the expected secure result. |
| 8 | malformed input | Define and assert the expected secure result. |
| 9 | oversized input | Define and assert the expected secure result. |
| 10 | maximum page | Define and assert the expected secure result. |
| 11 | excessive page | Define and assert the expected secure result. |
| 12 | repeated workflow | Define and assert the expected secure result. |
| 13 | expired credential | Define and assert the expected secure result. |
| 14 | invalid signature | Define and assert the expected secure result. |
| 15 | wrong audience | Define and assert the expected secure result. |
| 16 | dependency timeout | Define and assert the expected secure result. |
| 17 | malformed dependency response | Define and assert the expected secure result. |
| 18 | forbidden destination | Define and assert the expected secure result. |
| 19 | deprecated endpoint | Define and assert the expected secure result. |
| 20 | unexpected API version | Define and assert the expected secure result. |
| # | Test | Expected |
| --- | --- | --- |
| 1 | owner context | allow when policy permits |
| 2 | other user context | deny unless an explicit policy permits |
| 3 | other tenant context | deny unless an explicit policy permits |
| 4 | administrator context | deny unless an explicit policy permits |
| 5 | ordinary role context | deny unless an explicit policy permits |
| 6 | anonymous context | deny unless an explicit policy permits |

### 💻 Defensive pattern
```python
principal = authenticate(request)
validate_schema(request)
authorize_function(principal, request.operation)
resource = load_resource(request.id)
authorize_object(principal, resource)
authorize_properties(principal, request.body)
enforce_limits(request)
result = execute(principal, resource)
return serialize_allowed(principal, result)
```

### ❌ Dangerous pattern
```python
resource = database.get(request.id)
return resource
```
This pattern demonstrates why an identifier cannot be treated as proof of permission.

### 🔍 Review questions
- Who is calling?
- What exact object is involved?
- Which tenant owns it?
- Which function is being called?
- Which fields are readable?
- Which fields are writable?
- What is the resource budget?
- What happens on timeout?
- What happens when a dependency lies or fails?
- What test proves the boundary?
- What telemetry detects repeated violations?
- Who owns this endpoint?
- When is it retired?

## 🔐 Authentication
- Authentication establishes identity or credential context.
- A valid token is not universal authorization.
- Verify signatures before trusting claims.
- Validate expected algorithms.
- Validate issuer and audience where required.
- Validate expiration and relevant time claims.
- Protect refresh tokens.
- Do not log bearer credentials.
- Use short-lived credentials where practical.
- Separate development and production credentials.
- Use workload identity where appropriate.
- OAuth 2.0 is an authorization framework.
- OpenID Connect adds an identity layer.
- Use PKCE where appropriate.
- Protect authorization codes and redirect URIs.

## 🧱 Authorization
- Authorization must be server-side.
- Object authorization checks the exact resource.
- Function authorization checks the exact operation.
- Property authorization checks the exact field.
- Tenant authorization checks the customer boundary.
- Bulk operations require the same authorization reasoning as single-object operations.
- Exports can cross boundaries even when ordinary reads are safe.
- Background jobs must preserve authorization context.
- Do not trust client-provided role fields.
- Do not rely on UUID unpredictability as a substitute for authorization.
- Negative tests are essential.

## 🧾 Validation and Schema Security
- Validate types.
- Validate lengths.
- Bound arrays.
- Bound nested structures.
- Use explicit response schemas.
- Use explicit writable-field allowlists.
- Do not blindly serialize database models.
- Validate URLs according to their eventual use.
- Treat third-party responses as untrusted.
- Schema validation does not replace authorization.
- Schema validation does not replace business rules.
- Output encoding remains context-dependent.

## ⏱️ Resource Controls
- Rate limiting is only one control.
- Consider request frequency.
- Consider concurrency.
- Consider body size.
- Consider response size.
- Consider database query cost.
- Consider fan-out.
- Consider retry amplification.
- Consider queue depth.
- Consider third-party cost.
- Use pagination limits.
- Use timeouts.
- Use bounded retries.
- Use backoff.
- Use quotas for costly workflows.
- Use idempotency where duplicate actions are harmful.
- Measure endpoint cost before selecting limits.

## 💼 Sensitive Business Flows
- Identify workflows that create financial, operational, reputational, or resource impact.
- Examples include registration, password recovery, coupon redemption, ticket booking, inventory purchase, comment creation, email sending, SMS sending, exports, and report generation.
- Authentication alone may not prevent automated abuse.
- Use velocity controls.
- Use account-level quotas.
- Use state-transition checks.
- Use idempotency keys where appropriate.
- Monitor unusual sequences.
- Use additional verification when justified by risk.
- Do not rely solely on CAPTCHA.

## 🌐 SSRF
- Treat user-controlled URLs as untrusted.
- Prefer destination allowlists when practical.
- Do not rely on simple string-prefix checks.
- Consider DNS rebinding.
- Consider IPv4 and IPv6 representations.
- Restrict private and loopback destinations when appropriate.
- Control redirects.
- Use network egress filtering.
- Use timeouts.
- Limit response sizes.
- Consider a dedicated fetch service.
- Monitor blocked destinations.

## ⚙️ Configuration
- Disable debug mode in production.
- Suppress stack traces from client responses.
- Remove default credentials.
- Disable unused methods.
- Restrict diagnostic endpoints.
- Protect metrics.
- Review CORS deliberately.
- Use TLS appropriately.
- Set request-size limits.
- Separate environments.
- Automate configuration checks.
- Track and expire exceptions.

## 🗺️ Inventory
- Inventory public APIs.
- Inventory partner APIs.
- Inventory internal APIs.
- Inventory staging systems.
- Track versions.
- Track owners.
- Track consumers.
- Track authentication mechanisms.
- Track data classification.
- Track deprecation dates.
- Search for shadow APIs.
- Search for zombie APIs.
- Verify retired routes are unreachable.
- Reconcile documentation with observed deployment and traffic.

## 🔗 Third-Party APIs
- Use TLS.
- Validate certificates.
- Validate response schemas.
- Treat provider data as untrusted input.
- Use timeouts.
- Limit response sizes.
- Control redirects.
- Bound retries.
- Use exponential backoff where appropriate.
- Scope provider credentials.
- Rotate integration credentials.
- Monitor provider failures.
- Use circuit breakers where appropriate.
- Do not blindly pass provider data into SQL, shell commands, templates, or other interpreters.

## 🧪 Testing
- Unit tests can verify policy decisions.
- Integration tests can verify tenant boundaries.
- Contract tests can verify schemas and authentication requirements.
- SAST can identify dangerous code patterns.
- DAST can exercise runtime behavior.
- Fuzzing can exercise malformed inputs.
- Manual review remains important for business logic.
- Negative tests should be first-class regression tests.
- Security tests should run continuously rather than only before audits.

## 🚀 CI/CD
- Secret scanning.
- Dependency analysis.
- SAST.
- API schema validation.
- Authorization regression tests.
- Integration security tests.
- Infrastructure configuration checks.
- Authenticated DAST in controlled environments.
- Inventory reconciliation.
- Review of high-impact authorization changes.
- Actionable security failures.

## 🧠 Threat Modeling
- Identify assets.
- Identify actors.
- Identify trust boundaries.
- Identify entry points.
- Identify privileged operations.
- Identify object relationships.
- Identify sensitive workflows.
- Identify expensive operations.
- Identify external dependencies.
- Identify server-side network destinations.
- Map threats to controls.
- Map controls to tests.
- Map tests to CI or monitoring.

## 📡 Logging and Detection
- Log authentication failures.
- Log authorization denials.
- Track unusual object access.
- Track cross-tenant denial.
- Track privileged function invocation.
- Track rate-limit violations.
- Track quota exhaustion.
- Track SSRF policy denials.
- Track dependency failures.
- Track deprecated endpoint usage.
- Do not log passwords.
- Do not log bearer tokens.
- Do not log API keys.
- Use correlation identifiers.
- Correlate security events across services.

## 🚨 Incident Response
1. Preserve evidence.
2. Identify affected endpoints.
3. Identify affected identities.
4. Identify affected objects.
5. Identify affected tenants.
6. Review successful anomalous requests.
7. Review authorization failures.
8. Rotate compromised credentials.
9. Revoke affected sessions or tokens where appropriate.
10. Disable vulnerable functionality when necessary.
11. Patch the root cause.
12. Add a regression test.
13. Assess historical exploitation.
14. Assess data exposure.
15. Document lessons learned.

## ✅ Secure API Review Checklist
- [ ] Authentication requirements are documented.
- [ ] Token validation is explicit.
- [ ] Object authorization is tested.
- [ ] Property authorization is tested.
- [ ] Function authorization is tested.
- [ ] Tenant isolation is tested.
- [ ] Bulk operations are authorized.
- [ ] Resource limits exist.
- [ ] Sensitive workflows have abuse controls.
- [ ] SSRF destinations are constrained.
- [ ] Production configuration is hardened.
- [ ] API inventory has owners.
- [ ] Deprecated versions have retirement plans.
- [ ] Third-party responses are validated.
- [ ] Credentials are not logged.
- [ ] Security events are observable.
- [ ] Negative tests run in CI.
- [ ] Incident response covers API compromise.

## 🧪 Isolated Learning Labs
### Lab 1 — API1:2023 Broken Object Level Authorization
Create an isolated application that demonstrates the **object** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 2 — API2:2023 Broken Authentication
Create an isolated application that demonstrates the **identity** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 3 — API3:2023 Broken Object Property Level Authorization
Create an isolated application that demonstrates the **property** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 4 — API4:2023 Unrestricted Resource Consumption
Create an isolated application that demonstrates the **resource** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 5 — API5:2023 Broken Function Level Authorization
Create an isolated application that demonstrates the **function** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 6 — API6:2023 Unrestricted Access to Sensitive Business Flows
Create an isolated application that demonstrates the **workflow** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 7 — API7:2023 Server Side Request Forgery
Create an isolated application that demonstrates the **network** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 8 — API8:2023 Security Misconfiguration
Create an isolated application that demonstrates the **configuration** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 9 — API9:2023 Improper Inventory Management
Create an isolated application that demonstrates the **inventory** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 10 — API10:2023 Unsafe Consumption of APIs
Create an isolated application that demonstrates the **dependency** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 11 — API1:2023 Broken Object Level Authorization
Create an isolated application that demonstrates the **object** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 12 — API2:2023 Broken Authentication
Create an isolated application that demonstrates the **identity** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 13 — API3:2023 Broken Object Property Level Authorization
Create an isolated application that demonstrates the **property** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 14 — API4:2023 Unrestricted Resource Consumption
Create an isolated application that demonstrates the **resource** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 15 — API5:2023 Broken Function Level Authorization
Create an isolated application that demonstrates the **function** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 16 — API6:2023 Unrestricted Access to Sensitive Business Flows
Create an isolated application that demonstrates the **workflow** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 17 — API7:2023 Server Side Request Forgery
Create an isolated application that demonstrates the **network** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 18 — API8:2023 Security Misconfiguration
Create an isolated application that demonstrates the **configuration** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 19 — API9:2023 Improper Inventory Management
Create an isolated application that demonstrates the **inventory** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 20 — API10:2023 Unsafe Consumption of APIs
Create an isolated application that demonstrates the **dependency** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 21 — API1:2023 Broken Object Level Authorization
Create an isolated application that demonstrates the **object** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 22 — API2:2023 Broken Authentication
Create an isolated application that demonstrates the **identity** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 23 — API3:2023 Broken Object Property Level Authorization
Create an isolated application that demonstrates the **property** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 24 — API4:2023 Unrestricted Resource Consumption
Create an isolated application that demonstrates the **resource** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.
### Lab 25 — API5:2023 Broken Function Level Authorization
Create an isolated application that demonstrates the **function** boundary.
Create one allowed security context.
Create one forbidden security context.
Implement the server-side policy.
Write a positive test.
Write a negative test.
Add a security log.
Repeat the test after refactoring.
Document the invariant that must remain true.

## 🧭 Secure API Lifecycle
- Design → identify trust boundaries.
- Model → identify abuse cases.
- Implement → enforce server-side policy.
- Test → prove allowed and denied behavior.
- Deploy → harden configuration.
- Operate → monitor and investigate.
- Improve → feed incidents and findings into engineering.

## 🧠 Common Misconceptions
### A JWT makes an API secure.
A valid credential establishes authenticated context; it does not authorize every resource or operation.
### UUIDs prevent BOLA.
Unpredictability can reduce guessing but does not replace authorization.
### Internal APIs do not need security.
Internal services can be reached by compromised workloads or bad network assumptions.
### Rate limiting solves API abuse.
It helps but sensitive workflows can require stateful business controls.
### Third-party data is trusted.
External data remains untrusted input.
### OpenAPI documentation is the entire inventory.
Documentation can become stale; inventory must be reconciled with deployment.
### Schema validation prevents every attack.
Schemas do not prove authorization or business intent.
### The gateway replaces authorization.
Gateways can provide coarse controls; domain authorization remains application-specific.

## 📚 Glossary
| **API** | Application Programming Interface. |
| **BOLA** | Broken Object Level Authorization. |
| **Authentication** | Establishing identity or credential context. |
| **Authorization** | Deciding whether a principal may perform an operation. |
| **RBAC** | Role-Based Access Control. |
| **ABAC** | Attribute-Based Access Control. |
| **JWT** | JSON Web Token. |
| **OAuth 2.0** | Authorization framework. |
| **OIDC** | OpenID Connect. |
| **PKCE** | Proof Key for Code Exchange. |
| **SSRF** | Server-Side Request Forgery. |
| **WAF** | Web Application Firewall. |
| **SIEM** | Security Information and Event Management. |
| **SAST** | Static Application Security Testing. |
| **DAST** | Dynamic Application Security Testing. |
| **Least privilege** | Only the permissions required for the task. |
| **Deny by default** | Reject access unless an explicit policy allows it. |
| **Tenant** | A customer or organizational isolation boundary. |
| **Idempotency** | Safe handling of repeated operations. |

## 📚 References
- [OWASP API Security Project](https://owasp.org/www-project-api-security/)
- [OWASP API Security Top 10 — 2023](https://api-security.owasp.org/editions/2023/en/0x11-t10/)
- [OWASP Introduction](https://api-security.owasp.org/editions/2023/en/0x03-introduction/)
- [OWASP Methodology and Data](https://api-security.owasp.org/editions/2023/en/0xd0-about-data/)
- [OWASP Release Notes](https://api-security.owasp.org/editions/2023/en/0x04-release-notes/)
- [OWASP API Security Risks](https://api-security.owasp.org/editions/2023/en/0x10-api-security-risks/)
- [OWASP API Security Developer Guide](https://devguide.owasp.org/en/07-training-education/07-api-top-ten/)
- [OWASP REST Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
- [OWASP Authorization Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
- [OWASP SSRF Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html)
- [OWASP Input Validation Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html)
- [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [OWASP Threat Modeling](https://owasp.org/www-community/Threat_Modeling)
- [NIST SP 800-63 Digital Identity Guidelines](https://pages.nist.gov/800-63-4/)
- [NIST Secure Software Development Framework](https://csrc.nist.gov/Projects/ssdf)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [NIST 2026 Token and Assertion Protection Guidance](https://www.nist.gov/publications/protecting-tokens-and-assertions-forgery-theft-and-misuse-implementation)

## ⚠️ Responsible Use
This material is for education, defensive engineering, secure development, authorized assessment, and security research.
Only test systems you own or have explicit permission to assess.
Build vulnerable examples in isolated labs.
Never test third-party systems without authorization.

---

<div align="center">
**DevSec-Archive • Cybersecurity • API Security**

*Learn the failure mode. Understand the control. Test the boundary. Secure the system.*

</div>

**Maintained by [ItsWanheda](https://github.com/ItsWanheda)**
- Practice 1: document **identity** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 2: document **object authorization** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 3: document **property authorization** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 4: document **function authorization** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 5: document **tenant isolation** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 6: document **resource limits** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 7: document **business workflows** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 8: document **SSRF** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 9: document **configuration** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 10: document **inventory** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 11: document **third-party trust** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 12: document **logging** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 13: document **testing** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 14: document **CI/CD** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 15: document **threat modeling** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 16: document **incident response** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 17: document **secrets** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 18: document **transport security** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 19: document **versioning** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 20: document **observability** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 21: document **identity** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 22: document **object authorization** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 23: document **property authorization** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 24: document **function authorization** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 25: document **tenant isolation** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 26: document **resource limits** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 27: document **business workflows** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 28: document **SSRF** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 29: document **configuration** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 30: document **inventory** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 31: document **third-party trust** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 32: document **logging** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 33: document **testing** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 34: document **CI/CD** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 35: document **threat modeling** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 36: document **incident response** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 37: document **secrets** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 38: document **transport security** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 39: document **versioning** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 40: document **observability** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 41: document **identity** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 42: document **object authorization** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 43: document **property authorization** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 44: document **function authorization** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 45: document **tenant isolation** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 46: document **resource limits** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 47: document **business workflows** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 48: document **SSRF** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 49: document **configuration** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 50: document **inventory** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 51: document **third-party trust** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 52: document **logging** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 53: document **testing** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 54: document **CI/CD** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 55: document **threat modeling** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 56: document **incident response** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 57: document **secrets** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 58: document **transport security** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 59: document **versioning** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 60: document **observability** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 61: document **identity** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 62: document **object authorization** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 63: document **property authorization** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 64: document **function authorization** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 65: document **tenant isolation** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 66: document **resource limits** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 67: document **business workflows** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 68: document **SSRF** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 69: document **configuration** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 70: document **inventory** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 71: document **third-party trust** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 72: document **logging** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 73: document **testing** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 74: document **CI/CD** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 75: document **threat modeling** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 76: document **incident response** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 77: document **secrets** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 78: document **transport security** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 79: document **versioning** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 80: document **observability** for the **mobile API**; define the security invariant, add a negative test, and assign an owner.
- Practice 81: document **identity** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 82: document **object authorization** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 83: document **property authorization** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 84: document **function authorization** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 85: document **tenant isolation** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 86: document **resource limits** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 87: document **business workflows** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 88: document **SSRF** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 89: document **configuration** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 90: document **inventory** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 91: document **third-party trust** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 92: document **logging** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 93: document **testing** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 94: document **CI/CD** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 95: document **threat modeling** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 96: document **incident response** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 97: document **secrets** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 98: document **transport security** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 99: document **versioning** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 100: document **observability** for the **browser client**; define the security invariant, add a negative test, and assign an owner.
- Practice 101: document **identity** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 102: document **object authorization** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 103: document **property authorization** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 104: document **function authorization** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 105: document **tenant isolation** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 106: document **resource limits** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 107: document **business workflows** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 108: document **SSRF** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 109: document **configuration** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 110: document **inventory** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 111: document **third-party trust** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 112: document **logging** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 113: document **testing** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 114: document **CI/CD** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 115: document **threat modeling** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 116: document **incident response** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 117: document **secrets** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 118: document **transport security** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 119: document **versioning** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 120: document **observability** for the **service-to-service call**; define the security invariant, add a negative test, and assign an owner.
- Practice 121: document **identity** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 122: document **object authorization** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 123: document **property authorization** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 124: document **function authorization** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 125: document **tenant isolation** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 126: document **resource limits** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 127: document **business workflows** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 128: document **SSRF** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 129: document **configuration** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 130: document **inventory** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 131: document **third-party trust** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 132: document **logging** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 133: document **testing** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 134: document **CI/CD** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 135: document **threat modeling** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 136: document **incident response** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 137: document **secrets** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 138: document **transport security** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 139: document **versioning** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 140: document **observability** for the **webhook**; define the security invariant, add a negative test, and assign an owner.
- Practice 141: document **identity** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 142: document **object authorization** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 143: document **property authorization** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 144: document **function authorization** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 145: document **tenant isolation** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 146: document **resource limits** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 147: document **business workflows** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 148: document **SSRF** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 149: document **configuration** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 150: document **inventory** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 151: document **third-party trust** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 152: document **logging** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 153: document **testing** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 154: document **CI/CD** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 155: document **threat modeling** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 156: document **incident response** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 157: document **secrets** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 158: document **transport security** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 159: document **versioning** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 160: document **observability** for the **bulk endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 161: document **identity** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 162: document **object authorization** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 163: document **property authorization** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 164: document **function authorization** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 165: document **tenant isolation** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 166: document **resource limits** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 167: document **business workflows** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 168: document **SSRF** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 169: document **configuration** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 170: document **inventory** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 171: document **third-party trust** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 172: document **logging** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 173: document **testing** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 174: document **CI/CD** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 175: document **threat modeling** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 176: document **incident response** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 177: document **secrets** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 178: document **transport security** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 179: document **versioning** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 180: document **observability** for the **export endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 181: document **identity** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 182: document **object authorization** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 183: document **property authorization** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 184: document **function authorization** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 185: document **tenant isolation** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 186: document **resource limits** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 187: document **business workflows** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 188: document **SSRF** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 189: document **configuration** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 190: document **inventory** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 191: document **third-party trust** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 192: document **logging** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 193: document **testing** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 194: document **CI/CD** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 195: document **threat modeling** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 196: document **incident response** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 197: document **secrets** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 198: document **transport security** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 199: document **versioning** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 200: document **observability** for the **search endpoint**; define the security invariant, add a negative test, and assign an owner.
- Practice 201: document **identity** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 202: document **object authorization** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 203: document **property authorization** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 204: document **function authorization** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 205: document **tenant isolation** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 206: document **resource limits** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 207: document **business workflows** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 208: document **SSRF** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 209: document **configuration** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 210: document **inventory** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 211: document **third-party trust** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 212: document **logging** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 213: document **testing** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 214: document **CI/CD** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 215: document **threat modeling** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 216: document **incident response** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 217: document **secrets** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 218: document **transport security** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 219: document **versioning** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 220: document **observability** for the **background job**; define the security invariant, add a negative test, and assign an owner.
- Practice 221: document **identity** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 222: document **object authorization** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 223: document **property authorization** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 224: document **function authorization** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 225: document **tenant isolation** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 226: document **resource limits** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 227: document **business workflows** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 228: document **SSRF** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 229: document **configuration** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 230: document **inventory** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 231: document **third-party trust** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 232: document **logging** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 233: document **testing** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 234: document **CI/CD** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 235: document **threat modeling** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 236: document **incident response** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 237: document **secrets** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 238: document **transport security** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 239: document **versioning** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 240: document **observability** for the **administrative route**; define the security invariant, add a negative test, and assign an owner.
- Practice 241: document **identity** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 242: document **object authorization** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 243: document **property authorization** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 244: document **function authorization** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 245: document **tenant isolation** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 246: document **resource limits** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 247: document **business workflows** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 248: document **SSRF** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 249: document **configuration** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 250: document **inventory** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 251: document **third-party trust** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 252: document **logging** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 253: document **testing** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 254: document **CI/CD** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 255: document **threat modeling** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 256: document **incident response** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 257: document **secrets** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 258: document **transport security** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 259: document **versioning** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 260: document **observability** for the **multi-tenant service**; define the security invariant, add a negative test, and assign an owner.
- Practice 261: document **identity** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 262: document **object authorization** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 263: document **property authorization** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 264: document **function authorization** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 265: document **tenant isolation** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 266: document **resource limits** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 267: document **business workflows** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 268: document **SSRF** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 269: document **configuration** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 270: document **inventory** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 271: document **third-party trust** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 272: document **logging** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 273: document **testing** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 274: document **CI/CD** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 275: document **threat modeling** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 276: document **incident response** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 277: document **secrets** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 278: document **transport security** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 279: document **versioning** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 280: document **observability** for the **third-party integration**; define the security invariant, add a negative test, and assign an owner.
- Practice 281: document **identity** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 282: document **object authorization** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 283: document **property authorization** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 284: document **function authorization** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 285: document **tenant isolation** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 286: document **resource limits** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 287: document **business workflows** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 288: document **SSRF** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 289: document **configuration** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 290: document **inventory** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 291: document **third-party trust** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 292: document **logging** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 293: document **testing** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 294: document **CI/CD** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 295: document **threat modeling** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 296: document **incident response** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 297: document **secrets** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 298: document **transport security** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 299: document **versioning** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 300: document **observability** for the **legacy API**; define the security invariant, add a negative test, and assign an owner.
- Practice 301: implement **identity** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 302: implement **object authorization** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 303: implement **property authorization** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 304: implement **function authorization** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 305: implement **tenant isolation** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 306: implement **resource limits** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 307: implement **business workflows** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 308: implement **SSRF** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 309: implement **configuration** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 310: implement **inventory** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 311: implement **third-party trust** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 312: implement **logging** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 313: implement **testing** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 314: implement **CI/CD** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 315: implement **threat modeling** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 316: implement **incident response** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 317: implement **secrets** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 318: implement **transport security** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 319: implement **versioning** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 320: implement **observability** for the **public API**; define the security invariant, add a negative test, and assign an owner.
- Practice 321: implement **identity** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 322: implement **object authorization** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 323: implement **property authorization** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 324: implement **function authorization** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 325: implement **tenant isolation** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 326: implement **resource limits** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 327: implement **business workflows** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 328: implement **SSRF** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 329: implement **configuration** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 330: implement **inventory** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 331: implement **third-party trust** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 332: implement **logging** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 333: implement **testing** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 334: implement **CI/CD** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 335: implement **threat modeling** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 336: implement **incident response** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 337: implement **secrets** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 338: implement **transport security** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 339: implement **versioning** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 340: implement **observability** for the **partner API**; define the security invariant, add a negative test, and assign an owner.
- Practice 341: implement **identity** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 342: implement **object authorization** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 343: implement **property authorization** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 344: implement **function authorization** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 345: implement **tenant isolation** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 346: implement **resource limits** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 347: implement **business workflows** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 348: implement **SSRF** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 349: implement **configuration** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 350: implement **inventory** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 351: implement **third-party trust** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 352: implement **logging** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 353: implement **testing** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 354: implement **CI/CD** for the **internal API**; define the security invariant, add a negative test, and assign an owner.
- Practice 355: implement **threat modeling** for the **internal API**; define the security invariant, add a negative test, and assign an owner.