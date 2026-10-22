# deployment-solution-doc — Oxide-Style Customer Deployment Plan (Work Sample)

> **SYNTHETIC / METHOD DEMONSTRATION — synthetic / 方法演示**
> Every customer name, site, address plan, timeline, and workload in this
> repository is invented for demonstration. No real customer, employer, or
> production data appears anywhere in this repository. Statements about the
> Oxide product are a separate category: each one is cited to a public Oxide
> source (link in-line), and cited product facts must not be confused with
> the synthetic scenario wrapped around them.

## What this is

A work sample for a Solutions Architect application (Oxide Computer).
The Oxide Solutions Architect role, as published, includes: building deep
relationships across a customer's teams, engaging stakeholders from
executive leadership through application development, operations, security,
architecture and deployment teams, developing "comprehensive customer
deployment plans, including data center facilities prep, network
integration, operations tooling, and developer onboarding," and owning a
customer's success from earliest planning through operational deployment.
Source: https://oxide.computer/careers/solutions-architect

This repository demonstrates exactly that deliverable: one complete,
internally consistent deployment plan for one synthetic customer, written
the way the author has written rack-level build/deployment documentation
in production roles (L11/L12 rack integration build packs, data-center
SOPs), restructured for a pre-sales / customer-success setting.

Author background relevant to this sample (stated qualitatively, no
invented metrics): rack-level (L11/L12) AI server integration and POC
build documentation as a PE at Quanta; data-center infrastructure
engineering at Asiacom at roughly 1,000-rack scale, including
troubleshooting SOPs and onboarding new engineers; PMP since 2010.

## How to read it

Read in order; each document ends with open items and decision gates.

| # | Document | Question it answers |
|---|----------|---------------------|
| 0 | `docs/00-scenario-and-scope.md` | Who is the (synthetic) customer, what is in/out of scope, who decides what? |
| 1 | `docs/01-facilities-readiness.md` | Can the room take the rack — power, cooling, floor, path, clearances? |
| 2 | `docs/02-network-integration.md` | Where exactly does the rack plug into the existing network, and how do routes flow? |
| 3 | `docs/03-phased-cutover-and-rollback.md` | In what order do we go live, and how do we back out at each step? |
| 4 | `docs/04-operations-tooling-and-onboarding.md` | Who operates it on day 2, with what tools, trained how? |
| 5 | `docs/05-defense-qa.md` | One page: the hard technical and commercial questions, and the honest answers. |
| 6 | `docs/06-poc-sample-ai-support.md` | POC plan sample (AI customer-support agent): discovery questions, quantifiable success criteria, weekly timeline, risks, acceptance — on the author's own de-identified business case. |
| — | `VERIFICATION.md` | Sources consulted, link checks, and self-audit results. |

## Honesty boundaries (read this first if you are evaluating)

1. **Public product facts** — anything about Oxide hardware, software, or
   documented procedure carries an in-line source link to
   oxide.computer, docs.oxide.computer, the public Omicron documentation,
   or the public RFD site. If a product claim has no link, treat it as an
   error and flag it.
2. **Synthetic scenario** — the customer ("Meridian"), its sites, IP
   ranges, timelines, team names, and workload descriptions are invented.
   Example IP values reuse the documentation-style ranges that Oxide's
   own public guides use in their examples, or RFC documentation ranges,
   and are labelled as examples.
3. **No invented numbers** — this repository contains no fabricated
   first-pass-yield percentages, cost-savings percentages, performance
   benchmarks, or customer outcomes. Where a commercial question needs a
   number, the document specifies *which inputs to collect and how the
   calculation would be run with the customer*, instead of inventing the
   answer. Product numbers that do appear (dimensions, weight, power,
   temperatures) are quoted from the public specifications page with a
   link, and the specs page itself notes that actual specifications vary
   by configuration.
4. **Known limits of the sample** — the author has not operated an Oxide
   rack. Operational statements are built from Oxide's public operator
   and system guides, cited as such; anything that in a real engagement
   would be confirmed with Oxide delivery/support is listed as an open
   item rather than asserted.

## Structure

```text
deployment-solution-doc/
  README.md            (this file)
  VERIFICATION.md      (sources + self-audit)
  docs/
    00-scenario-and-scope.md
    01-facilities-readiness.md
    02-network-integration.md
    03-phased-cutover-and-rollback.md
    04-operations-tooling-and-onboarding.md
    05-defense-qa.md
```
