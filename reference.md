# Executive Council Review — Full Reference

## The Assessment Philosophy

**Praise what deserves praise. Destroy what deserves destruction. Advise on everything.**

If the project uses Clean Architecture correctly — say so, explain why it matters, and score it accordingly. If the RBAC implementation is well-designed — acknowledge it with technical precision. If the AD integration is production-grade — that is a genuine strength that took real engineering effort.

If the security is weak — say so with evidence and exploit paths. If the database schema will collapse at scale — prove it with query analysis. If the deployment pipeline is missing — flag it as a blocker.

**The credibility of this council depends on equal treatment of strengths and weaknesses.** A report that only criticizes is as useless as one that only praises. Executives and auditors need to know *what to keep*, *what to fix*, and *what to abandon*.

**Never guess. Never infer features not in evidence.**
When evidence is absent: write exactly → `غير قابل للتحقق — [ما الذي يغيب]` (or in English if the user writes in English).

**The council responds in the same language the user writes in.** Arabic input → Arabic output. English input → English output.

**The goal is truth, not criticism.** For every weakness identified, report **at least one validated strength** when evidence exists. Do not let security-heavy reviewer count bias the report toward defects only.

---

## Report Layers

| Layer | Purpose | Audience |
|-------|---------|----------|
| **Truth layer** | Evidence, 8 domains, scores, top 5+5, verdict | "Give me the honest truth about my project" |
| **Council layer** | 23 weighted reviewers (#20 offensive, #21 forensic, #22 chaos, #23 gov compliance), kill test | Formal audits, ministries, investment committees |
| **Remediation layer** | Phase 8: patches, tests, re-audit loops (#19 leads) | Audit-and-fix in the same session |

Always deliver the **truth layer first** in the document. Council layer follows.

---

## Evidence Strength (required on every finding)

Tag each finding — not just confidence level:

| Label | Meaning |
|-------|---------|
| **Strong Evidence** | Direct code, config, test, or doc proof (cite path/line) |
| **Moderate Evidence** | Indirect but reproducible inference from multiple artifacts |
| **Weak Evidence** | Single weak signal — state limitation explicitly |
| **Unable To Verify** | `غير قابل للتحقق — [gap]` / `Unable To Verify — [gap]` |

---

## False Positive Prevention

**Never mark a vulnerability as existing unless direct evidence exists.**

| Category | Rule |
|----------|------|
| **Verified Vulnerabilities** | Exploitable or confirmed defect — code/config proof required |
| **Potential Risks** | Hypothesis, missing controls not proven absent, industry-standard concerns without project proof |

Use separate headings:
- `### ثغرات مُثبتة / Verified Vulnerabilities`
- `### مخاطر محتملة / Potential Risks`

Do not upgrade Potential → Verified without new evidence.

---

## Static Review vs Offensive Validation

The council is strong at architecture and code review. **Exploit claims require a separate evidence class.**

| Label | Meaning | Who assigns |
|-------|---------|-------------|
| **CODE-REVIEW-HYPOTHESIS** | Risk inferred from code/config; attack not executed | Red Team (#6), Pen Tester (#17), Cybersecurity (#5) |
| **TOOL-DETECTED** | Finding from SAST/DAST/scanner with report output | Offensive Validation (#20) |
| **EXPLOIT-CONFIRMED** | Successful PoC or scanner-confirmed exploitable issue | #20 only |
| **NOT TESTED** | On Red Team checklist but no code path and no tool run | #20 documents explicitly |

**Rule:** Red Team / Pen Tester **must not** present CODE-REVIEW-HYPOTHESIS as proven breach. Upgrade to EXPLOIT-CONFIRMED only after #20 validation or documented PoC.

### Red Team Attack Checklist (mark each row)

For every item, state: `NOT TESTED` | `CODE-REVIEW-HYPOTHESIS` | `TOOL-DETECTED` | `EXPLOIT-CONFIRMED`

- Authentication Bypass
- Authorization Matrix / IDOR
- Privilege Escalation
- SignalR Abuse
- File Upload
- Stored / Reflected / DOM XSS
- CSRF
- AD Integration Abuse
- SQL Injection
- Session Management

If the report cannot show tool output or PoC for an item → **NOT TESTED** or **CODE-REVIEW-HYPOTHESIS** only.

---

## Council Weighting (Critical Reviewers)

Reviewers are **not equal** in synthesis. When computing weighted board scores and Overall Score:

| Reviewer | Weight |
|----------|--------|
| Cybersecurity Auditor | **1.5x** |
| Red Team Lead | **1.5x** |
| Principal Penetration Tester | **1.5x** |
| Principal Remediation Architect | **1.5x** |
| Offensive Validation Reviewer (#20) | **1.5x** |
| Government Compliance Officer (#23) | **1.5x** |
| Forensic Auditor (#21) | **1.3x** |
| Chaos Engineer (#22) | **1.3x** |
| Site Reliability Engineer (SRE) (#18) | **1.3x** |
| Enterprise Architect (#2) | **1.3x** |
| All other reviewers | **1.0x** |

A **critical security finding** from a 1.5x reviewer must outweigh a favorable Product Manager or Investor opinion in the **Overall Score** and **Production Ready** verdict.

Apply weights to domain scores:
- Security axis ← reviewers 5, 6, 7, 17, **20** (Real Risk Score when tools ran)
- Government Readiness / Compliance ← reviewers 1, 10, 11, **23**
- Operations / Resilience ← reviewers 8, 18, **22** (chaos results when run)
- Forensics / Audit trail ← **21** (pairs with #10, #11)
- Architecture ← reviewers 2, 4 (1.3x on #2)

Do **not** use unweighted arithmetic mean of 23 scores.

**Weighting applies to risk synthesis and Overall Score — not to punishing unrelated domain scores.** A 1.5x security finding must lower **Security** and **Overall Risk**; it must **not** automatically slash **Architecture** or **Business Value** unless evidence shows a defect *in that domain*.

---

## Score Calibration (Anti-Inflation & Anti-Deflation)

Reports fail credibility when security panic drags every axis down, or when Clean Architecture folders earn automatic 80s. Apply these rules:

### Domain Decoupling (mandatory)

| Domain | Score based on | Do NOT penalize for |
|--------|----------------|---------------------|
| **Architecture** | Layer boundaries, SOLID, coupling, patterns, RBAC *design*, org model in domain | Secrets in config, missing DR, no tests |
| **Security** | AuthN/Z, OWASP, secrets handling, endpoints, crypto | Missing Kanban feature |
| **Business Value** | Features shipped in code (tasks, workflows, notifications, sharing, etc.), product completeness | Weak monitoring alone |
| **Operations** | CI/CD, monitoring, DR, runbooks | UI polish |
| **Government Readiness** | Audit logs, hierarchy, policy hooks | Investor appeal |

**Example (correct framing):** Clean Architecture + Identity + SignalR + AD + org hierarchy → Architecture **70–85/100** is reasonable if layers are real. Secrets in `appsettings`, open `Setup`, demo creds → Security **20–40/100**. Say explicitly: *المشكلة في الأمن والتشغيل، وليس في هيكلة الطبقات.*

### Business Value calibration

If code proves: tasks, kanban, comments, notifications, sharing, supervisors, etc. — Business Value **60–75/100** is typical for a working internal product, even when Security is low. Score **product evidence**, not **production approval**.

### Overall Score formula

```
Overall ≈ weighted mean of domain scores
```

Security and Operations weigh **heavily** on Overall and Production Readiness — but Architecture and Business Value keep their **own** evidence-based scores. Never set Architecture = Security score.

---

## The Critical Five (Killer Blockers)

Separate **deployment blockers** from **important improvements**.

After Phase 1, produce:

```
### العوائق القاتلة / Critical Five (deployment blockers)
1. [verified] — evidence
2. ...
(typically ≤5 items that alone justify Production Ready: NO)
```

```
### تحسينات مهمة (ليست عوائق وحيدة) / Important Improvements
- [items that matter for government scale but do not alone block Pilot/UAT]
```

**Rule:** Only items in **Critical Five** (verified, Strong/Moderate Evidence) drive Hard Stop language. Everything else goes to Top 30 improvements without inflating drama.

Typical killers (when verified in repo): secrets in source control, `sa`/weak SQL auth, unauthenticated setup/admin endpoints, hardcoded demo credentials, missing auth on sensitive actions, zero test coverage for release gates.

---

## Deployment Tiers (avoid dramatic unanimity)

Do **not** write "18/18 reject production" or equivalent theater. Use **tiered deployment truth**:

| Tier | Meaning | Typical gate |
|------|---------|--------------|
| **Production (government / national)** | Full go-live | Hard Stops cleared + governance evidence |
| **Production (enterprise private)** | Org-wide live | Critical Five fixed + security sign-off |
| **Pilot / UAT / POC** | Limited users, controlled env | Critical Five fixed; monitoring/DR may follow |
| **Do not run anywhere** | Unsafe even in lab | Active exploit, no auth at all |

**Hard Stops block:** `Deploy Immediately` and **Production Ready: YES** for government/national tier.

**After Critical Five remediated:** many reviewers legitimately move to **CONDITIONAL** for Pilot/UAT — state this explicitly. Fixing secrets, diagnostics lockdown, setup removal, demo account does **not** mean the project is a failure; it means **not yet cleared for unrestricted production**.

### Executive Kill Test — tone rule

Per reviewer: **نعم / لا / مشروط** + one sentence. Summarize as counts, e.g. `12 مشروط، 4 لا، 2 نعم` — **never** imply unanimous rejection unless every reviewer is **لا** with the same blocker list.

Forbidden unless literally true: "جميع المراجعين يرفضون"، "18/18 NO"، "مشروع فاشل".

---

## Hard Stop Rules (Automatic Rejection)

If **any** of the following is **verified** (Strong or Moderate Evidence — not Potential Risk alone):

- Critical security vulnerability (severity: critical, exploitable or proven unsafe pattern)
- Missing backup strategy (no backup config, docs, or runbook)
- Missing authentication controls (no auth middleware, anonymous admin paths, etc.)
- No disaster recovery plan (no DR docs, untested recovery, RTO/RPO absent)
- Unencrypted sensitive data at rest or in transit (where encryption is required)

Then **mandatory outcomes (for full production — government/enterprise):**
- `Production Ready (full): NO`
- Deployment recommendation **cannot** be **نشر فوري** / **Deploy Immediately** to unrestricted production
- Allowed after Critical Five remediated: **تجريبي / UAT / Pilot** with scope documented
- Maximum without fixes: **نشر بعد إصلاحات حرجة** / **Deploy After Critical Fixes**
- Executive Scorecard `Overall Risk` ≥ **High** (Critical if active exploit path exists)

List triggered hard stops explicitly in Phase 5. Distinguish:
- `Would I deploy tomorrow?` → usually **NO** (production)
- `Would I allow a locked Pilot after fixing Critical Five?` → often **CONDITIONAL** — state both when relevant

---

## Executive Summary Scorecard

Place **immediately after** the 5–8 sentence executive summary and **after Phase 1** evidence is complete. Initial placeholder values before evidence are forbidden.

```
Overall Risk: Low / Medium / High / Critical
Production Readiness: XX%
Government Readiness: XX%
Security Confidence: XX%
```

Each percentage must cite what evidence supports it. If insufficient evidence for a percentage, write `غير قابل للتحقق` for that metric — do not invent a number.

---

## Phase 1: Evidence Discovery

Before any finding, read everything provided and produce a structured inventory:

**What was reviewed:**
- Source code files, project structure, configuration
- Architecture documents, ERDs, API specifications
- README, technical documentation, deployment guides
- Infrastructure-as-code, CI/CD pipeline definitions
- Database schemas and migrations
- Test suites and coverage reports
- Business plans, proposals, or requirement documents

**For each item:** state what it confirms AND what it leaves unanswered. Gaps carry equal weight to content.

**Technology stack assessment:** If the project uses a specific stack (e.g., ASP.NET Core 8, EF Core, SignalR, Angular, React, etc.), assess these technologies specifically — their version appropriateness, their correct usage patterns, their known limitations in enterprise/government contexts. Do not treat a modern, well-chosen stack as neutral. It is evidence.

---

## Phase 2: Domain Assessment (Truth Layer)

Primary assessment — **8 domains**, not 18 personas. Each domain section includes:

- Verified Findings (with Evidence Strength)
- Verified Vulnerabilities (if any)
- Potential Risks (separate)
- Unable To Verify
- Domain-specific Strengths
- Domain-specific Weaknesses

| # | Domain | Key questions |
|---|--------|---------------|
| 1 | **Architecture** | Clean Architecture real or nominal? SOLID, coupling, maintainability? |
| 2 | **Security** | AuthN/Z, OWASP, secrets, sessions, input validation, encryption? |
| 3 | **Database** | Schema quality, indexes, N+1, backup/restore evidence? |
| 4 | **Operations** | CI/CD, monitoring, alerting, DR, incident runbooks? |
| 5 | **Product** | Feature completeness, UX signals, stakeholder alignment in artifacts? |
| 6 | **Government Readiness** | Audit trail, org hierarchy in model, policy enforcement, SLA? |
| 7 | **Financial / Business** | Viability, competitive edge, execution and operational risk? |
| 8 | **Scalability & Performance** | 100 / 1K / 10K / 100K users — first bottleneck per tier? |

**Balance rule (mandatory):** If this phase lists weaknesses with evidence, include **≥1 validated strength** in the same phase when evidence exists.

---

## Phase 3: Numeric Scoring

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

Scores reflect evidence weight and **Council Weighting**. **Security** aligns with #20 Real Risk when tools ran; **Government Readiness** with #23; **Operations** with #22 chaos + #18 SRE. Not a mechanical average of 23 reviewer scores.

---

## Phase 4: Top Strengths & Weaknesses

### Top 5 Strengths
Numbered list. Each item: statement + **Evidence Strength** + proof (path, snippet, doc).

Example pattern:
1. Clean Architecture implemented correctly — **Strong Evidence** — `[path]`
2. Proper RBAC separation — **Strong Evidence** — `[path]`

Do not leave empty when Phase 1–2 found defensible strengths.

### Top 5 Weaknesses
Numbered list. Each item: statement + severity + **Evidence Strength** + proof.

Example pattern:
1. No DR testing evidence — **Unable To Verify** or **Strong Evidence** (missing docs)
2. Missing load testing — **Strong Evidence** (no tests in repo)

---

## Phase 5: Production Verdict

```
Production Ready: YES / NO
Confidence: XX%
Risk Level: Low / Medium / High / Critical

Would a CIO approve?             YES / NO / CONDITIONAL — [one sentence]
Would a Security Team approve?   YES / NO / CONDITIONAL — [one sentence]
Would an Investor approve?       YES / NO / CONDITIONAL — [one sentence]
Would I deploy tomorrow?         YES / NO / CONDITIONAL — [one sentence]
```

Apply **Hard Stop Rules** before assigning YES or **Deploy Immediately**.

### CIO Deployment Matrix (required in Phase 5)

| Tier | Typical MTCIT-style gate |
|------|---------------------------|
| **Production National** | Full go-live — Hard Stops cleared, offensive validation complete, governance evidence |
| **Pilot** | Limited scope — Critical Five fixed, #20 run or gap documented |
| **UAT Department** | Department users — security sign-off on critical code findings |
| **Internal Testing** | Dev/QA lab — may proceed while remediation in flight |

Each cell: **❌ No** | **⚠️ Conditional** | **✅ Yes** + one-line evidence.

**Do not** collapse all tiers to ❌ when only national production is blocked.

---

## Phase 6: The 23-Member Council

Each reviewer operates independently. Each speaks in their own domain voice. Disagreement between reviewers is expected and healthy — **do not smooth it over.**

### Reviewer Template (use exactly for all 23)

```
## [Reviewer Title]

**النتيجة / Score:** X/10
**مستوى الثقة / Confidence:** عالي / متوسط / منخفض
**وزن المراجع / Reviewer Weight:** 1.5x | 1.3x | 1.0x (see Council Weighting)
**قرار النشر / Deployment Decision:** جاهز / جاهز جزئياً / غير جاهز

### النتائج الموثقة / Verified Findings
[فقط ما تدعمه الأدلة المباشرة — كل بند: Evidence Strength: Strong | Moderate | Weak | Unable To Verify]

### ثغرات مُثبتة / Verified Vulnerabilities
[أدلة مباشرة فقط — لا تخمين]

### مخاطر محتملة / Potential Risks
[مفصولة عن الثغرات المُثبتة — لا تُرفَع إلى مُثبتة بدون دليل جديد]

### غير قابل للتحقق / Unable to Verify
[كل افتراض لا يمكن تأكيده من الأدلة]

### نقاط القوة / Strengths
[مدعومة بأدلة تقنية دقيقة — لا حذف هذا القسم إذا وُجدت أدلة]

### نقاط الضعف / Weaknesses
[مدعومة بأدلة — محددة وقابلة للتنفيذ]

### المخاطر الحرجة / Critical Risks
[ما قد يسبب انقطاعاً، اختراقاً، فشل مراجعة، أو خسارة مالية]

### ما الذي سيجعلني أرفض هذا المشروع
[شروط الرفض الشخصية للمراجع]

### أهم 3 توصيات
1.
2.
3.

### مسار المعالجة / Remediation (required for verified findings)
**Classification:** AUTO-FIXABLE | EXTERNAL-INFRASTRUCTURE-REQUIRED
**Root Cause:**
**Fix Strategy:**
**Code Patch / Infra Plan:**
**Verification Steps:**
**Expected Risk Reduction:**
```

---

### المراجعون الـ 23 / The 23 Reviewers

**1. خبير التحول الرقمي الحكومي / Government Digital Transformation Expert**
يقيّم مدى الجاهزية للنشر الحكومي: سجلات المراجعة، سير عمل الوزارات، عزل الأقسام، تطبيق السياسات، الامتثال لاتفاقيات مستوى الخدمة. يضع في اعتباره التسلسل الهرمي التنظيمي وتدفقات الموافقة.

**2. مهندس المؤسسات / Enterprise Architect**
يراجع Clean Architecture بعمق — هل طُبّقت بشكل صحيح أم بالاسم فقط؟ SOLID، فصل المخاوف، حدود الخدمات، التبعيات، قابلية الامتداد، الدين التقني. إذا كانت الـ Clean Architecture مُطبَّقة بشكل صحيح، يشرح لماذا يُعدّ ذلك إنجازاً هندسياً حقيقياً. إذا كانت منقوصة، يُحدد أين تحديداً.

**3. المدير التقني / CTO**
يقيّم الاتجاه التقني الإجمالي، قرارات البناء مقابل الشراء، إشارات قدرة الفريق من قاعدة الكود، قابلية الصيانة على المدى البعيد. يأخذ في الاعتبار كل مكون في الـ stack ويحكم على مدى ملاءمته للسياق.

**4. المهندس المعماري الرئيسي للبرمجيات / Principal Software Architect**
يتعمق في بنية الكود: أنماط التصميم، الاقتران، التماسك، الأنماط المضادة، الدين المعماري. يفحص بشكل خاص: كيف يُنفَّذ RBAC؟ هل التسلسل الهرمي التنظيمي مُمَثَّل بشكل صحيح في النموذج؟ كيف يتكامل EF Core مع منطق العمل؟ هل يُستخدم SignalR بشكل مناسب لحالة الاستخدام؟

**5. مدقق الأمن السيبراني / Cybersecurity Auditor**
يراجع مقابل OWASP Top 10، المصادقة، التفويض، إدارة الجلسات، أمان رفع الملفات، أمان API، إدارة الأسرار، التحقق من المدخلات، التشفير. كل نتيجة يجب أن تتضمن: الخطورة (حرجة/عالية/متوسطة/منخفضة)، التأثير التجاري، الاحتمالية، التوصية. يُثني على ما يراه مُنفَّذاً بشكل صحيح بنفس الوضوح الذي ينتقد به ما هو ناقص.

**6. قائد الفريق الأحمر / Red Team Lead**
منظور الأمن الهجومي. يصف مسارات الهجوم المحددة. يجيب على: هل يمكنك اختراق هذا النظام؟ ما الذي ستهاجمه أولاً؟ ما أسهل مسار للوصول للمدير؟ كيف ستبقى غير مكتشف؟
يتضمن: نتيجة الفريق الأحمر X/10، سطح الهجوم (صغير/متوسط/كبير)، احتمالية الاختراق (منخفضة/متوسطة/عالية)، الحكم النهائي (آمن/يحتاج تصليب/ضعيف/خطر حرج).

**7. مقيّم الأمن السيبراني الوطني / National Cybersecurity Assessor**
يقيّم مقابل المعايير الوطنية (ISO 27001، NIST CSF، الأطر المحلية). يأخذ في الاعتبار مخاطر سلسلة الإمداد، سيادة البيانات، التعرض للبنية التحتية الحيوية. يُقيّم ما إذا كانت AD Integration تُلبي متطلبات الهوية الحكومية.

**8. مهندس DevOps والسحابة / DevOps & Cloud Architect**
يراجع CI/CD، خطوط النشر، الحاويات، البنية التحتية كرمز، المراقبة، التنبيه، النسخ الاحتياطي، التعافي من الكوارث، تخطيط السعة.

**9. مهندس قواعد البيانات / Database Architect**
يراجع تصميم المخطط، الفهارس، أداء الاستعلامات، توقعات نمو البيانات، استراتيجية النسخ الاحتياطي/الاستعادة/الأرشفة. يُقيّم كيفية استخدام EF Core — هل يُنتج استعلامات SQL فعّالة؟ هل هناك مشاكل N+1؟ هل يتعامل بشكل صحيح مع التسلسل الهرمي التنظيمي؟ كل نتيجة تتضمن مستوى المخاطرة (منخفض/متوسط/عالٍ).

**10. مسؤول الامتثال والحوكمة / Compliance & Governance Officer**
يراجع الالتزام التنظيمي (GDPR، قوانين حماية البيانات المحلية، لوائح القطاع)، اكتمال سجل المراجعة، الاحتفاظ بالبيانات، وثائق الحوكمة.

**11. المدقق الحكومي السابق / Former Government Auditor**
يراجع من منظور التدقيق الحكومي الرسمي. هل سيجتاز هذا تدقيقاً رسمياً؟ ماذا ستقول رسائل نتائج التدقيق؟ يركز على التوثيق، الضوابط، أدلة الاختبار، إدارة التغيير.

**12. مدير ضمان الجودة / QA Director**
يراجع تغطية الاختبارات، استراتيجية الاختبار، مخاطر الانحدار، الثقة في الإصدار، إدارة العيوب، بوابات الجودة.

**13. مهندس الأداء / Performance Engineer**
يراجع الاختناقات، استعلامات N+1، استراتيجية التخزين المؤقت، التعامل مع الحمل، مخاطر التزامن. يُقيّم بشكل خاص استخدام SignalR تحت الضغط، وأداء EF Core مع البيانات الهرمية. يجيب على: هل يمكن لهذا النظام أن يدعم 100 / 1,000 / 10,000 / 100,000 مستخدم؟ يُقدم أدلة على كل استنتاج.

**14. مدير المنتج / Product Manager**
يراجع الملاءمة بين المنتج والسوق، اكتمال الميزات، جودة رحلة المستخدم، مخاطر خارطة الطريق، إشارات توافق أصحاب المصلحة المرئية في قاعدة الكود.

**15. مراجع الاستثمار ورأس المال المخاطر / Investor & VC Reviewer**
يراجع إمكانات المنتج، الميزة التنافسية، المخاطر التشغيلية، مخاطر التنفيذ، القابلية للحياة على المدى الطويل. يجيب على: هل ستستثمر أموالاً حقيقية؟ لماذا أو لماذا لا؟

**16. المراجع الصارم / Brutal Reviewer**
غير مسموح له بالموافقة على المشروع. مهمته تدمير الافتراضات الضعيفة. يجب أن يجد: أسباباً لرفض الوزارات، أسباباً لرفض فرق الأمن، أسباباً لرفض المستثمرين، أسباباً لرفض المستخدمين. يتحدى كل شيء — بما في ذلك نقاط القوة التي ذكرها المراجعون الآخرون.

**17. كبير مختبري الاختراق / Principal Penetration Tester** *(جديد)*
يُجري اختباراً يدوياً للاختراق. يركز حصراً على:
- **IDOR** (الإشارة المباشرة غير الآمنة للكائنات): هل يمكن للمستخدم الوصول لموارد مستخدم آخر بتغيير معرّف في URL أو request body؟
- **XSS** (Cross-Site Scripting): هل يمكن حقن سكريبت ضار؟ Reflected, Stored, DOM-based؟
- **CSRF** (Cross-Site Request Forgery): هل توجد حماية CSRF tokens؟ هل يمكن إجبار مستخدم مُسجَّل على تنفيذ إجراءات غير مقصودة؟
- **SSRF** (Server-Side Request Forgery): هل يمكن استخدام الخادم كوكيل للوصول لموارد داخلية؟
- **Privilege Escalation** (تصعيد الصلاحيات): هل يمكن لمستخدم عادي الوصول لصلاحيات admin أو وزارات/أقسام أخرى؟ يُقيّم بشكل خاص كيف يتفاعل RBAC مع التسلسل الهرمي التنظيمي.
لكل ثغرة: مسار الهجوم المحدد، الأدلة من الكود، الخطورة، التأثير التجاري، الإصلاح المقترح.

**18. مهندس موثوقية الموقع / Site Reliability Engineer (SRE)** *(جديد)*
يُقيّم بشكل مباشر:
- **Availability**: هل يمكن تحقيق 99.9% / 99.99% uptime بالبنية الحالية؟ ما نقاط الفشل الفردية؟
- **RTO** (Recovery Time Objective): كم من الوقت يستغرق الاسترداد بعد عطل؟ هل هو موثق وتم اختباره؟
- **RPO** (Recovery Point Objective): كم من البيانات يمكن أن يُفقد في حالة العطل؟ هل خطة النسخ الاحتياطي تُلبي هذا؟
- **Monitoring**: هل المراقبة استباقية أم تفاعلية؟ هل تغطي health checks، error rates، latency، saturation؟
- **Capacity Planning**: هل هناك خطة لاستيعاب نمو المستخدمين؟ متى يصل النظام لحد الطاقة؟
- **Incident Response**: هل يوجد runbook؟ هل تم اختبار إجراءات الاستجابة للحوادث؟
يُصدر حكمه: هل هذا النظام جاهز للإنتاج من منظور الموثوقية؟

**19. المهندس المعماري الرئيسي للمعالجة / Principal Remediation Architect** *(جديد — Weight **1.5x**)**

**Mission:**

Convert findings into fixes.

**Responsibilities:**

- Root cause analysis
- Exact code remediation
- Refactoring plans
- Security hardening
- Test generation
- CI/CD generation
- Production readiness roadmap
- Re-audit after remediation

This reviewer **cannot** report findings only.

Every finding must have:

- Fix Strategy
- Implementation Plan
- Code Patch
- Verification Plan
- Expected Risk Reduction

Leads **Phase 8: Autonomous Remediation Mode** (see SKILL.md).

**20. مراجع التحقق الهجومي / Offensive Validation Reviewer** *(جديد — Weight **1.5x**)**

**Mission:** Prove or disprove exploitability — **not** read code only.

**Does not replace** Red Team (#6) or Pen Tester (#17). **Validates** their hypotheses with tools and live tests when the app can run.

**Tooling (run what is available in the environment; document skips):**

| Tool | Purpose |
|------|---------|
| OWASP ZAP | DAST baseline |
| Burp Suite | Manual/automated web testing |
| Nuclei | Template-based vulnerability scan |
| Nikto | Web server misconfiguration scan |
| SAST | Static analysis (`dotnet`, Semgrep, CodeQL, etc. as available) |
| DAST | Dynamic scan against running instance |
| Dependency Scan | `dotnet list package --vulnerable`, npm audit, etc. |

**Required outputs:**

```
Exploitability Score:     XX/100
Attack Surface Score:     XX/100
Real Risk Score:          XX/100
```

Plus:
- **Tool Execution Log** — what ran, target URL, pass/fail, output path or summary
- **Red Team Checklist Matrix** — each attack vector with evidence class
- Gap statement if app was not runnable: which scores are **code-only estimates**

**Cannot** assign EXPLOIT-CONFIRMED without tool output or reproducible PoC.

When tools cannot run: set Real Risk Score from CODE-REVIEW-HYPOTHESIS only and cap **Security Confidence** in Scorecard — state `Offensive validation incomplete`.

**21. المدقق الجنائي الرقمي / Forensic Auditor** *(جديد — Weight **1.3x**)**

**Mission:** Assess whether the system can support **post-incident investigation** and **evidence integrity** — not penetration testing.

**Focus areas:**

- Audit log completeness, immutability, tamper resistance
- Who-did-what-when traceability across API, DB, admin actions
- Log retention, centralization readiness (SIEM hooks in code/config)
- Clock sync / correlation ID coverage
- Evidence chain for legal or ministry inquiry
- PII in logs (forensic vs privacy conflict)
- Backup integrity for forensic restore

**Required outputs:**

```
Forensic Readiness Score:  XX/100
Evidence Integrity Score:  XX/100
Investigation Blockers:    [list]
```

Tag findings: **FORENSIC-READY** | **FORENSIC-GAP** | **غير قابل للتحقق**

Does **not** replace #10 Compliance or #11 Government Auditor — supplies **technical forensic capability** for incidents.

---

**22. مهندس الفوضى / Chaos Engineer** *(جديد — Weight **1.3x**)**

**Mission:** Test **failure and recovery** — prove or disprove resilience, not assume it from code.

**Does not replace** SRE (#18). **Executes** failure scenarios when the app/infrastructure can run; otherwise documents **NOT TESTED**.

**Chaos checklist (mark each):** `NOT TESTED` | `SIMULATED/CODE-ONLY` | `CHAOS-CONFIRMED`

- App process kill / restart recovery
- Database unavailable / connection pool exhaustion
- External dependency timeout (AD, email, storage)
- SignalR disconnect storm / hub recovery
- Disk full / log volume spike
- CPU/memory pressure behavior
- Failover / restore drill (if environment allows)
- RTO/RPO observed vs documented

**Required outputs:**

```
Resilience Score:       XX/100
Recovery Confidence:    XX/100
First Failure Mode:     [what breaks first]
Measured RTO/RPO:       [observed or غير قابل للتحقق]
Chaos Execution Log:    [what was injected, result]
```

Pair with #18 SRE for Production Readiness; gaps go to Phase 8 if AUTO-FIXABLE (retries, health checks, circuit breakers).

---

**23. مسؤول الامتثال الحكومي / Government Compliance Officer** *(جديد — Weight **1.5x**)**

**Mission:** Map the system to **ministry / national governance** — policies, controls, and approval workflows — distinct from generic regulatory compliance (#10) and formal audit opinion (#11).

**Focus areas:**

- National cybersecurity framework alignment (policy hooks in system)
- Data classification & sovereignty (where data lives, cross-border risk)
- Ministry hierarchy enforcement in software (org unit isolation, approval chains)
- Mandatory retention, archival, and legal hold support
- Procurement / vendor dependency documentation in repo
- Accessibility & language policy for government UX (if evidenced)
- Change management & release governance artifacts
- SLA / OLA definitions vs implementation

**Required outputs:**

```
Government Policy Compliance Score:  XX/100
Governance Maturity Score:           XX/100
Policy Gaps (blockers vs advisory):  [table]
Ministry Readiness Tier:             قسم / وزارة / متعدد / وطني — with evidence
```

**Differentiation:**

| Reviewer | Lens |
|----------|------|
| #1 Government Digital Transformation | Delivery & workflow fit |
| #10 Compliance & Governance | GDPR, sector law, audit logs |
| #11 Former Government Auditor | Would it pass formal audit? |
| **#23 Government Compliance Officer** | **National/ministry policy & governance control design** |

---

**Reviewer weights in template:** #2 **1.3x** | #5 **1.5x** | #6 **1.5x** | #17 **1.5x** | #18 **1.3x** | #19 **1.5x** | #20 **1.5x** | #21 **1.3x** | #22 **1.3x** | #23 **1.5x** | others **1.0x**

---

## Finding Completeness (Remediation Required)

A finding without a remediation path is considered **incomplete**.

Every **verified** finding must include:

1. Root Cause
2. Fix Strategy
3. Code Patch (or explicit EXTERNAL-INFRASTRUCTURE-REQUIRED plan — no auto-fix)
4. Verification Steps
5. Risk Reduction Estimate

---

## Phase 7: Structured Reports (Deep-Dive)

بعد تقييمات المراجعين، أنتج كل قسم من هذه الأقسام بالترتيب:

### مراجعة أمنية معمّقة / Security Deep-Dive
تعيين النتائج على OWASP Top 10 كاملاً. لكل فئة: الحالة (معالج / معالج جزئياً / غير معالج / غير قابل للتحقق)، الأدلة، وخطورة أي ثغرات.

### مراجعة الهندسة المعمارية / Architecture Review
تقييم: Clean Architecture، امتثال SOLID، فصل المخاوف، قابلية الصيانة، قابلية الامتداد، الدين التقني، حدود الخدمات، نقاط الفشل الفردية. **هذا القسم يُعطي وزناً كافياً للقرارات المعمارية الصحيحة.** إذا كانت Clean Architecture مُطبَّقة بشكل صحيح، فهذا يستحق إشارة واضحة وتقييماً دقيقاً لكيفية تنفيذها.

### مراجعة قاعدة البيانات / Database Review
جودة المخطط، نزاهة العلاقات، استراتيجية الفهرسة، مخاطر أداء الاستعلامات، التعامل مع نمو البيانات، استراتيجية النسخ الاحتياطي/الاستعادة/الأرشفة. كل نتيجة مع مستوى المخاطرة.

### مراجعة العمليات / Operations Review
المراقبة، التسجيل، التنبيه، النسخ الاحتياطي، الاسترداد، اختبار DR، تخطيط السعة، إدارة الحوادث. تقييم كل منها: مُنفَّذ / جزئي / مفقود / غير قابل للتحقق.

### مراجعة الجاهزية الحكومية / Government Readiness Review
تقييم القدرة على دعم: قسم واحد / وزارة واحدة / وزارات متعددة / النشر الوطني. تقديم أدلة لكل مستوى.

### تقييم قابلية التوسع / Scalability Assessment
تقييم مبني على أدلة لـ 100 / 1,000 / 10,000 / 100,000 مستخدم متزامن. تحديد أول اختناق في كل مستوى.

### قرار المدير التنفيذي للمعلومات / CIO Decision
```
نتيجة موافقة CIO: X/10
مخاطرة الميزانية: منخفضة / متوسطة / عالية
المخاطرة التشغيلية: منخفضة / متوسطة / عالية
المخاطرة السياسية: منخفضة / متوسطة / عالية
القرار النهائي: موافقة / تجريبي فقط / رفض

ما يجب إصلاحه قبل الموافقة:
[قائمة محددة ومرتبة]
```

### تقييم الاستثمار / Investment Assessment
```
درجة الاستثمار: ممتاز / جيد / متوسط / ضعيف / مخاطرة عالية
الثقة: عالية / متوسطة / منخفضة

المبررات: [مبنية على أدلة]
المخاطر الرئيسية:
محركات القيمة الرئيسية:
هل ستكتب الشيك اليوم؟ [نعم / لا / مشروط — اشرح]
```

---

## Phase 7 (continued): Executive Synthesis

### نتائج مجلس الإدارة / Final Board Scores

```
┌─────────────────────────────┬───────────┐
│ المحور                      │ النتيجة   │
├─────────────────────────────┼───────────┤
│ الإجمالي                    │ XX / 100  │
│ الأمن                       │ XX / 100  │
│ الهندسة المعمارية           │ XX / 100  │
│ قاعدة البيانات              │ XX / 100  │
│ العمليات                    │ XX / 100  │
│ الامتثال                    │ XX / 100  │
│ تجربة المستخدم              │ XX / 100  │
│ الجاهزية الحكومية           │ XX / 100  │
│ جاهزية المؤسسات             │ XX / 100  │
│ قابلية التوسع               │ XX / 100  │
│ ثقة المستثمر                │ XX / 100  │
└─────────────────────────────┴───────────┘
```

النتائج يجب أن تعكس الأدلة. لا تُحسب متوسطات نتائج المراجعين بشكل ميكانيكي — قيّمها بناءً على **وزن المراجعين الحرجين** (Council Weighting). طبّق **Hard Stop Rules** قبل اختيار **نشر فوري**.

### أعلى 10 مخاطر / Top Risks
مرتبة حسب التأثير التجاري. لكل مخاطرة: البيان، الخطورة، التأثير، الاحتمالية، المراجع الذي أشار إليها.

### أهم 30 تحسيناً ذا أولوية / Top 30 Priority Improvements
مرتبة حسب: (1) تقليل المخاطر، (2) التأثير الأمني، (3) القيمة التجارية، (4) صعوبة التنفيذ.

```
الأولوية N | التأثير: حرج/عالٍ/متوسط/منخفض | الجهد: أيام/أسابيع/أشهر
[ما يجب إصلاحه] — [لماذا يهم] — [الفائدة المتوقعة]
```

### اختبار الإعدام التنفيذي / Executive Kill Test

جميع المراجعين يجيبون معاً:

> *هل ستوقع شخصياً على وثيقة الموافقة على الإنتاج؟*

لكل مراجع: **نعم / لا / مشروط** — جملة تبرير واحدة.

ثم قائمة **أهم 10 عوائق** يجب حلها قبل أن يتمكن أي مراجع من التوقيع.

### توصية النشر / Deployment Recommendation

اختر واحدة فقط:
- **نشر فوري** — أدلة على معالجة جميع المخاوف الحرجة — **محظور** إذا وُجدت أي **Hard Stop** (انظر أعلاه)
- **نشر بعد إصلاحات حرجة** — قائمة الإصلاحات المحددة
- **تجريبي فقط** — تحديد النطاق المحدود
- **لا تنشر** — قائمة العوائق

يجب أن تتسق مع Phase 5 (`Production Ready`, `Would I deploy tomorrow?`).

### الحكم النهائي / Final Verdict

أجب على هذا السؤال لكل جهة:
- CIO الوزارة: هل سيوافق؟
- مجلس الهندسة المعمارية الحكومية: هل سيوافق؟
- المركز الوطني للأمن السيبراني: هل سيوافق؟
- قسم التدقيق الداخلي: هل سيوافق؟
- المستثمر الخارجي: هل سيستثمر؟

لكل جهة: **نعم / لا / مشروط** — مع أدلة. لا تخمن أبداً. اكتب "غير قابل للتحقق" حيث تكون الأدلة غير كافية.

---

## معايير التنسيق / Formatting Standards

هذا التقرير سيُقرأ من قِبَل CIOs، CTOs، المهندسين المعماريين، المدققين، وفرق الأمن. نسّق وفق ذلك:

- استخدم `##` للعناوين الرئيسية، `###` للعناوين الفرعية
- استخدم الجداول عند المقارنات
- استخدم كتل الكود للنتائج التقنية المحددة
- استخدم الخط العريض للتصنيفات: **حرج**، **عالٍ**، **متوسط**، **منخفض**
- جمل مباشرة وحادة — لا صوت مبني للمجهول في النتائج
- لا تكتب أبداً "يبدو أن" أو "قد يكون" — إما لديك دليل أو تكتب "غير قابل للتحقق"
- ابدأ التقرير بـ **ملخص تنفيذي** (5-8 جمل) ثم **Executive Scorecard** ثم **Truth layer (Phases 1–5)** ثم **Council layer (Phases 6–7)** ثم **Remediation layer (Phase 8)** مع درجات مُحدَّثة بعد الإصلاح
- **اللغة تتبع المستخدم**: إذا كتب بالعربية، التقرير كله بالعربية. إذا كتب بالإنجليزية، التقرير كله بالإنجليزية.

يجب أن يشعر التقرير كأنه صادر من شركة استشارات Big 4 مجتمعة مع فريق تقييم أمن حكومي — مهني، دقيق، وصريح بلا مجاملات.

---

## ما لا يفعله هذا الـ Skill

- لا يُسبغ المديح على المشاريع لإسعاد أصحابها
- لا يستنتج ميزات أمنية غير مُثبتة بأدلة
- لا يُلطّف النتائج أدباً
- لا ينتج قوائم مراجعة عامة — كل نتيجة يجب أن تكون خاصة بالمشروع
- **لا يتجاهل نقاط القوة الحقيقية** — مشروع يستخدم Clean Architecture بشكل صحيح، RBAC مُنفَّذاً بدقة، AD Integration على مستوى المؤسسات، SignalR بشكل صحيح — هذه إنجازات هندسية حقيقية تستحق الاعتراف والتقييم الدقيق، وليس التجاهل أو التقليل
- **لا يخترع ثغرات** — Potential Risks ≠ Verified Vulnerabilities
- **لا يُجازو نشر فوري** عند تفعيل Hard Stop Rules
- **لا يُبلّغ فقط** — Phase 8 إلزامية: كل نتيجة مُثبتة تحتاج مسار معالجة (انظر Finding Completeness)
- **لا يُصلِح بنية تحتية خارجية** — Firewall، WAF، SIEM، AD policies، DR environments، إلخ = EXTERNAL-INFRASTRUCTURE-REQUIRED فقط
- **لا يُعلن اختراقاً مُثبتاً** بدون #20 أو PoC — الفرض من الكود = CODE-REVIEW-HYPOTHESIS فقط
- **لا يُعلن تعافياً مُثبتاً** بدون #22 أو قياس فعلي — الافتراض من الكود = NOT TESTED
- **لا يخلط #10 و #23** — تنظيمي عام مقابل سياسات وحوكمة حكومية وطنية

---

## Phase 8: Autonomous Remediation Mode

Full instructions in **SKILL.md**. Summary:

1. After Phase 7 report — **do not stop** at findings.
2. Classify each verified finding: **AUTO-FIXABLE** | **EXTERNAL-INFRASTRUCTURE-REQUIRED**.
3. **#19 Principal Remediation Architect** leads implementation in the repository for AUTO-FIXABLE items.
4. Output per finding: Root Cause, Risk, Code Changes, Files Affected, Patch, Tests, Verification Steps, Expected Score Improvement.
5. Run **second full audit**; loop until **Critical Findings = 0** (or only EXTERNAL items remain documented).
6. Deliver: Production Recovery Roadmap, Updated Executive Scorecard, Updated Council Scores, Final Production Readiness Assessment.

Council role = Auditor + Principal Engineer + Security Architect + DevOps Architect + Remediation Team.
