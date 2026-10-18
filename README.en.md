# ene-grid-permit-checker

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain Profile](https://img.shields.io/badge/domain-ene%20%7C%20domain%2F1.0-blue.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED%20%7C%20open%20claim-orange.svg)](domain.manifest.json)
[![Gate: five-level](https://img.shields.io/badge/gate-silent%2Fwarn%2Fconfirm%2Fblock%2Frequire--owner-8a2be2.svg)](gate-map.json)
[![Privacy: default-off](https://img.shields.io/badge/privacy-default--off-brightgreen.svg)](privacy.json)

> **Status: RESERVED (placeholder · open for claim)** — this repository ships the four-piece industry-access skeleton already built to the platform protocol, and waits for an operator with the proper domain qualification to claim it and fill in the real rules.

It turns "may this power site connect to the grid, and under which carbon-accounting method" into verifiable attributes: the AI only surfaces what route to take and which evidence is missing — it does **not** issue a connection conclusion.

## What this domain covers

Power-business licence, grid connection and carbon-accounting scope verification. Power-business licensing, grid dispatch relationships and carbon-accounting methods each have statutory definitions and large regional variation. The AI lays out the route and the missing pieces; the grid operator and the regulator hold the connection and pricing decisions.

## Quick start

This repo is a **domain-profile skeleton** (four JSON files), not a runnable application. Validate the structure with standard JSON tooling:

```bash
# 1. clone
git clone https://github.com/SynomosAI/ene-grid-permit-checker.git
cd ene-grid-permit-checker

# 2. validate the four pieces are well-formed JSON
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json        > /dev/null
python -m json.tool privacy.json         > /dev/null
python -m json.tool checker.io.json      > /dev/null
```

The checker (an MCP tool) input/output contract is in `checker.io.json`. A minimal call:

```json
{
  "subject_ref": "mpp:cn:9f2c1a7b",
  "attribute_keys": ["ene.power_business_license", "ene.grid_connection_status"],
  "evidence": ["tsa:2026-09-12#sha256:3b1f..."],
  "request_id": "req-20260912-0001"
}
```

> The checker has exactly one action — *show and verify*. It has **no "judge" option**: pass/fail, "already compliant / regulator-approved", and "approve / reject" conclusions are all on the `_forbidden_outputs` blacklist in `checker.io.json`.

## Domain identity

| Item | Value |
|---|---|
| Domain ID | `ene` (globally unique; never reused) |
| Domain name | Energy · Power-business licence & grid connection |
| Profile version | `domain/1.0` |
| Status | `RESERVED` |
| Placeholder since | 2026-09-12 |

## Attributes

| Key | Type | Meaning |
|---|---|---|
| `ene.power_business_license` | enum | Current power-business licence (supply/generation) state · licensed/exempt/unlicensed/pending |
| `ene.grid_connection_status` | enum | Grid-connection opinion and current networking state · connected/approved_not_connected/pending/rejected/not_applicable |
| `ene.dispatch_relationship` | enum | Whether under dispatch management, per the dispatch agreement · dispatched/not_dispatched/unknown |
| `ene.carbon_accounting_method` | string | Carbon-accounting method and reporting basis (standard no. + boundary) |
| `ene.protection_zone` | enum | Whether inside a power-facility protection zone · within/outside/unknown/not_applicable |

## Red lines (machine-readable)

1. Connection conditions and accounting basis must follow grid-operator and regulator documents; the AI must not self-determine them.
2. No "may connect / may transmit / may supply" go-ahead wording — that follows the grid operator's written opinion.
3. No carbon-quota or emission-reduction accounting conclusions; accounting must be done by a qualified body.

Each red line has a corresponding blocking rule in `gate-map.json`. The platform validator checks that every red line is covered — an uncovered red line fails validation by design.

## The four-piece access set

| File | Role |
|---|---|
| `domain.manifest.json` | Domain declaration: attribute list, issuer requirements, validity, red lines |
| `gate-map.json` | How much friction per action: silent / warn / confirm / block / require-owner |
| `privacy.json` | Privacy declaration: default-off, minimal, withdrawable, deletable |
| `checker.io.json` | Checker (MCP tool) IO contract — show & verify, never judge |

## Core principles

**The platform is a ring, not a shelf.** The checker only answers "is this claim verifiable, and which evidence is missing" — it does not answer "is this compliant / should it be done". Judgment stays with the qualified domain body, the regulator and the holder.

**Privacy is an access condition, not a remediation item.** Missing `privacy.json` or any non-conforming required field fails the conformance check outright — it is a refusal of access, not a warning.

## References

- Electricity Law of the People's Republic of China
- Rules on the Administration of Power Business Licences
- Regulations on the Protection of Electric Power Facilities
- GB/T 32150 General guidelines for accounting and reporting of greenhouse gas emissions from industrial enterprises

> The above references only explain the source and basis of the domain attributes; they do not constitute legal advice. Applicability follows the currently effective text and the regulator's interpretation.

## How to claim

This domain is open to institutions with the corresponding qualification. To claim it:

1. Fork this repo and fill in `operator` and `checker_endpoint`.
2. Calibrate attribute values and red-line wording per currently effective rules.
3. Run the platform-side validator self-check (all five criteria must pass before submission).
4. Open a PR with qualification evidence and the rule basis.

## License / 署名

Code and configurations are used under the MIT licence. Document copyright belongs to SynomosAI.

© 2026 SynomosAI. All rights reserved.
