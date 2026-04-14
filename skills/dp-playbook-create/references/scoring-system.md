# Scoring System — Playbook Quality Assessment

## Score Formula

```
S_total = Σ(check_score × W_severity × W_category) / Σ(max_score × W_severity × W_category) × 100
```

Result: **0-100 Quality Score**

## Score Interpretation

| Score | Rating | Action |
|-------|--------|--------|
| 90-100 | Excellent | Ready to publish |
| 75-89 | Good | Minor polish needed, publishable |
| 60-74 | Needs Work | Significant improvements required |
| 40-59 | Poor | Major rewrite needed |
| 0-39 | Fail | Restart or fundamental restructure |

## Categories & Weights

| Category | Weight | What it measures |
|----------|--------|-----------------|
| Structure | 20% | HTML validity, section pattern compliance, sommaire links |
| Content Quality | 30% | Actionability, specificity, QUOI/POURQUOI/COMMENT/MESURE |
| Voice & Tone | 15% | DP Créateur voice, no fluff, direct address, mentor tone |
| Completeness | 20% | All required sections present, no placeholders, word count |
| Readability | 15% | Paragraph length, list usage, visual variety, scanability |

## Severity Multipliers

| Severity | Multiplier | Criteria |
|----------|-----------|----------|
| Critical | 5.0 | Blocks publication. Placeholder text, broken HTML, missing sections. |
| High | 3.0 | Significantly impacts quality. Thin content, wrong voice, no CTAs. |
| Medium | 1.5 | Optimization opportunity. Could be better, publishable as-is. |
| Low | 0.5 | Nice to have. Minor improvements, style preferences. |

## Individual Checks

### Structure (20%)
| ID | Check | Severity | Pass Criteria |
|----|-------|----------|---------------|
| S01 | Valid HTML structure | Critical | DOCTYPE, html, head, body, article.ebook |
| S02 | CSS embedded | Critical | Full design system in <style> tag |
| S03 | Section IDs unique | Critical | No duplicate IDs, all kebab-case |
| S04 | Sommaire links valid | Critical | Every <a href="#id"> points to existing ID |
| S05 | Value-block per section | High | Every h2 section opens with .value-block |
| S06 | Recap-block per section | High | Every h2 section closes with .recap-block |
| S07 | Lang attribute set | Medium | <html lang="fr"> or <html lang="en"> |
| S08 | Title tag descriptive | Medium | <title> contains product name |
| S09 | Print styles included | Low | @media print rules present |

### Content Quality (30%)
| ID | Check | Severity | Pass Criteria |
|----|-------|----------|---------------|
| C01 | No placeholder text | Critical | Zero instances of [TODO], [INSERT], Lorem ipsum |
| C02 | QUOI defined | High | Each section states what reader will DO |
| C03 | POURQUOI defined | High | Each section explains WHY it matters |
| C04 | COMMENT defined | Critical | Each section has steps/script/template/checklist |
| C05 | MESURE defined | Medium | Each section has success KPI or signal |
| C06 | Specific actions | High | "Send 20 DMs/day" not "reach out to people" |
| C07 | Templates included | Medium | At least 1 copy-paste template per 3 sections |
| C08 | Honest caveats | Medium | Results-vary disclaimers where appropriate |

### Voice & Tone (15%)
| ID | Check | Severity | Pass Criteria |
|----|-------|----------|---------------|
| V01 | Direct address (tu/you) | High | No "nous", no "on", no passive |
| V02 | No motivational fluff | High | Zero "Tu peux le faire!", "Crois en toi!" |
| V03 | No academic tone | Medium | Zero "Il convient de noter", "Il est important" |
| V04 | No buzzword salad | Medium | Zero "levier synergies", "optimiser le ROI" |
| V05 | Action-first sentences | Medium | Lead with point, then explain |
| V06 | Short paragraphs | Medium | Max 5 lines / 80 words per paragraph |

### Completeness (20%)
| ID | Check | Severity | Pass Criteria |
|----|-------|----------|---------------|
| K01 | Word count met | High | playbook ≥5000, guide ≥3000, lead-magnet ≥1500 |
| K02 | Section count met | High | playbook 8-12, guide 5-7, lead-magnet 3-5 |
| K03 | Header section present | Critical | "Lis ça d'abord" or "Read This First" |
| K04 | TOC present | High | Sommaire with working links |
| K05 | CTA present (lead-magnet) | Critical | Clear CTA to paid product |
| K06 | Annexes present (playbook) | Medium | Templates, scripts, checklists |
| K07 | Action plan present (playbook) | Medium | Day-by-day or step-by-step calendar |
| K08 | Troubleshooting present (playbook) | Low | Common problems + solutions |

### Readability (15%)
| ID | Check | Severity | Pass Criteria |
|----|-------|----------|---------------|
| R01 | List variety | Medium | At least 1 list per 500 words |
| R02 | Template/script inclusion | Medium | At least 1 per 3 sections |
| R03 | H3 sub-sections | High | 3-7 per h2 section |
| R04 | Visual blocks used | Medium | Value/tools/recap blocks present |
| R05 | No text walls | High | No section without lists, blocks, or breaks |
