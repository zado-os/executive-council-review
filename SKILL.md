---
name: executive-council-review
description: >
  Runs a boardroom-grade Executive Review Council on any project: source code, web apps, SaaS, government systems, enterprise software, AI projects, architecture docs, or business plans. Produces an evidence-based truth report plus 23 weighted reviewers (Offensive Validation, Forensic Audit, Chaos Engineering, Government Compliance, remediation) and Autonomous Remediation Mode. Use for audit, audit-and-fix, offensive validation, chaos/DR testing, or executive evaluation — "review my project", "audit this", "red team this", "run DAST", "security review", "is this production-ready", "fix findings", "government deployment review", "what would a CIO think".
---

# Executive Council Review

> **The goal is truth, not criticism.** Praise what deserves praise. Flag what deserves flags. Never invent findings.

Two layers in every report:
1. **Truth layer (primary)** — Evidence → 8 domains → scores → top strengths/weaknesses → verdict. What executives need first.
2. **Council layer (formal)** — 23 weighted reviewers + offensive/forensic/chaos validation + deep-dives + kill test. See [reference.md](reference.md).
3. **Remediation layer** — Phase 8: patches, tests, re-audit loops. Auditor + engineer + security + DevOps.

## Before You Start

1. Read [reference.md](reference.md) — weighting, hard stops, static vs offensive evidence, council template, all 23 reviewers, remediation rules.
2. **Gather evidence** with code review **and** run available tools (#20 offensive, #22 chaos when app runs). Do not claim exploits or failover tests executed without proof.
3. **Match user language:** Arabic → Arabic. English → English.

## Workflow (execute in order)

```
Progress:
- [ ] Phase 1: Evidence Discovery
- [ ] Executive Scorecard (populate after Phase 1 — place at TOP of final report)
- [ ] Phase 2: Domain Assessment (8 domains)
- [ ] Phase 3: Numeric Scoring
- [ ] Phase 4: Top 5 Strengths + Top 5 Weaknesses
- [ ] Phase 5: Production Verdict
- [ ] Phase 6: 23-Member Weighted Council (#20–#23 validation roles)
- [ ] Phase 7: Structured Deep-Dives + Executive Synthesis
- [ ] Phase 8: Autonomous Remediation Mode
```

### Phase 1: Evidence Discovery

Answer explicitly:
- **What exists?** (with file/path references)
- **What is missing?**
- **What cannot be verified?** → `غير قابل للتحقق — [gap]` / `Unable To Verify — [gap]`

Tag every finding with **Evidence Strength:** Strong / Moderate / Weak / Unable To Verify (see reference.md).

Technology stack: assess versions, usage patterns, enterprise/government fit — strengths count as evidence.

### Executive Scorecard (top of report, after Phase 1)

```
Overall Risk: Low / Medium / High / Critical
Production Readiness: XX%
Government Readiness: XX%
Security Confidence: XX%
```

Percentages must trace to Phase 1 inventory — not guesses.

### Phase 2: Domain Assessment (primary truth)

Assess these **8 domains** with verified findings only. Separate **Verified Vulnerabilities** from **Potential Risks** (reference.md).

| Domain | Covers |
|--------|--------|
| Architecture | Clean Architecture, SOLID, coupling, maintainability |
| Security | AuthN/Z, OWASP, secrets, encryption |
| Database | Schema, indexes, EF/SQL, N+1, backup evidence |
| Operations | CI/CD, monitoring, DR, incident response |
| Product | UX signals in code, feature completeness |
| Government Readiness | Audit logs, org hierarchy, policy, SLA |
| Financial / Business | Viability, execution risk, value drivers |
| Scalability & Performance | Load paths, SignalR, concurrency, user tiers |

**Balance rule:** For every weakness with evidence, report **at least one validated strength** when evidence exists.

### Phase 3: Numeric Scoring

Evidence-weighted scores per **domain decoupling** (reference.md): weak security must not automatically slash Architecture or Business Value. Apply council weighting to **Overall Risk**, not to copy Security score into every axis.

List **Critical Five** killers vs **Important Improvements** before final scores.

```
Security ............ XX/100
Architecture ........ XX/100
Database ............ XX/100
Operations .......... XX/100
Scalability ......... XX/100
Maintainability ..... XX/100
Government Ready .... XX/100
Business Value ...... XX/100

Overall Score ....... XX/100
```

### Phase 4: Strengths & Weaknesses

**Top 5 Strengths** — each with evidence (code path, config, doc). Never empty if Phase 1 found real strengths.

**Top 5 Weaknesses** — each with evidence and severity. Potential risks listed separately, not mixed with verified issues.

### Phase 5: Production Verdict

```
Production Ready (full national): YES / NO
Confidence: XX%
Risk Level: Low / Medium / High / Critical

Would a CIO approve?        YES / NO / CONDITIONAL — one line
Would a Security Team approve? YES / NO / CONDITIONAL — one line
Would an Investor approve?   YES / NO / CONDITIONAL — one line
Would I deploy tomorrow?     YES / NO / CONDITIONAL — one line
```

**CIO Deployment Matrix** (required — evidence per tier):

| Tier | Decision |
|------|----------|
| Production National | ❌ No / ⚠️ Conditional / ✅ Yes |
| Pilot | ❌ / ⚠️ / ✅ |
| UAT Department | ❌ / ⚠️ / ✅ |
| Internal Testing | ❌ / ⚠️ / ✅ |

**Hard Stops:** block **Production National** and **Deploy Immediately** when triggered (reference.md). **UAT / Internal / Pilot** may be ✅ after Critical Five fixes — not blanket rejection.

### Phase 6: 23-Member Weighted Council

All 23 reviewers, independent voices, template in reference.md.

| # | Role | Must produce |
|---|------|----------------|
| 20 | Offensive Validation | Tool log, Real Risk scores, Red Team matrix |
| 21 | Forensic Auditor | Log/audit integrity, forensic readiness score |
| 22 | Chaos Engineer | Failure tests run or NOT TESTED, recovery metrics |
| 23 | Government Compliance Officer | Policy/governance gap matrix vs ministry requirements |

**#20:** Red Team / Pen Tester (#6, #17) stay **CODE-REVIEW-HYPOTHESIS** until #20 confirms **EXPLOIT-CONFIRMED**.

Apply **Critical Reviewer Weighting** when synthesizing scores. Do not smooth disagreement.

### Phase 7: Structured Deep-Dives + Synthesis

OWASP mapping, architecture/DB/ops/government/scalability deep-dives, CIO block, investment block, board scores table, top 10 risks, top 30 improvements, executive kill test, deployment recommendation (respect hard stops), final stakeholder verdicts.

### Phase 8: Autonomous Remediation Mode

After completing all audit phases and producing the final report:

DO NOT stop at reporting findings.

The council must automatically switch into Autonomous Remediation Mode.

For every verified finding:

Identify the root cause.
Explain why it exists.
Estimate implementation effort.
Estimate risk reduction.
Generate exact code modifications.
Generate replacement code where applicable.
Generate migration steps.
Generate unit tests.
Generate integration tests.
Generate validation steps.
Re-audit the modified implementation.
Update readiness scores.

Every finding must be classified as:

AUTO-FIXABLE
or
EXTERNAL-INFRASTRUCTURE-REQUIRED

AUTO-FIXABLE findings must be fully remediated.

Examples:

Authorization issues
Missing validation
Hardcoded secrets
Missing health checks
Missing CI/CD pipelines
Missing tests
Missing logging
Missing security headers
SignalR authorization issues
Dependency vulnerabilities
RBAC weaknesses
Configuration mistakes

EXTERNAL-INFRASTRUCTURE-REQUIRED findings must never be auto-fixed.

Examples:

Firewall configuration
WAF
SIEM
Active Directory policies
Backup systems
Disaster Recovery environments
Network segmentation
Production certificates
Hardware capacity

For every AUTO-FIXABLE finding the council must output:

Root Cause
Risk
Code Changes
Files Affected
Patch
Tests
Verification Steps
Expected Score Improvement

After all fixes are generated:

Run a complete second audit.

If Critical findings remain:
Repeat remediation and audit cycles.

Continue until:

Critical Findings = 0

Then generate:

Production Recovery Roadmap
Updated Executive Scorecard
Updated Council Scores
Final Production Readiness Assessment

The council must behave as:

Auditor
+
Principal Engineer
+
Security Architect
+
DevOps Architect
+
Remediation Team

not merely as an auditor.

## Non-Negotiable Rules

| Rule | Requirement |
|------|-------------|
| False positives | Never mark a **vulnerability** without direct code/config evidence |
| Potential vs verified | **Potential Risks** ≠ **Verified Vulnerabilities** — separate sections |
| Hard stops | Critical vuln, no backup, no auth, no DR, unencrypted sensitive data → no immediate deploy |
| Weighting | #20 #23 **1.5x**; #2 #18 #21 #22 **1.3x**; see reference.md |
| Offensive proof | No **exploit confirmed** without #20 tool run or PoC |
| Chaos proof | No **resilience confirmed** without #22 test or measured drill — else NOT TESTED |
| Forensic / Gov | #21 forensic gaps and #23 policy gaps scored separately from Security |
| Truth balance | Weakness + existing strength evidence → report at least one strength |
| Domain decoupling | Architecture/Business scored on their own evidence — not security panic |
| Critical Five | Only verified killers block production; rest = improvements |
| No drama | No "18/18 reject" unless literally all **لا**; use deployment tiers |
| Language | No "يبدو أن" / "may seem" — evidence or Unable To Verify |
| **Incomplete finding** | A finding without a remediation path is considered **incomplete** |
| **Remediation package** | Every **verified** finding must include: (1) Root Cause, (2) Fix Strategy, (3) Code Patch, (4) Verification Steps, (5) Risk Reduction Estimate |
| **Phase 8** | AUTO-FIXABLE items must be remediated in-repo; never auto-fix EXTERNAL-INFRASTRUCTURE-REQUIRED |

## Report Order (final document)

1. Executive Summary (5–8 sentences, board-ready)
2. Executive Scorecard
3. Phases 1–5 (truth layer)
4. Phases 6–7 (council + synthesis)
5. Phase 8 (remediation artifacts, re-audit, updated scores)
6. Code citations and patches for critical findings

## What This Skill Must Not Do

See [reference.md](reference.md) § prohibitions.
