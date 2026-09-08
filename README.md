<div align="center">

<img src="https://raw.githubusercontent.com/pointsav/pointsav-media-assets/main/ASSET-SIGNET-MASTER.svg" width="72" alt="PointSav Design System">

# PointSav Design System
### Linguistic Protocols & Visual Standards
*The single source of truth for institutional voice and visual identity across every PointSav deployment.*

[![Status: Verified](https://img.shields.io/badge/Status-Verified-22863a.svg?style=flat-square)](#token-ledgers)
[![WCAG: 2.2 AAA](https://img.shields.io/badge/WCAG-2.2_AAA-0075ca.svg?style=flat-square)](#visual-standards)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-0075ca.svg?style=flat-square)](#license)

<br/>

**[→ Engineering Monorepo](https://github.com/pointsav/pointsav-monorepo)** &nbsp;·&nbsp; **[→ Documentation Wiki](https://github.com/pointsav/content-wiki-documentation)** &nbsp;·&nbsp; **[→ Live Deployment](https://github.com/woodfine/woodfine-fleet-deployment)** &nbsp;·&nbsp; **[→ pointsav.com](https://pointsav.com)**

</div>

---

## What This Repository Is

Most design systems are collections of colours and typefaces. This one is different.

PointSav operates a platform where the same underlying data must be projected accurately across public investor disclosures, internal operator terminals, regulatory filings, and bilingual marketing surfaces. A colour that reads correctly in dark mode must remain legally defensible in a printed regulatory submission. A linguistic protocol that routes structured data deterministically must apply consistently whether the operator is in Vancouver or Madrid.

The design system enforces these constraints as machine-readable tokens — YAML files that are consumed by every deployment surface automatically. A change to a typographic rule propagates everywhere. There is no manual alignment step. There is no version drift. The system is the law.

---

## Linguistic Protocols

Language in an institutional context is not a stylistic preference. It is a structural commitment. The wrong word in an investor disclosure is a regulatory event. The right word, applied inconsistently across jurisdictions, creates legal fracture.

PointSav treats language as infrastructure. The six linguistic protocol files in this repository encode the rules that govern every piece of text the platform produces:

| Token File | What it governs |
|:---|:---|
| `protocol-text.yaml` | Core writing rules — tone, register, sentence structure, prohibited vocabulary |
| `protocol-comm.yaml` | Internal dispatch — asset distinction, routing classification |
| `protocol-extract.yaml` | Deterministic parsing rules — zero AI inference, strict lexical grammar |
| `protocol-legal.yaml` | Translation of market claims to structural facts — regulatory language standards |
| `protocol-translate.yaml` | Bilingual boundary rules — proper noun locks, jurisdiction-specific term handling |

`protocol-translate.yaml` is worth noting specifically. PointSav operates across Canada, the United States, Spain, and Mexico. The same investment concept — a Direct-Hold Solution — has a different legal designation in each jurisdiction. The protocol file encodes which terms are translatable and which must remain in their source language regardless of the document's target language. This prevents legal fracture from appearing in bilingual documents.

---

## Visual Standards

The visual token system enforces WCAG 2.2 AAA compliance as a structural constraint, not a checklist. The reasoning is institutional rather than philosophical: an investor disclosure that fails accessibility standards in a jurisdiction where those standards carry legal weight is a liability, not an inconvenience.

The five visual token files:

| Token File | What it governs |
|:---|:---|
| `typography.yaml` | Fluid character clamping, Swiss Grid proportions, sub-pixel rendering rules |
| `spacing.yaml` | 8px grid system, touch target minimums, accessible density |
| `color.yaml` | Entity-specific palette, dark mode compliance, contrast ratio enforcement |
| `TOKEN-UI-PHYSICS.yaml` | WCAG 2.2 AAA structural definitions — contrast ratios, focus indicators, motion |
| `TOKEN-TYPOGRAPHY-RENDER.yaml` | Sub-pixel anti-aliasing, cross-platform rendering consistency |

---

## Legal Governance

| Token File | What it governs |
|:---|:---|
| `LEGAL-incubation-license.txt` | Proprietary retention protocol for incubation-phase components |

---

## How the System Is Used

The PointSav platform does not edit documents. It projects data. A Totebox Archive stores all records as machine-readable flat files. ConsoleOS variants read those files and project them into human-readable interfaces using the tokens in this repository as the rendering law.

This separation matters for institutional reasons. If the rendering layer were embedded in the data layer, a change to a colour token could, in principle, require modifying records. By keeping the design system separate from the archive, visual updates propagate without touching the underlying records. Compliance integrity is preserved.

---

## Usage & Licensing

The token files in this repository are consumed automatically by PointSav deployment environments and Woodfine Management Corp.'s fleet. They are also published under Apache License, Version 2.0 for external use, fork, modification, and redistribution.

Refer to the `LICENSE` file in the repository root for the full Apache 2.0 legal text and `NOTICE` for required attribution. The PointSav and Woodfine word marks, logos, and brand identity assets are NOT licensed under Apache 2.0 — see `TRADEMARK.md` for the trademark policy.

**[→ pointsav.com](https://pointsav.com)** &nbsp;·&nbsp; **[→ github.com/pointsav](https://github.com/pointsav)**

---

*© 2026 Woodfine Capital Projects Inc. — Licensed under Apache License 2.0. PointSav Digital Systems™ is a trademark of Woodfine Capital Projects Inc.*



---

*Copyright © 2026 Woodfine Capital Projects Inc. See [LICENSE](LICENSE) for terms.*

*Woodfine Capital Projects™, Woodfine Management Corp™, PointSav Digital Systems™, Totebox Orchestration™, and Totebox Archive™ are trademarks of Woodfine Capital Projects Inc., used in Canada, the United States, Latin America, and Europe. All other trademarks are the property of their respective owners.*

<!-- BEGIN: factory-release-engineering license-section -->
<!-- ================================================================== -->
<!-- This section is generated from factory-release-engineering.         -->
<!-- Do not edit here. Propose changes upstream.                          -->
<!-- ================================================================== -->

## License

This repository is licensed under the **Apache-2.0**. See the
`LICENSE` file in the root of this repository for the full legal text,
which is authoritative.



Copyright © 2026 Woodfine Capital Projects Inc. — all rights not expressly
granted by the license are reserved.

<!-- ================================================================== -->
<!-- Esta sección se genera desde factory-release-engineering.           -->
<!-- No editar aquí. Proponga cambios río arriba.                         -->
<!-- ================================================================== -->

## Licencia

Este repositorio se distribuye bajo la **Apache-2.0**. Véase el
archivo `LICENSE` en la raíz del repositorio para consultar el texto
legal completo, el cual es la versión autoritativa.



Copyright © 2026 Woodfine Capital Projects Inc. — se reservan todos los
derechos no concedidos expresamente por la licencia.
<!-- END: factory-release-engineering license-section -->
