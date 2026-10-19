# 00 — Scenario, Scope, and Stakeholders

> **SYNTHETIC / METHOD DEMONSTRATION — synthetic / 方法演示**
> "Meridian" does not exist. Its sites, people, addresses, and timelines are
> invented so the plan can be concrete. Oxide product statements are cited
> to public sources and are the only non-synthetic content in this file.

## 1. The synthetic customer

**Meridian Analytics (synthetic)** — a mid-size analytics company:

- One owned data hall ("Hall B") plus a small legacy virtualization estate
  it wants to start retiring; a second phase (not in this plan's scope)
  may add a rack at a colocation site.
- Driving needs, stated in the customer's own terms for this exercise:
  (a) capacity it owns and can audit, in its own facility;
  (b) a self-service, API-driven way for internal platform teams to get
  VMs, instead of ticket-driven provisioning;
  (c) no forklift migration — existing workloads keep running while the
  first rack lands and proves itself.
- First purchase in scope: **one Oxide rack**, installed in Hall B,
  integrated with Meridian's existing aggregation network, DNS, NTP, and
  corporate identity provider, with two internal teams onboarded as the
  first tenants.

Why one rack first is a deliberate design choice of this plan, not a
limitation of the product: it bounds facilities work to one rack
position, keeps rollback trivial (Section 03), and gives the customer a
measured decision point before any multi-rack commitment. Product fact,
for orientation only: an Oxide rack is a single integrated rack-scale
system — up to 32 1st-generation or up to 24 2nd-generation compute sleds
per rack on the current public specifications page, with two network
switches and redundant power shelves.
Source: https://oxide.computer/product/specifications

## 2. Scope

### In scope (this plan)

1. Facilities readiness for one rack position in Hall B (Section 01).
2. Network integration: management (technician) access, data uplinks,
   routing (static first, BGP as a designed option), DNS delegation,
   IP pools (Section 02).
3. Rack initialization and configuration to a defined "ready for
   tenants" state (Section 03, Phases 0–3).
4. Identity integration: one silo per onboarded team, corporate SAML IdP,
   role mapping (Section 04).
5. Operations handover: tooling, runbook set, support path, update
   posture (Section 04).
6. Pilot onboarding of two internal teams and a go/no-go review before
   any workload migration (Sections 03–04).

### Explicitly out of scope

- Migrating any named production workload (pilot workloads only; bulk
  migration is a follow-on plan written after the go/no-go review).
- Retiring the legacy estate (no decommission dates are promised here).
- A second site / second rack, Kubernetes platform build-out, and any
  public-cloud repatriation programme — all named as Phase-2 candidates
  so nobody mistakes silence for a promise.
- Any cost or savings figure. See the honesty rule in the README: this
  plan defines the cost model inputs (Section 05) but invents no result.

## 3. Stakeholder map (mirrors the role's engagement surface)

The published role engages "a wide range of stakeholders within customer
IT organizations, from executive and organizational leadership through
application development, operations, security, architecture and
deployment teams."
Source: https://oxide.computer/careers/solutions-architect

| Stakeholder (synthetic role) | What they must get from this plan | Where |
|---|---|---|
| CIO / IT leadership | Decision gates, risk posture, what is *not* promised | 03, 05 |
| Enterprise architecture | Target topology, routing/DNS design, exit/rollback story | 02, 03 |
| Data-center facilities | Power/cooling/floor/path checklist with pass criteria | 01 |
| Network engineering | Uplink, MTU, routing, firewall, technician-port isolation detail | 02 |
| Security / CISO office | Silo isolation model, IdP integration, source-IP allowance, support access path | 02, 04 |
| Operations (NOC / SRE) | Day-2 tooling, monitoring limits stated honestly, escalation path | 04 |
| Application / platform teams | Onboarding path: silo, quotas, IP pools, CLI/API/Terraform entry points | 04 |
| Oxide delivery & support (vendor side) | Facilities sign-off inputs, install-day dependencies, update handling | 01, 03, 04 |

## 4. Governing principles

1. **Nothing irreversible before the go/no-go gate.** Until the pilot
   review, every step can be backed out by removing the rack's routes
   and uplinks; legacy systems are untouched (Section 03).
2. **Configuration is written down before it is typed.** The rack's
   initial network configuration is prepared in advance as a reviewed
   file (`rack.toml` in Oxide's documented flow) — the public Initial
   Rack Setup guide is built around exactly this prepared-configuration
   model. Source: https://docs.oxide.computer/guides/system/initial-rack-setup
3. **Isolation by design, not by convention.** Tenant separation uses
   Oxide silos; management access uses physically and logically isolated
   technician-port segments (Sections 02, 04).
4. **Honest limits in writing.** Where the public release notes state a
   current limitation (for example, operator-driven software update and
   some telemetry in earlier releases), the plan designs around the
   documented state and re-checks the current release notes at each
   gate, instead of promising the limitation away.
   Source (example, v10 notes): https://docs.oxide.computer/release-notes/system/10

## 5. Open items owned by this document

- Confirm with Oxide which rack generation/configuration is being quoted
  before Section 01 numbers are frozen (specs vary by configuration).
- Confirm the customer's IdP product (the plan assumes SAML; Oxide's
  public guides include worked examples for several IdPs — Section 04).
- Sponsorship/contracting questions are commercial, out of this
  technical plan's scope, and are not addressed or assumed anywhere in
  this repository.
