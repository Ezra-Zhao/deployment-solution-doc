# 01 — Facilities Readiness Checklist

> **SYNTHETIC / METHOD DEMONSTRATION — synthetic / 方法演示**
> The site ("Hall B") is invented. Every product requirement below is
> quoted or paraphrased from Oxide's public specifications page or public
> facilities/installation guides, with the source linked per section.
> In a real engagement this checklist is signed line-by-line by the
> customer's facilities lead and the Oxide delivery team.

Gate rule: **no ship date is committed until every item in Sections 1–5
is marked Pass with evidence (photo, meter reading, or drawing
reference).** This mirrors Oxide's own installation flow, which begins
by confirming "that the following facilities work is complete" — rack
positioned and power connected and validated — before network work
starts. Source: https://docs.oxide.computer/guides/system/rack-installation

## 1. Physical: dimensions, weight, path, clearances

Public product facts (specifications page; actual specs vary by
configuration): rack dimensions 2354 mm (92.7 in) high × 600 mm
(23.7 in) wide × 1060 mm (41.8 in) deep without doors, door thickness
adds 51 mm; a fully configured rack weighs ~2,518 lbs (~1,145 kg);
front clearance at least 1000 mm (39.37 in) to remove sleds and
switches; rear doors need ~290 mm to open; the shipping crate needs a
loading dock that accommodates at least 102 in high × 48 in wide ×
60 in deep; the rack ships fully assembled on casters, with levelling
feet, and seismic kits are available.
Source: https://oxide.computer/product/specifications

Checklist:

- [ ] Rack position surveyed; point loads from the order summary checked
      against floor rating at the position **and along the whole
      transport path, including freight/standard elevators** (the specs
      page explicitly includes the path, not just the final position).
- [ ] Doorways, hallways, turns, and ramps on the path measured against
      crate dimensions and against rack dimensions.
- [ ] Cold-aisle / hot-aisle orientation confirmed: rack front faces the
      cold aisle, back faces the hot aisle (specs page, Temperature).
- [ ] Front clearance ≥ 1000 mm and rear door swing verified in the row
      layout drawing (synthetic drawing reference: `HALLB-ROW3-v2`,
      invented for this sample).
- [ ] Seismic kit decision recorded (required / not required) for the
      site jurisdiction — decision recorded, not assumed.
- [ ] Lockable door option decision recorded (racks can be supplied with
      a lockable door on request — specifications page).

Synthetic rack-position layout (illustrative only):

```text
        COLD AISLE (front)
  +-------------------+-------------------+
  |  existing row 3   |  OXIDE RACK (new)   |  <- front faces cold aisle
  |  (legacy, stays)  |  600 mm wide        |
  +-------------------+-------------------+
        HOT AISLE (rear, >=290 mm door swing)
   power whips: from above (see Section 2)   uplinks: top-of-rack patch panel
```

## 2. Power

Public product facts: three-phase power; power-shelf input 3-phase
Delta 208–240 V or 3-phase WYE 380–480 V; connectors CS8365C (Delta) or
L22-20P (WYE); per-supply output 2400 W typical / 3600 W max; two power
shelves, 1+1 redundant or 2+0 non-redundant, six supplies per shelf
(5+1 or 3+3); the current specifications page lists max power draw up
to 21.6 kW redundant or 30 kW non-redundant, while its Facilities
section states the system "will support up to 15 kW" and that actual
draw depends on chosen rack density. **This plan treats that spread as
an open item (see below), not as a number to average away.**
Sources: https://oxide.computer/product/specifications and
https://docs.oxide.computer/guides/facilities/power-connection

The public Power Connection guide's pre-install checklist (2–4 weeks
before installation) requires, in its own terms: redundancy
configuration decided (N+1 / N+N; A/B or non-redundant feeds), total
power drops calculated (2 minimum, 4 recommended), connector type
verified (L22-20P, CS8365C, or IEC 60309), cable entry direction
specified (whips can enter from above or below), drops installed and
active, breakers labelled and accessible, A/B feed separation verified,
and electrical validation (voltage at each drop, phase balance,
ground/earth verified, breakers sized to connector and local code).
Source: https://docs.oxide.computer/guides/facilities/power-connection

Checklist (synthetic site decisions shown as examples):

- [ ] Configuration decision recorded: redundant (A/B) feeds — synthetic
      example decision; the real decision belongs to the customer and is
      driven by the workload criticality discussion in Section 05.
- [ ] Drop count and connector type verified against the order.
- [ ] Cable entry: overhead (synthetic example) — recorded on the order,
      since entry direction is an order-time specification.
- [ ] Meter readings at each drop attached (voltage, phase balance,
      ground) — evidence, not assertion.
- [ ] **OPEN ITEM (blocking):** reconcile the power budget. The specs
      page's headline max-draw figures and its Facilities-section
      figure differ; the binding number for this site is the one on the
      actual order summary for the quoted configuration. Facilities
      sizing (breakers, UPS, generator) is signed against the order
      summary, and this checklist is not Pass until that document is in
      hand. This is the correct SA behaviour: quote the order, not the
      web page, when the two could diverge.

## 3. Cooling and environment

Public product facts: ambient temperature must be between 35 °F (2 °C)
and 95 °F (35 °C); relative humidity 8–80%, non-condensing; site
elevation below 9,842 ft (3,000 m); airflow at the rack position must be
at least 145.8 × kVA in CFM; max thermal output on the specifications
page is listed as 122,832 BTU/hr (configuration-dependent).
Source: https://oxide.computer/product/specifications

Checklist:

- [ ] CRAH/CRAC capacity at the row confirmed against the order-summary
      power figure (same open item as Section 2 — cooling is sized from
      the same source document).
- [ ] Airflow calculation recorded: required CFM = 145.8 × kVA for the
      ordered configuration; containment (cold/hot aisle) integrity
      checked for gaps at the new position.
- [ ] Temperature/humidity sensor coverage at the rack front verified;
      readings logged for a representative week before install day
      (synthetic procedure, stated as procedure, not as data).
- [ ] Elevation: site is below the documented limit (check the box only
      with the site's actual elevation recorded).

## 4. Network-adjacent facilities items

(Full design in Section 02; listed here because they are physical.)

- [ ] Fiber ready at the rack position, of the type matching the chosen
      transceivers — Oxide states it provides optics compatible with the
      fiber ready at the rack position, and the Network Preparations
      guide warns that single-mode transceivers require single-mode
      fiber and multi-mode require multi-mode.
      Sources: https://oxide.computer/product/specifications and
      https://docs.oxide.computer/guides/system/network-preparations
- [ ] Minimum two physical links between the rack's two network devices
      and the local network (specifications page, Network Switches).
- [ ] RJ45 cabling for technician ports: 2–4 cables, at least one per
      switch (Network Preparations guide).
- [ ] A jumpbox / technician laptop with IPv6 SLAAC enabled is on site
      for install day (Rack Installation guide readiness list).

## 5. Install-day readiness gate

Copied in structure from Oxide's public Rack Installation guide — before
proceeding: facilities work complete, and all rack LEDs solid (power
shelf LEDs solid with no blinking or red/amber, all sled power LEDs
solid, both switch power LEDs solid); compatible transceivers present
and plugged in; uplink fiber and technician-port RJ45 cables on site.
If power validation failed or any LED is abnormal, the guide sends you
back to power troubleshooting before network connectivity work.
Source: https://docs.oxide.computer/guides/system/rack-installation

- [ ] Named facilities engineer present for power validation.
- [ ] Named network engineer on standby for uplink turn-up (Section 02).
- [ ] Abort criteria agreed in advance (Section 03): any failed power
      validation or abnormal LED state stops the day; no workarounds
      that skip validation.
