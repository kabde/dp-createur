# Quality Gates — Playbook Create

## Hard Rules (Never Violate)

| Gate | Rule | Rationale |
|------|------|-----------|
| QG-01 | Never include placeholder text (`[TODO]`, `[INSERT]`, `[EXAMPLE]`, `Lorem ipsum`) | Destroys credibility. Buyers expect finished products. |
| QG-02 | Never promise specific income or revenue results | Legal risk. Use "results vary" + honest caveats. |
| QG-03 | Every h2 section MUST have a value-block AND recap-block | Structural consistency. Reader knows what they'll learn and retain. |
| QG-04 | No section under 300 words (playbook) / 200 words (guide) / 100 words (lead-magnet) | Thin sections signal low effort. Combine or expand. |
| QG-05 | No section over 2000 words without sub-sections (h3) | Wall of text = reader drops off. Break into digestible chunks. |
| QG-06 | Maximum 7 h3 sub-sections per h2 section | Cognitive overload. Split into 2 sections if >7. |
| QG-07 | Every actionable claim must include HOW (steps, script, template) | "Be authentic" without steps is useless advice. |
| QG-08 | No external CSS or JS dependencies (except Google Fonts) | Standalone file. Must render offline + in PDF conversion. |
| QG-09 | All sommaire links must point to existing IDs | Broken TOC = unprofessional. Verify every anchor. |
| QG-10 | Never use passive voice in action steps | "Send 20 DMs" not "DMs should be sent". Direct address only. |
| QG-11 | No paragraph longer than 5 lines / 80 words | Readability. Short paragraphs for digital reading. |
| QG-12 | CTA in lead-magnets must reference specific paid product | Lead magnet without CTA = wasted opportunity. |
| QG-13 | File must pass HTML validation (well-formed tags, closed elements) | Broken HTML = broken PDF export. |
| QG-14 | Price claims must match business-profile.md if it exists | Consistency across all outputs. |

## Warning Rules (Flag but Don't Block)

| Gate | Rule | Action |
|------|------|--------|
| WG-01 | Section has no tools-block | Flag: "Consider adding tool recommendations if applicable" |
| WG-02 | Recap-block has fewer than 3 takeaways | Flag: "Aim for exactly 3 takeaways per section" |
| WG-03 | Content reads academic/formal | Flag: "Rewrite in DP Créateur voice (direct, mentor-like)" |
| WG-04 | More than 2 consecutive sections without a list or template | Flag: "Add visual variety — checklist, script, or template" |
| WG-05 | No print styles in CSS | Flag: "Add @media print for PDF export readiness" |
