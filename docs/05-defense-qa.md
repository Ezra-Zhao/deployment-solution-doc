# 05 — Defense Q&A (One Page)

> **SYNTHETIC / METHOD DEMONSTRATION — synthetic / 方法演示**
> The questions are the ones a competent customer's team should ask.
> Answers never invent a number: product facts cite public sources;
> commercial answers specify the model and the inputs, not a result.

## Technical challenges

**Q1. "Your power numbers don't agree with each other. Which is it?"**
Correct catch — the public specifications page lists max draw up to
21.6 kW redundant / 30 kW non-redundant and, in its Facilities section,
"will support up to 15 kW" depending on density. We size your room from
the order summary for your exact configuration, and Section 01 does not
sign off until that document is in hand.
Source: https://oxide.computer/product/specifications

**Q2. "What breaks if one rack switch dies?"**
The design connects both switches toward the gateway path; the
documented basic topology uses BFD so traffic can redirect to the
surviving switch during failure or maintenance. Pilot runs on static
routes first, so the failure behaviour you accept is the one you have
already seen in test, not a slide.
Source: https://docs.oxide.computer/guides/architecture/networking

**Q3. "Can it take our full routing table?"**
No — the switches currently import approximately 1,000 routes and full
Internet tables are explicitly not supported. The design aggregates at
your border; if your architecture needs full tables at the rack, that
is a genuine mismatch we should surface now, not after delivery.
Source: https://docs.oxide.computer/guides/system/network-preparations

**Q4. "How do we monitor it — temperatures, power, VM metrics?"**
Honestly: the vendor's own release notes have listed hardware metrics
(temperatures, fan speeds, power) as not exposed to the control plane,
and early releases lacked guest VM CPU/memory metrics. We monitor what
the API and network state expose, cover the physical layer with your
facilities sensors, and re-verify the current release notes at every
gate. If a metric you require is not there, that is a decision input,
not something I will paper over.
Sources: https://docs.oxide.computer/release-notes/system/10 and
https://docs.oxide.computer/release-notes/system/1-0-1

**Q5. "Who patches it?"**
Release notes across versions state operator-driven update has been
unavailable, with updates performed by Oxide technicians; newer notes
describe supported upgrades with instance shutdown recommended. So the
honest answer is: updates are a coordinated vendor window under the
current release's documented behaviour — planned in Section 04, never
assumed to be self-service until the notes for your release say so.
Sources: https://docs.oxide.computer/release-notes/system/14 and
https://docs.oxide.computer/release-notes/system/10

**Q6. "Identity integration — what if we need to change the IdP setup?"**
Plan it before you create the silo: the vendor's IdP guides warn that
changing a provider definition can require deleting and recreating the
silo, with cascading effects. Our onboarding checklist verifies every
IdP value against your IdP first, and offboarding is a written runbook
item because the release notes call out its limits.
Sources: https://docs.oxide.computer/guides/operator/ms-entra and
https://docs.oxide.computer/release-notes/system/10

**Q7. "Is this just a proprietary box we're locked into?"**
The vendor's public position: open APIs, SDKs, and a Terraform
provider; standard VMs so existing tooling works; open-source firmware
and control plane you can inspect. Your exit story in this plan is
structural, though, not rhetorical: until Gate G4 nothing has migrated,
and rollback is removing routes and uplinks.
Sources: https://oxide.computer/solutions/sovereign-cloud and
`03-phased-cutover-and-rollback.md` in this repository.

## Commercial challenges

**Q8. "What will it cost / what will we save?"**
I will not invent a percentage. The model we run together: your current
fully loaded cost of the capacity in scope (hardware refresh, support
contracts, power/cooling at your rates, operations hours by activity)
versus the quoted rack configuration's purchase and support terms over
the same horizon, with utilisation measured from your existing estate —
your numbers, your rates, shown with the arithmetic. Oxide's public
compensation note is a reminder that even vendor pricing has
exceptions (sales roles may differ), so the only cost figures that
count are the ones on your quote.
Source: https://oxide.computer/careers

**Q9. "Why not just stay on public cloud / refresh what we have?"**
Sometimes you should. This plan is justified only where ownership,
auditability, locality, or data-residency requirements bite — the
vendor's sovereign-cloud framing is explicit that data stays in your
authority on hardware you own and can verify, and that it runs fully
air-gapped. If those are not your constraints, the refresh option may
win, and Section 00 says so by scoping this to one rack and a pilot
gate rather than a wholesale commitment.
Source: https://oxide.computer/solutions/sovereign-cloud

**Q10. "What's the risk if we say yes?"**
Bounded by construction: one rack, one room, no migrated production
workloads before the G4 review, rollback = disconnect. The residual
risks are named, not hidden: facilities sign-off depends on the order
summary (Q1), monitoring has documented gaps (Q4), updates are a
vendor-coordinated activity (Q5), and IdP changes are expensive if
rushed (Q6). A plan that lists its own weaknesses is the kind you can
actually manage.
