# 06 — POC Plan Sample: AI Customer-Support Agent (WhatsApp)

> **SAMPLE / SELF-RUN BUSINESS CASE — de-identified**
> This is a proof-of-concept (POC) plan sample written in the format I
> would propose to a customer, worked through on a real deployment: the
> WhatsApp AI customer-service agent I built and operate for **my own
> small auto-parts business** (San Jose / Santa Clara area). It is
> labelled, here and in interviews, as exactly that — **a self-run
> business case （自营业务案例）, de-identified （已脱敏）**: no customer
> name, no third-party business is represented, and no result is claimed
> for anyone else. Numbers below are either (a) facts of my own
> deployment that I can source, listed in Section 1, or (b) forward-
> looking POC targets to be agreed with a customer — never past results
> dressed up as targets already met. Any number I cannot source is not
> written; that is a standing rule of this document.

Companion material (one line, no duplication): the full case pack behind
this sample — customer journey, health-score design, QBR sample, and
at-risk playbook, all on the same self-run case — lives in
`~/workspace/your_files/求职/Decagon案例包/`; this file is only the
POC-plan view of that same case and does not repeat its contents.

## 1. Scenario and sourced facts

A small business takes most of its enquiries on WhatsApp, many outside
business hours. Customers ask the same families of questions — product
fitment for their vehicle, price, installation, warranty, payment — and
unanswered enquiries are lost sales. The POC question: **can an AI agent
handle the repetitive enquiry flow end to end (fitment lookup, quote,
FAQ, order-intent capture, hand-off to a human or a physical store),
under business rules the owner controls, without ever inventing an
answer?**

Facts of the reference deployment (sourced from my own records; these
are the only deployment numbers used anywhere in this sample):

- Production go-live: **2026-10-01**, on Meta WhatsApp Cloud API with the
  service hosted on Render.
- Dialogue is a state machine: greeting → collect vehicle → fitment
  lookup → quote → order intent; FAQs are answered inside the quoting
  step without breaking the sales flow.
- Fitment catalogue: **63 common vehicle generations**, of which **42
  entries are multi-source verified**; a query with no catalogue match
  is written to an *unknown-queries log* for human follow-up and
  catalogue backfill — the agent never guesses a fitment.
- Business rules are deterministic, not model-improvised: negotiation is
  allowed **once, capped at 5%**; prices come from the catalogue, not
  from the language model.
- Languages: replies in the customer's language — Chinese, English,
  Spanish, Russian, Persian templates implemented, with an honest
  quality boundary: the non-core templates are functional translations,
  and the Persian set is a minimal phrase set still needing native
  review.
- Hardening round completed 2026-10-07: webhook signature verification
  enforced fail-closed, session state persisted across process restarts
  (verified by kill-and-restart test), monthly usage counters with
  alerts at **70% and 90%** of quota, **56 tests passing** at that round.

Explicitly **not** claimed, because I have no systematic record of them:
conversation volume, resolution rate, automation rate, satisfaction,
retention, or revenue figures. A POC exists precisely to start measuring
such things under agreed definitions — see Section 3.

## 2. Discovery question list (14 questions)

Asked before any build, in this order — business first, technology last.
Each question exists because getting it wrong surfaces in week two as a
production failure.

**Business and scope**

1. Which enquiry types actually arrive, in what rough mix — and which
   three types, if the agent handled only those, would already make
   this worthwhile?
2. What is the business outcome of a handled conversation: an answer, a
   quote, an order intent, a booking, a visit to a physical location?
   Where exactly does "handled" end?
3. What happens today to an enquiry that arrives at 11 pm — and what is
   it costing, in your own terms?
4. Which questions must the agent **never** answer on its own (pricing
   exceptions, complaints, safety, anything contractual) — the
   hand-off list, agreed by the business owner, not by engineering?

**Data and business rules**

5. What is the authoritative source for product / fitment / price data,
   who owns it, and how stale is it allowed to get before the agent
   must stop quoting from it?
6. Which answers are rules (price, discount limits, warranty terms) and
   which are language (explanations, recommendations)? Who signs off
   the rule wording?
7. When the data has no answer — a product or model the catalogue does
   not cover — should the agent say so and log it, or try to be
   helpful? (My standing recommendation: say so and log it. A logged
   miss is fixable; an invented answer is a liability.)
8. Which languages do your customers actually write in, and for each:
   is launch quality required, or is a functional template with a
   stated quality boundary acceptable for the POC?

**Channels, people, and operations**

9. Where do customers enter (marketplace listing, QR code, website,
   existing number) — and who will physically test every entry point
   with a second phone before launch?
10. Who takes hand-offs, on what hours, and what does the customer see
    while waiting? Who is the named human owner on the customer side?
11. Who reviews agent conversations, how often, and who has authority
    to change an answer, a rule, or the catalogue — and how fast can a
    bad answer be pulled?
12. What volume do you expect in the pilot window, and what platform
    quotas (messaging tiers, model usage) does that imply — including
    what alert thresholds (for example 70% / 90%) should fire before
    a limit is hit, not after?

**Success and risk**

13. What would make you call this POC a failure at the review meeting —
    in one sentence? (Asked deliberately: it usually surfaces the real
    success criterion faster than asking what success looks like.)
14. What is explicitly **out of scope** for this POC, and what happens
    to the agent, the number, and the data if the decision is "no"?

## 3. Success criteria (quantifiable, agreed before build)

Criteria are pass/fail against a defined test set and a defined pilot
window — not impressions. Targets below are the ones I would propose
for this scenario; in a real engagement each is confirmed or adjusted
with the customer at the end of discovery (Week 1 gate).

| # | Criterion | Target | How it is measured |
|---|---|---|---|
| S1 | No dead ends | **100%** of conversations in the acceptance test set end in exactly one of: answered, order intent captured, or handed to a human with context | Scripted test set covering every in-scope intent plus known edge cases; each run scored end-state by end-state |
| S2 | No invented answers | **0** fabricated fitments, prices, or policy statements in the acceptance test set and in the pilot conversation review | Test set includes out-of-catalogue queries on purpose; every agent answer traceable to catalogue / rule / FAQ source |
| S3 | Unknowns are captured, not dropped | **100%** of unanswerable queries present in the unknown-queries log with the customer's original wording | Log audited against sampled raw conversations at pilot end |
| S4 | Rule integrity | **100%** of negotiation test cases respect the agreed rule (one concession, capped at the agreed maximum); **0** price quotes that differ from the catalogue | Scripted negotiation probes, run before go-live and weekly during pilot |
| S5 | Hand-off integrity | **100%** of conversations hitting a hand-off trigger reach the agreed human channel with a summary (intent, data collected, what the agent already told the customer) | Test probes per trigger type; pilot spot-checks |
| S6 | Security gate | **100%** of unsigned / bad-signature webhook requests rejected in pre-launch testing; **0** accepted | Negative testing before go-live, exactly as done in the reference deployment (bad-signature requests returned 403) |
| S7 | State survival | Conversation state restored correctly after a service restart in **100%** of restart tests (including mid-negotiation state) | Kill-and-restart test before go-live, repeated once during pilot |
| S8 | Language behaviour | In the test set, replies stay in the customer's language for **100%** of in-scope exchanges; a mid-conversation short reply ("OK") does not flip the language | Scripted multilingual test conversations per launch language |
| S9 | Usage headroom | Usage alerts configured and test-fired at the agreed thresholds (reference deployment: **70% / 90%** of monthly quota) before pilot traffic starts | Alert test message observed in the agreed channel |
| S10 | Owner verdict | The business owner completes one end-to-end conversation as a customer, unaided, and signs the acceptance checklist — or the POC does not pass, whatever the test scores say | Signed checklist (Section 5) |

Deliberately absent: any deflection-rate, satisfaction, or revenue
target. Those become measurable only after a baseline exists; agreeing
them in a first POC would mean inventing the baseline, which this plan
refuses to do.

## 4. Timeline (week granularity)

| Week | Work | Gate / deliverable |
|---|---|---|
| Week 1 | Discovery: the 14 questions of Section 2, enquiry-type inventory, rule and hand-off lists signed by the business owner; success criteria of Section 3 confirmed or adjusted | **Gate 1 — scope freeze**: signed one-page POC charter (scope, out-of-scope, criteria, pilot window dates) |
| Week 2 | Build / configuration: dialogue flows for in-scope intents, catalogue and FAQ loaded from the owner's authoritative source, rules implemented as deterministic logic, hand-off channel wired | Working agent in a test number; rule table reviewed line by line with the owner |
| Week 3 | Acceptance testing against the Section 3 criteria: scripted test set, negative (security) tests, restart test, multilingual runs; fixes and re-tests; entry-point check (every QR code / link physically scanned with a second phone) | **Gate 2 — go / no-go for pilot**: all S-criteria pass on the test set, or the failing criterion is named and the pilot is re-scoped in writing |
| Week 4 | Limited pilot: live on the real entry points, conversations reviewed daily, unknown-queries log worked (catalogue backfill), weekly rule-integrity probes (S4) re-run | Daily review notes; mid-pilot checkpoint with the owner at week's end |
| Week 5 | Acceptance review: criteria scored against pilot evidence, S10 owner verdict, cost / quota report for the pilot window, recommendation (adopt / extend pilot / stop) with the rollback state confirmed either way | **Gate 3 — decision meeting**: signed acceptance record; if "stop", Section 5 rollback executed and verified |

Total elapsed time: **5 weeks** from charter to decision. In the
reference deployment the same arc ran compressed — production go-live
on 2026-10-01 with the hardening round closed on 2026-10-07 — because
the builder and the business owner were the same person; for a customer
engagement the 5-week shape above is the honest version, since the
gates are where the owner, not the builder, decides.

## 5. Risks and mitigations

| # | Risk | What it looks like when it happens | Mitigation built into this plan |
|---|---|---|---|
| R1 | Agent invents an answer (wrong fitment, wrong price) | A confident, wrong reply the customer acts on — the highest-harm failure in support | Answers sourced only from catalogue / rules / FAQ; unknowns logged, never guessed (S2, S3); catalogue is the owner's signed source (discovery Q5) |
| R2 | Entry point silently broken | Wrong QR code or stale number on listing material: zero traffic, no error anywhere | Physical scan-test of every entry point in Week 3 and at pilot start (this failure was caught in the reference business precisely by questioning a poster QR code) |
| R3 | Platform quota hit mid-pilot | Messaging or model-usage limit reached; agent degrades or stops without warning | Usage counters with alerts at agreed thresholds (70% / 90%) test-fired before traffic (S9); fallback behaviour defined at Gate 1 |
| R4 | Session state lost on restart | Host restarts; a customer mid-negotiation restarts from zero — or worse, the discount rule resets | Persisted session state; kill-and-restart acceptance test (S7); rules enforced in state, not in the model's memory |
| R5 | Rules drift from the business | Owner changes a price or policy verbally; the agent keeps quoting the old rule | Single rule table, owner-signed, changed only through the Week-4 review loop; weekly rule probes (S4) catch divergence |
| R6 | Language quality overstated | Templates in a lower-volume language are functional, not native-grade; customers notice before the business does | Quality boundary stated in writing per language at discovery (Q8); native review listed as a pre-scale action, never silently assumed |
| R7 | Hand-off is a dead letter | Agent escalates correctly, but no human is actually watching the channel | Named human owner and hours agreed at discovery (Q10); hand-off probes tested end to end (S5), not just agent-side |
| R8 | Scope creep during pilot | "While you are here, can it also…" expands the POC past its criteria and its window | Out-of-scope list in the charter (Q14); new requests logged for the post-POC roadmap, decided at Gate 3, not absorbed mid-pilot |
| R9 | Configuration split across platforms | Settings live in two consoles (messaging platform + hosting); a change on one side silently breaks the other | Deployment checklist covering both consoles; any change re-runs the checklist regression (S1, S6 spot re-tests) |

## 6. Acceptance method

1. **Test-set acceptance (Week 3):** the Section 3 criteria are scored
   on a scripted test set that includes, by design, out-of-catalogue
   queries, negotiation probes, hand-off triggers, bad-signature
   requests, and a restart mid-conversation. A criterion either passes
   at its stated target or is named as failed in the gate record —
   there is no aggregate score to hide behind.
2. **Pilot evidence (Week 4):** daily conversation review by the
   customer-side owner and the engineer together; the unknown-queries
   log and the rule-probe results are the standing agenda. Any invented
   answer found in review (S2 breach) stops the pilot until fixed and
   re-tested — that stop rule is agreed at Gate 1, while everyone is
   still friendly.
3. **Owner acceptance (Week 5):** the signed checklist — criteria
   scores, pilot evidence summary, quota report, open risks — plus the
   owner's own unaided end-to-end conversation (S10). 
Acceptance is
   a decision, recorded, not a vibe.
4. **Rollback, defined in advance:** at any gate the customer can end
   the POC by (a) disabling the agent's auto-reply so the number
   reverts to plain human-operated WhatsApp, (b) removing the pilot
   entry points, and (c) exporting the conversation and unknown-query
   logs, which belong to the business. No customer data is held hostage
   in the agent platform; the catalogue and rule table are the owner's
   documents from day one.

## 7. How I would talk about this sample in an interview (10 minutes)

Minute 0–2: the scenario and the honesty label — my own business, my
own money, de-identified here. Minutes 2–5: discovery — why the rule
questions (Q4–Q7) come before any technology question, and what the
unknown-queries log is for. Minutes 5–8: success criteria — why S2
(zero invented answers) outranks any deflection number, and why there
is no satisfaction target in a first POC. Minutes 8–10: the two failures
this plan is built around — a silently broken entry point (R2) and a
stateless restart (R4) — both of which I met in the real deployment,
not in a textbook.

---

*Sources for the reference deployment's facts (Section 1): production
README and records of the Wiper-Service-Agent / WhatsServe project
(go-live 2026-10-01; catalogue counts; 2026-10-07 hardening round with
56 passing tests, fail-closed signature verification, persisted
sessions, 70% / 90% usage alerts), and the case pack cross-referenced at
the top of this file. Everything else in this document is plan, not
history.*
