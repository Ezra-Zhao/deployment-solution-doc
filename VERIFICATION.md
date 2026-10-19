# VERIFICATION

> Verification performed 2026-10-19 (America/Los_Angeles), at the time of
> writing. Anything not actually checked is said so, in those words.

## 1. Public sources consulted

Read in full via browser fetch (page text), 2026-10-19:

| Source | What was taken from it |
|---|---|
| https://oxide.computer/product/specifications | Rack/sled/switch/power-shelf dimensions, weight, power, temperature, humidity, elevation, airflow, crate, clearances, transceiver and fiber types, technician-port setup |
| https://docs.oxide.computer/guides/system/network-preparations | Network pre-work list, technician ports, L3-router behaviour, /31 uplink examples, MTU, service-pool sizing, ~1,000-route import limit, IPv6-only option |
| https://docs.oxide.computer/guides/system/rack-installation | Facilities-complete precondition, LED gate, patch-panel/QSFP mapping, technician-port isolation and its failure modes, SLAAC checks, 2–4 h setup-phase note |

Consulted via browser search results with substantive page excerpts,
2026-10-19 (not full-page fetches — stated honestly):

| Source | What was taken from it |
|---|---|
| https://oxide.computer/careers/solutions-architect | Role duties quoted in README and Section 00 |
| https://oxide.computer/careers | Compensation note and its sales exception (Section 05, Q8) |
| https://docs.oxide.computer/guides/facilities/power-connection | 2–4-week pre-install checklist, connector types, entry direction, electrical validation items |
| https://docs.oxide.computer/guides/system/initial-rack-setup | `rack.toml` structure, service pool min 13 / recommended 20, source-IP allowlist option |
| https://docs.oxide.computer/guides/system/completing-rack-config | Silo/IP-pool linking, NAT sizing example, post-setup connectivity checks, local-user limits |
| https://docs.oxide.computer/guides/architecture/networking | OPTE/DDM/Maghemite overview, basic static-route topology with BFD |
| https://docs.oxide.computer/guides/operator/configuring-bgp | BGP port/peer configuration flow |
| https://docs.oxide.computer/guides/operator/expanding-connectivity | Address-lot and VLAN-tagged peering expansion flow |
| https://docs.oxide.computer/guides/configuring-access | Group role assignment, `admin_group_name`, `mapped_fleet_roles` |
| https://docs.oxide.computer/guides/operator/ms-entra | IdP-change-requires-silo-recreation warning |
| https://docs.oxide.computer/guides/operator/keycloak | SAML integration example exists (existence only) |
| https://oxidecomputer.github.io/omicron/docs/control-plane-architecture/ | Nexus / CockroachDB / OpenAPI control-plane description |
| https://rfd.shared.oxide.computer/rfd/0053 | RFD 53 exists and covers control-plane data storage requirements (existence + topic only; no design claim in this repo rests on its details) |
| https://oxide.computer/solutions/sovereign-cloud | API coverage, open-source/verifiability and integration claims, air-gapped operation |
| https://docs.oxide.computer/release-notes/system/14 | Supported upgrade path, shutdown recommendation, auto-restart, anti-affinity groups |
| https://docs.oxide.computer/release-notes/system/11 | Upgrade notes, instance auto-restart policy |
| https://docs.oxide.computer/release-notes/system/10 | Operator-driven update unavailable; hardware telemetry not exposed; offboarding limit |
| https://docs.oxide.computer/release-notes/system/5 | Silo quotas; sled exclusion from placement |
| https://docs.oxide.computer/release-notes/system/1-0-1 | Guest VM CPU/memory metrics unavailable (early release) |

## 2. Link reachability check

Method: `curl -L` with a browser user-agent, 2026-10-19, for the 15
primary links below. Result recorded exactly as returned by the
completed check: **all 15 returned HTTP 200** —
oxide.computer/careers/solutions-architect, product/specifications,
oxide.computer/solutions/sovereign-cloud; guides
network-preparations, rack-installation, facilities/power-connection,
initial-rack-setup, completing-rack-config, architecture/networking,
operator/configuring-bgp, operator/expanding-connectivity,
configuring-access; omicron control-plane-architecture;
rfd.shared.oxide.computer/rfd/0053; release-notes/system/14.
Note on RFD 53: reachability is now confirmed (200), but its *content*
was consulted only via the search-index excerpt, so this repository
still makes no detailed claim resting on RFD 53's details.

Links used in the documents but **not** individually curl-checked:
the release-notes pages for v5, v10, v11, and v1.0.1 and the
ms-entra / keycloak operator guides. They were consulted via search
excerpts (Section 1) on 2026-10-19; a full per-link curl pass over
these secondary links was not done and is not claimed.

## 3. Document self-audit

Performed on the finished files, 2026-10-19:

- [x] Every document (README, 00–05) carries the
      "SYNTHETIC / METHOD DEMONSTRATION — synthetic / 方法演示" banner
      near the top.
- [x] Every Oxide product statement in Sections 01–05 carries an
      in-line public source link, or is explicitly flagged as an open
      item (the power-budget discrepancy in Section 01 is the worked
      example, and is also used as Q1 in Section 05).
- [x] No fabricated metrics: text was searched for invented savings,
      yield, uptime, or benchmark figures — none are asserted. Numbers
      present are either (a) quoted product specifications with a
      link, (b) vendor-guide rules of thumb attributed to the guide,
      or (c) clearly synthetic planning artefacts (team names, the
      example zone, example ranges).
- [x] Candidate background is stated qualitatively only (rack-level
      integration, ~1,000-rack facility, PMP since 2010); no
      percentages or employer/customer data appear.
- [x] Mermaid diagram in Section 02 has an ASCII equivalent with the
      same topology.
- [x] Acceptance test from the source specification: facilities,
      network integration with topology, phased plan with rollback,
      operations tooling and onboarding, and a one-page defense Q&A
      covering cost, risk, and migration-path questions — all present
      as Sections 01–05.

## 4. Not done / cannot be done from here

- No hands-on Oxide rack was available; nothing in this repository is
  presented as operational experience with the product.
- Full-text fetch of every secondary source (see Section 2) was not
  completed; excerpts were used and are labelled as such.
- The power-budget discrepancy between sections of the public
  specifications page is reported, not resolved — resolution requires
  the order summary for a real configuration, which does not exist for
  a synthetic customer.
