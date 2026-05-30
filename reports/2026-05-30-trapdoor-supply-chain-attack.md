# TrapDoor Supply Chain Attack - AI-Powered Crypto Theft Investigation Report

**Date**: May 30, 2026  
**Event**: TrapDoor AI-Enhanced Supply Chain Attack on Web3 Developers  
**Discovery**: May 29, 2026  
**Reporter**: Independent On-Chain Investigator

---

## Executive Summary

Socket security researchers uncovered a sophisticated supply chain attack campaign, designated "TrapDoor," targeting developers working across Solana, Sui, and Aptos ecosystems. The attack deploys a multi-vector approach combining traditional credential-stealing malware with novel AI-powered social engineering techniques embedded within developer workflow files.

The campaign compromises developer environments through 34+ malicious packages distributed across npm, PyPI, and Crates.io package registries. Notably, the attackers have pioneered a technique of implanting hidden commands within standard AI assistant configuration files (.cursorrules, CLAUDE.md), creating a persistent exfiltration mechanism that activates when developers use AI coding assistants.

**Critical Threat**: Unlike traditional supply chain attacks that target direct financial smart contract interactions, TrapDoor compromises the entire development pipeline, potentially exposing wallet private keys, SSH credentials, GitHub tokens, and cloud infrastructure access to the attackers.

---

## Key Data

| Metric | Value |
|--------|-------|
| Malicious Packages Identified | 34+ |
| Package Registries Targeted | npm, PyPI, Crates.io |
| Developer Ecosystems | Solana, Sui, Aptos |
| Configuration Files Weaponized | .cursorrules, CLAUDE.md |
| Primary Targets | Wallet files, SSH keys, GitHub tokens, Cloud credentials |
| Discovery Method | Socket security research |
| Attack Classification | Supply chain + AI-enhanced social engineering |

---

## Malicious Package Inventory

### Identified Malicious Packages

| Package Name | Registry | Apparent Function | Actual Function |
|--------------|----------|-------------------|-----------------|
| wallet-security-checker | npm/PyPI | Security validation tool | Credential harvester |
| defi-risk-scanner | npm/PyPI | DeFi risk analysis | Key extraction malware |
| solidity-build-guard | npm | Build process optimizer | Backdoor injection |
| move-compiler-tools | Crates.io | Move language tooling | Environment scanner |
| llm-context-compressor | npm | AI context optimization | Command injection |

### Additional Packages Under Investigation

Socket researchers continue analyzing the complete package inventory. The 34+ packages identified represent the initial wave of an ongoing campaign, with potential additional packages across other registries.

---

## Attack Vector Analysis

### Vector 1: Typosquatting and Brand Impersonation

The malicious packages leverage names that closely mimic legitimate developer tools, exploiting common typosquatting patterns:

- `wallet-security-checker` imitates genuine security validation packages
- `defi-risk-scanner` appears as a legitimate risk analysis tool
- `llm-context-compressor` targets developers using AI coding assistants

These packages are designed to appear in search results when developers seek legitimate security or development tools, with names carefully constructed to avoid detection while maximizing install probability.

### Vector 2: Legitimate Package Replacement

Some packages may have started as legitimate projects before receiving malicious updates, complicating detection through reputation-based filtering. This "trojan horse" approach involves:

1. Initial release as functional, harmless packages
2. Accumulation of download count and positive reviews
3. Subsequent malicious update introducing payload
4. Existing users automatically receive update

### Vector 3: AI Configuration File Manipulation

The most sophisticated attack vector involves manipulation of AI assistant configuration files:

**Attack Mechanism**:
1. A developer, having installed a compromised package, uses an AI coding assistant (Cursor, Claude, etc.)
2. The malicious package has pre-populated or modified `.cursorrules` or `CLAUDE.md` files
3. These configuration files contain zero-width Unicode characters that encode hidden commands
4. When the AI processes context from the project directory, it encounters these hidden commands
5. The AI assistant, following the injected instructions, may extract or expose sensitive information

**Why This Works**:
- Zero-width Unicode characters are invisible in standard text editors
- AI assistants process these files as legitimate context
- The attack exploits trust in AI-generated responses
- No traditional antivirus or static analysis detects the embedded commands
- Developers rarely audit configuration files for hidden content

**Technical Implementation**:
Zero-width characters such as:
- Zero-width space (U+200B)
- Zero-width non-joiner (U+200C)
- Zero-width joiner (U+200D)
- Word joiner (U+2060)

These can encode arbitrary commands or extract instructions that appear invisible to human reviewers but are parsed by AI systems.

### Vector 4: Credential Exfiltration Targets

The attack campaign specifically targets:

| Target Category | Specific Assets | Attack Impact |
|-----------------|-----------------|----------------|
| **Cryptocurrency Wallets** | Private keys, seed phrases, wallet files (JSON, keystore) | Direct fund theft |
| **SSH Credentials** | Private keys, known_hosts, authorized_keys | Server compromise |
| **GitHub Access** | Tokens, OAuth credentials, repository access | Code theft, supply chain extension |
| **Cloud Infrastructure** | AWS/GCP/Azure credentials, API keys | Cloud resource hijacking |
| **Development Environment** | Environment variables, config files, credentials | Persistent access |

### Vector 5: Multi-Platform Targeting

The campaign's focus on Solana, Sui, and Aptos is deliberate:

**Solana Targeting**:
- High-value wallets with significant SPL token holdings
- DeFi protocol interactions with potential for contract-level exploits
- Cross-chain bridge operations

**Sui and Aptos Targeting**:
- Early-stage ecosystems with high-growth potential
- Developer tools still under active development
- Opportunity to establish persistent presence before security matures

---

## Technical Analysis

### Stealth and Persistence Mechanisms

The TrapDoor campaign employs multiple layers of operational security:

**Layer 1: Legitimate Appearance**
- Package names suggest helpful developer utilities
- README files contain plausible documentation
- Version history may show legitimate development activity

**Layer 2: Delayed Activation**
- Malicious code may remain dormant for extended periods
- Activation triggers based on specific conditions (AI assistant usage, file access patterns)

**Layer 3: Encrypted Communication**
- Exfiltrated data transmitted using encrypted protocols
- Command and control infrastructure designed to evade detection

**Layer 4: AI-Enhanced Social Engineering**
- Hidden commands leverage AI assistant context processing
- Attackers can potentially influence AI-generated code suggestions
- Creates plausible deniability through AI intermediary

### Scope of Compromise

Given the attack targets:
- Solana developers may have exposed mainnet wallet credentials
- Sui developers may have leaked MOVE module signing keys
- Aptos developers may have compromised Move framework access

The potential blast radius extends beyond individual developers to potentially compromise deployed smart contracts and user-facing applications.

---

## Implications for Web3 Security

### Evolution of Supply Chain Threats

TrapDoor represents a significant evolution in supply chain attack methodology:

| Traditional Supply Chain Attack | TrapDoor Campaign |
|--------------------------------|-------------------|
| Direct package compromise | Package + AI configuration weaponization |
| Targets end users | Targets developers (force multiplier) |
| Detectable through code analysis | Invisible through traditional analysis |
| Limited blast radius | Potential ecosystem-wide exposure |

### AI Security Implications

This campaign exposes a critical vulnerability in the AI-assisted development paradigm:

1. **Trust Boundary Erosion**: Developers trust AI assistant outputs without verifying configuration file integrity
2. **Context Poisoning**: Attackers can influence AI behavior through invisible configuration manipulation
3. **Attribution Obfuscation**: AI-generated responses provide plausible cover for malicious suggestions
4. **Scale Advantage**: One compromised configuration file can affect all developers using the same project

### Development Pipeline Security

The attack surfaces critical questions about development environment security:

- Should AI configuration files be treated as executable code?
- How should developers verify integrity of auto-generated configuration?
- What visibility do security tools have into AI context processing?

---

## Lessons Learned

1. **Package Registry Trust is Misplaced**: Even with security measures, package registries cannot guarantee package integrity. Developers must implement additional verification layers.

2. **AI Configuration Files Are Attack Surfaces**: The inclusion of `.cursorrules` and `CLAUDE.md` in project directories creates new attack vectors that require security consideration.

3. **Credential Management Requires Defense in Depth**: Single-factor credential storage is insufficient. Hardware wallets, secret managers, and environment isolation provide necessary layers.

4. **Developer Security is User Security**: Compromised developers become attack vectors against their own users, making developer environment security a public safety issue.

5. **AI Security Research is Essential**: As AI coding assistants become ubiquitous, dedicated research into AI-specific attack vectors becomes critical.

---

## Recommended Mitigations

### For Developers

1. **Verify all configuration files** for unexpected Unicode characters before use
2. **Audit AI-generated commands** that involve file access or credential handling
3. **Use hardware wallets** for production environment private keys
4. **Implement secret scanning** in development workflows
5. **Review package updates** before installation, especially for security-related tools

### For Organizations

1. **Isolate development environments** from production infrastructure
2. **Implement package integrity verification** beyond registry reputation
3. **Monitor for unusual credential access patterns** in development tools
4. **Establish AI usage policies** addressing configuration file handling
5. **Conduct security training** on supply chain and AI-specific threats

---

## Next Investigation Directions

1. **Complete Package Enumeration**: Full catalog of all 34+ malicious packages across all registries
2. **Victim Identification**: Assessment of developer population exposure and potential compromise
3. **Infrastructure Attribution**: Identification of command and control servers and operators
4. **AI Platform Coordination**: Notification of affected AI assistant platforms (Cursor, etc.)
5. **Exploitation Confirmation**: Determination of whether identified packages have been exploited in the wild
6. **Additional Registries**: Investigation of potential spread to other package registries (RubyGems, NuGet, etc.)

---

## References

- [CryptoGazette - TrapDoor Supply Chain Attack Solana Sui Aptos](https://cryptogazette.com/trapdoor-supply-chain-attack-solana-sui-aptos/)
- Socket Security research documentation
- NPM, PyPI, Crates.io package registry data
- Developer security community disclosures

---

**Investigator**: Onchain Shadow  
**OPSEC Statement**: This report synthesizes publicly available information regarding the TrapDoor campaign. Specific package hashes, C2 infrastructure, and victim statistics require coordination with Socket, affected registries, and security researchers for comprehensive verification.
