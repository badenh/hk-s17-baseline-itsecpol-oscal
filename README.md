# Hong Kong Baseline IT Security Policy [S17] in OSCAL Format

## Introduction

The **Baseline IT Security Policy [S17]** is the Hong Kong SAR Government's
mandatory minimum security standard for government information systems and data
assets. It is issued by the Digital Policy Office (DPO, formerly OGCIO) and sits
alongside the Security Regulations (SR) and the IT Security Guidelines (G3) in
the Government's security document set. It adopts and adapts ISO/IEC 27001:2022
and ISO/IEC 27002:2022, and it binds every bureau and department (B/D), their
vendors, contractors and consultants.

The authoritative source is a single PDF:

https://www.govcert.gov.hk/doc/S17_EN.pdf

The Open Security Controls Assessment Language (OSCAL) was developed by the
National Institute of Standards and Technology (NIST) to enable automation of
risk management and compliance frameworks based on security controls and
functional requirements, such as SOC 2, FedRAMP, ISO-27001, StateRAMP, CMMC,
HIPAA, and PCI. OSCAL is an open machine-readable information exchange format
that enables tools to interoperate. More info on OSCAL is available at:

https://pages.nist.gov/OSCAL/

The DPO publishes S17 as a PDF and in no other form. There is no HTML edition,
no XML, no JSON, no API — and, unlike Singapore's GovTech, no OSCAL. A GRC team
that wants S17 in its tooling has to retype it, and retyping 142 policy
statements is how a compliance baseline acquires quiet errors.

We wrote some scripts which extract the policy from the PDF and render it as
OSCAL. In this repository we provide:

- the S17 control catalog — all 142 policy statements across the fourteen
  mandatory security areas, with the Government's own governance roles, glossary and
  normative references
- one OSCAL profile — the full S17 baseline
- the resolved profile catalog for it
- all of it in JSON, XML and YAML

While *unofficial*, we have published this openly to support HKSAR government
bureaux and departments, their delivery partners, and the security and
compliance engineers and GRC analysts working to these standards, in using
compliance automation tooling.

If you have questions - raise an issue or send us an email.

## About this repository

This repository is a passive publishing destination — no build or sync logic
lives here. The OSCAL artifacts are produced by the scripts in the sibling
`hk-s17-baseline-itsecpol-oscal-builder` project and pushed here by that project.
If you are interested in those scripts, please contact the maintainer.

## Contents

_Last updated: 2026-11-16_

    s17-catalog.{json,xml,yaml}                    the S17 control catalog  =  3 files
    s17-baseline-profile.{json,xml,yaml}           the full baseline profile =  3 files
    s17-baseline-resolved-catalog.{json,xml,yaml}  resolved profile catalog  =  3 files

Layout is flat: all nine artifacts sit at the repository root and are
disambiguated by filename prefix.

The version lives in each document's `metadata.version` and is **S17's own
edition number** — `8.2`, April 2025 — not a build number of ours. All artifacts
target **OSCAL 1.2.3**, the current NIST release.

The profile's import href is `s17-catalog.json` (and `.xml` / `.yaml` for the
matching serialisations), a plain relative URI resolving alongside the profile,
so the tree works as-is in `oscal-cli`, in a browser, or in anything else doing
ordinary relative-URI resolution. Nothing here needs a workspace tool.

### The catalog

`s17-catalog.{json,xml,yaml}` — 142 controls across 14 security areas and 36
subsections.

| § | Security area | Controls | Subsections |
| ---: | --- | ---: | ---: |
| 7 | Management Responsibilities | 7 | 2 |
| 8 | IT Security Policies | 3 | 1 |
| 9 | Human Resource Security | 9 | 1 |
| 10 | Asset Management | 9 | 3 |
| 11 | Access Control | 23 | 6 |
| 12 | Cryptography | 1 | 1 |
| 13 | Physical and Environmental Security | 12 | 2 |
| 14 | Operations Security | 30 | 7 |
| 15 | Communications Security | 17 | 2 |
| 16 | System Acquisition, Development and Maintenance | 9 | 3 |
| 17 | Outsourcing Security | 7 | 3 |
| 18 | Security Incident Management | 6 | 1 |
| 19 | IT Security Aspects of Business Continuity Management | 2 | 2 |
| 20 | Compliance | 7 | 2 |

Control identifiers are the document's own clause numbers with an `s17-` prefix:
clause 11.2.3 is control `s17-11.2.3`, in group `s17-11.2`, in group `s17-11`.
The printed clause number is also carried as `props[name=label]`, so a finding
can cite "S17 §11.2.3" without string-surgery on the id.

Each control carries:

- its policy statement verbatim as `parts[name=statement]`;
- `props[name=conformance]` — which of section 4.2's three convention words the
  statement uses: `mandatory` (136 controls), `best-practice` (5) or
  `unqualified` (1). See finding 4 below; this is not the uniform picture the
  document's framing suggests;
- `props[name=advisory-clauses]` on the 18 mandatory statements that *also*
  carry "should" or "may" clauses, because in S17 those modals usually qualify
  part of a mandatory statement rather than standing alone;
- `props[name=title-source]` — see "Control titles are ours" below;
- `links[rel=reference]` into back-matter where the statement cites a named
  instrument (the Personal Data (Privacy) Ordinance, the Civil Service
  Regulations).

The catalog also carries three things from outside the control sections, because
they are what makes the controls usable:

- **`metadata.roles` — the fifteen bodies and post-holders S17 §5.1–5.3 defines**:
  ISMC, ITSWG, GIRO, GovCERT.HK and B/Ds at the government level; DITSO, the
  Information Security Steering Committee, DSO, the ISIRT Commander and the IT
  Security Management Unit at the departmental level; and IT Security
  Administrators, Information Owners, LAN/System Administrators, the Application
  Development & Maintenance Team and Users as operational roles. Each carries its
  responsibilities as its `description`. These are the roles an OSCAL system
  security plan should be assigning implementation responsibility to.
- **`back-matter` — 9 resources**: the six normative references of §3 (the
  Security Regulations, the Civil Service Regulations, ISO/IEC 27001:2022,
  ISO/IEC 27002:2022, GB/T 22239-2019, General Circular No. 6/2024), the IT
  Security Guidelines (G3), the Personal Data (Privacy) Ordinance (Cap. 486),
  and S17 itself with a sha256 of the exact PDF the artifacts were built from.
- **the `s17-definitions` group — §4.1's sixteen defined terms**, including the
  Tier 1 / Tier 2 / Tier 3 information system definitions that §7.2.2's
  classified-protection requirement turns on. It is a `front-matter` group with
  parts and no controls: a definition is not a requirement and should not be
  assessable.

### Profile and resolved profile catalog

| Profile | Controls | Resolved profile catalog |
| --- | ---: | --- |
| `s17-baseline-profile.*` | 142 | `s17-baseline-resolved-catalog.*` |

`*` is `json`, `xml` or `yaml`.

There is a **single** profile here and it includes every control in the catalog.
S17 has no assurance levels, no maturity tiers, no risk-graded control sets and
no sub-baselines; the whole policy is the baseline. Scoping to a single security
area is a downstream tailoring decision, not a shape the source document takes,
so no per-area profiles are published — if you need one, write a tailoring
profile that imports `s17-baseline-profile` (or `s17-catalog` directly) with
`include-controls` listing the clause ids you want.

There is deliberately no conformance-graded profile either. 136 of the 142
statements are "shall" statements, but drawing a line there would require
deciding which side 11.4.4 falls on — see finding 4 — and that decision is the
DPO's, not ours. The catalog marks each statement so you can draw the line
yourself if you are entitled to.

The resolved catalog is the baseline profile's selection materialised — in
practice, the catalog itself with `as-is` merging. The `s17-definitions` group
is preserved so an assessor reading the resolved artifact still has S17's own
vocabulary to hand.

### Working with it

Every control in the catalog, as clause number and title:

```bash
jq -r '.catalog.groups[] | .groups[]? | .controls[]?
       | "\((.props[]|select(.name=="label")).value) \(.title)"' \
   s17-catalog.json
```

The full text of the access control area (`s17-11`), ready to paste into a
review:

```bash
jq -r '.catalog.groups[] | select(.id=="s17-11") | .. | objects
       | select(.parts? and (.parts[]|select(.name=="statement")))
       | "### \((.props[]|select(.name=="label")).value) \(.title)\n\((.parts[]|select(.name=="statement")).prose)\n"' \
   s17-baseline-resolved-catalog.json
```

The 18 statements that carry a best-practice clause as well as a mandatory one —
where "shall" and "should" are mixed and an auditor needs to read carefully:

```bash
jq -r '.. | objects | select(.props? and (.props[]|select(.name=="advisory-clauses")))
       | "\(.id)  \(.title)"' \
   s17-catalog.json
```

The Government's own role definitions, for populating a system security plan:

```bash
jq -r '.catalog.metadata.roles[] | "\(.id)\t\(.title)"' s17-catalog.json
```

## Validation

These artifacts are validated before publication. Every file here has been
checked against:

- the NIST OSCAL 1.2.3 **JSON Schema** (catalog and profile)
- the NIST OSCAL 1.2.3 **XSD**
- YAML/JSON structural equality
- XML/JSON control-set equality
- profile import resolution — every selected control id exists in the catalog,
  every import href resolves
- resolved-catalog versus profile-selection equality
- catalog structural invariants — unique ids, no truncated statements, no
  dangling back-matter links, no page furniture in control content, every
  control titled and declaring where its title came from
- **control-by-control fidelity back to the source PDF** — all 142 statements
  must appear verbatim in the text extracted from the published document, and
  the set of clause numbers S17 prints must equal the set of clause labels in
  the catalog, in both directions

Status at last publish: **24 checks, 0 failures**, 3 JSON and 3 XML documents
valid, 142/142 statements verified verbatim against the source.

Two independent runs of the build produce byte-identical output.

That said, validate independently before feeding these into tooling that will
reject invalid input silently — `oscal-cli`, `compliance-trestle`, or NIST's
online OSCAL validator are all reasonable second opinions.

## Control titles are ours, and are marked

**This is the one place the conversion adds words, and you should know about it
before you quote anything.**

S17 numbers its policy statements. It does not name them. OSCAL requires a
`title` on every control. So all 142 titles here — "Least privilege in assigning
resources and privileges", "Clock synchronisation to a trusted time source" —
were written by this project, not by the Digital Policy Office.

Every control declares this in `props[name=title-source]`. The value is
`editorial` for a title from the project's reviewed title map, and `derived` for
one produced by the mechanical fallback. In the current artifacts all 142 are
`editorial`.

The statement text under `parts[name=statement]` is the Government's, verbatim,
and is verified as such on every build. If you are citing S17 in a finding, an
audit report or a contract, cite the statement and the clause number, not our
title.

## What we found while building this

Converting a published framework tends to surface things. Four are worth
flagging.

**1. S17 has no assurance or tier axis, and that is a deliberate design, not a
gap.** Section 7.2.2 *does* require "classified protection of IT security" with
tiered controls by system classification, and section 4.1 defines Tier 1, Tier 2
and Tier 3 information systems — but S17 never assigns statements to tiers. That
assignment lives elsewhere in the Government's documentation, or in each B/D's
own risk work. Anyone expecting an NZISM- or FedRAMP-style low/moderate/high
split will not find one, and any OSCAL rendering that offers one has invented
it.

**2. The whole policy is 142 statements — which is smaller than it looks.**
Operations Security alone is 30 of them; Cryptography is one (key life cycle
management). S17 is a *baseline*: it deliberately states objectives and defers
technique to the IT Security Guidelines (G3) and the practice guides. Reading the
catalog without G3 will leave gaps that are real gaps in your programme, not
gaps in this conversion.

**3. Quantities are deliberately unstated, so the catalog declares no
parameters.** "A pre-defined period of time" (13.2.4), "regular intervals"
(14.3.1), "at least minimum password length" (11.4.3) — S17 leaves these to each
B/D on purpose. We could have declared OSCAL parameters for them, but that would
have meant editing the Government's control text to insert placeholders it does
not contain. A downstream tailoring profile is the right place to bind those
values, and can add `set-parameters` against these controls without this catalog
pretending to an authority it does not have.

**4. Six statements are not "shall" statements, and one of those is the
interesting one.** Section 4.2 makes "shall" mandatory, "should" best practice
and "may" desirable, and the document reads as though sections 7–20 are wall-to-
wall "shall". They are not quite. Five statements are best practice only —
14.2.6 (blocking non-business websites), 14.2.8 (executing mobile code from the
Internet), 14.3.4 (protecting backup media), 14.6.4 (risk-evaluating patches
before applying them) and 15.2.7 (opening messages from suspicious sources). It
is worth noticing what is on that list: four of the five are things most
security teams would assume were mandatory, and 14.3.4 in particular sits beside
four "shall" statements about backup.

The sixth is 11.4.4: *"Staff are prohibited from capturing or otherwise
obtaining passwords, decryption keys, or any other access control mechanism
which could permit unauthorised access."* That is plainly binding and it uses
none of section 4.2's three convention words. A conversion that classified by
keyword alone would file the Government's flat prohibition on password capture
as non-mandatory. This one records it as `unqualified` and leaves the reading to
the DPO, which is why there is no conformance-graded profile here.

Separately, **18 statements mix a mandatory requirement with a best-practice
clause** — 11.3.2 forbids password sharing and then recommends rotation
frequency for shared passwords; 9.1.4 mandates integrity checks for civil
servants and recommends background verification for everyone else. A reader
skimming for "shall" over-reads those; a reader skimming for "should" misses
them. `props[name=advisory-clauses]` marks every one.

## Known caveats

- **The titles are ours.** See above. Marked on every control; cite the
  statement, not the title.
- **The catalog has no parameters.** Where S17 says "a pre-defined period", the
  OSCAL says "a pre-defined period".
- **Section 6's Core Security Principles are not controls.** They are normative
  ("B/Ds shall observe these principles") but the document numbers them only as
  bullets. Minting identifiers for them would put non-source identifiers into a
  compliance artefact, so they are not in the catalog. Read section 6 of the PDF.
- **`conformance` is a marking, not a judgment.** It reports which section 4.2
  convention word a statement uses. `unqualified` does not mean optional — see
  11.4.4 in finding 4.
- **The profile carries no party attribution.** The DPO published S17; it did
  not author this profile. Attributing a compliance artefact to a government
  office that has not seen it would be a false statement, so `roles`, `parties`
  and `responsible-parties` are omitted. The catalog names the DPO as
  `publisher`, which is true.
- **XML and YAML profiles import their own format.** The JSON profile imports
  the JSON catalog, the XML profile the XML catalog, the YAML profile the YAML
  catalog. This is the one place the three serialisations of a document
  deliberately differ.
- **The resolved catalog is produced natively**, not by an OSCAL profile
  resolver. The profile uses only `import` + `include-all` with `as-is` merging
  and no `modify`, and the resolved catalog is asserted equal to the profile's
  selection. If you need canonical resolution, run the profile through
  `oscal-cli`.
- **This is the English edition only.** GovCERT.HK also publishes S17 in Chinese.
- **Dates.** `metadata.version` is S17's own edition (`8.2`) and `published` /
  `last-modified` are the cover page's date (April 2025) at `00:00:00+08:00`. The
  PDF's internal creation timestamp is 28 April 2025; the cover date is what the
  document asserts about itself, so that is what is used.
- **S17 is one of three.** The Security Regulations (SR) and the IT Security
  Guidelines (G3) are referenced from the back-matter but are not converted. G3
  in particular carries the implementation detail S17 defers.

## Copyright

The Baseline IT Security Policy [S17] is © The Government of the Hong Kong
Special Administrative Region of the People's Republic of China, published by the
Digital Policy Office. Framework content is copyright the HKSAR Government and
the copyright notice in the source document governs any redistribution of these
derivative artifacts.

The OSCAL derivative artifacts in this repository are created by independent
security and compliance engineer Baden Hughes. They are a mechanical
transformation of published material and add no framework content — with the
single, marked exception of the 142 control titles, which OSCAL requires and S17
does not supply.

This is an **unofficial** conversion. It is not endorsed by, affiliated with, or
supported by the Digital Policy Office or the Government of the Hong Kong
Special Administrative Region.
