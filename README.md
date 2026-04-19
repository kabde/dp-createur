# DP Createur

**La suite complete de skills Claude Code pour creer, lancer et vendre des produits digitaux.**

De l'idee a la vente : validez votre marche, creez un ebook de 60+ pages, exportez en PDF, lancez votre landing page avec thank you page et pages legales, deployez vos campagnes ads, votre strategie blog SEO, vos emails de lancement, et maximisez votre revenu par client avec des upsells — le tout guide par des questions intelligentes et adapte a votre marque.

[![Skills](https://img.shields.io/badge/skills-22-059669)](skills/)
[![References](https://img.shields.io/badge/references-30+-10b981)](skills/)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-blueviolet)](https://claude.ai/code)

---

## Ce que fait DP Createur

```
Idee → Validation → Ebook 60+ pages → Cover → PDF → Landing Page + Thank You
  → Blog SEO → WordPress → Ads (Meta + Google) → Email → Media Plan → Upsells
```

### Fondation

| Skill | Ce qu'il produit |
|-------|-----------------|
| `/dp-business-profile` | Profil business central (couleurs, voix, audience) — lu par tous les skills |
| `/dp-market-research` | Validation d'idee avec score 0-100 et verdict GO / TEST / STOP |

### Creation du produit

| Skill | Ce qu'il produit |
|-------|-----------------|
| `/dp-playbook-create` | Ebook/playbook 60+ pages aux couleurs de votre marque |
| `/dp-playbook-section` | Sections supplementaires, exercices, templates |
| `/dp-playbook-audit` | Score qualite 0-100 (HTML et PDF) avec issues priorisees |
| `/dp-playbook-sync` | Synchronisation FR ↔ EN |
| `/dp-export-pdf` | PDF professionnel (navigateur, Puppeteer, ou wkhtmltopdf) |
| `/dp-ebook-cover` | Couverture HTML + mockup 3D CSS + prompts IA + brief Canva |
| `/dp-lead-magnet-create` | Ressource gratuite (checklist, mini-guide, cheat sheet) |

### Vente & Monetisation

| Skill | Ce qu'il produit |
|-------|-----------------|
| `/dp-landing-page` | Page de vente + Thank You page + Privacy Policy + CGV en modales |
| `/dp-sales-funnel` | Architecture de funnel avec math de conversion |
| `/dp-upsell-strategy` | Echelle de valeur, order bump, upsell, downsell, affiliation, calcul LTV |

### Contenu & SEO

| Skill | Ce qu'il produit |
|-------|-----------------|
| `/dp-blog-strategy` | Plan de 10-30 articles en topic clusters avec maillage |
| `/dp-blog-article` | Articles SEO avec E-E-A-T, schema JSON-LD, maillage interne |
| `/dp-blog-publish` | Publication directe sur WordPress via API REST |

### Promotion

| Skill | Ce qu'il produit |
|-------|-----------------|
| `/dp-email-sequence` | Sequences email (welcome, launch, abandon, nurture, post-purchase) |
| `/dp-social-caption` | Captions par plateforme (IG, LinkedIn, TikTok, FB, X) |
| `/dp-mediaplan` | Calendrier editorial 4 semaines avec briefs detailles |
| `/dp-ad-angles-meta` | Angles publicitaires Facebook/Instagram avec copies A/B |
| `/dp-ad-angles-google` | Campagnes Google Search/YouTube/Display |

### Analyse & Qualite

| Skill | Ce qu'il produit |
|-------|-----------------|
| `/dp-competitor-analysis` | Analyse concurrentielle avec matrice et counter-angles |
| `/dp-copy-review` | Audit et optimisation de tout type de copy |

---

## Installation

### Prerequis

- [Claude Code](https://claude.ai/code) installe et configure

### Installation rapide

```bash
git clone https://github.com/kabde/dp-createur.git
cd dp-createur
```

Claude Code detecte automatiquement les skills dans le dossier `skills/`.

> Guide detaille : [INSTALL.md](INSTALL.md)

---

## Demarrage rapide

### 1. Configurer votre profil (une seule fois)

```
/dp-business-profile
```

### 2. Valider votre idee

```
/dp-market-research [votre idee de produit]
```

### 3. Creer votre ebook

```
/dp-playbook-create [sujet]
```

### 4. Creer la couverture + exporter en PDF

```
/dp-ebook-cover [titre]
/dp-export-pdf [fichier.html]
```

### 5. Creer la page de vente

```
/dp-landing-page
```

### 6. Lancer la promotion

```
/dp-blog-strategy [niche]
/dp-email-sequence
/dp-ad-angles-meta
/dp-mediaplan
```

### 7. Maximiser le revenu

```
/dp-upsell-strategy
```

---

## Workflow complet

```
/dp-business-profile         Configurer (1x)
        |
/dp-market-research          Valider l'idee (GO/TEST/STOP)
        |
/dp-playbook-create          Creer l'ebook (60+ pages)
        |
/dp-playbook-audit           Verifier la qualite (HTML ou PDF)
        |
/dp-ebook-cover              Creer la couverture + mockup 3D
        |
/dp-export-pdf               Convertir en PDF
        |
   +---------+---------+---------+
   |         |         |         |
/dp-landing /dp-blog  /dp-email /dp-upsell
  -page      -strategy  -sequence -strategy
   |         |         |         |
   |    /dp-blog       |     Order bump
   |     -article      |     Upsell
   |         |         |     Downsell
   |    /dp-blog       |     Affiliation
   |     -publish      |
   |         |         |
   +---------+---------+
        |
   +---------+---------+
   |         |         |
/dp-ad     /dp-ad    /dp-media
 -angles    -angles    -plan
 -meta      -google
```

---

## Chaque skill inclut

- **Context intake** : questions guidees par blocs, adapte au contexte du projet
- **Quality gates** : regles strictes avec severite (Critical/High/Medium)
- **Error handling** : scenarios d'erreur avec actions correctives
- **Cross-skill integration** : chaque skill sait d'ou il vient et ou il va
- **Fichiers de reference** : exemples concrets bases sur un produit fictif (FitPro Academy)

---

## Exemples d'outputs

Tous les exemples utilisent un produit fictif : **FitPro Academy** — "Le Playbook du Coach Fitness" a 47 euros.

| Output | Skill | Exemple |
|--------|-------|---------|
| Validation marche (score 77/100) | dp-market-research | [validation-example.md](skills/dp-market-research/references/validation-example.md) |
| Section ebook (2000 mots) | dp-playbook-create | [example-section.md](skills/dp-playbook-create/references/example-section.md) |
| Couvertures 5 styles | dp-ebook-cover | [cover-examples.md](skills/dp-ebook-cover/references/cover-examples.md) |
| Copy landing page (3 prix) | dp-landing-page | [copy-templates.md](skills/dp-landing-page/references/copy-templates.md) |
| 7 emails de lancement | dp-email-sequence | [launch-sequence-templates.md](skills/dp-email-sequence/references/launch-sequence-templates.md) |
| 3 angles Meta Ads | dp-ad-angles-meta | [ad-copy-examples.md](skills/dp-ad-angles-meta/references/ad-copy-examples.md) |
| 2 RSA Google Ads | dp-ad-angles-google | [rsa-examples.md](skills/dp-ad-angles-google/references/rsa-examples.md) |
| Strategie 20 articles | dp-blog-strategy | [strategy-example.md](skills/dp-blog-strategy/references/strategy-example.md) |
| 7 briefs Instagram | dp-mediaplan | [post-briefs-example.md](skills/dp-mediaplan/references/post-briefs-example.md) |
| Funnel complet avec math | dp-sales-funnel | [funnel-example.md](skills/dp-sales-funnel/references/funnel-example.md) |
| Upsell strategy + LTV 100€ | dp-upsell-strategy | [upsell-example.md](skills/dp-upsell-strategy/references/upsell-example.md) |
| Analyse concurrent | dp-competitor-analysis | [analysis-example.md](skills/dp-competitor-analysis/references/analysis-example.md) |

---

## Auteur

**Abderrahim KHALID** — 20 ans d'experience dans le digital.

Specialise en strategie digitale, creation de produits, marketing et automatisation. DP Createur est le resultat de deux decennies de pratique condensees en un systeme actionnable.

---

## Licence

[MIT](LICENSE) — Libre d'utilisation, modification et distribution.

---

## Contribuer

Les contributions sont les bienvenues. Consultez [CONTRIBUTING.md](CONTRIBUTING.md) pour les guidelines.
