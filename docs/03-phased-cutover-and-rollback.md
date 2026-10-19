# 03 — Phased Go-Live Plan and Rollback

> **SYNTHETIC / METHOD DEMONSTRATION — synthetic / 方法演示**
> Durations and team names are illustrative planning artefacts for the
> synthetic customer, not commitments and not Oxide-published figures,
> except where a public guide states a duration (noted in-line).
> "Rollback" here is defined honestly: until Gate G4, nothing has been
> migrated, so rollback means *removing the new rack from service* —
> not restoring workloads, because workloads were never moved.

## Phase overview

| Phase | Content | Entry gate | Rollback if it fails |
|---|---|---|---|
| 0 | Readiness: facilities + network pre-work frozen | — | Nothing installed; stop |
| 1 | Physical install + power validation | G1: Section 01 checklist Pass | Remove rack; position returns to empty |
| 2 | Initial rack setup via wicket; control plane online | G2: technician-port isolation demonstrated | Power down rack; no customer routes exist yet |
| 3 | Rack configuration: silos, IdP, IP pools, quotas | G3: API/DNS reachable per Section 02 checks | Delete silos/pools; rack is an island again |
| 4 | Pilot: two teams onboard, pilot workloads only | G4: go/no-go review with all stakeholders | Remove routes + uplinks; legacy estate untouched throughout |
| 5 | Steady state + optional BGP step (Section 02) | G5: pilot exit review | BGP change reverts to the documented static topology |

## Phase 0 — Readiness (target: complete 2–4 weeks before install)

The 2–4 week figure is taken from Oxide's public Power Connection
guide, whose checklist is to be completed 2–4 weeks before
installation. Source: https://docs.oxide.computer/guides/facilities/power-connection

- Section 01 checklist items evidenced (meter readings, drawings).
- Section 02 pre-work frozen into a reviewed `rack.toml`: the public
  Initial Rack Setup guide is structured around this file (upstream
  DNS/NTP, delegated zone, external DNS IPs, service pool, sled list,
  source-IP allowlist, per-switch uplink configuration).
  Source: https://docs.oxide.computer/guides/system/initial-rack-setup
- Transceivers, fiber, and RJ45 on site; jumpbox with IPv6 SLAAC
  enabled verified (Rack Installation readiness list).
  Source: https://docs.oxide.computer/guides/system/rack-installation
- **G1 decision:** facilities lead + SA sign; unsigned, no ship date.

## Phase 1 — Physical install and power validation

- Uncrate, position, level; connect power per the order (entry
  direction as ordered); validate power.
- LED gate from the public guide: all power-shelf LEDs solid (no
  blinking/red/amber), all sled power LEDs solid, both switch power
  LEDs solid. Any failure returns the work to power troubleshooting
  before network connectivity — the guide's own instruction.
  Source: https://docs.oxide.computer/guides/system/rack-installation
- Insert transceivers into the planned QSFP ports; connect uplink fiber
  via the top-of-rack patch panel; verify the physical port ↔ logical
  `qsfpN` mapping matches what will be configured (the guide warns the
  patch-panel layout can be non-intuitive), clean fiber, confirm link
  LEDs; an LED that is off or blinking, or solid with no upstream link,
  each has a documented diagnosis path in the guide (same source).
- **Rollback:** the rack has no network identity yet; backing out is a
  facilities action only.

## Phase 2 — Initial rack setup

- Connect the jumpbox to one technician port per switch; verify SLAAC
  advertisements are received and SSH to `wicket` succeeds on both
  switches (the guide gives the expected neighbor output shape and a
  symptom/cause/solution table if addresses do not appear).
  Source: https://docs.oxide.computer/guides/system/rack-installation
- Run initial setup in wicket: component validation, software update
  step as presented by the installer, `rack.toml` configuration, rack
  initialization. The public guide budgets this phase at 2–4 hours with
  a network engineer on standby — the one duration in this plan taken
  from a vendor source rather than invented.
  Source: https://docs.oxide.computer/guides/system/rack-installation
- Transceiver signal quality (Rx/Tx power) is verified in this phase in
  the wicket TUI (noted in the Rack Installation guide).
- **G3 checks (from the public Completing Rack Configuration guide):**
  reach the rack's uplink gateway, confirm uplink TX/RX counters move,
  resolve the `recovery` silo name under the delegated zone.
  Source: https://docs.oxide.computer/guides/system/completing-rack-config
- **Rollback:** rack is powered/configured but carries no customer
  routes and no tenants; backing out = remove uplinks, power down.

## Phase 3 — Configuration to "ready for tenants"

- Delegate the zone in the parent DNS (glue records to the rack-hosted
  DNS addresses) — the mechanism described in Network Preparations.
  Source: https://docs.oxide.computer/guides/system/network-preparations
- Add static routes on customer routers toward service and instance
  prefixes via the rack uplink addresses (Section 02 design).
- Create the first silos, link IP pools, make the default pool, set
  quotas; configure the SAML identity provider (Section 04).
  Sources: https://docs.oxide.computer/guides/system/completing-rack-config
  and https://docs.oxide.computer/guides/configuring-access
- **Rollback:** delete silos/pools/routes. Note the documented sharp
  edge, planned around in Section 04: changing an IdP definition can
  require deleting and recreating the silo, so IdP values are verified
  against the IdP side *before* silo creation, in a pre-production
  checklist, not discovered after.
  Source (Entra example): https://docs.oxide.computer/guides/operator/ms-entra

## Phase 4 — Pilot and the go/no-go gate (G4)

- Two synthetic pilot teams onboard per Section 04; pilot workloads
  only — new, non-critical, and reproducible elsewhere.
- Pilot exit criteria (agreed at G1, judged at G4): control plane and
  API usable by both teams without SA intervention; support path
  exercised at least once end-to-end (a real ticket, however small);
  operations team confirms the Section 04 runbook set is sufficient to
  handle the incidents actually encountered; open limitations
  (Section 04) re-checked against the then-current release notes.
- **Rollback:** because no production workload has migrated, rollback
  remains: drain pilot instances, remove routes and uplinks, rack
  returns to vendor per commercial terms (commercial terms are outside
  this technical plan and are not asserted here).

## Phase 5 — Steady state and optional changes

- Optional BGP enablement per Section 02, as its own change with its
  own rollback (revert to the static configuration, which is kept as
  the documented fallback).
- Software updates: handled per the posture in Section 04 (release
  notes in earlier releases state operator-driven update is unavailable
  and updates are performed by Oxide technicians; the plan re-verifies
  the current release's notes before scheduling any update window
  rather than assuming either state).
  Source (example): https://docs.oxide.computer/release-notes/system/14
  and https://docs.oxide.computer/release-notes/system/10
- Any second-rack or migration programme starts as a new plan that
  cites this one's pilot exit report — not as a silent extension of
  this plan.
