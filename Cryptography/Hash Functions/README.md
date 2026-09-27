# Hash Functions: The Complete Guide to Cryptographic Hashing, Security, and Real-World Applications
> **Author**: Cybersecurity Research &      Cryptography Editorial Team
    <br>
    **Reading Time: ~45 minutes**
    <br>
    **Skill Level: Beginner → Advanced**
    <br>
    **Last Updated: 2026**

---

# 📚 Table of Contents

- [🚀 Introduction](#-introduction)
- [🔍 What is a Hash Function?](#-what-is-a-hash-function)
- [🛡️ Properties of Cryptographic Hash Functions](#️-properties-of-cryptographic-hash-functions)
- [⚙️ How Hashing Works](#️-how-hashing-works)
- [🏗️ Hash Function Architecture](#️-hash-function-architecture)
- [📖 Common Hash Algorithms](#-common-hash-algorithms)
- [💥 MD5 Explained](#-md5-explained)
- [🔐 SHA Family](#-sha-family)
- [⚡ SHA-256 Deep Dive](#-sha-256-deep-dive)
- [🧽 SHA-3 (Keccak)](#-sha-3-keccak)
- [⚖️ Hashing vs Encryption](#️-hashing-vs-encryption)
- [🧮 Hashing vs Checksums](#-hashing-vs-checksums)
- [🔑 Password Hashing](#-password-hashing)
- [🔒 Password Hashing Algorithms](#-password-hashing-algorithms)
- [🌶️ Pepper Explained](#️-pepper-explained)
- [💣 Hash Collisions](#-hash-collisions)
- [🛠️ Hash Cracking](#️-hash-cracking)
- [🌈 Rainbow Tables](#-rainbow-tables)
- [✅ File Integrity Verification](#-file-integrity-verification)
- [✍️ Digital Signatures](#️-digital-signatures)
- [⛓️ Blockchain](#️-blockchain)
- [🌳 Merkle Trees](#-merkle-trees)
- [📦 Hash-Based Data Structures](#-hash-based-data-structures)
- [🌍 Real-World Applications](#-real-world-applications)
- [💻 Hashing in Programming](#-hashing-in-programming)
- [📈 Performance](#-performance)
- [⭐ Best Practices](#-best-practices)
- [❌ Common Mistakes](#-common-mistakes)
- [❓ Frequently Asked Questions](#-frequently-asked-questions)
- [🚀 Future of Hash Functions](#-future-of-hash-functions)
- [📝 Summary](#-summary)
- [📚 Glossary](#-glossary)
- [📖 References](#-references)

---

# 🚀 Introduction
Hash functions are among the **most fundamental primitives in modern cryptography and computer science**. From verifying the integrity of downloaded files to securing billions of passwords, from powering blockchain networks to enabling digital signatures — hash functions are silently running underneath almost every secure system we interact with daily.

## What is a Hash Function?
A **cryptographic hash function** maps an input byte string to a fixed-size digest. The digest is a compact representation of the input; it is not a reversible encoding and it is not automatically an authentication mechanism. The same input produces the same digest under the same algorithm and encoding. A small input change should produce an output that is computationally indistinguishable from a fresh random digest.

## Why Hash Functions Exist
Hash functions solve a critical problem: how do you verify that data hasn't been tampered with, identify content uniquely, or securely store sensitive information like passwords? Without hash functions:
* **File integrity** couldn't be verified
* **Passwords** would have to be stored in plaintext
* **Digital signatures** wouldn't exist
* **Blockchain** wouldn't be possible
* **Version control systems** like Git couldn't track changes

## Why Hashing is a Foundation of Cybersecurity
Hashing underpins:
| Domain | Use Case |
|--------|----------|
| Authentication | Password storage |
| Integrity | File & message verification |
| Authentication | Digital signatures |
| Networking | TLS handshake (HMAC, KDFs) |
| Storage | Deduplication, integrity |
| Cryptocurrencies | Proof-of-Work, Merkle Trees |
| Forensics | Evidence integrity |

## Historical Background
The history of hashing reflects the evolution of computing itself:
* **1950s** — Hans Peter Luhn (IBM) proposes hashing for data retrieval.
* 1970s — Early non-cryptographic hashes emerge for hash tables.
* **1989** — **MD2**, **MD4**, and **MD5** developed by Ronald Rivest.
* **1993** — **SHA-0** published by NIST.
* **1995** — **SHA-1** standardized.
* **2001** — **SHA-2** family released.
* **2004** — Collision attacks on MD5 demonstrated.
* **2008** — Theoretical attacks on SHA-1 published.
* **2012** — **Keccak** wins the SHA-3 competition.
* **2015** — **SHA-3** officially standardized (FIPS 202).
* **2017** — **Google SHAttered**: first practical **SHA-1** collision.
* **2019** — **Argon2** wins the Password Hashing Competition.
* **2020+** — **BLAKE3** released, hybrid post-quantum research accelerates.

## Evolution of Hashing Algorithms
```text
Speed & Insecurity Era → Brute-Force Resistance Era → Memory-Hard Era
   (MD5, SHA-1)              (SHA-2, SHA-3)              (Argon2, scrypt)
```
The trajectory is clear: as hardware gets faster, hash functions must evolve to remain resistant to attack.

---

# 🔍 What is a Hash Function?
## Definition
A **hash function** `H` takes an input `m` of arbitrary length and returns a fixed-length output `h`:
```text
H(m) = h
```
Where:
* `m` = input (message, file, password, etc.)
* `H` = hash function (algorithm)
* `h` = hash value / digest (fixed size)
## Visual Diagram
```text
   Input (Any Size)              Hash Function              Output (Fixed Size)
  ┌──────────────────┐         ┌───────────────┐         ┌──────────────┐
  │ "Hello World"    │         │               │         │ a591a6d40bf4 │
  │ (11 bytes)       │ ──────► │   SHA-256     │ ──────► │ 20f2ee5b21e9 │
  └──────────────────┘         │               │         │ 8c746c91c4d4 │
                               └───────────────┘         └──────────────┘
                                                              (32 bytes)         
```
## Key Characteristics
### 1. Arbitrary Input Size
A hash function can process any input — from a single byte to terabytes of data.
```text
H("a")                 = e4b9096b...
H("Hello World")       = a591a6d4...
H(entire movie file)   = 6c1b07bc...
```
All three outputs are **the same fixed length**.
### 2. Fixed Output Size
Regardless of input size, the output length is constant for a given algorithm:
| Algorithm | Output Size |
|-----------|-------------|
| MD5 | 128 bits (16 bytes) |
| SHA-1 | 160 bits (20 bytes) |
| SHA-256 | 256 bits (32 bytes) |
| SHA-512 | 512 bits (64 bytes) |
| BLAKE3 | 256 bits (configurable) |
### 3. One-Way Transformation (Pre-image Resistance)
A secure hash is designed to make finding a preimage computationally infeasible; this does not mean that every real-world hash value is impossible to recover. Low-entropy inputs such as common passwords can be guessed and re-hashed.
```text
Forward:    "password123"  ───► SHA-256 ───► "ef92b778..."
Backward:   "ef92b778..."  ───► ??? ──────► NOT FEASIBLE
```
### 4. Deterministic Output
The same input always produces the same output:
```python
SHA-256("hello") = "2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824"
SHA-256("hello") = "2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824"
```
This determinism is what makes verification possible.
### Practical Examples
```bash
$ echo -n "Hello World" | sha256sum
a591a6d40bf420404a011733cfb7b190d62c65bf0bcda32b57b277d9ad9f146e

$ echo -n "Hello World." | sha256sum
e4d7ba2ff84b6f0b8b2c9e9c5a3f5b6c7d8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a
```
Notice how just adding a period completely changes the output.

---

# 🛡️ Properties of Cryptographic Hash Functions
For a hash function to be considered **cryptographically secure**, it must satisfy the following properties:
## 1. Deterministic
**Definition**: The same input must always produce the same output.

**Why it matters**: Without this property, you couldn't verify if a file has been tampered with — the hash would change randomly every time.
## 2. Fast Computation
**Definition**: Given an input, the hash must be computed quickly.

**Why it matters**: Systems need to verify integrity, sign transactions, and process authentication requests at high speed. A slow hash function would cripple performance.
> ⚠️ Important distinction: Fast for integrity hashing ≠ fast for password hashing. Password hashing algorithms intentionally slow down computation (see Section 14).

## 3. Pre-image Resistance
**Definition**: Given a hash `h`, it should be computationally infeasible to find any input `m` such that `H(m) = h`.
```text
Given:   h = SHA-256("secret")
Find:    m such that SHA-256(m) = h
Result:  INFEASIBLE
```
**Why it matters**: Attackers who steal a password hash database should not be able to recover the original passwords.
## 4. Second Pre-image Resistance
**Definition**: Given an input `m₁`, it should be computationally infeasible to find a different input `m₂` such that `H(m₁) = H(m₂)`.
**Why it matters**: Prevents an attacker from substituting a malicious file with the same hash as a legitimate file.
## 5. Collision Resistance
Definition: It should be computationally infeasible to find any two different inputs `m₁ ≠ m₂` such that `H(m₁) = H(m₂)`.
**Why it matters**: Collisions undermine digital signatures — if two documents share a hash, a signature for one becomes valid for the other.
## 6. Avalanche Effect
**Definition**: A tiny change in input should drastically change the output. Ideally, ~50% of output bits flip.
```text
SHA-256("hello")  = 2cf24dba...b9824
SHA-256("Hello")  = 185f8db3...e7e7e
                   ↑ ALL bits changed
```
**Why it matters**: Ensures outputs appear random and don't leak information about the input.
## 7. Uniform Distribution
**Definition**: Hash outputs should be uniformly distributed across the output space.
**Why it matters**: Non-uniform distribution creates predictable patterns that attackers can exploit.
## 8. Fixed-Length Output
**Definition**: The output length is constant regardless of input length.
**Why it matters**: Predictable output size is essential for data structures (hash tables) and protocol design.
## Property Comparison Table
| Property | MD5 | SHA-1 | SHA-256 | SHA-3 | BLAKE3 |
|----------|-----|-------|---------|-------|--------|
| Deterministic | ✅ | ✅ | ✅ | ✅ | ✅ |
| Fast | ✅ | ✅ | ✅ | ✅ | ✅✅ |
| Pre-image Resistant | ❌ | ⚠️ | ✅ | ✅ | ✅ |
| Second Pre-image Resistant | ❌ | ⚠️ | ✅ | ✅ | ✅ |
| Collision Resistant | ❌ | ❌ | ✅ | ✅ | ✅ |
| Avalanche Effect | ✅ | ✅ | ✅ | ✅ | ✅ |

---

# ⚙️ How Hashing Works
Hashing operates through a series of well-defined stages. Let's trace a small message through a typical hash function:
## Step-by-Step Process
```text
Step 1:  Input Message
         ┌──────────────────────┐
         │ "Hello World"        │
         └──────────────────────┘
                │
                ▼
Step 2:  Convert to Binary
         01001000 01100101 01101100 01101100 01101111 ...
                │
                ▼
Step 3:  Padding (align to block size)
         [Message] [1 bit] [0...0] [Length in bits]
                │
                ▼
Step 4:  Split into Blocks
         Block 1 (512 bits) | Block 2 (512 bits) | ...
                │
                ▼
Step 5:  Initialize State (IV)
         h₀ = 0x6a09e667, h₁ = 0xbb67ae85, ...
                │
                ▼
Step 6:  Compression Function (multiple rounds)
         for each block:
             state = Compress(state, block)
                │
                ▼
Step 7:  Finalize
         output = state
                │
                ▼
Step 8:  Digest
         ┌──────────────────────────────────────────┐
         │ a591a6d40bf420404a011733cfb7b190d62c65... │
         └──────────────────────────────────────────┘
```
## Internal Round Example (Conceptual)
```text
       Block (512 bits)
            │
            ▼
   ┌────────────────┐
   │ Message Schedule│ (expand block)
   └────────┬───────┘
            │
            ▼
   ┌────────────────┐
   │ Round 1        │ ── K₁ constant
   └────────┬───────┘
            │
            ▼
   ┌────────────────┐
   │ Round 2        │ ── K₂ constant
   └────────┬───────┘
            │
            ▼
        ... (64 rounds for SHA-256)
            │
            ▼
   ┌────────────────┐
   │ Round 64       │ ── K₆₄ constant
   └────────┬───────┘
            │
            ▼
     New State (256 bits)
```
## Why This Matters
The internal rounds use:

* **Bitwise operations** (AND, OR, XOR, NOT)
* **Modular addition**
* **Rotation and shifting**
* **Non-linear functions** (mimic confusion and diffusion)

These operations are intentionally designed to:

1. **Confuse** — make the relationship between input and output unintelligible
2. **Diffuse** — spread the influence of each input bit across the entire output

---

# 🏗️ Hash Function Architecture
## Core Components
### 1. Initialization Vector (IV)
The IV is a fixed starting value that initializes the internal state of the hash function.
```text
SHA-256 IV:
H₀ = 6a09e667
H₁ = bb67ae85
H₂ = 3c6ef372
H₃ = a54ff53a
H₄ = 510e527f
H₅ = 9b05688c
H₆ = 1f83d9ab
H₇ = 5be0cd19
```
### 2. Padding
Ensures the input length is a multiple of the block size.
```text
Original:  "Hello World" (88 bits)
Padded:    "Hello World" + 1 bit + 0 bits + 64-bit length
           = 512 bits (one block)
```
### 3. Message Blocks
The padded input is split into fixed-size blocks:
```text
Block 1  Block 2  Block 3  ...  Block N
[512b]   [512b]   [512b]        [512b]
```
### 4. Compression Function
Each block is processed by the compression function, updating the internal state.
```text
State(i) ──┐
           ├──► Compression Function ──► State(i+1)
Block(i) ──┘
```
### 5. Rounds
Multiple rounds (iterations) apply transformations to the state:
```text
Round 1 → Round 2 → ... → Round N → New State
```
### 6. Internal State
The state holds intermediate values during hashing:
```text
State = [A, B, C, D, E, F, G, H]   (SHA-256: 8 × 32-bit words)
```
### 7. Finalization
The final state is post-processed to produce the digest.
## Merkle–Damgård Construction (SHA-2 Style)
```text
        IV
         │
         ▼
   ┌──────────┐
   │ Block 1  │──┐
   └──────────┘  │
         ▲       ▼
         │   ┌──────────┐
         └───┤   State  │
             └──────────┘
                 │
                 ▼
             ┌──────────┐
             │ Block 2  │──┐
             └──────────┘  │
                  ▲        ▼
                  │    ┌──────────┐
                  └────┤   State  │
                       └──────────┘
                          │
                          ▼
                       Output
```
## Sponge Construction (SHA-3 Style)
```text
         Rate (r bits)          Capacity (c bits)
        ┌───────────────┐      ┌───────────────┐
        │               │      │               │
   ────►│  Absorbing    │◄────►│  Sponge       │
        │  Phase        │      │  Function (f) │
        │               │      │               │
        └───────┬───────┘      └───────────────┘
                │
                ▼
        ┌───────────────┐
        │  Squeezing    │───► Output
        │  Phase        │
        └───────────────┘
```

---

# 📖 Common Hash Algorithms
## Comparison Table
| Algorithm | Year | Output (bits) | Speed | Security | Status |
|-----------|------|---------------|-------|----------|--------|
| MD5 | 1991 | 128 | Very Fast | ❌ Broken | Deprecated |
| SHA-1 | 1995 | 160 | Fast | ❌ Broken | Deprecated |
| SHA-224 | 2001 | 224 | Moderate | ✅ Secure | Acceptable |
| SHA-256 | 2001 | 256 | Moderate | ✅ Secure | Recommended |
| SHA-384 | 2001 | 384 | Moderate | ✅ Secure | Recommended |
| SHA-512 | 2001 | 512 | Fast on 64-bit | ✅ Secure | Recommended |
| SHA-3-256 | 2015 | 256 | Moderate | ✅ Secure | Recommended |
| SHA-3-512 | 2015 | 512 | Moderate | ✅ Secure | Recommended |
| BLAKE2 | 2012 | 256-512 | Very Fast | ✅ Secure | Recommended |
| BLAKE3 | 2020 | 256 | Extremely Fast | ✅ Secure | Recommended |
| RIPEMD-160 | 1996 | 160 | Fast | ⚠️ Limited | Legacy (Bitcoin) |
| Whirlpool | 2000 | 512 | Moderate | ✅ Secure | Acceptable |
| Tiger | 1995 | 192 | Fast | ⚠️ Limited | Legacy |
## Detailed Algorithm Summaries
### MD5 (Message Digest 5)
**Output**: 128 bits
**Status**: Cryptographically broken for collision resistance; do not use it for security-sensitive integrity, signatures, or password storage.
**Use today**: Only for non-security checksums
### SHA-1 (Secure Hash Algorithm 1)
**Output**: 160 bits
**Status**: Collision resistance is broken; the 2017 SHAttered demonstration produced a practical SHA-1 collision. NIST recommends transitioning away from SHA-1.
**Use today**: Legacy systems only — migrate away
### SHA-2 Family
**Members**: SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, SHA-512/256
**Construction**: Merkle–Damgård
**Status**: Industry standard, widely deployed
**Use today**: TLS, certificates, signatures, blockchain
### SHA-3 (Keccak)
**Construction**: Sponge construction
**Status**: Standardized as FIPS 202 (2015)
**Use today**: Diversity from SHA-2, future-proofing
### BLAKE2
**Speed**: Faster than MD5, as secure as SHA-3
**Use today**: High-performance applications, hashing in libsodium
### BLAKE3
**Speed**: Faster than BLAKE2, parallelizable
**Use today**: Modern applications needing extreme throughput
### RIPEMD-160
**Output**: 160 bits
**Use today**: Bitcoin address generation

---

# 💥 MD5 Explained
## History
MD5 was designed by **Ronald Rivest** in **1991** as a successor to MD4. It became one of the most widely used hash functions for two decades.
## How It Works
```text
Input → 512-bit blocks → 64 rounds → 128-bit digest
```
The algorithm processes messages in 512-bit blocks through 4 rounds of 16 operations each.
## Collision Attacks
In **2004**, researchers demonstrated practical collision attacks on MD5. By **2008**, researchers forged an SSL certificate using MD5 collisions.
```python
# Demonstration: Two different inputs with the same MD5 hash
hash1 = md5(b"d131dd02c5e6eec4693d9a0698aff95c2fcab58712467eab4004583eb8fb7f89")
hash2 = md5(b"d131dd02c5e6eec4693d9a0698aff95c2fcab50712467eab4004583eb8fb7f89")

# hash1 == hash2  → COLLISION FOUND
```
## Security Weaknesses
* **Collision resistance**: Broken
* **Pre-image resistance**: Weakened but not yet fully broken
* **Speed**: Too fast for password hashing
## Why It's No Longer Secure
Modern GPUs can compute **billions** of MD5 hashes per second, making brute-force and rainbow table attacks trivial.
> 🚫 Verdict: MD5 must never be used for security purposes. Use it only for non-cryptographic checksums or legacy compatibility.

---

# 🔐 SHA Family
## SHA-0
* **Published**: 1993
* **Withdrawn**: Quickly after SHA-1 release due to undisclosed weakness
* **Status**: Broken
## SHA-1
* **Published**: 1995
* **Output**: 160 bits
* **Attack**: SHAttered (2017) — first practical collision
* **Cost at the time**: ~$110,000 of compute
```text
SHA-1("The quick brown fox jumps over the lazy dog")
  = 2fd4e1c67a2d28fced849ee1bb76e7391b93eb12

SHA-1("The quick brown fox jumps over the lazy dog.")
  = 408d94384216f890ff7a0c3528e8bed1e0b01621
```
## SHA-2 Family
Includes **SHA-224**, **SHA-256**, **SHA-384**, **SHA-512**. All use Merkle–Damgård construction.
| Variant | Block Size | Word Size | Rounds | Output |
|---------|-----------|-----------|--------|--------|
| SHA-224 | 512 | 32 | 64 | 224 |
| SHA-256 | 512 | 32 | 64 | 256 |
| SHA-384 | 1024 | 64 | 80 | 384 |
| SHA-512 | 1024 | 64 | 80 | 512 |
## SHA-3
* **Algorithm winner**: Keccak (Bertoni, Daemen, Peeters, Van Assche)
* **Standardized**: FIPS 202 (2015)
* **Construction**: Sponge (radically different from SHA-2)
## SHA Comparison Table
| Property | SHA-1 | SHA-256 | SHA-384 | SHA-512 | SHA3-256 |
|----------|-------|---------|---------|---------|----------|
| Output bits | 160 | 256 | 384 | 512 | 256 |
| Block bits | 512 | 512 | 1024 | 1024 | 1088 |
| Rounds | 80 | 64 | 80 | 80 | 24 (Keccak-f) |
| Construction | MD | MD | MD | MD | Sponge |
| Speed (64-bit) | Fast | Moderate | Fast | Very Fast | Moderate |
| Security | ❌ | ✅ | ✅ | ✅ | ✅ |

---

# ⚡ SHA-256 Deep Dive
SHA-256 is the **workhorse of modern cryptography** — used in Bitcoin, TLS, code signing, and digital signatures.
## Algorithm Steps
```text
Input Message (any length)
        │
        ▼
1. Padding
   Message + 1 bit + 0 bits + 64-bit length
        │
        ▼
2. Parse into 512-bit Blocks M₁, M₂, ..., Mₙ
        │
        ▼
3. Initialize Hash Values (H₀)
   H₀[0..7] = first 32 bits of fractional parts of √(2,3,5,7,11,13,17,19)
        │
        ▼
4. For each block:
   a. Message Schedule: W₀..W₆₃
   b. Initialize working variables a,b,c,d,e,f,g,h
   c. 64 rounds of compression
   d. Update H₀ = H₀ + working vars
        │
        ▼
5. Output: Concatenation of H₀[0..7]
```
## Constants
SHA-256 uses **64 round constants** (`K[0..63]`) — the first 32 bits of fractional parts of cube roots of the first 64 primes.
```python
K = [
    0x428a2f98, 0x71374491, 0xb5c0fbcf, 0xe9b5dba5,
    0x3956c25b, 0x59f111f1, 0x923f82a4, 0xab1c5ed5,
    # ... 56 more constants
]
```
## Round Function
```text
T1 = h + Σ₁(e) + Ch(e,f,g) + Kₜ + Wₜ
T2 = Σ₀(a) + Maj(a,b,c)
h = g
g = f
f = e
e = d + T1
d = c
c = b
b = a
a = T1 + T2
```
Where:

* `Ch(x,y,z) = (x AND y) XOR (NOT x AND z)` — Choose
* `Maj(x,y,z) = (x AND y) XOR (x AND z) XOR (y AND z)` — Majority
* `Σ₀(x)` and `Σ₁(x)` — Sigma rotations
## Real-World Applications of SHA-256
* **Bitcoin Mining**: Miners search for a nonce such that `SHA-256(SHA-256(block_header))` starts with many zeros.
* **TLS/SSL**: Certificates are signed using SHA-256.
* **Digital Signatures**: RSA, ECDSA, Ed25519 use SHA-256.
* **Git**: Every commit, tree, and blob is identified by its SHA-1 (migrating to SHA-256).
* **Software Integrity**: Linux distributions publish SHA-256 checksums.
* **HMAC**: Message authentication codes use SHA-256.
```bash
# Practical example: Verify a file
$ sha256sum ubuntu-22.04.iso
b3a07e89...  ubuntu-22.04.iso
```

---

# 🧽 SHA-3 (Keccak)
## Background
SHA-3 is the result of an open NIST competition (2007–2012). **Keccak**, designed by a Belgian team, won due to its elegant sponge construction.
## Sponge Construction
```text
                ┌─────────────────────────────────────┐
                │                                     │
                │   ┌──────────────────────────┐      │
   Input ──────►│   │                          │      │
                │   │     State (1600 bits)    │      │
                └──►│   ┌──────┐  ┌──────┐     │◄─────┘
                    │   │ Rate │  │ Cap  │     │
                    │   │(1088)│  │ (512)│     │
                    │   └──────┘  └──────┘     │
                    │          │               │
                    │          ▼               │
                    │     Keccak-f       │
                    │     (24 rounds)          │
                    │                          │
                    └──────────────────────────┘
                              │
                              ▼
                          Output
```
## Absorbing Phase
The input is XORed into the rate portion of the state, then the permutation `f` is applied repeatedly.
## Squeezing Phase
Output bits are read from the rate portion, applying `f` between reads when needed.
## Advantages of SHA-3
**Different construction** from SHA-2 — provides algorithmic diversity
**No length-extension vulnerability** (SHA-2 is vulnerable)
**Efficient hardware implementation**
**Flexible output length**
## Differences from SHA-2
| Feature | SHA-2 | SHA-3 |
|---------|-------|-------|
| Construction | Merkle–Damgård | Sponge |
| Internal state | 256-512 bits | 1600 bits |
| Block size | 512 or 1024 bits | 1088 or 576 bits |
| Length-extension | ⚠️ Vulnerable | ✅ Resistant |
| Hardware speed | Moderate | Fast |
| Software speed | Fast | Moderate |

---

# ⚖️ Hashing vs Encryption
This is one of the most commonly confused distinctions in cryptography.
## Comparison Table
| Property | Hashing | Encryption | Encoding | Obfuscation | Signing |
|----------|---------|------------|----------|-------------|---------|
| **Reversible** | ❌ No | ✅ Yes (with key) | ✅ Yes (no key) | ✅ Yes (often) | ❌ No |
| **Key required** | ❌ No | ✅ Yes | ❌ No | ❌ No | ✅ Yes (private) |
| **Purpose** | Integrity | Confidentiality | Data format | Hide intent | Authenticity |
| **Output size** | Fixed | Variable | Same as input | Variable | Variable |
| **Deterministic** | ✅ Yes | ❌ No (IV) | ✅ Yes | ✅ Yes | ❌ No (nonce) |
| **Security purpose** | Verify | Protect | N/A | N/A | Sign |
## Hashing
* **One-way** function
* Used for integrity and authentication
* Example: `SHA-256("password")`
## Encryption
* **Two-way** function (reversible with a key)
* Used for confidentiality
* Example: `AES-256(plaintext, key)` → ciphertext
## Encoding
* **Reversible without a key**
* Used for data representation, not security
* Examples: Base64, Hex, URL encoding
## Obfuscation
* Hides intent but is **not security**
* Example: JavaScript minification, string reversal
* ⚠️ Never rely on obfuscation for confidentiality
## Signing
Uses **hashing + asymmetric encryption**
Proves authenticity and integrity
Example: `Sign(SHA-256(message), private_key)`
## Visual Comparison
```text
┌─────────────────────────────────────────────────────────────┐
│                       PLAINTEXT                              │
└───────┬──────────────────┬──────────────────┬───────────────┘
        │                  │                  │
        ▼                  ▼                  ▼
    HASHING           ENCRYPTION           SIGNING
        │                  │                  │
   SHA-256(plain)    AES(plain, key)     Sign(SHA-256(plain),
        │                  │               priv_key)
        │                  │                  │
        ▼                  ▼                  ▼
   ┌─────────┐        ┌──────────┐       ┌──────────┐
   │  Digest │        │Ciphertext│       │Signature │
   │ (fixed) │        │(variable)│       │(variable)│
   └─────────┘        └──────────┘       └──────────┘
   One-way            Reversible         Verifiable
                                          with public key
```

---

# 🧮 Hashing vs Checksums
## What is a Checksum?
A checksum is a small value used to detect **accidental** errors in data — not malicious tampering.
## Comparison Table
| Property | CRC32 | Adler32 | MD5 | SHA-256 |
|----------|-------|---------|-----|---------|
| Cryptographic | ❌ No | ❌ No | ⚠️ Broken | ✅ Yes |
| Speed | Very Fast | Very Fast | Fast | Moderate |
| Detects errors | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| Detects tampering | ❌ No | ❌ No | ❌ No | ✅ Yes |
| Typical use | Network | zlib | Legacy | Security |
## When to Use Each
* **CRC32** / Adler32**: Detect transmission errors (Ethernet, ZIP, PNG)
* **MD5 / SHA-1**: Non-security fingerprinting only
* **SHA-256 / SHA-3**: Cryptographic integrity, digital signatures
* **BLAKE3**: High-speed integrity verification
> 💡 Tip: Use BLAKE3 for new applications needing fast cryptographic checksums. It outperforms both MD5 and SHA-256 while being secure.

---

# 🔑 Password Hashing
## Why Plain Hashes Are Insufficient
Even though hashes are one-way, attackers don't need to reverse them — they can:
1. **Pre-compute** millions of password candidates
2. **Look up** hashes in massive databases (rainbow tables)
3. **Brute-force** through GPU-accelerated systems
```text
Plain Hash (INSECURE):
User: alice
DB:   5f4dcc3b5aa765d61d8327deb882cf99   ← MD5("password")
Attack: lookup "password" → instant match ❌
```
## The Solution: Password-Specific Hashing
A proper password hashing strategy includes:
1. **Salting** — random per-user data
2. **Peppering** — secret server-side data
3. **Key stretching** — intentional slowness
4. **Adaptive cost** — increase over time
5. **Memory hardness** — resist GPU/ASIC attacks
## Visual Comparison
```text
INSECURE:
password ──► SHA-256 ──► same hash every time
                ▼
        [trivially attacked]

SECURE:
password + unique_salt + server_pepper
        │
        ▼
   Argon2id (high cost, memory hard)
        │
        ▼
   unique hash per user ✅
```
## Key Concepts
### Salting
A unique random value added to each password before hashing.
### Peppering
A secret value stored separately from the database (e.g., HSM, env var).
### Key Stretching
Iterating the hash function many times to slow down attackers.
### Work Factor
A tunable parameter that controls computational cost.
### Adaptive Hashing
Ability to increase cost over time as hardware improves.

---

# 🔒 Password Hashing Algorithms
## Comparison Table
| Algorithm | Year | Memory Hard | Tunable | Recommended |
|-----------|------|-------------|---------|-------------|
| PBKDF2 | 2000 | ❌ No | Iterations | ✅ (NIST approved) |
| bcrypt | 1999 | ❌ No | Cost factor | ✅ (legacy choice) |
| scrypt | 2009 | ✅ Yes | N, r, p | ✅ (good) |
| Argon2d | 2015 | ✅ Yes | memory, time, parallelism | ⚠️ Side-channel |
| Argon2i | 2015 | ✅ Yes | memory, time, parallelism | ⚠️ Slower |
| **Argon2id** | 2015 | ✅ Yes | memory, time, parallelism | ✅ **Best choice** |
## PBKDF2 (Password-Based Key Derivation Function 2)
* **Standard**: RFC 2898, NIST approved
* **Inputs**: Password, salt, iterations, length
* **Weakness**: Not memory-hard — vulnerable to GPU/ASIC attacks
* **Recommended**: At least **600,000 iterations** (OWASP 2023)
```python
import hashlib
hash = hashlib.pbkdf2_hmac(
    'sha256',
    b'password',
    salt,
    iterations=600_000
)
```
## bcrypt
* **Origin**: Based on Blowfish cipher (1999)
* **Cost factor**: Logarithmic (4–31); recommended 12+
* **Salt**: Built-in, 128-bit
* **Weakness**: Limited memory hardness (only 4KB)
```javascript
const bcrypt = require('bcrypt');
const hash = await bcrypt.hash('password', 12);
```
## scrypt
* **Memory hardness**: Yes — configurable memory cost
* **Parameters**: N (CPU/memory), r (block size), p (parallelism)
* **Use case**: When Argon2 isn't available
```python
import hashlib
hash = hashlib.scrypt(b'password', salt=salt, n=2**14, r=8, p=1)
```
## Argon2
Winner of the **Password Hashing Competition (2015)**.
### Variants
* **Argon2d**: Data-dependent memory access (fast but side-channel vulnerable)
* **Argon2**i: Data-independent memory access (side-channel resistant, slower)
* **Argon2id**: Hybrid — recommended for most uses
### Recommended Parameters (OWASP, 2023+)
| Parameter | Recommended |
|-----------|-------------|
| Memory | 19 MiB |
| Iterations | 2 |
| Parallelism | 1 |
| Output length | 32 bytes |
```python
import argon2

ph = argon2.PasswordHasher(
    memory_cost=19456,
    time_cost=2,
    parallelism=1
)
hash = ph.hash('password123')
```
## Algorithm Selection Guide
```text
                   Need to choose?
                         │
                         ▼
              ┌─────────────────────┐
              │ Is the system NEW?  │
              └─────────┬───────────┘
                        │
              ┌─────────┴─────────┐
              │                   │
             YES                NO
              │                   │
              ▼                   ▼
         Argon2id        bcrypt (if Argon2 unavailable)
              │                   │
              ▼                   ▼
       Tune params      Upgrade over time
```

---

# 🌶️ Pepper Explained
## What is a Salt?
A **salt** is a random, unique value added to a password before hashing.
```text
password = "hunter2"
salt     = "x9f4a2b" (random, unique per user)
hash     = Argon2id(password + salt)
```
## Why Salts Exist
Without salts:
```text
User 1: "password123" → MD5 → 482c811da5d5b4bc6d497ffa98491e38
User 2: "password123" → MD5 → 482c811da5d5b4bc6d497ffa98491e38
                              ↑ SAME HASH!
```
With salts:
```text
User 1: "password123" + "abc123" → unique hash
User 2: "password123" + "xyz789" → different unique hash
```
## Salt Requirements
| Requirement | Reason |
|-------------|--------|
| Unique per user | Prevents hash reuse attacks |
| Random | Unpredictable to attacker |
| Long enough | At least 16 bytes (128 bits) |
| Stored with hash | Needed for verification |
## Database Example
```sql
CREATE TABLE users (
    id           INT PRIMARY KEY,
    username     VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,  -- contains salt + hash
    created_at   TIMESTAMP DEFAULT NOW()
);
```
Example stored hash (Argon2id):
```text
$argon2id$v=19$m=19456,t=2,p=1$Y3VycmVudHNhbHQ$RdescudvJCsgt3ub+b+dWRWJTmaaJObG
```
Format: `$algorithm$version$parameters$salt$hash`
## Best Practices
* ✅ Generate salts with a CSPRNG (`crypto.randomBytes, secrets.token_bytes`)
* ✅ Use at least **16 bytes** of entropy
* ✅ Store salts **with** the hash (not separately)
* ✅ Use Argon2id (which generates salts internally)
* ❌ Never reuse salts across users
* ❌ Never derive salts from usernames

---

# 💣 Hash Collisions
## What is a Pepper?
A **pepper** is a secret value added to a password before hashing, stored **separately** from the database.
```text
password + salt + pepper → Argon2id → hash
                          ↑
              stored in HSM/env/secret manager
```
## Difference from Salt
| Property | Salt | Pepper |
|----------|------|--------|
| Per-user | ✅ Unique | ❌ Same for all users |
| Stored with hash | ✅ Yes | ❌ No (separate) |
| Visibility | Public (in DB) | Secret |
| Purpose | Defeat rainbow tables | Defense in depth |
## Storage Recommendations
* 🔐 **Hardware Security Module** (HSM)
* 🔐 **Secret manager** (HashiCorp Vault, AWS Secrets Manager)
* 🔐 **Environment variables** (less ideal)
* ❌ **Never** in source code or config files
## Security Benefits
Even if an attacker steals the entire database:

* They have hashes + salts but **no pepper**
* Without pepper, brute-forcing is significantly harder
* Combined with Argon2id memory hardness, attack cost is enormous
## Implementation Example
```python
import argon2
import os

PEPPER = os.environ["APP_PEPPER"].encode()  # from secret manager

def hash_password(password: str, salt: bytes) -> str:
    combined = password.encode() + salt + PEPPER
    return argon2.hash(combined)

def verify_password(password: str, salt: bytes, expected_hash: str) -> bool:
    combined = password.encode() + salt + PEPPER
    return argon2.verify(expected_hash, combined)
```
> ⚠️ Caution: Losing the pepper means losing all user passwords. Have a secure backup strategy.

---

# 🛠️ Hash Cracking
## What is a Collision?
A collision occurs when two different inputs produce the same hash output.
```text
H(m₁) = H(m₂)
where m₁ ≠ m₂
```
## Birthday Paradox
In a room of just **23 people**, there's a **50%** chance two share a birthday. Counter-intuitive, but true.

For a hash with `n`-bit output, collisions are likely after approximately `2^(n/2)` attempts.
| Hash | Output bits | Birthday attack complexity |
|------|-------------|----------------------------|
| MD5 | 128 | ~2^64 (still huge, but feasible) |
| SHA-1 | 160 | ~2^80 (theoretical) |
| SHA-256 | 256 | ~2^128 (infeasible) |
## Birthday Attack
Generates many random inputs, hoping two will collide. Success probability follows the birthday paradox.
## Real-World Collisions
### MD5 Collisions (2004–2008)
```python
# Two different binaries with the same MD5 hash
binary_1 = bytes.fromhex("d131dd02c5e6eec4693d9a0698aff95c2fcab58712467eab4004583eb8fb7f89")
binary_2 = bytes.fromhex("d131dd02c5e6eec4693d9a0698aff95c2fcab50712467eab4004583eb8fb7f89")

# MD5(binary_1) == MD5(binary_2)
```
This was used to forge an SSL certificate in **2008**.
## SHA-1 SHAttered (2017)
Google and CWI Amsterdam produced the first practical SHA-1 collision:
```text
Two different PDF files with the same SHA-1 hash:
https://shattered.io/static/shattered-1.pdf
https://shattered.io/static/shattered-2.pdf

Both have SHA-1: 38762cf7f55934b34d179ae6a4c80cadccbb7f0a
```
Cost at the time: ~$110,000 of compute.
### Chosen-Prefix Collisions
Attackers craft two different messages that share the same hash by computing special prefixes — used to forge MD5-signed certificates.
## Impact of Collisions
* ❌ Digital signature forgery
* ❌ Malicious software sharing legitimate hashes
* ❌ Blockchain transaction manipulation (in vulnerable systems)

---

# 🌈 Rainbow Tables
## Attack Methods
### 1. Dictionary Attack
Tests passwords from a wordlist:
```text
rockyou.txt (14 million passwords)
hashcat -m 0 -a 0 hash.txt rockyou.txt
```
### 2. Brute Force
Tries every possible character combination.
```text
Password length: 8 chars
Character set: a-z, A-Z, 0-9, symbols (94 chars)
Total combinations: 94^8 = 6 quadrillion
With SHA-256 on modern GPU: minutes to hours
```
### 3. Rainbow Tables
Pre-computed lookup tables of hashes to plaintexts.
### 4. Hybrid Attacks
Combines dictionary + rules:
```text
password → P@ssw0rd → p@ssw0rd! → P@ssw0rd!1
```
### 5. GPU Cracking
Modern GPUs can compute billions of hashes per second:
| Hash | Hashes/sec (RTX 4090) |
|------|------------------------|
| MD5 | ~80 GH/s |
| SHA-1 | ~20 GH/s |
| SHA-256 | ~5 GH/s |
| bcrypt (cost 12) | ~50 kH/s |
| Argon2id (19MB) | ~50 H/s |
### 6. Password Spraying
Tries common passwords across many accounts to avoid lockouts.
### 7. Credential Stuffing
Uses leaked credentials from one breach on other sites.
## Defense Pyramid
```text
Strong password
    │
    ▼
Unique per site
    │
    ▼
Multi-factor auth
    │
    ▼
Argon2id hashing
    │
    ▼
Salt + Pepper
    │
    ▼
Rate limiting
```

---

# SECTION 19 — Rainbow Tables
## What are Rainbow Tables?
Pre-computed tables mapping hashes back to plaintexts, using **reduction functions** and chains to compress storage.
## How They Work
```text
Start ──► Hash ──► Reduce ──► Hash ──► Reduce ──► ... ──► End
   │                                                           │
   └──────────── Stored: (Start, End) ─────────────────────────┘
```
To find a hash:

* Apply reductions and hashes until you match a stored endpoint
* Re-derive the chain from the stored start
* The hash's plaintext is somewhere in the chain
## Generation
Rainbow tables for common algorithms can take **weeks** to generate but enable **instant lookups**.
## Why Salts Defeat Rainbow Tables
```text
Without salt:
  hash("password") = 5f4dcc3b5aa765d61d8327deb882cf99
                                         ↑
                              Same for all users
                              Lookup: instant ❌

With salt:
  hash("password" + "userA_salt") = xyz
  hash("password" + "userB_salt") = abc
                                         ↑
                              Different for each user
                              Must pre-compute for EACH salt
                              Cost: 2^128x more storage ❌
```
## Salt Defeats Rainbow Tables Because
1. Each user has a **unique salt**
2. Attackers must generate a **separate table per salt**
3. With 16-byte salts, the storage requirement becomes **physically impossible**
> ✅ With salts, attackers must fall back to brute-force per user — which is slow when using Argon2id.

---

# ✅ File Integrity Verification
## Why Verify Downloads?
Without verification, you can't tell if a download was:

* Corrupted during transfer
* Tampered with by a man-in-the-middle
* Replaced with malware
## SHA-256 Verification Example
### Step 1: Download the file and the official checksum
```bash
$ wget https://releases.ubuntu.com/22.04/ubuntu-22.04-desktop-amd64.iso
$ wget https://releases.ubuntu.com/22.04/SHA256SUMS
```
### Step 2: Compute the checksum
```bash
$ sha256sum ubuntu-22.04-desktop-amd64.iso
b3a07e89...  ubuntu-22.04-desktop-amd64.iso
```
### Step 3: Compare against official
```bash
$ grep "ubuntu-22.04-desktop-amd64.iso" SHA256SUMS
b3a07e89...  ubuntu-22.04-desktop-amd64.iso
```
Match? ✅ File is authentic.
## Software Release Verification
Many projects sign their SHA-256SUMS with GPG:
```bash
$ gpg --verify SHA256SUMS.gpg SHA256SUMS
$ sha256sum -c SHA256SUMS
```
## Operating Systems
* **Debian/Ubuntu**: `apt` verifies package integrity with SHA-256
* **Fedora/RHEL**: RPM packages use SHA-256
* **macOS**: Code signing uses SHA-256
## Package Managers
```bash
# npm uses SHA-512 integrity hashes
$ npm view express dist.integrity
sha512-xyz...

# pip verifies package hashes
$ pip install --require-hashes -r requirements.txt
```
## Git Commit Hashing
Git uses SHA-1 (migrating to SHA-256) to identify every object:
```bash
$ git hash-object hello.txt
ce013625030ba8dba906f756967f9e9ca394464a
```
## Docker Image Verification
```bash
$ docker pull alpine@sha256:abc123...
```
The `sha256:...` digest guarantees the exact image contents.

---

# ✍️ Digital Signatures
## How Hashing Enables Signatures
Digital signatures combine hashing with asymmetric cryptography:
```text
┌─────────────────────────────────────────────────────────────┐
│                    SIGNING PROCESS                            │
└─────────────────────────────────────────────────────────────┘

Document (large)
      │
      ▼
   SHA-256  ─────► Digest (32 bytes)
      │                │
      │                ▼
      │         Sign with Private Key
      │                │
      │                ▼
      │           Signature
      │                
      ▼
   Original document + Signature sent
```
```text
┌─────────────────────────────────────────────────────────────┐
│                   VERIFICATION PROCESS                        │
└─────────────────────────────────────────────────────────────┘

Document ───► SHA-256 ───► Digest A
                                │
Signature ──► Verify with Public Key ──► Digest B
                                          │
                                          ▼
                              Digest A == Digest B?
                                  ✅ Yes  → Authentic
                                  ❌ No   → Tampered
```
## Why Hash First?
* ✅ **Efficiency**: Signing a 32-byte digest is faster than signing megabytes
* ✅ **Security**: Hash functions are designed to expose any tampering
* ✅ **Compatibility**: Same digest format for any document size
## Relationship to Certificates and PKI
```text
Certificate Hierarchy:
Root CA (self-signed, SHA-256)
    │
    ├──► Intermediate CA (signed by Root)
    │        │
    │        └──► Server Certificate (signed by Intermediate)
    │                  │
    │                  └──► TLS Connection
    │
Each signature in the chain uses SHA-256.
```

---

# ⛓️ Blockchain
## How Hashing Secures Blockchain
Hash functions are the **backbone** of blockchain technology.
## Bitcoin's Use of SHA-256
### Block Hashing
```text
Block Header:
- Version
- Previous Block Hash
- Merkle Root
- Timestamp
- Difficulty Target
- Nonce

SHA-256(SHA-256(block_header)) = Block Hash
```
### Mining (Proof of Work)
Miners search for a `nonce` that makes the block hash start with many zeros:
```text
Target: hash < 0x0000000000000000000a3b2c...

Block Header = version + prev_hash + merkle_root + time + bits + nonce

For nonce in range(0, 2^32):
    if SHA-256(SHA-256(header)) < target:
        return nonce  ← FOUND IT!
```
Difficulty adjusts every 2016 blocks to maintain ~10-minute block times.
## Merkle Trees
```text
                Root Hash
               /         \
         H(AB)           H(CD)
         /   \           /   \
       H(A)  H(B)     H(C)  H(D)
        |     |        |     |
       TxA   TxB      TxC   TxD
```
Changing any transaction invalidates the entire tree up to the root.
## Ethereum's Use
**Block hashing**: Keccak-256 (SHA-3 variant)
**Account addressing**: Last 20 bytes of Keccak-256(public_key)
**State tri**: Merkle Patricia Trie with Keccak-256

---

# 🌳 Merkle Trees
## Structure
A **Merkle Tree** is a binary tree where:

* **Leaves** = hashes of data blocks
* **Internal nodes** = hashes of concatenated child hashes
* **Root** = single hash representing all data
```text
                      Root
                    /      \
                 H(AB)      H(CD)
                /    \      /    \
              H(A)  H(B)  H(C)  H(D)
               |     |     |     |
              Tx1   Tx2   Tx3   Tx4
```
## Verification (Merkle Proof)
To prove `Tx2` is in the tree, provide:
```text
H(Tx2), H(AB), H(CD)
Verifier computes:
H(Tx2) → H(AB) [with H(A)] → Root [with H(CD)]
Compare to known Root.
```
This requires only `O(log n)` hashes — efficient for large datasets.
## Applications
| Domain | Use |
|--------|-----|
| Bitcoin | Transaction inclusion |
| Git | Content-addressable storage |
| IPFS | File deduplication |
| Certificate Transparency | Log integrity |
| Databases | Verifiable queries |
| Cloud storage | Integrity proofs |

---

# 📦 Hash-Based Data Structures
## Hash Tables
A data structure mapping keys to values using a hash function:
```text
Key: "alice" ──► Hash("alice") = 42 ──► Index 42 in array
```
```text
Array index: 0    1    2    ...   42   ...  99
Content:    [ ] [ ] [ ]      ["alice"]
```
## Properties
* **Average lookup**: O(1)
* **Worst case**: O(n) (collisions)
* **Collisions handled by**: chaining or open addressing

## Hash Maps vs Hash Sets
| Structure | Stores | Allows Duplicates |
|-----------|--------|-------------------|
| Hash Map | Key-value pairs | Keys unique |
| Hash Set | Keys only | No duplicates |
## Consistent Hashing
Used in **distributed systems** for scalable key distribution:
```text
         Hash Ring (0 ───── 2^32)

        Node A
         ●  hash("A")
              \
               \
        ● hash("user1")  ● hash("user2")
               \           /
                \         /
                 ● hash("B") Node B
```
When a node is added/removed, only `1/n` of keys need to be remapped.
## Real-World Uses
* **Amazon DynamoDB / Cassandra**: Consistent hashing for partitioning
* **CDNs**: Mapping requests to edge servers
* **Distributed caches**: Memcached, Redis cluster

---

# 🌍 Real-World Applications
## TLS / HTTPS
* **Handshake**: TLS 1.3 uses SHA-256 for HMAC and key derivation
* **Certificates**: Signed with SHA-256
* **Session keys**: Derived using HKDF with SHA-256
## JWT (JSON Web Tokens)
```json
{
  "alg": "HS256",
  "typ": "JWT",
  "payload": { "user": "alice", "exp": 1699999999 }
}
```
The signature uses `HMAC-SHA256(payload, secret)`.
## Git
* Every object (commit, tree, blob) is identified by its hash
* Currently uses SHA-1 (with collision detection)
* Migrating to SHA-256
## Docker
* Image layers identified by SHA-256 digests
* Content-addressable storage
## Package Managers
* **npm**: SHA-512 integrity hashes
* **pip**: `--require-hashes` mode
* **apt**: SHA-256 verification
* **Homebrew**: SHA-256 checksums
## Malware Detection
Antivirus vendors maintain hash databases of known malware:
```text
# ClamAV signature database uses MD5/SHA-256 hashes
$ clamscan --database malware.hdb infected_file.exe
```
## Digital Forensics
Investigators hash evidence to prove integrity:
```text
Evidence: disk.dd
Hash (at acquisition): a1b2c3...
Hash (at trial):      a1b2c3...  ← matches → evidence untampered
```
## Cloud Security
* AWS S3 stores object hashes for integrity
* Azure Blob Storage supports customer-provided encryption keys + hashes
## Databases
PostgreSQL: `pgcrypto` extension provides digest() for hashing
Used for password storage, data anonymization
## Password Managers
1Password, Bitwarden, KeePass use Argon2id (or PBKDF2)
Master password derives encryption key via KDF

---

# 💻 Hashing in Programming
## Python
```python
import hashlib
import argon2

# SHA-256
msg = b"Hello World"
sha256 = hashlib.sha256(msg).hexdigest()
print(f"SHA-256: {sha256}")

# SHA-512
sha512 = hashlib.sha512(msg).hexdigest()

# BLAKE2 (fast)
blake2b = hashlib.blake2b(msg).hexdigest()

# HMAC
import hmac
mac = hmac.new(b"secret_key", msg, hashlib.sha256).hexdigest()

# Password hashing with Argon2id
ph = argon2.PasswordHasher()
hash = ph.hash("user_password")
ph.verify(hash, "user_password")  # raises if invalid
```
## JavaScript (Node.js)
```Javascript
const crypto = require('crypto');
const argon2 = require('argon2');

// SHA-256
const sha256 = crypto.createHash('sha256')
                     .update('Hello World')
                     .digest('hex');

// SHA-512
const sha512 = crypto.createHash('sha512')
                     .update('Hello World')
                     .digest('hex');

// HMAC
const hmac = crypto.createHmac('sha256', 'secret_key')
                   .update('Hello World')
                   .digest('hex');

// Password hashing with Argon2id
async function hashPassword(password) {
    return await argon2.hash(password, { type: argon2.argon2id });
}
```
## Go
```go
package main

import (
    "crypto/sha256"
    "crypto/hmac"
    "crypto/rand"
    "encoding/hex"
    "golang.org/x/crypto/argon2"
)

func sha256Hash(data []byte) string {
    sum := sha256.Sum256(data)
    return hex.EncodeToString(sum[:])
}

func hashPassword(password string) string {
    salt := make([]byte, 16)
    rand.Read(salt)
    
    key := argon2.IDKey([]byte(password), salt, 2, 19456, 1, 32)
    return hex.EncodeToString(salt) + ":" + hex.EncodeToString(key)
}
```
## Java
```Java
import java.security.MessageDigest;
import de.mkammerer.argon2.Argon2;
import de.mkammerer.argon2.Argon2Factory;

public class Hashing {
    public static String sha256(String input) throws Exception {
        MessageDigest md = MessageDigest.getInstance("SHA-256");
        byte[] digest = md.digest(input.getBytes());
        return HexFormat.of().formatHex(digest);
    }
    
    public static String hashPassword(String password) {
        Argon2 argon2 = Argon2Factory.create(Argon2Types.ARGON2id);
        return argon2.hash(2, 19456, 1, password.toCharArray());
    }
}
```
## Rust
```rust
use sha2::{Sha256, Digest};
use argon2::{Argon2, PasswordHasher, PasswordVerifier, SaltString};

fn sha256_hash(input: &str) -> String {
    let mut hasher = Sha256::new();
    hasher.update(input);
    format!("{:x}", hasher.finalize())
}

fn hash_password(password: &str) -> Result<String, argon2::password_hash::Error> {
    let salt = SaltString::generate(&mut argon2::password_hash::rand_core::OsRng);
    let argon2 = Argon2::default();
    Ok(argon2.hash_password(password.as_bytes(), &salt)?.to_string())
}
```
## C#
```csharp
using System.Security.Cryptography;
using Konscious.Security.Cryptography;

string sha256(string input) {
    using var sha = SHA256.Create();
    byte[] bytes = sha.ComputeHash(System.Text.Encoding.UTF8.GetBytes(input));
    return Convert.ToHexString(bytes);
}

string HashPassword(string password) {
    var salt = RandomNumberGenerator.GetBytes(16);
    using var argon2 = new Argon2id(System.Text.Encoding.UTF8.GetBytes(password)) {
        Salt = salt,
        DegreeOfParallelism = 1,
        MemorySize = 19456,
        Iterations = 2
    };
    return Convert.ToBase64String(argon2.GetBytes(32));
}
```
## PHP
```php
// SHA-256
$hash = hash('sha256', 'Hello World');

// Argon2id
$hash = password_hash('password', PASSWORD_ARGON2ID, [
    'memory_cost' => 19456,
    'time_cost'   => 2,
    'threads'     => 1
]);

// Verify
password_verify('password', $hash);
```

---

# 📈 Performance
## Benchmark Comparison (Approximate, single-threaded, modern x86-64)
| Algorithm | Throughput | Relative Speed |
|-----------|------------|----------------|
| MD5 | ~700 MB/s | 100x |
| SHA-1 | ~400 MB/s | 60x |
| SHA-256 | ~300 MB/s | 45x |
| SHA-512 | ~600 MB/s | 90x |
| SHA-3-256 | ~200 MB/s | 30x |
| BLAKE2b | ~900 MB/s | 130x |
| BLAKE3 (single core) | ~1,500 MB/s | 215x |
| BLAKE3 (multi-core) | ~10,000 MB/s | 1,400x |
| bcrypt (cost 12) | ~50 hashes/s | — |
| Argon2id (19 MiB, t=2) | ~50 hashes/s | — |
## Key Takeaways
* ✅ **BLAKE3** is the fastest secure hash function today
* ✅ **SHA-512** is faster than SHA-256 on 64-bit hardware
* ❌ **bcrypt and Argon2id** are intentionally slow (good for passwords)
* ❌ **MD5/SHA-1** are fast but **insecure**
## Choosing Based on Performance
```text
Need raw speed for integrity? ──► BLAKE3
Need compatibility?           ──► SHA-256
Hashing passwords?            ──► Argon2id
Hardware constraints?         ──► SHA-256 (universal)
```

---

# ⭐ Best Practices
## Algorithm Selection
## ✅ DO:

* Use **SHA-256** or **SHA-3-256** for general integrity
* Use **BLAKE3** for new high-performance applications
* Use **Argon2id** for password storage
* Use **HMAC-SHA-256** for message authentication
* Use **HMAC-SHA-512** for high-security MACs
## ❌ DON'T:

* Use **MD5** for any security purpose
* Use **SHA-1** for new applications
* Use raw **SHA-256** for passwords
* Roll your own hash function
Use CRC32 for security
## Password Storage
```python
# ✅ CORRECT
hash = argon2.PasswordHasher().hash(password)

# ❌ WRONG
hash = hashlib.sha256(password.encode()).hexdigest()
hash = hashlib.md5(password.encode()).hexdigest()
hash = hashlib.sha1(password.encode() + salt).hexdigest()
```
## General Security
* ✅ Always use HTTPS (TLS 1.3)
* ✅ Keep cryptographic libraries updated
* ✅ Use constant-time comparison for hash/MAC checks
* ✅ Protect secrets (keys, peppers) in secret managers
* ✅ Implement rate limiting on authentication
* ✅ Log security events
* ✅ Use MFA wherever possible
* ✅ Audit dependencies regularly
## Code Review Checklist

[] No MD5 or SHA-1 in security contexts

[] Passwords use Argon2id / bcrypt / scrypt

[] Each password has unique salt

[] Pepper is stored separately

[] HMAC uses secure compare

[] Constant-time comparisons for sensitive data

[] Library versions are recent

[] No custom crypto

---

# ❌ Common Mistakes
## Mistake 1: Storing Plain Passwords
```python
# ❌ NEVER DO THIS
db.users.insert({"username": "alice", "password": "password123"})
```
## Mistake 2: Using Fast Hashes for Passwords
```python
# ❌ Wrong — too fast, vulnerable to brute force
hash = hashlib.sha256(password + salt).hexdigest()
```
## Mistake 3: Reusing Salts
```python
# ❌ Same salt for all users = rainbow table risk
SALT = b"my_fixed_salt"
```
## Mistake 4: Ignoring Collisions
Assuming that hashes are always unique leads to vulnerabilities in signature systems.
## Mistake 5: Using Deprecated Algorithms
```python
# ❌ Deprecated
import md5
import sha
```
## Mistake 6: Storing Hashes Without Salt
```sql
-- ❌ Vulnerable to rainbow tables
INSERT INTO users (username, password_hash) VALUES ('alice', '5f4dcc3b...');
```
## Mistake 7: Using Username as Salt
```python
# ❌ Predictable, not unique
salt = username.encode()
```
## Mistake 8: Comparing Hashes with `==`
```python
# ❌ Vulnerable to timing attacks
if computed_hash == stored_hash:
    return True

# ✅ Use constant-time comparison
import hmac
if hmac.compare_digest(computed_hash, stored_hash):
    return True
```
## Mistake 9: Custom Hash Functions
Never invent your own hash function. Cryptanalysis requires decades of peer review.

## Mistake 10: No Upgrade Path
Choose algorithms that allow tuning parameters as hardware improves (Argon2id, bcrypt).

---

# ❓ Frequently Asked Questions
## Beginner Questions
### Q1: What is a hash function in simple terms?
A hash function is a mathematical function that converts any input into a fixed-size string of characters, which represents the data uniquely.

### Q2: Can a hash be reversed?
No — cryptographic hashes are designed to be one-way. You cannot feasibly recover the original input from the hash.

### Q3: Why are hashes important?
They enable secure password storage, data integrity verification, digital signatures, and many other security features.

### Q4: What's the difference between hashing and encryption?
Hashing is one-way (cannot be reversed). Encryption is two-way (can be reversed with a key).

### Q5: Is MD5 ever safe to use?
Only for non-security purposes like file corruption detection. Never for passwords, signatures, or integrity.

### Q6: What hash should I use for files?
SHA-256 is the universal standard. BLAKE3 if you need speed.

### Q7: What's a salt?
A random value added to a password before hashing to prevent identical passwords from producing identical hashes.

### Q8: What's a pepper?
A secret value added to a password and stored separately from the database, adding a second layer of defense.

### Q9: What's the strongest hash function?
SHA-3-512, SHA-512, and BLAKE3 are all considered cryptographically strong.

### Q10: Why is SHA-256 used in Bitcoin?
It provides strong collision resistance, is widely audited, and ASICs can be built to compute it efficiently for mining.
## Intermediate Questions
### Q11: What is the avalanche effect?
A small change in input should produce a large change in output (ideally ~50% of bits flip).

### Q12: What is HMAC?
Hash-based Message Authentication Code — uses a hash function with a secret key to verify both integrity and authenticity.

### Q13: Can two different files have the same SHA-256 hash?
In theory, yes (Pigeonhole principle), but finding such a collision is computationally infeasible.

### Q14: What is a Merkle tree?
A tree of hashes where each leaf is a data hash and each parent is the hash of its children. Used for efficient verification.

### Q15: What is key stretching?
Repeatedly applying a hash function to slow down brute-force attacks, common in password hashing.

### Q16: How long should a salt be?
At least 16 bytes (128 bits) of random data per user.

### Q17: What is HMAC-SHA256?
A specific construction combining HMAC with SHA-256, widely used in JWT and API authentication.

### Q18: What's the difference between SHA-256 and SHA-3-256?
SHA-256 uses Merkle–Damgård; SHA-3 uses sponge construction. Both produce 256-bit outputs but differ internally.

### Q19: Can quantum computers break SHA-256?
Grover's algorithm reduces security to 128 bits — still considered secure, but SHA-384/SHA-512 may be preferred for long-term security.

### Q20: What's a nonce in mining?
A number miners vary to find a hash below the target difficulty.

## Advanced Questions
### Q21: What is a length extension attack?
An attack against SHA-2 (and SHA-1) where given H(m) and len(m), an attacker can compute H(m || padding || m') without knowing m. Use HMAC or SHA-3 to avoid.

### Q22: What are sponge functions?
A construction used in SHA-3 that processes input by absorbing into state and producing output by squeezing from state.

### Q23: What is domain separation?
Including a context tag in the hash input to prevent cross-protocol attacks.

### Q24: What is a KDF (Key Derivation Function)?
A function that derives cryptographic keys from input secrets (e.g., HKDF, Argon2id).

### Q25: Why is Argon2id better than bcrypt?
Argon2id is memory-hard, making it resistant to GPU/ASIC attacks. bcrypt has only 4KB of memory usage.

### Q26: What is a chosen-prefix collision?
A collision attack where the attacker can choose different prefixes for two messages, then append colliding content.

### Q27: How does Git handle SHA-1 collisions?
Git detects collisions during write operations and aborts, mitigating many attack scenarios.

### Q28: What is HKDF?
HMAC-based Key Derivation Function — used to derive multiple keys from a single secret.

### Q29: What's the difference between HMAC and a plain hash?
HMAC includes a secret key, providing authenticity in addition to integrity.

### Q30: Will SHA-256 still be secure in 20 years?
Current analysis suggests yes, but NIST recommends planning for migration to SHA-384/SHA-512 or SHA-3 for long-term security.

---

# 🚀 Future of Hash Functions
## Post-Quantum Cryptography
Quantum computers threaten current asymmetric cryptography (RSA, ECDSA), but their impact on hash functions is more limited:
* **Grover's algorithm** reduces brute-force security from `2^n` to `2^(n/2)`
* SHA-256 effectively becomes 128-bit secure — still infeasible
* **Recommendation**: SHA-384 or SHA-512 for long-term security
## NIST PQC Standardization
While NIST's PQC competition focused on signatures and KEMs (Kyber, Dilithium), hash functions remain essential:
* Stateless hash-based signatures: **XMSS, LMS** (RFC 8391)
* SPHINCS+ — stateless hash-based signature scheme
## Emerging Algorithms
**BLAKE3** continues gaining adoption for performance-critical applications
**Parallel hash functions** for multi-core systems
**Quantum-resistant hash constructions** under research
## Hardware Acceleration
Modern CPUs include dedicated SHA instructions:
* **Intel SHA Extensions** (SHA-1, SHA-256)
* **ARMv8 SHA-256 instructions**
* **Apple Silicon** hardware SHA
## Cloud Computing
* HSMs (Hardware Security Modules) for key/pepper protection
* Cloud KMS integrating hashing primitives
* Serverless hashing with managed services
## AI and ML Security
Hashing is critical for:

* Model integrity verification
* Dataset fingerprinting
* Tamper detection in ML pipelines
## Zero Trust Architecture
In Zero Trust models, every request must be verified:

* HMAC-based request signing
* Hash-based integrity checks
* Continuous authentication
## Future Standards (Anticipated)
* NIST SP 800-208: Stateful hash-based signatures
* Continued evolution of Argon2 parameters
* Standardization of memory-hard primitives

---

# 📝 Summary
Hash functions are the **unsung heroes of modern cybersecurity**. They quietly secure passwords, verify downloads, power blockchains, and underpin digital signatures. Understanding them is essential for any security professional.
## Key Takeaways
🔑 **Hash Functions** convert any input into a fixed-size output, irreversibly.

🔒 **Cryptographic Properties** (pre-image resistance, collision resistance, avalanche effect) make them suitable for security.

📚 **Algorithm Evolution** has gone from MD5 (broken) → SHA-1 (broken) → SHA-2/SHA-3 (secure) → BLAKE3 (fast & secure).

🔐 **Password Hashing** is its own discipline — use Argon2id with salts (and ideally peppers).

⚡ **Performance matters**: BLAKE3 for speed, SHA-256 for compatibility, Argon2id for passwords.

✅ **Best Practices** include using modern algorithms, salting passwords, and avoiding deprecated hashes.

🚫 **Common Mistakes** include MD5 for passwords, missing salts, and custom crypto.

🌐 **Applications** span every layer of computing: from JWT to Git, TLS to blockchain, forensics to file integrity.

As you build secure systems, remember:
> **Hashing ≠ Encryption**. Choose the right tool for the job, follow current best practices, and stay updated as cryptography evolves.

---

# 📚 Glossary
**Algorithm** — A well-defined procedure or formula for solving a problem.

**Argon2id** — The recommended variant of the Argon2 password hashing algorithm, combining resistance to side-channel and GPU attacks.

**Avalanche Effect** — Property where small input changes cause large, unpredictable output changes.

**Birthday Attack** — Cryptographic attack exploiting the birthday paradox to find collisions.

**BLAKE2** — Fast and secure cryptographic hash function, alternative to SHA-3.

**BLAKE3** — Successor to BLAKE2 with parallel processing support.

**Block (in hashing)** — Fixed-size chunk of input processed by the compression function.

**Brute Force** — Trying all possible inputs until finding one that matches.

**Checksum** — Small value computed from data to detect errors, not secure.

**Collision** — When two different inputs produce the same hash output.

**Collision Resistance** — Property making it hard to find collisions.

**Compression Function** — Core component that processes each message block.

**Constant-Time Comparison** — Comparing values in time independent of content, preventing timing attacks.

**CRC (Cyclic Redundancy Check)** — Non-cryptographic checksum used in networks and storage.

**Cryptographic Hash Function** — Hash function with security properties for cryptographic use.

**Deterministic** — Always producing the same output for the same input.

**Digest** — The output of a hash function.

**Digital Signature** — Cryptographic proof of authenticity using hashing and asymmetric crypto.

**Encoding** — Reversible transformation for data representation (e.g., Base64), not for security.

**Encryption** — Reversible transformation using a key, for confidentiality.

**FIPS** — Federal Information Processing Standards (US government).

**Hash** — The output of a hash function, also the function itself.

**HKDF** — HMAC-based Key Derivation Function (RFC 5869).

**HMAC** — Hash-based Message Authentication Code (RFC 2104).

**IV (Initialization Vector)** — Starting value for hash function state.

**Keccak** — The cryptographic primitive underlying SHA-3.

**Key Stretching** — Repeatedly applying a hash function to slow down attacks.

**Merkle Tree** — Tree of hashes enabling efficient data verification.

**MD5** — Deprecated cryptographic hash function (broken).

**Nonce** — Number used once, varied in mining and signing.

**One-Way Function** — Function that is easy to compute but hard to reverse.

**PBKDF2** — Password-Based Key Derivation Function 2 (RFC 2898).

**Pepper** — Secret value added to password, stored separately.

**Pre-image Resistance** — Hard to find input that produces a given hash.

**Proof of Work** — System requiring computational work (often hashing) to deter abuse.

**Rainbow Table** — Pre-computed table of hashes to plaintexts.

**RIPEMD-160** — 160-bit hash function used in Bitcoin addresses.

**Salt** — Random per-user value added to passwords before hashing.

**SHA (Secure Hash Algorithm)** — Family of NIST-standardized hash functions.

**SHA-1** — Deprecated; collisions demonstrated in 2017.

**SHA-2** — Family including SHA-256, SHA-384, SHA-512.

**SHA-3** — NIST-standardized hash based on Keccak/sponge construction.

**Sponge Construction** — SHA-3's underlying design with absorbing and squeezing phases.

**Tiger** — Older 192-bit hash function, now legacy.

**Whirlpool** — 512-bit hash function, ISO/IEC 10118-3 standard.

---

# 📖 References
## Standards and Specifications
* **NIST FIPS 180-4** — Secure Hash Standard (SHA-1, SHA-2)
* **NIST FIPS 202** — SHA-3 Standard (Permutation-Based Hash)
* **NIST SP 800-107** — Recommendation for Applications Using Approved Hash Algorithms
* **NIST SP 800-117** — Guide to Securing WiMAX Systems
* **NIST SP 800-208** — Recommendation for Stateful Hash-Based Signature Schemes
* **RFC 1321** — The MD5 Message-Digest Algorithm
* **RFC 2104** — HMAC: Keyed-Hashing for Message Authentication
* **RFC 2898** — PKCS #5: Password-Based Cryptography Specification
* **RFC 5869** — HMAC-based Extract-and-Expand Key Derivation Function (HKDF)
* **RFC 6234** — US Secure Hash Algorithms (SHA and SHA-based HMAC and HKDF)
* **RFC 7914** — The scrypt Password-Based Key Derivation Function
* **RFC 8018** — PKCS #5: Password-Based Cryptography Specification v2.1
* **RFC 9106** — Argon2 Memory-Hard Function
## Best Practices
* [**OWASP Password Storage Cheat Sheet**](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
* [**OWASP Cryptographic Storage Cheat Sheet**](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
* OWASP Authentication Cheat Sheet
## Academic Papers
* **Damgård**, I.B. (1989). A Design Principle for Hash Functions.
* **Merkle, R.C.** (1989). One Way Hash Functions and DES.
* **Bertoni, G., Daemen, J., Peeters, M., Van Assche, G.** (2007-2011). Keccak sponge function family.
* **Biryukov, A., Dinu, D., Khovratovich, D.** (2016). Argon2: New Generation of Memory-Hard Functions.
* **Aumasson, J.P., Neves, S., Wilcox-O'Hearn, Z., Winnerlein, C.** (2013). BLAKE2: Simpler, Smaller, Fast as MD5.
## Books
* **Katz, J., Lindell, Y.** — Introduction to Modern Cryptography
* **Ferguson, N., Schneier, B., Kohno, T.** — Cryptography Engineering
* **Paar, C., Pelzl, J.** — Understanding Cryptography
* **Hoffstein, J., Pipher, J., Silverman, J.H.** — An Introduction to Mathematical Cryptography
## Online Resources
* [**NIST Computer Security Resource Center**](https://csrc.nist.gov/)
* [**IACR (International Association for Cryptologic Research**](https://www.iacr.org/)
* [**Crypto StackExchange**](https://crypto.stackexchange.com/)
* [**Daniel J. Bernstein's Crypto Pages**](https://cr.yp.to/)
## Documentation
* [**Python hashlib**](https://docs.python.org/3/library/hashlib.html)
* [**Node.js crypto**](https://nodejs.org/api/crypto.html)
* [**Go crypto packages**](https://pkg.go.dev/crypto/)
* [**OpenSSL documentation**](https://www.openssl.org/docs/)

---
# Conclusion
Hash functions are a cornerstone of modern cybersecurity — from the passwords that protect your accounts to the blockchain networks that secure digital currency. Whether you're a student learning the fundamentals, a developer implementing authentication, or a security professional hardening systems, a deep understanding of hash functions is indispensable.

Stay curious, stay updated, and always choose your primitives wisely.

### Happy hashing! 🔐
---
This article is intended for educational purposes. Always consult current standards (NIST, OWASP, IETF) and qualified security professionals before making cryptographic decisions in production systems.

---
*Maintained with curiosity by [ItsWanheda](https://github.com/ItsWanheda)*

---

# 🧠 Advanced Learning Extension

> This extension adds implementation-level reasoning, laboratories, review checklists, and protocol-design guidance to the core chapter. It is intentionally structured as a learning reference rather than a list of memorization facts.

## 🧭 1. Learning Objectives — What You Should Be Able to Do

After completing this chapter, you should be able to explain what a cryptographic hash is without confusing it with encryption.

You should be able to distinguish a general-purpose hash from a password hashing function.

You should be able to explain preimage resistance, second-preimage resistance, and collision resistance.

You should be able to explain why collision resistance is approximately a birthday-bound property.

You should be able to choose SHA-256, SHA-512, SHA3-256, SHAKE, BLAKE2, or BLAKE3 for an appropriate engineering context.

You should know why MD5 and SHA-1 are unsuitable for new security-sensitive designs.

You should understand why SHA-256 is not a password-storage algorithm.

You should be able to explain the role of a unique random password salt.

You should be able to explain what a pepper is and where its secret should live.

You should understand HMAC as a keyed construction rather than a plain hash.

You should understand HKDF as a key-derivation construction built from HMAC.

You should recognize when a length-extension attack is relevant.

You should understand why canonicalization and encoding can be as important as the hash itself.

You should know why domain separation prevents accidental cross-protocol reuse.

You should be able to use test vectors to validate an implementation.

You should be able to design a migration path away from a legacy password hash.

You should be able to review a hash-related pull request for common security mistakes.

You should be able to document algorithm choice, parameters, compatibility requirements, and upgrade plans.

## 📐 2. The Mathematical Model

Let M be a finite byte string and let H be a hash function.

The digest is written as H(M) = D.

For SHA-256, D has 256 bits, or 32 bytes.

For SHA3-256, D also has 256 bits, but the internal construction is different.

The domain may be extremely large while the output space is finite.

This immediately implies that collisions must exist for any fixed-length hash.

A collision is a pair M1 != M2 such that H(M1) = H(M2).

Security does not require collisions to be mathematically impossible.

Security requires finding useful collisions to be computationally infeasible under the relevant attack model.

A hash is deterministic: the same bytes and algorithm produce the same digest.

Hashing text requires an encoding decision because characters are not themselves bytes.

UTF-8 is a common interoperable choice, but protocols must define their encoding explicitly.

Whitespace, normalization, line endings, and serialization can therefore change the digest.

Never compare the visual appearance of two strings when the security decision depends on their bytes.

Define the exact byte representation before defining the hash operation.

Cryptographic specifications normally operate on byte strings, not vague notions of human-readable text.

## 🎯 3. Security Properties — Precise Definitions

Preimage resistance asks whether an attacker can find M for a chosen digest D such that H(M) = D.

Second-preimage resistance starts with a known M and asks for a different M2 with H(M2) = H(M).

Collision resistance asks for any two distinct messages that hash to the same digest.

These properties are related but are not interchangeable.

A collision attacker has more freedom than a second-preimage attacker.

A second-preimage attack starts from an existing message chosen outside the attacker's control.

Collision resistance is especially important for digital signatures and certificate-style workflows.

Preimage resistance is relevant when a digest itself is exposed and treated as a commitment.

Second-preimage resistance matters when an existing authenticated object must not be replaceable.

A secure hash also aims for strong diffusion across its output.

The avalanche effect is an observed design property, not a substitute for formal cryptanalysis.

Uniform-looking outputs do not prove cryptographic security.

Fast computation is desirable for integrity hashing but undesirable for password hashing.

Hash output length is a security parameter, not merely a storage-format preference.

Security claims must always be tied to an algorithm, construction, output length, and attack model.

Do not describe a hash as simply 'secure' without explaining what property and use case you mean.

## 🎂 4. Birthday Bound and Collision Probability

For an ideal n-bit hash, there are 2^n possible digest values.

Because multiple messages map into that finite space, collisions eventually become likely.

The birthday phenomenon places the generic collision-search scale around 2^(n/2).

For a 256-bit ideal hash, generic collision search is therefore associated with roughly 2^128 work.

This is not the same as saying a 256-bit hash has only 128 bits of security for every purpose.

Preimage and second-preimage security have different generic bounds.

A collision attack can exploit the freedom to choose both messages.

An application must therefore select a digest length appropriate to its collision-resistance requirement.

The birthday bound is a generic mathematical baseline, not a forecast of the cost of every practical attack.

Real algorithms can have structural attacks that are better than generic search.

Conversely, engineering controls may make a theoretical collision irrelevant to a particular protocol.

Never convert a digest length directly into a universal security rating.

Do not say 'there can never be a collision' for a fixed-length hash.

Instead, say that finding a useful collision is computationally infeasible for the chosen algorithm and threat model.

Longer outputs increase the generic collision-search space.

Algorithm selection and protocol construction still matter even when the digest is long.

## 🔬 5. SHA-2 — Engineering View

SHA-2 is a family, not one single algorithm.

NIST FIPS 180-4 specifies SHA-224, SHA-256, SHA-384, SHA-512, SHA-512/224, and SHA-512/256.

SHA-256 produces a 256-bit digest.

SHA-512 produces a 512-bit digest.

SHA-384 produces a 384-bit digest derived from the SHA-512 family construction.

SHA-512/256 produces a 256-bit digest using a distinct initialization value from SHA-512.

SHA-2 functions use a Merkle-Damgard-style iterative structure.

SHA-256 processes 512-bit message blocks.

SHA-512-family functions process 1024-bit message blocks.

SHA-256 uses 32-bit working words.

SHA-512-family functions use 64-bit working words.

Implementations should normally come from maintained cryptographic libraries.

Do not implement SHA-256 from a textbook for production authentication or integrity controls.

Use official test vectors when validating an implementation or interoperability layer.

NIST has announced a revision of FIPS 180-4, so standards tracking should be part of long-lived engineering work.

## 🧽 6. SHA-3 — Sponge Construction

SHA-3 is based on Keccak and was standardized by NIST in FIPS 202.

SHA3-224, SHA3-256, SHA3-384, and SHA3-512 are fixed-output hash functions.

SHAKE128 and SHAKE256 are extendable-output functions.

An XOF can produce output of a caller-selected length.

SHA-3 uses a sponge construction rather than the SHA-2 Merkle-Damgard construction.

The sponge has a state divided conceptually into rate and capacity.

During absorption, input is incorporated into the state through repeated permutations.

During squeezing, output is read from the state and more permutation rounds are applied as needed.

The construction provides a useful architectural diversity from SHA-2.

SHA-3 does not automatically make SHA-2 obsolete.

NIST explicitly describes SHA-3 as a supplement to SHA-1 and SHA-2.

Applications should select based on requirements, interoperability, library support, and protocol specifications.

SHAKE can be useful when a protocol needs a variable-length digest or derived byte stream.

Do not invent a SHAKE truncation convention without specifying it precisely.

Record the algorithm, output length, encoding, and domain/context rules in the protocol.

## 🧩 7. SHAKE and Extendable-Output Functions

An ordinary fixed-length hash has a predetermined digest size.

An XOF allows the caller to request a chosen number of output bytes.

SHAKE128 and SHAKE256 are standardized by NIST FIPS 202.

An XOF should not be treated as a magical source of unlimited independent entropy.

Its security properties depend on the construction, requested length, and protocol use.

Protocol designers must specify exactly how many bytes are consumed.

If several values are derived from one XOF invocation, define their boundaries explicitly.

Domain separation is especially useful when one primitive serves multiple protocol roles.

A context string can distinguish one derivation from another.

Do not silently truncate or concatenate XOF outputs in ways the protocol does not define.

Test vectors should cover short outputs, long outputs, empty inputs, and context values where applicable.

XOFs can simplify constructions that would otherwise need several fixed-length hashes.

They are especially attractive in modern cryptographic protocols requiring flexible output lengths.

Use a well-maintained implementation rather than coding Keccak permutations yourself.

Document whether the protocol uses SHAKE128 or SHAKE256 and why.

## 🔑 8. HMAC — Hashing With a Secret Key

HMAC is a keyed message-authentication construction defined by RFC 2104.

It combines a secret key with a cryptographic hash function.

HMAC provides message authentication and integrity when the secret key is protected.

A plain hash does not prove who created the message.

Anyone who can calculate H(M) can calculate the same plain digest.

With HMAC, an attacker without the key should not be able to create a valid tag.

HMAC is therefore appropriate when two parties share a secret key.

HMAC-SHA-256 is widely deployed and broadly supported.

The exact key handling, tag length, encoding, and comparison rules belong to the protocol.

Do not use a homemade construction such as H(secret || message) when HMAC is available.

Do not use a plain hash as a substitute for authentication.

Verify MACs before processing security-sensitive message contents.

Use constant-time comparison for MAC verification.

Protect HMAC keys using the same discipline applied to other cryptographic secrets.

Rotate keys according to the protocol's threat model and operational requirements.

## 🌿 9. HKDF — Turning Secrets Into Keys

HKDF is the HMAC-based Extract-and-Expand Key Derivation Function specified in RFC 5869.

HKDF is designed to derive cryptographic keys from input keying material.

HKDF separates extraction from expansion.

The extract step turns potentially uneven input keying material into a pseudorandom key.

The expand step derives one or more context-specific outputs.

The salt parameter is not the same concept as a password-storage salt.

HKDF's info field can bind derived material to an application context.

Domain separation through explicit context values reduces accidental key reuse.

One master secret can therefore produce independent subkeys for different protocol purposes.

Never reuse the same derived key for unrelated cryptographic operations without an explicit design reason.

Record the hash function, salt behavior, info encoding, and output lengths.

Test HKDF implementations against published vectors.

HKDF does not replace a password hashing function.

HKDF also does not turn a weak password into a secure password database.

Password-derived key material requires a password-specific KDF such as Argon2id, scrypt, or PBKDF2 when appropriate.

## 🔐 10. Password Hashing Is a Different Problem

General-purpose cryptographic hashes are designed to be computationally efficient.

Password hashing has the opposite operational objective: make each guess expensive.

Passwords are often low-entropy human-chosen secrets.

An attacker who obtains a password database can perform guesses offline.

SHA-256 is therefore not an appropriate password-storage primitive by itself.

Modern password hashing uses adaptive cost parameters.

Argon2id, scrypt, bcrypt, and PBKDF2 are common choices under appropriate requirements.

OWASP currently recommends Argon2id for new password-storage deployments.

OWASP also documents scrypt and bcrypt alternatives and PBKDF2 when FIPS-140 requirements apply.

The exact parameters must be benchmarked against the application's hardware and authentication workload.

A parameter set copied blindly from another environment may be too weak or too expensive.

Store enough metadata to know which algorithm and parameters protected each password.

Design the authentication layer so parameters can be upgraded over time.

Never log passwords or password-derived secrets.

Never use a global static salt as the sole defense against offline cracking.

## 🧂 11. Password Salts — What They Actually Do

A password salt is normally a unique random value associated with one password record.

The salt is not a secret.

The salt can usually be stored alongside the password hash.

Its purpose includes preventing identical passwords from producing identical stored values.

Unique salts also prevent one precomputed lookup table from directly serving many records.

An attacker who steals the database still sees the salt.

That is acceptable and expected.

Salt uniqueness is more important than salt secrecy.

Generate salts with a cryptographically secure random source when the password hashing library does not generate them automatically.

Do not use a username, email address, or timestamp as a replacement for a random salt.

Do not reuse one salt across every account.

Modern password-hashing formats commonly encode the salt and cost parameters into the stored representation.

Salt handling should be delegated to mature libraries where possible.

Do not manually concatenate password and salt if the password hashing API already defines the construction.

Test that two identical passwords produce different stored hashes under the selected password-hashing library.

## 🌶️ 12. Peppers — A Separate Secret

A pepper is an additional secret used in a password-protection design.

Unlike a salt, a pepper is intended to remain secret.

A pepper can provide defense in depth if an attacker obtains only the password database.

The pepper must therefore be stored separately from the database.

A secret manager, HSM, or protected application configuration can be appropriate depending on the architecture.

A pepper does not compensate for weak passwords or a poor password hashing algorithm.

If the pepper is compromised, the system must have a recovery and rotation strategy.

Pepper rotation is harder than salt rotation because the original password is normally unavailable.

OWASP describes pre-hashing and post-hashing pepper strategies.

Post-hashing HMAC can separate database compromise from possession of the pepper.

Do not place the pepper in source control.

Do not place the pepper in a public repository or client-side application.

Do not describe a pepper as a replacement for a salt.

Document the operational assumptions around availability and rotation before deploying a pepper.

## 🧾 13. PHC-Style Password Records and Upgrade Metadata

Password records should carry enough information to verify and upgrade them safely.

A robust record can identify the algorithm, version, cost parameters, salt, and derived output.

The Password Hashing Competition format is commonly represented by modular strings.

An example conceptual record can look like: `$argon2id$v=19$m=19456,t=2,p=1$...$...`.

The exact encoding must follow the library's documented format.

Do not parse cryptographic record formats with fragile ad-hoc string splitting when a maintained library exists.

On successful login, the verifier can determine whether the stored parameters are outdated.

If they are outdated, the password can be re-hashed using the current policy.

This is often called opportunistic or just-in-time password rehashing.

Users do not need to know which internal password-hashing algorithm protects their account.

Migration should be transparent when possible.

Records should never contain the plaintext password.

Do not store a reversible encryption of the password merely to simplify migration.

Plan for accounts that do not log in during the migration period.

Password reset workflows can complete migration for dormant accounts.

## 📏 14. Encoding, Serialization, and Canonicalization

Hash functions operate on bytes.

Applications often begin with higher-level values such as JSON objects, strings, or database rows.

Two equivalent-looking structures can have different byte encodings.

JSON key order can differ.

Whitespace can differ.

Unicode normalization can differ.

Line endings can differ between operating systems.

Number formatting can differ between serializers.

Base64 can have multiple representational choices depending on alphabet and padding rules.

Hexadecimal is an encoding of bytes, not a cryptographic transformation.

Canonicalization defines exactly which bytes represent a logical object.

Security protocols should specify canonicalization when hashes or signatures cover structured data.

Never hash a language-specific object representation and assume another implementation will reproduce it.

Use protocol-defined serialization rules.

Include test vectors that show the exact serialized bytes before hashing.

Debugging should compare bytes first and human-readable values second.

## 🛡️ 15. Length-Extension Attacks

Some iterated hash constructions expose a property known as length extension.

SHA-1 and SHA-2 are examples of constructions for which naive secret-prefix MAC designs can be vulnerable.

Suppose an attacker knows H(K || M) and can infer the length of K || M.

In affected constructions, the attacker may be able to calculate a valid digest for M || padding || M2 without knowing K.

This does not mean SHA-256 itself is broken.

The weakness comes from using the primitive in an unsafe construction.

Do not build authentication as H(secret || message) when HMAC is available.

HMAC is specifically designed to avoid this class of naive composition problem.

SHA-3's sponge design has different structural properties, but protocol design still matters.

A protocol should specify the exact authenticated construction rather than saying 'the message is hashed with a secret'.

Length extension is a classic example of why cryptographic primitives and cryptographic protocols are different layers.

Library APIs for HMAC are safer than manually reproducing the construction.

Security reviews should search for homemade keyed-hash constructions.

Include negative tests for altered messages and altered authentication tags.

Treat cryptographic composition as a design problem, not a string-concatenation problem.

## 🧱 16. Domain Separation

Domain separation gives related cryptographic operations distinct contexts.

A context string or structured label can prevent accidental cross-use of the same primitive output.

For example, a protocol might distinguish `key-derivation` from `token-hash`.

The exact domain-separation mechanism depends on the primitive and protocol.

Do not rely on comments to separate cryptographic domains.

Encode the domain explicitly into the input or use the construction's built-in context field.

HKDF provides an `info` field specifically useful for contextual derivation.

Hash-to-curve suites require domain separation as part of their design.

Structured protocols should define labels as exact byte strings.

Avoid ambiguous concatenation such as `H(A || B)` when multiple `(A,B)` pairs can encode the same byte sequence.

Length-prefixing or a canonical serialization can remove ambiguity.

Domain separation can reduce cross-protocol substitution opportunities.

It also makes code review easier because each derived value has an explicit purpose.

Use stable, versioned labels when compatibility matters.

Changing a domain label can intentionally create a cryptographic namespace boundary.

## 🧪 17. Test Vectors and Verification

Cryptographic code should be validated against known-answer tests.

A known-answer test supplies fixed input and expected output.

Test vectors can detect encoding errors, padding errors, endianness mistakes, and parameter mismatches.

Include the empty input because it catches many implementation mistakes.

Include a short ASCII input.

Include a multi-block input.

Include non-ASCII UTF-8 data when the application handles text.

Include long inputs when streaming behavior matters.

Verify both hexadecimal and raw-byte representations where applicable.

Do not copy a digest from a random web page as an authoritative test vector.

Prefer NIST standards, RFC test vectors, or official algorithm specifications.

Test vectors are evidence that an implementation matches a specification.

They are not proof that the application uses the primitive safely.

Add regression tests whenever a cryptographic bug is discovered.

Keep test vectors deterministic and version-controlled.

Never include real production secrets in test vectors.

## 📦 18. Streaming Hashing

Hash functions are naturally useful for large files and streams.

Reading an entire multi-gigabyte file into memory is unnecessary.

Most mature libraries provide incremental update APIs.

The application can read a chunk, update the hash state, and continue until EOF.

The final digest is produced after the final chunk.

Chunk size affects I/O behavior but does not change the mathematical digest.

Binary mode must be used for file hashing.

Text mode can alter line endings or perform decoding.

File hashing should therefore operate on raw bytes.

Parallel hashing requires an algorithm or construction designed for parallel composition.

BLAKE3 is designed with a tree structure that enables parallel processing.

Do not assume that parallelizing arbitrary SHA-256 calls produces one equivalent SHA-256 digest.

For resumable transfers, store enough state or use a protocol designed for chunk verification.

An overall file digest can be combined with per-chunk hashes when the threat model requires localized verification.

Define whether metadata such as filenames is authenticated separately from file content.

## 📁 19. File Integrity Is Not Automatically Authenticity

A published SHA-256 digest can detect accidental or malicious modification only if the digest itself is trusted.

If an attacker can replace both the file and the published digest, plain hashing provides no authenticity.

A trusted signature, authenticated channel, or independently protected digest is needed for stronger authenticity.

This distinction is critical for software downloads.

A website that serves a file and its digest from the same compromised origin does not provide independent trust.

Digital signatures bind a digest to a signing key.

Package managers may use signatures, transparency systems, or trusted metadata.

TLS protects a connection but does not by itself establish long-term artifact provenance.

Software supply-chain security therefore combines hashing with authentication and provenance.

Record the exact artifact bytes before calculating the digest.

Do not hash an archive after extraction when the distribution artifact itself is what users need to verify.

Do not compare only filenames or file sizes.

Use reproducible builds where practical so independent parties can calculate matching artifacts.

Hash verification is one control inside a larger software supply-chain model.

## ✍️ 20. Hashes and Digital Signatures

Digital signature schemes commonly hash a message before signing or internally incorporate a digest.

Hashing makes large messages manageable for signature operations.

The signer uses a private key while verifiers use a public key.

A valid signature does not mean the hash function alone provides authenticity.

Authenticity comes from the signature scheme and trust in the public key.

Collision resistance is important because an attacker should not be able to substitute a different message with the same signed digest.

Signature specifications normally identify the exact hash function and encoding rules.

Do not mix-and-match signature encodings without following the protocol.

RSA signatures require carefully specified padding and encoding.

Modern elliptic-curve signature systems have their own message-processing rules.

EdDSA uses a defined hash-based internal construction rather than simply exposing a generic 'hash then sign' API.

Post-quantum signature systems may also use hash functions extensively.

Hash-based signatures such as LMS and XMSS rely heavily on one-way hash constructions.

Signature verification should be performed by a mature cryptographic library.

Do not implement signature padding or key parsing yourself unless the task is explicitly educational.

## 🌳 21. Merkle Trees

A Merkle tree commits to a collection of values using a hierarchy of hashes.

Leaf nodes commonly represent hashes of individual data items.

Parent nodes are derived from child values according to the protocol.

The root digest represents the complete tree state.

A Merkle proof can demonstrate inclusion without sending the entire dataset.

Proof size grows logarithmically with the number of leaves in a balanced tree.

Real protocols must define leaf encoding and internal-node encoding precisely.

Naive concatenation can create ambiguity if boundaries are not encoded.

Domain separation between leaf and internal-node hashing can prevent structural ambiguity.

Bitcoin uses Merkle trees for transaction commitments inside blocks.

Distributed systems can use Merkle structures to compare datasets efficiently.

Content-addressed storage systems use related hash-based commitment ideas.

Changing one leaf changes the hashes on its path to the root.

The root can therefore act as a compact commitment to a large set.

Merkle trees provide integrity commitments, not confidentiality.

## 🔗 22. Content Addressing

Content addressing names an object by a digest derived from its content.

If the content changes, the identifier changes.

This makes content identity naturally tied to integrity.

Git uses cryptographic object identifiers as part of its object model.

Modern Git deployments have additional protections and migration considerations around hash algorithms.

Content addressing is useful for caching and deduplication.

It can also improve immutability guarantees when references are managed correctly.

A digest alone does not tell you whether the content is trustworthy.

An attacker can create a different object with its own valid digest.

Trust comes from the mechanism that tells you which digest you should expect.

Content-addressed systems therefore commonly combine hashes with signed references, authenticated metadata, or access controls.

Collision resistance becomes particularly important when identifiers are used as object identity.

Never assume a short truncated digest is collision-safe merely because the full algorithm is strong.

Document truncation rules whenever a digest is shortened for display or indexing.

Use full-width identifiers for security decisions unless a specification explicitly permits truncation.

## 🐙 23. Git and Hash Migration

Git historically used SHA-1 for object identifiers.

The existence of practical SHA-1 collision attacks changed the threat model for systems that use SHA-1 as an object identity primitive.

Modern Git includes collision-detection and ongoing work around SHA-256 repositories.

This is an important lesson: cryptographic migration is a system-design problem, not simply a search-and-replace operation.

Object formats, references, interoperability, caches, signatures, and tooling may all depend on the hash.

Security-sensitive migration should preserve the ability to validate historical data.

Do not rewrite identifiers casually when external systems depend on them.

Record the algorithm version alongside persistent cryptographic identifiers.

Migration plans should define compatibility periods.

New objects can sometimes use a stronger scheme while legacy objects remain readable.

Applications should avoid assuming that every Git object identifier is always 40 hexadecimal characters.

Do not build security logic around a fixed textual hash length without a protocol requirement.

The broader engineering lesson applies to databases, package registries, and artifact stores.

Cryptographic agility must be designed before an emergency migration is required.

## 🌐 24. Hashing Inside TLS and Secure Protocols

Hash functions appear throughout secure communication protocols.

They can be used in transcript hashing, authentication, key derivation, and message authentication.

TLS 1.3 uses transcript hashes as part of the handshake state.

HKDF is central to TLS 1.3 key schedule construction.

HMAC is used as a primitive inside HKDF.

This demonstrates that hashing is rarely an isolated feature in a modern protocol.

The protocol defines how the hash is composed with keys, labels, and transcript data.

Implementations should therefore use the protocol stack's cryptographic library rather than manually reproducing handshake derivations.

Changing one hash primitive in a protocol can break interoperability.

Cryptographic negotiation must be constrained to supported secure algorithms.

Do not assume that seeing SHA-256 in a protocol means the protocol is simply hashing the message with SHA-256.

Context, labels, keys, and transcript construction are all part of the security design.

Protocol specifications are the authority for these details.

Testing should include interoperability against independent implementations.

Protocol version and cipher-suite documentation should be treated as security-sensitive configuration.

## ⛓️ 25. Hashes in Blockchains

Blockchains use hashes for multiple purposes, not just mining.

Transaction identifiers may be derived from hashes.

Merkle roots can summarize transactions included in a block.

Block headers can include hashes that connect one block to another.

Proof-of-work systems can require finding a hash below a target.

Bitcoin uses SHA-256 extensively in its proof-of-work construction.

Hash-based commitments can make later modification detectable.

A hash does not by itself create consensus.

Consensus rules determine which hashed structures are accepted.

Mining difficulty changes the probability of finding an acceptable proof.

The security of a blockchain depends on the full protocol and economic model.

Never describe a blockchain as secure simply because it uses SHA-256.

Wallets, signatures, networking, consensus, key management, and implementation correctness all matter.

Hash collision resistance is one part of a larger security argument.

Educational experiments should use isolated networks or test environments rather than manipulating production assets.

## 🧬 26. Hash-to-Curve and Modern Cryptographic Protocols

Some cryptographic protocols need to map arbitrary messages to elliptic-curve points.

RFC 9380 standardizes hashing-to-curve procedures.

This is different from simply computing SHA-256(message) and interpreting the bytes as a coordinate.

The mapping must satisfy mathematical and security requirements of the target group.

Hash-to-curve suites specify the underlying hash or XOF, encoding, domain separation, and curve mapping.

Domain separation is mandatory in the RFC-defined encoding framework.

BLS signatures are a prominent application of hashing messages to elliptic-curve groups.

Verifiable random functions and other protocols can use similar techniques.

Implementers should use standardized suites rather than inventing their own map-to-curve procedure.

The correct suite depends on the curve and protocol.

Hashing to a field element and hashing to a curve point are distinct operations.

Uniformity in the target group is an important requirement.

These constructions show how hashing has evolved beyond file checksums and password storage.

Protocol-level cryptography requires mathematical domain knowledge in addition to API knowledge.

Always start from the protocol specification and its test vectors.

## ⚖️ 27. Hashing vs Encryption vs Encoding vs MAC

Hashing maps data to a digest and is not designed for reversible recovery.

Encryption transforms plaintext into ciphertext using a key and is designed for controlled recovery.

Encoding transforms data into another representation without providing confidentiality.

Base64 is encoding, not encryption.

A MAC authenticates data using a secret key.

HMAC is a MAC construction based on a hash function.

A digital signature authenticates data using asymmetric keys.

Password hashing protects stored password verifiers against offline guessing.

A checksum detects many accidental errors but is not designed to resist an active attacker.

CRC32 is useful for error detection but should not replace cryptographic authentication.

Choosing the primitive should start with the security property required.

Confidentiality requires encryption or another confidentiality mechanism.

Integrity against an active attacker requires authentication such as a MAC or signature.

Password verification requires an appropriate password hashing function.

Data fingerprinting may require a general-purpose cryptographic hash.

One primitive cannot safely substitute for every cryptographic requirement.

## 🧠 28. Threat Modeling Hash Usage

Start by identifying what the digest is protecting.

Ask whether the attacker can modify the input.

Ask whether the attacker can modify the digest.

Ask whether the attacker can choose both inputs in a collision attack.

Ask whether the attacker can perform unlimited offline guesses.

Ask whether the attacker can observe timing differences.

Ask whether multiple protocols consume the same derived value.

Ask whether the hash crosses a trust boundary.

Ask whether the digest is public or secret.

Ask whether the digest is used as an identifier.

Ask whether truncation is permitted.

Ask whether future algorithm migration is required.

Ask whether compliance rules constrain the available primitives.

Ask whether the implementation runs on constrained hardware.

Ask whether an attacker can obtain GPU or cloud compute for offline attacks.

These questions convert 'which hash should I use?' into a concrete security-design decision.

## 🚨 29. Common Design Failures

Failure: using SHA-256 directly for passwords.

Correction: use an adaptive password hashing function.

Failure: storing passwords in plaintext.

Correction: store password verifiers generated by a password hashing library.

Failure: using one static salt for every account.

Correction: use a unique salt per password record.

Failure: treating a salt as a secret.

Correction: protect the pepper or other secret separately; salts can be stored with hashes.

Failure: authenticating requests with a plain hash.

Correction: use HMAC or a digital signature as appropriate.

Failure: constructing `hash(secret || message)` manually.

Correction: use HMAC or a protocol-defined authenticated construction.

Failure: hashing ambiguous serialized data.

Correction: define canonical serialization or unambiguous framing.

Failure: trusting an unsigned download hash from the same compromised origin.

Correction: authenticate the digest through a trusted signature, channel, or metadata system.

## 🧯 30. Operational Failure Modes

Cryptography can fail operationally even when the underlying algorithm is sound.

A correct algorithm with an obsolete library can still create security exposure.

Incorrect random-number generation can undermine salts, keys, or tokens.

Secrets in logs can defeat otherwise strong cryptographic protections.

Backups can preserve old password hashes long after a migration.

Database exports can expose password verifiers to attackers.

Debug output can accidentally reveal raw inputs and digests.

Metrics can leak sensitive identifiers if they contain unsanitized cryptographic material.

Configuration drift can cause different servers to use different password-hashing costs.

Time-based comparisons can create side channels in sensitive verification code.

Missing rate limits can make online guessing easier even when password storage is strong.

Broken key rotation can leave compromised authentication keys active.

Dependency upgrades can change accepted algorithm parameters or parsing behavior.

Security monitoring should distinguish authentication failures from infrastructure errors.

Incident response plans should include credential and key rotation procedures.

Operational controls are part of cryptographic security.

## 🐍 31. Python Practical Patterns

Python's standard library provides `hashlib` for several common hash functions.

Use `hashlib.sha256(data).digest()` when raw digest bytes are required.

Use `.hexdigest()` when a textual hexadecimal representation is required.

For large files, use `hashlib` incrementally rather than reading the whole file into memory.

Example: `h = hashlib.sha256()`.

Then repeatedly call `h.update(chunk)`.

Finally call `h.hexdigest()`.

For HMAC, use the standard library `hmac` module.

Use `hmac.compare_digest()` for sensitive equality checks.

Do not use `hashlib.sha256(password.encode()).hexdigest()` for password storage.

For password storage, use a maintained Argon2id, scrypt, bcrypt, or PBKDF2 library according to your requirements.

Use `secrets.token_bytes()` for security-sensitive random values.

Keep byte encoding explicit at application boundaries.

Prefer library APIs over handwritten cryptographic constructions.

Pin and regularly update cryptographic dependencies according to your organization's policy.

## 🟦 32. Node.js Practical Patterns

Node.js exposes cryptographic primitives through the `node:crypto` module.

`createHash('sha256')` creates a SHA-256 hashing stream.

Hash objects support incremental updates for large inputs.

`digest('hex')` produces a hexadecimal representation.

`createHmac()` creates an HMAC instance.

Use `timingSafeEqual()` where appropriate for fixed-length sensitive byte comparisons.

Do not use a generic hash API as a password-storage primitive.

Use a maintained password-hashing package or framework facility designed for Argon2id, scrypt, bcrypt, or PBKDF2.

Random secrets should come from cryptographically secure APIs.

Encoding choices should be explicit and interoperable.

Do not silently convert Unicode strings between encodings.

File hashing should use streams for large files.

Protocol implementations should follow the protocol's specified hash and serialization rules.

Do not expose secret keys in browser bundles.

Review dependency security and native bindings when adding cryptographic packages.

## 🐹 33. Go Practical Patterns

Go's standard library provides packages under `crypto` and `hash` for common primitives.

`crypto/sha256` provides SHA-256 and related helpers.

`crypto/sha512` provides SHA-384 and SHA-512 family functions.

`crypto/hmac` provides HMAC construction.

`crypto/subtle` provides constant-time comparison helpers.

Use `io.Copy()` with a hash writer when hashing large streams.

Do not convert arbitrary binary data to strings unnecessarily.

Keep algorithm selection explicit in security-sensitive code.

Use standard library implementations unless a protocol requires a vetted external implementation.

Password storage should use a maintained Argon2id or other appropriate password hashing implementation.

Random secrets should use `crypto/rand`, not `math/rand`.

Tests should compare against official vectors.

Error handling should not accidentally expose secret values.

Benchmark password hashing on the actual deployment class.

Document the selected cost parameters and upgrade policy.

## 🧱 34. API Design for Hashing Services

An internal hashing service should define exactly what bytes it receives.

Do not make a generic endpoint such as `/hash?value=...` for secrets.

Query parameters can leak sensitive values through logs, proxies, and browser history.

Prefer structured request bodies with explicit content types when a service truly needs to receive data.

Never send plaintext passwords to a generic hashing microservice unless the architecture explicitly requires it and transport is protected.

Password hashing is normally best performed close to the authentication boundary.

File hashing services should enforce input-size and resource limits.

Streaming endpoints should prevent unbounded memory consumption.

Authentication and authorization still apply to cryptographic services.

Rate limits matter when the service performs expensive password hashing.

Do not expose internal cryptographic configuration unnecessarily.

Responses should identify algorithm and parameters when clients need to verify or migrate records.

Version the API if the wire representation changes.

Include negative tests for malformed digests and unsupported algorithms.

Security-sensitive APIs should have an explicit threat model and abuse-case analysis.

## 🔍 35. Code Review Checklist — Hashing

□ Is the chosen primitive appropriate for the security property?

□ Is the algorithm currently supported by authoritative standards or the protocol?

□ Is the digest length appropriate?

□ Is text encoding explicit?

□ Is serialization canonical or unambiguous?

□ Is authentication being confused with integrity-only hashing?

□ Are passwords protected by an adaptive password hashing function?

□ Does each password have a unique salt?

□ Are password parameters stored with the verifier?

□ Is a pepper required, and if so, is it stored separately?

□ Are HMACs verified with a constant-time comparison?

□ Are cryptographic keys and peppers excluded from source control?

□ Are test vectors present?

□ Are empty and multi-block inputs tested?

□ Is there a migration path for algorithm changes?

□ Are deprecated algorithms isolated to legacy compatibility code?

□ Are dependencies maintained?

□ Are logs free of secrets and sensitive plaintext?

□ Are resource limits defined for expensive hashing operations?

□ Does documentation explain the threat model?

## 🧪 36. Laboratory 1 — Verify a SHA-256 Digest

Goal: learn the difference between bytes, hexadecimal text, and a digest.

Create a file containing exactly the bytes you intend to hash.

On Linux, use `sha256sum` in binary-safe workflows.

On macOS, `shasum -a 256` is a common command-line option.

On Windows PowerShell, `Get-FileHash` can calculate SHA-256 for a file.

Compare the digest across two independent implementations.

Change one byte in the file.

Calculate the digest again.

Observe that the complete digest changes.

Restore the original byte.

Verify that the original digest returns.

Repeat with an empty file.

Repeat with a binary file.

Document the exact file bytes and expected digest.

Lesson: reproducibility depends on hashing the same bytes, not merely the same-looking content.

## 🧪 37. Laboratory 2 — Demonstrate Encoding Differences

Goal: understand why text encoding must be explicit.

Choose a non-ASCII string such as a word containing accented characters.

Encode it as UTF-8.

Encode the same logical text using another supported encoding where possible.

Hash each byte sequence separately.

Compare the resulting digests.

They will differ because the byte sequences differ.

Inspect the encoded bytes before hashing.

Repeat after changing Unicode normalization.

Observe that visually similar strings can have different code-point sequences.

Define one canonical encoding for your application.

Add an interoperability test in a second programming language.

Record the exact expected byte sequence.

Record the expected digest.

Lesson: a cryptographic hash cannot repair an underspecified serialization format.

## 🧪 38. Laboratory 3 — Password Hashing Benchmark

Goal: understand why password hashing must be deliberately expensive.

Create a local test account using a password hashing library.

Benchmark verification on the same machine used by the application.

Measure CPU time and memory consumption.

Increase the work factor.

Repeat the benchmark.

Observe the effect on legitimate login latency.

Then consider the same operation from an offline attacker's perspective.

Fast general-purpose hashes allow enormous numbers of guesses.

Memory-hard password hashing increases the attacker's resource requirements.

Do not benchmark using real user passwords.

Do not publish password candidates in source control.

Choose parameters based on the deployment environment and availability requirements.

Store the algorithm and parameters with the password verifier.

Lesson: password hashing is intentionally different from file hashing.

## 🧪 39. Laboratory 4 — HMAC Tamper Detection

Goal: distinguish a plain digest from an authenticated digest.

Generate a random test key with a cryptographically secure random source.

Calculate HMAC-SHA-256 over a test message.

Modify one message byte.

Verify that the original MAC no longer validates.

Try to calculate a valid MAC without the key.

Use a constant-time comparison routine for verification.

Repeat with several message lengths.

Test an empty message.

Test a multi-block message.

Rotate the key and verify that old tags fail under the new key.

Never hard-code the laboratory key in production code.

Document the key lifecycle separately from the message format.

Explain why publishing a plain SHA-256 digest would not prove possession of the secret key.

Lesson: authentication requires a secret or an asymmetric trust mechanism.

## 🧪 40. Laboratory 5 — Merkle Tree Commitment

Goal: build a small educational Merkle tree.

Create four leaf messages.

Hash each leaf with a clearly defined leaf encoding.

Combine adjacent child digests using an unambiguous node encoding.

Hash each pair to create the parent level.

Repeat until one root remains.

Change one leaf.

Recalculate the path to the root.

Observe that the root changes.

Construct an inclusion proof containing the sibling hashes on the path.

Verify the proof without the other leaves.

Document whether the tree uses duplicate-last-leaf or another odd-node rule.

Document leaf and node domain separation.

Compare the root produced by two independent implementations.

Lesson: data structures become cryptographic protocols when their exact encoding affects security.

## 🧪 41. Laboratory 6 — Length-Extension Awareness

Goal: understand why naive secret-prefix authentication is dangerous.

Study the structure of Merkle-Damgard hashes.

Review how SHA-256 maintains an internal chaining state.

Consider a construction of the form H(secret || message).

Understand that the digest exposes the final chaining state.

Study the role of message padding.

Observe why a compatible extension can be constructed in affected designs.

Do not deploy such a construction.

Replace it with HMAC-SHA-256.

Verify that changing the message invalidates the HMAC.

Compare the design to a plain SHA-256 digest.

Read the HMAC specification before implementing any custom MAC.

Treat this laboratory as a protocol-composition lesson.

Never perform security testing against systems you do not own or have authorization to test.

Lesson: a secure primitive can be misused inside an insecure construction.

## 🧪 42. Laboratory 7 — Algorithm Migration Simulation

Goal: practice migrating legacy password verifiers.

Create a local database containing synthetic test accounts.

Mark some records as legacy hashes.

Mark newer records with the current password hashing scheme.

On login, detect the stored algorithm identifier.

Verify the password using the legacy verifier only when required.

After successful verification, generate a new verifier.

Replace the legacy record atomically.

Track how many legacy records remain.

Define a password-reset path for accounts that never log in.

Do not convert a password hash by hashing the hash unless the migration design explicitly justifies it.

Prefer direct re-hashing of the original password after successful authentication.

Document a final retirement date for legacy verification code.

Remove legacy dependencies after the migration is complete.

Lesson: cryptographic agility is a lifecycle capability.

## 🧭 43. Algorithm Selection Decision Guide

Need a standardized general-purpose digest with broad interoperability? Consider SHA-256.

Need a standardized SHA-2 digest on a 64-bit platform? SHA-512-family functions may be appropriate.

Need a standardized alternative construction to SHA-2? Consider SHA-3.

Need variable-length standardized output? Consider SHAKE.

Need password storage? Use a password hashing function such as Argon2id rather than a raw hash.

Need keyed authentication with a shared secret? Consider HMAC.

Need several keys from one high-quality secret? Consider HKDF.

Need content addressing? Use a cryptographic digest with enough collision resistance for the namespace.

Need software authenticity? Combine hashing with signatures or authenticated distribution metadata.

Need accidental corruption detection only? A checksum may be sufficient.

Need confidentiality? Hashing is not the solution; use authenticated encryption or another suitable encryption design.

Need interoperability? Prefer algorithms mandated by the protocol or ecosystem.

Need compliance? Verify the current applicable standard and validated implementation requirements.

Need future migration? Store algorithm identifiers and versioned parameters.

Do not choose an algorithm solely because a benchmark says it is fastest.

## 📚 44. Authoritative Reference Map

NIST FIPS 180-4 defines the Secure Hash Standard and the SHA-2 family.

NIST FIPS 202 defines SHA-3 and the SHAKE extendable-output functions.

NIST's Hash Functions project tracks approved hash algorithms and policy information.

NIST has stated that SHA-1 is being transitioned out of approved use and recommends SHA-2 or SHA-3 for new applications.

RFC 2104 specifies HMAC.

RFC 5869 specifies HKDF.

RFC 8018 specifies PBKDF2 and related password-based cryptography mechanisms.

RFC 9106 specifies Argon2 version 1.3.

RFC 9380 specifies hashing to elliptic curves and associated domain-separation requirements.

OWASP Password Storage Cheat Sheet provides current application guidance for password hashing.

OWASP Cryptographic Storage Cheat Sheet covers broader cryptographic storage decisions.

Python `hashlib` documents standard-library hashing interfaces.

Node.js `crypto` documents hashing, HMAC, and constant-time comparison APIs.

Go's `crypto` packages provide standard cryptographic primitives.

Standards and protocol specifications should take precedence over informal tutorials when requirements conflict.

## 🗓️ 45. Standards Tracking and 2026 Maintenance Notes

This chapter is written for a 2026 learning environment.

NIST FIPS 180-4 is still the published Secure Hash Standard while NIST has announced a revision.

NIST FIPS 202 remains the published SHA-3 standard while NIST has announced an update.

NIST's current policy page states that SHA-1 is being transitioned away from approved use.

Do not freeze this chapter's parameter recommendations indefinitely.

Password-hashing cost parameters should be reviewed as hardware changes.

OWASP updates its cheat sheets as practical guidance evolves.

RFC errata and updates should be checked before implementing a protocol from an older RFC.

Library documentation can change independently of algorithm standards.

Security guidance should record the date it was reviewed.

Production cryptographic decisions should be revisited after significant vulnerabilities, standard changes, or infrastructure changes.

Do not treat a 2026 article as a permanent cryptographic policy.

Keep a cryptographic inventory in mature systems.

Record where each algorithm is used, why it is used, and how it can be migrated.

Security engineering is a lifecycle discipline rather than a one-time algorithm choice.


---

# 🧩 46. Practical Review Exercises

## Exercise A — Identify the Primitive

Read each requirement before choosing an algorithm.

- "I need to detect whether a downloaded file changed." → cryptographic hash plus a trusted reference.
- "I need to store user passwords." → adaptive password hashing.
- "I need to authenticate an API request between two services with a shared secret." → HMAC or a protocol-defined MAC.
- "I need to derive several independent keys from a handshake secret." → a KDF such as HKDF.
- "I need to hide customer data." → encryption, preferably authenticated encryption.
- "I need to represent binary data in JSON." → encoding such as Base64.
- "I need to detect accidental network corruption." → an appropriate checksum may be enough.
- "I need to prove that a package came from a publisher." → signatures or authenticated provenance metadata.
- "I need a content identifier." → a collision-resistant cryptographic digest with a defined namespace.
- "I need a password reset token." → a cryptographically secure random token, not a predictable hash of account data.

## Exercise B — Explain the Mistake

Consider:

```python
stored = sha256(password.encode()).hexdigest()
```

The code produces a deterministic digest.

The problem is not that SHA-256 is a bad hash.

The problem is that password guessing is an offline computation.

An attacker can calculate SHA-256 extremely quickly.

A fast verifier lets the attacker test many candidate passwords.

A unique salt is also absent.

Therefore identical passwords can produce identical stored values.

A password-hashing function such as Argon2id is designed specifically for this threat.

The correct implementation should use a maintained password-hashing API.

The application should store the complete verifier string returned by the library.

The verifier should contain the algorithm and cost information required for future verification.

## Exercise C — Explain the Trust Boundary

Suppose a website publishes:

```text
application.zip
SHA256(application.zip) = abc123...
```

Ask where the digest came from.

If the same compromised server can replace both files, the attacker can also replace the digest.

A trusted signature changes the model.

A separately authenticated release manifest can also change the model.

An independently obtained digest can provide stronger assurance.

The lesson is not "hashes are useless."

The lesson is "a digest needs an appropriate trust path."

---

# 🧭 47. Final Engineering Checklist

Before approving a hash-related implementation:

- [ ] The security property is explicitly identified.
- [ ] The selected primitive matches that property.
- [ ] The algorithm is current for the intended use.
- [ ] Deprecated algorithms are restricted to justified compatibility paths.
- [ ] Passwords use an adaptive password-hashing scheme.
- [ ] Password salts are unique.
- [ ] Secret peppers, if used, are stored separately.
- [ ] HMAC is used instead of a homemade keyed hash.
- [ ] Sensitive comparisons use constant-time primitives where appropriate.
- [ ] Text encoding is explicit.
- [ ] Structured data has canonical or unambiguous serialization.
- [ ] Domain separation is defined for multi-purpose derivations.
- [ ] Digest truncation is justified and specified.
- [ ] Test vectors are included.
- [ ] Empty-input behavior is tested.
- [ ] Large-input behavior is tested.
- [ ] Binary data is processed as bytes.
- [ ] Cryptographic dependencies are maintained.
- [ ] Secrets are absent from source control.
- [ ] Secrets are absent from logs.
- [ ] Error messages do not disclose secret material.
- [ ] Expensive password verification has resource controls.
- [ ] Algorithm identifiers are stored when migration requires them.
- [ ] A future upgrade path exists.
- [ ] The implementation is reviewed against authoritative standards.
- [ ] Production decisions are documented with date and rationale.

---

# 🎓 48. What Mastery Looks Like

A beginner can define a hash.

An intermediate engineer can use SHA-256 correctly.

A security engineer can distinguish hashing from authentication.

A backend engineer can design password storage with migration support.

A protocol engineer can reason about encoding, domain separation, and composition.

A cryptography engineer can analyze the security properties of a construction.

An experienced reviewer can identify when a correct primitive is being used incorrectly.

The goal of this chapter is not to memorize algorithm names.

The goal is to develop the habit of asking:

1. What security property do I need?
2. What attacker am I defending against?
3. What exact bytes are being processed?
4. What primitive and construction provide the property?
5. What standard specifies the construction?
6. How are secrets generated and stored?
7. How is the result verified?
8. How will the design migrate when requirements change?

If those questions are answered precisely, cryptographic engineering becomes much less mysterious.

---

# 📖 49. Source Notes

The primary standards for this chapter are the NIST Secure Hash Standard and SHA-3 Standard.

NIST's Hash Functions project identifies FIPS 180-4 and FIPS 202 as the standards for approved hash algorithms.

NIST's current policy information is especially important for SHA-1 because algorithm status changes over time.

OWASP guidance is used for application-level password storage recommendations.

IETF RFCs are used for HMAC, HKDF, PBKDF2, Argon2, and hash-to-curve protocol details.

Implementation examples are intentionally framed as patterns.

They are not a substitute for the documentation of the exact library version used in production.

Performance numbers are deliberately not treated as universal facts.

Hardware, CPU instructions, compiler settings, language runtime, library implementation, input size, concurrency, and workload all affect performance.

When performance matters, benchmark the exact implementation on representative infrastructure.

When compliance matters, verify the applicable validation and policy requirements rather than relying on a generic algorithm list.

When security matters, prefer standardized constructions and mature implementations.

---

# 🛡️ 50. Responsible Cryptography Practice

Cryptography is defensive technology.

Use these concepts to protect systems, users, software artifacts, and communications.

Do not use this material to attack systems without authorization.

Password-hashing laboratories should use synthetic accounts.

File-integrity laboratories should use files created for testing.

Protocol experiments should use local or explicitly authorized environments.

Do not submit real credentials to demonstration programs.

Do not place production keys in examples.

Do not publish private keys, recovery secrets, peppers, or authentication tokens.

Do not test authentication bypasses against systems you do not own or have permission to assess.

Responsible security engineering combines technical knowledge with authorization, privacy, and operational discipline.

---

# 🔗 51. Curated Reference Links

### Standards

- NIST — Secure Hash Standard (FIPS 180-4): https://csrc.nist.gov/pubs/fips/180-4/upd1/final
- NIST — SHA-3 Standard (FIPS 202): https://csrc.nist.gov/pubs/fips/202/final
- NIST — Hash Functions Project: https://csrc.nist.gov/projects/hash-functions
- NIST — Hash Function Policy: https://csrc.nist.gov/projects/hash-functions/hash-function-policy

### IETF / RFC

- RFC 2104 — HMAC: https://www.rfc-editor.org/rfc/rfc2104
- RFC 5869 — HKDF: https://www.rfc-editor.org/rfc/rfc5869
- RFC 8018 — PKCS #5 v2.1 / PBKDF2: https://www.rfc-editor.org/rfc/rfc8018
- RFC 9106 — Argon2: https://www.rfc-editor.org/rfc/rfc9106
- RFC 9380 — Hashing to Elliptic Curves: https://www.rfc-editor.org/rfc/rfc9380
- RFC 7693 — BLAKE2: https://www.rfc-editor.org/rfc/rfc7693

### Application Security

- OWASP Password Storage Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html
- OWASP Cryptographic Storage Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html
- OWASP ASVS: https://owasp.org/www-project-application-security-verification-standard/

### Developer Documentation

- Python hashlib: https://docs.python.org/3/library/hashlib.html
- Python hmac: https://docs.python.org/3/library/hmac.html
- Node.js crypto: https://nodejs.org/api/crypto.html
- Go crypto packages: https://pkg.go.dev/crypto

---

# ✅ 52. Completion Checklist for the Reader

After finishing the article, verify that you can answer all of these without looking them up:

- What is a cryptographic hash?
- What is a digest?
- Why must fixed-length hashes have collisions?
- What is preimage resistance?
- What is second-preimage resistance?
- What is collision resistance?
- Why is the birthday bound approximately 2^(n/2)?
- Why is SHA-256 not a password hashing algorithm?
- What is a salt?
- Does a salt need to be secret?
- What is a pepper?
- Where should a pepper be stored?
- What is HMAC?
- Why is HMAC preferable to a homemade secret-prefix hash?
- What is HKDF?
- What is domain separation?
- What is canonicalization?
- What is a length-extension attack?
- What is a Merkle tree?
- What is content addressing?
- Why does a published hash not automatically prove authenticity?
- Why do digital signatures commonly involve hashing?
- What are SHA-2 and SHA-3?
- What is SHAKE?
- What is the difference between a hash and an XOF?
- What is Argon2id?
- When might PBKDF2 be selected?
- Why does password-hashing cost need periodic review?
- Why are test vectors important?
- Why should production cryptography use mature libraries?
- Why is cryptographic agility an engineering requirement?
- Why must algorithm choice be tied to a threat model?

If you can answer these questions and explain the reasoning behind each answer, you have moved beyond memorizing terminology and into practical cryptographic engineering.

---

## 🔐 Closing Principle

> **Do not ask only, "Which hash is strongest?" Ask, "What security property does this system require, what attacker can challenge it, and which standardized construction provides that property safely?"**

That question is the foundation of sound hash-function engineering.

---

*Maintained as part of [DevSec-Archive](https://github.com/ItsWanheda/DevSec-Archive) by [ItsWanheda](https://github.com/ItsWanheda).*  
*Educational material should be reviewed against current NIST, IETF, OWASP, and library documentation before production use.*
