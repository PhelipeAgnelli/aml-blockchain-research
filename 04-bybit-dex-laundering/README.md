# $1.46 Billion in 10 Days: An AML Investigator's Analysis of the Bybit Hack and the Six Blind Spots Nobody Covered

**Report:** AML-IR-2026-004 · **Author:** Phelipe Agnelli · **Date:** May 2026  
**Sources:** ZachXBT · TRM Labs · Chainalysis · ChainArgos/Allium · Nansen · Crystal Intelligence · FBI PSA · NCC Group  
**Frameworks:** ACAMS CAMS 10th Ed. · FATF VA Guidance 2021 · BSA/FinCEN · IEEPA · DORA · GENIUS Act 2025

---

![Bybit Hack — Supply Chain Attack · DEX/Bridge Laundering · 6 Blind Spots](./bybit-diagram.svg)

---

## Case Overview

On **February 21, 2025**, Bybit suffered the largest cryptocurrency theft in history — **$1.46 billion in ETH** drained from its cold wallet in a single transaction. Within 48 hours, the FBI and ZachXBT had attributed the attack to North Korea's **Lazarus Group** (TraderTraitor / APT38). Within 10 days, $1.2 billion had been laundered through THORChain alone.

What distinguishes this case from every previous analysis is the focus of this report: not on what happened, but on **what the industry failed to detect, failed to act on, and failed to prevent** — and what an AML analyst could have done differently at each stage.

In this report, I document six blind spots that existing analyses missed, map each phase of the attack and laundering operation against applicable AML frameworks, and propose concrete monitoring controls that would have shortened the window of undetected activity.

---

## The Attack: A Supply Chain Exploit, Not a Direct Hack

### What happened

The Lazarus Group did not breach Bybit's infrastructure directly. Instead, they executed a **supply chain attack** targeting Safe{Wallet}, a third-party multi-signature wallet platform used by Bybit for cold wallet management.

The attack sequence:

1. **Reconnaissance (weeks prior):** Attackers compromised a Safe{Wallet} developer's machine
2. **Malicious code injection:** JavaScript was injected into the Safe UI frontend specifically for Bybit's transactions
3. **UI manipulation:** When Bybit's signers — including CEO Ben Zhou — reviewed what appeared to be a routine cold-to-warm wallet transfer, the UI displayed a legitimate transaction while the underlying code authorized a transfer to attacker-controlled addresses
4. **Test transaction:** At **14:15 UTC**, a test transaction of 90 USDT confirmed the exploit was working
5. **Main drain:** One minute later at **14:16 UTC**, 401,347 ETH was transferred
6. **First response:** Bybit posted publicly on X at **15:51 UTC** — 95 minutes after the drain began

### Investigator's analysis

**Red flags at this stage:**

> **ACAMS 2.1 — Predicate Offence Identification**  
> State-sponsored theft confirmed by FBI. All funds are legally tainted from the first transaction. Any VASP receiving funds traceable to the theft address is handling proceeds of crime under FATF R.3 and IEEPA — regardless of whether they identified the origin.

> **ACAMS 6.2.1 — Third-Party Provider Risk**  
> The compromise occurred not at Bybit's infrastructure but at a trusted third-party provider. This is a documented supply chain risk that multi-sig wallet users must assess as part of their vendor due diligence. The same attack vector had been used against WazirX and Phemex prior to Bybit.

**What should have been done:**  
Every exchange using a multi-sig wallet provider should require independent verification of the signing UI — meaning a separate, independently hosted interface that allows signers to verify the actual transaction parameters before signing, not just the UI display. The 90 USDT test transaction at 14:15 was itself a red flag — an anomalous small transaction immediately before a large cold wallet transfer should trigger a mandatory review pause.

---

## Phase 1 — Placement: Liquidity Conversion via DEX

### What happened

Within 13 minutes of the main drain (14:29 UTC), the Lazarus Group began converting the stolen illiquid tokens — stETH, mETH, cmETH — to plain ETH via decentralized exchanges, including Uniswap, OKX DEX, Sky Protocol, and **PancakeSwap**. At least $200 million in staked tokens were converted to ETH within hours.

### Investigator's analysis

**Red flags at this stage:**

> **ACAMS 4.3.3 — Rapid Layering via DEX**  
> Immediate conversion of illiquid assets to fungible ETH within minutes of a theft is a textbook placement-to-layering transition. The speed — 13 minutes — indicates automation, not manual execution. This level of operational efficiency is consistent with Lazarus Group's documented playbook.

> **FATF VA Guidance 2021, §5.3 — DEX Use for Obfuscation**  
> DEX swaps break the direct on-chain link between the theft address and the resulting ETH, creating a pseudo-layering effect even before cross-chain movement begins.

---

### **Blind Spot #1: PancakeSwap — The Protocol Nobody Mentioned**

Every initial report on the Bybit hack documented THORChain, Paraswap, DODO, and Li.Fi as laundering venues. **No report mentioned PancakeSwap.**

ChainArgos, in a post-hack analysis published weeks after the incident, revealed that Lazarus laundered **$263 million — nearly one-fifth of the total stolen — through PancakeSwap alone**. This is the first report in this research series to document it in an AML investigative context.

The implication is significant: compliance teams at exchanges receiving BTC or ETH that trace back through PancakeSwap to the hack period would not have found it in initial blacklists. Any analyst relying solely on the FBI's 51 published addresses or ZachXBT's initial cluster would have missed this entire laundering channel.

**What I document from this:** Any post-incident AML review must include cross-chain DeFi aggregator analysis — not just direct hop tracing. The assumption that the most prominent bridges are the only laundering venues is a structural blind spot in standard investigation methodology.

---

## Phase 2 — Layering: THORChain at Scale

### What happened

Between February 24 and March 3, 2025, the Lazarus Group moved the vast majority of stolen ETH through THORChain — **$1.2 billion in 10 days**. THORChain's daily volume exceeded $1 billion, and weekly volume reached $4.66 billion — its highest on record. By March 3, all stolen ETH had been moved to new addresses, with the majority bridged to Bitcoin.

Simultaneously, the group employed what TRM Labs and the FBI confirmed as a deliberate **"flood the zone"** technique — generating high-frequency transactions across multiple platforms to overwhelm compliance teams, blockchain analysts, and law enforcement simultaneously.

### Investigator's analysis

**Red flags at this stage:**

> **FATF VA Guidance 2021, §5.3 — Cross-Chain Bridge Laundering**  
> THORChain enables ETH-to-BTC conversion without a centralized intermediary, no KYC, and no freeze mechanism. Once funds enter the protocol, there is no compliance intervention point until they exit on the other side.

> **ACAMS 4.3.3 — Fan-Out Dispersal**  
> 4,876+ BTC addresses were used to receive the bridged funds — creating 4,876 simultaneous investigation trails that require coordinated multi-VASP response.

---

### **Blind Spot #2: "Flood the Zone" as an AML Typology**

The technique of generating deliberate high-frequency transactions to overwhelm investigators is not formally documented in the ACAMS CAMS framework as a named typology. TRM Labs identified it as a deliberate tactic: *"The Bybit exploit indicates that the regime is intensifying its 'flood the zone' technique — overwhelming compliance teams, blockchain analysts, and law enforcement agencies with rapid, high-frequency transactions across multiple platforms."*

I document this here as an emerging typology that AML practitioners need to recognize: **velocity of transactions across multiple platforms simultaneously, within a compressed timeframe following a known theft, is itself a red flag — independent of whether individual transactions exceed reporting thresholds.**

---

### **Blind Spot #3: Volume Anomaly Detection Without Address Tagging**

ETH deposits to THORChain increased **155% on February 23** — two days after the hack. ChainArgos demonstrated that this surge was detectable through pure volume monitoring, without having tagged a single Bybit hacker address.

The question I ask from an AML perspective is: **Why wasn't this detected?**

Any financial intelligence unit or blockchain analytics provider monitoring macro-level DEX/bridge activity would have seen an unprecedented volume spike on a protocol with a documented history of money laundering involvement. That spike, combined with the publicly known Bybit hack, should have triggered an immediate escalation.

**What should have been done:**  
I would implement a macro volume monitoring rule: any protocol experiencing greater than 50% volume increase vs. its 30-day average triggers an automatic alert for the compliance team, regardless of whether specific addresses have been flagged. This is the equivalent of transaction monitoring based on behavioral patterns — not just entity screening.

---

## Phase 3 — The Dark Zone: eXch, Mixers, and 27% "Gone Dark"

### What happened

Approximately **27% of the stolen funds — roughly $400 million — went permanently dark** through eXch (a self-described "proudly non-compliant" instant exchange), Wasabi Mixer, CryptoMixer, Railgun, and residual Tornado Cash flows. As of the date of this report, these funds remain untraceable.

### Investigator's analysis

**Red flags at this stage:**

> **ACAMS 5.1.1 — No-KYC Instant Exchange as Relay**  
> eXch's explicit policy of non-compliance with AML regulations makes it a high-risk relay by definition. Any VASP that receives funds from eXch should apply EDD regardless of the apparent transaction size.

---

### **Blind Spot #4: Circle's 5-Hour USDC Freeze Delay**

Tether froze $106,000 USDT within hours of the hack being confirmed. Circle, despite having the technical capability to freeze USDC immediately, waited **5 hours** before acting. ZachXBT publicly criticized Circle as a "bad actor" in response to Circle CEO Jeremy Allaire's tweet.

The amount frozen was minimal ($115,000 USDC) relative to the total stolen — but the principle matters enormously for AML practice. Stablecoin issuers with freeze capabilities have a de facto compliance obligation when a theft event of this scale is publicly confirmed. A 5-hour delay is not a technical limitation — it is an operational choice.

**What I recommend:** A standardized **1-hour SLA** for stablecoin issuers to respond to confirmed theft events with address freezing. This should be formalized as a VASP obligation under FATF R.20 and the GENIUS Act 2025, which now extends BSA obligations to stablecoin issuers.

---

### **Blind Spot #5: THORChain's $5 Million Fee Problem**

THORChain earned approximately **$5 million in fees** from processing the Bybit laundering flows. The protocol did not refund these fees. Multiple other protocols and exchanges that processed illicit funds chose to return fees or donate proceeds — THORChain did not.

This creates a regulatory question that no analysis has addressed: **Can a decentralized protocol that earns fees from confirmed illicit flows have regulatory obligations attached to those fees?**

The 5th Circuit's ruling in *Van Loon v. Treasury* (Tornado Cash case, November 2024) established that immutable smart contracts cannot be sanctioned as "property." But THORChain is not fully immutable — it has approximately 100 node operators who voted on whether to block the Lazarus addresses. They chose not to block consistently.

**What I document:** The THORChain case creates a new AML typology boundary question. If a protocol with identifiable governance actors profits from confirmed illicit flows and declines to act when technically capable of doing so, the regulatory gap between "permissionless protocol" and "willful facilitator" narrows significantly.

---

### **Blind Spot #6: eXch Closed — But No Regulatory Action**

eXch announced its closure in May 2025, citing pressure from the Bybit hack investigation. No regulatory action was taken against the platform, no seizure was executed, and no prosecution was initiated. The platform processed an estimated **$94 million of Bybit stolen funds**.

**What this means for AML practice:** The absence of enforcement against a self-described non-compliant service that processed $94 million in confirmed criminal proceeds sets a dangerous precedent. It signals to similar services that the enforcement gap remains exploitable.

---

## The Numbers: What Was Recovered vs. What Was Lost

| Category | Amount | % of Total | Status |
|----------|--------|-----------|--------|
| Traceable on-chain | ~$1.12B | ~77% | Monitored but not seized |
| "Gone dark" (eXch/mixers) | ~$400M | ~27% | Permanently untraceable |
| Frozen (Tether/Circle/others) | <$1M | <0.1% | Recovered |
| Bybit self-recovered | $1.46B | 100% | Covered from reserves + bridge loan |

The paradox of this case: **Bybit recovered operationally but the laundering succeeded forensically.** 77% of funds remained traceable — but traceable is not the same as frozen or recovered.

---

## AML Typologies Identified

| # | Typology | Phase | Framework | Risk |
|---|----------|-------|-----------|------|
| 1 | Supply chain attack — predicate offence | Attack | ACAMS 2.1 · FATF R.3 | Critical |
| 2 | Third-party wallet provider compromise | Attack | ACAMS 6.2.1 · DORA | Critical |
| 3 | Rapid DEX conversion (illiquid → fungible) | Placement | ACAMS 4.3.3 · FATF VA §5.3 | High |
| 4 | PancakeSwap — undocumented laundering channel | Layering | ACAMS 4.3.3 | High |
| 5 | "Flood the zone" — high-frequency overload | Layering | Emerging typology — 2025 | High |
| 6 | Cross-chain bridge (THORChain) at scale | Layering | FATF VA §5.3 · ACAMS 4.3.3 | Critical |
| 7 | Fan-out to 4,876+ BTC addresses | Layering | ACAMS 4.3.3 · FATF R.20 | High |
| 8 | No-KYC instant exchange (eXch) | Layering | ACAMS 5.1.1 · FATF R.15 | High |
| 9 | Mixer chain (Wasabi → CryptoMixer → Railgun) | Layering | FATF VA §5.2 | High |
| 10 | Stablecoin freeze delay (Circle) | Integration | FATF R.20 · GENIUS Act 2025 | High |

---

## Recommendations

### For AML Analysts and Compliance Teams

1. **Implement macro volume monitoring** on DEX/bridge protocols — +50% vs. 30-day average triggers alert regardless of address tagging status
2. **Document "flood the zone" as a monitoring rule** — high-frequency cross-platform transactions within 48 hours of a known theft event = automatic escalation
3. **Expand DEX coverage in post-incident reviews** — PancakeSwap, Li.Fi, DODO, Paraswap are not captured in standard initial analysis
4. **Require independent signing UI verification** for any multi-sig wallet setup — separate hosted interface that shows raw transaction parameters, not just UI display

### For VASPs

5. **1-hour SLA for stablecoin freeze response** — operationalize Circle and Tether cooperation as part of incident response protocol
6. **Pre-block all known no-KYC instant exchanges** pending individual transaction review
7. **Subscribe to Bybit Blacklist API** and equivalent real-time feeds for post-incident address screening

### For Regulators

8. **Address protocol fee disgorgement** — if a protocol with governance capability profits from confirmed illicit flows and declines to act, fees should be subject to seizure
9. **Enforce against non-compliant instant exchanges** — eXch's closure without prosecution creates an exploitable gap
10. **Formalize volume anomaly monitoring** as a VASP obligation under FATF R.15 (new technologies risk assessment)

---

## Investigative Conclusions

Three takeaways I apply from this case:

**1. 77% traceability is both a success and a failure.**  
Blockchain's transparency enabled investigators to track the majority of funds in real time. But traceable funds that cannot be frozen or seized do not represent effective enforcement. The gap between forensic visibility and operational intervention is where future AML investment needs to focus.

**2. The weakest link in the ecosystem is not technical — it is organizational.**  
THORChain could have blocked more. Circle could have frozen faster. Regulators could have acted against eXch. The blockchain provided the data. The failure was in the response chain.

**3. Address-based detection alone is insufficient.**  
The PancakeSwap blind spot and the volume anomaly missed opportunity both demonstrate that next-generation AML monitoring must incorporate macro behavioral signals — not just entity screening. An analyst who waits for a flagged address will always be behind a sophisticated actor who diversifies across undocumented channels.

> *$1.46 billion. 10 days. 27% permanently dark. The blockchain recorded everything. The industry read only part of what was written.*

---

## Verify & References

- ZachXBT Telegram: [t.me/zachxbt](https://t.me/zachxbt)
- TRM Labs — Bybit laundering update: [trmlabs.com/resources/blog/bybit-hack-update](https://www.trmlabs.com/resources/blog/bybit-hack-update-north-korea-moves-to-next-stage-of-laundering)
- ChainArgos/Allium — PancakeSwap analysis: [chainargos.com](https://www.chainargos.com/a-deeper-dive-into-the-1-5-billion-bybit-hack/)
- Chainalysis — Bybit collaboration: [chainalysis.com/blog/bybit-exchange-hack-february-2025](https://www.chainalysis.com/blog/bybit-exchange-hack-february-2025-crypto-security-dprk/)
- NCC Group — Technical analysis: [nccgroup.com/research/in-depth-technical-analysis-of-the-bybit-hack](https://www.nccgroup.com/research/in-depth-technical-analysis-of-the-bybit-hack/)
- FBI PSA — TraderTraitor addresses: [ic3.gov](https://www.ic3.gov)
- Nansen — Fund flow analysis: [research.nansen.ai](https://research.nansen.ai/articles/by-bit-hack-what-happened-and-where-are-the-funds-going-on-chain)

---

*AML-IR-2026-004 · Phelipe Agnelli — AML & Blockchain Forensics*  
*All data from public sources. Published for educational purposes.*
