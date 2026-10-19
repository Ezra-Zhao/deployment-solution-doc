# 04 — Operations Tooling and Onboarding

> **SYNTHETIC / METHOD DEMONSTRATION — synthetic / 方法演示**
> Team names, schedules, and the training plan are invented for the
> synthetic customer. Product/tooling statements are cited to public
> Oxide sources. Known limitations are stated from the vendor's own
> public release notes, not softened.

## 1. Operating model after handover

| Function | Owner (synthetic) | Tooling surface |
|---|---|---|
| Fleet / rack administration | Platform operations team | Oxide console + CLI; API for automation |
| Tenant administration | Silo admins (per team) | Console/CLI within their silo; quotas set at fleet level |
| Developer self-service | Application teams | API, CLI, Terraform provider, console |
| Low-level troubleshooting | Oxide support, via operations | Support shell over technician ports (SSH 22) |
| Change & update windows | Platform operations + Oxide | Scheduled per current release-notes posture (Section 4) |

Product grounding: Oxide's public materials describe complete API
coverage for all operations, with official integrations including a
Terraform provider and REST API tooling, plus SAML and LDAP listed
among standard tools on the sovereign-cloud page; the control plane
itself is API-driven (Nexus receives requests via the external API and
persists state in CockroachDB, per the public control-plane
architecture document).
Sources: https://oxide.computer/solutions/sovereign-cloud and
https://oxidecomputer.github.io/omicron/docs/control-plane-architecture/

## 2. Identity and access (the onboarding foundation)

- **Silos are the tenant boundary.** Each onboarded team gets a silo;
  silo DNS names live under the delegated zone as
  `$silo.sys.<zone>` (Network Preparations / Initial Rack Setup).
  Sources: https://docs.oxide.computer/guides/system/network-preparations
  and https://docs.oxide.computer/guides/system/initial-rack-setup
- **Corporate IdP via SAML.** Oxide's public guides document creating a
  SAML provider per silo from IdP metadata (URL or base64 XML), with
  group attributes mapping IdP groups to Oxide roles; worked examples
  exist for several IdPs (for example Entra ID, Keycloak, Duo).
  Sources: https://docs.oxide.computer/guides/system/completing-rack-config
  and https://docs.oxide.computer/guides/operator/ms-entra and
  https://docs.oxide.computer/guides/operator/keycloak
- **Bootstrap pattern (from the Access Control guide):** set
  `admin_group_name` so a designated operator group automatically
  receives the silo admin role, and use `mapped_fleet_roles` to confer
  fleet roles from silo roles (the guide's own example maps silo
  `admin` to fleet `viewer`, and describes combining both to bootstrap
  fleet access on a SAML silo).
  Source: https://docs.oxide.computer/guides/configuring-access
- **Roles to groups, not to people** — the same guide notes roles can
  be assigned to users and groups, and that group assignment is
  particularly useful because members inherit permissions; this plan
  makes group-based assignment a hard onboarding rule.
- **Local-only fallback:** silos can be configured for local
  authentication; the first local user and admin grant are CLI
  operations, and the console does not currently support local user
  creation/deletion (Completing Rack Configuration guide). The plan
  keeps exactly one break-glass local path, documented and sealed,
  instead of making local accounts the norm.
- **Known sharp edges, briefed to the security team in advance:**
  (a) changing an IdP definition can require deleting/recreating the
  silo (Entra guide); (b) user offboarding is called out in release
  notes as not fully supported — removing a user from IdP groups does
  not by itself remove directly granted IAM roles, which operations
  must also remove (release notes). Both are process items in the
  runbook set, not surprises.
  Sources: https://docs.oxide.computer/guides/operator/ms-entra and
  https://docs.oxide.computer/release-notes/system/10

## 3. Developer / tenant onboarding path (per team, ~one working session)

A checklist, run identically for each team — the author's SOP habit
applied to onboarding:

1. Silo created; quotas set (fleet admins can set per-silo limits on
   vCPU, memory, and storage; enforced at disk provisioning and
   instance start, per the v5 release notes' silo-quota description —
   re-verify against current notes at delivery).
   Source: https://docs.oxide.computer/release-notes/system/5
2. IdP group mapped; two named silo admins confirm they can sign in
   via the corporate IdP and hold the expected roles.
3. IP pool linked to the silo and set as default (console or CLI, per
   Completing Rack Configuration); pool sized with the guide's NAT
   rule of thumb (Section 02).
4. First instance created three ways — console, CLI, and Terraform —
   by the team's own engineer, not by the SA. Onboarding is not done
   when it is demoed; it is done when the tenant does it unaided.
5. VPC/firewall defaults reviewed with the team's security contact.
6. Team added to the operations channel; escalation path (below)
   walked through with a worked example.

## 4. Day-2 operations: monitoring, support, updates — with limits stated

- **What operations watches:** control-plane/service reachability
  (API, DNS), uplink counters and routing state (the configuration
  guide's own connectivity checks), BGP session state if/when BGP is
  enabled (the BGP guide's port-settings/peer displays), and instance
  lifecycle events via the API.
  Sources: https://docs.oxide.computer/guides/system/completing-rack-config
  and https://docs.oxide.computer/guides/operator/configuring-bgp
- **Stated limits (from the vendor's release notes, re-checked at each
  gate):** earlier release notes list that hardware metrics such as
  temperatures, fan speeds, and power consumption are not exposed to
  the control plane, and that guest VM CPU/memory metrics were
  unavailable in the earliest releases. The monitoring design therefore
  does not promise dashboards the product does not expose; gaps are
  covered by facilities-side monitoring (Section 01 sensors) and by
  the support path below, and the current release notes are the
  authority at delivery time.
  Sources: https://docs.oxide.computer/release-notes/system/10 and
  https://docs.oxide.computer/release-notes/system/1-0-1
- **Support path:** first line is the customer's platform operations
  team using the runbook set; low-level issues escalate to Oxide
  support, whose documented access path is the support shell over the
  technician ports — which is why those segments stay provisioned and
  isolated after install day instead of being torn down (Section 02).
  Source: https://docs.oxide.computer/guides/system/network-preparations
- **Updates:** several public release notes state operator-driven
  software update is currently unavailable and updates are performed
  by Oxide technicians; newer notes (v11/v14) describe supported
  upgrades with the recommendation to shut down instances first and
  notes on instance auto-restart behaviour. The plan's posture:
  schedule updates as coordinated windows with Oxide, shut down or
  explicitly accept the documented instance-state behaviour, and cite
  the *current* release's notes in the change record — never a
  generic "patch Tuesday" assumption.
  Sources: https://docs.oxide.computer/release-notes/system/14 and
  https://docs.oxide.computer/release-notes/system/11 and
  https://docs.oxide.computer/release-notes/system/10
- **Capacity behaviour to brief tenants on:** anti-affinity groups are
  available to spread instances across sleds (v14 notes), and a sled
  can be excluded from new workload placement by operators when
  diagnosing sled issues (v5 notes) — both appear in the runbook set
  as the documented levers during a sled incident.
  Sources: https://docs.oxide.computer/release-notes/system/14 and
  https://docs.oxide.computer/release-notes/system/5

## 5. Runbook set delivered with this plan (titles + purpose)

1. RB-01 Uplink down / degraded (LED and counter checks per Section 03).
2. RB-02 Technician-port access and wicket entry (isolation rules).
3. RB-03 Silo onboarding and IdP group mapping (Section 3 checklist).
4. RB-04 User offboarding (IdP groups **plus** directly granted roles).
5. RB-05 IP pool exhaustion (add ranges; NAT sizing rule).
6. RB-06 Sled incident (placement exclusion; anti-affinity review).
7. RB-07 Software update window (with Oxide; instance shutdown plan).
8. RB-08 DNS delegation or service-pool incident.

Each runbook in a real engagement would be written to the standard the
author used for data-center SOPs: preconditions, exact commands or
console paths, expected output, failure branch, escalation trigger.
Only the titles/purposes are included in this sample; inventing full
command transcripts for a system the author has not operated would
violate this repository's honesty rules.

## 6. Training arrangement

- Platform operations: one working session on fleet administration,
  one on the runbook set, each ending with the trainee performing the
  task unaided (same standard as Section 3, step 4).
- Silo admins: the Section 3 onboarding session, plus the offboarding
  and quota items as named responsibilities.
- Executives/stakeholders: the Section 05 defense page is the briefing
  artefact; no separate slideware is promised in this plan.
