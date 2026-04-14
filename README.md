# DP Createur

**La suite de skills Claude Code pour creer, lancer et vendre des produits digitaux.**

Creez un ebook de 60+ pages, une landing page, des campagnes publicitaires, une strategie de contenu blog, des sequences email et un plan media — le tout guide par des questions intelligentes et adapte a votre marque.

[![Skills](https://img.shields.io/badge/skills-19-059669)](skills/)
[![References](https://img.shields.io/badge/references-28-10b981)](skills/)
[![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-blueviolet)](https://claude.ai/code)

---

## Ce que fait DP Createur

```
Idee → Ebook 60+ pages → PDF → Landing Page → Blog SEO → Ads → Email → Media Plan
```

| Etape | Skill | Ce qu'il produit |
|-------|-------|-----------------|
| Configuration | `/dp-business-profile` | Profil business central (couleurs, voix, audience) |
| Creation ebook | `/dp-playbook-create` | Ebook/playbook 60+ pages aux couleurs de votre marque |
| Enrichissement | `/dp-playbook-section` | Sections supplementaires, exercices, templates |
| Qualite | `/dp-playbook-audit` | Score qualite 0-100 avec issues priorisees |
| Traduction | `/dp-playbook-sync` | Synchronisation FR ↔ EN |
| Export | `/dp-export-pdf` | PDF professionnel pret a vendre |
| Lead magnet | `/dp-lead-magnet-create` | Ressource gratuite (checklist, mini-guide) pour capturer des emails |
| Vente | `/dp-landing-page` | Page de vente responsive avec CTA |
| Funnel | `/dp-sales-funnel` | Architecture de funnel avec math de conversion |
| Strategie blog | `/dp-blog-strategy` | Plan de 10-30 articles en topic clusters |
| Articles SEO | `/dp-blog-article` | Articles optimises avec maillage interne et schema JSON-LD |
| Publication | `/dp-blog-publish` | Publication directe sur WordPress via API REST |
| Emails | `/dp-email-sequence` | Sequences email (welcome, launch, abandon, nurture) |
| Captions | `/dp-social-caption` | Captions par plateforme (IG, LinkedIn, TikTok, FB, X) |
| Plan media | `/dp-mediaplan` | Calendrier editorial 4 semaines avec briefs detailles |
| Ads Meta | `/dp-ad-angles-meta` | Angles publicitaires Facebook/Instagram avec copies A/B |
| Ads Google | `/dp-ad-angles-google` | Campagnes Google Search/YouTube/Display |
| Concurrence | `/dp-competitor-analysis` | Analyse concurrentielle avec matrice et counter-angles |
| Revue copy | `/dp-copy-review` | Audit et optimisation de tout type de copy |

---

## Installation

### Prerequis

- [Claude Code](https://claude.ai/code) installe et configure

### Installation rapide

```bash
# Cloner le repository
git clone https://github.com/kabde/dp-createur.git

# Aller dans le dossier
cd dp-createur
```

Claude Code detecte automatiquement les skills dans le dossier `skills/`.

> Pour le guide d'installation detaille, consultez [INSTALL.md](INSTALL.md).

---

## Utilisation rapide

### 1. Configurer votre profil business (une seule fois)

```
/dp-business-profile
```

Repond aux questions sur votre marque, vos couleurs, votre audience. Ce fichier est lu par tous les autres skills.

### 2. Creer votre ebook

```
/dp-playbook-create [sujet de votre ebook]
```

Le skill vous guide etape par etape :
- Questions sur votre expertise et votre audience
- Choix des couleurs et du style visuel
- Generation du plan avec compteur de pages
- Redaction section par section
- Score qualite avant livraison

### 3. Exporter en PDF

```
/dp-export-pdf [fichier.html]
```

### 4. Creer la page de vente

```
/dp-landing-page
```

### 5. Lancer la promotion

```
/dp-blog-strategy [niche]        # Planifier 20 articles
/dp-email-sequence               # Creer la sequence de lancement
/dp-ad-angles-meta               # Generer les ads Facebook/Instagram
/dp-mediaplan                    # Planifier le contenu social
```

---

## Workflow complet

```
/dp-business-profile          Configurer (1x)
        |
/dp-playbook-create           Creer l'ebook (60+ pages)
        |
/dp-playbook-audit            Verifier la qualite
        |
/dp-export-pdf                Convertir en PDF
        |
   +---------+---------+
   |         |         |
/dp-landing  /dp-blog   /dp-email
  -page      -strategy   -sequence
   |         |         |
   |    /dp-blog       |
   |     -article      |
   |         |         |
   |    /dp-blog       |
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

- **Context intake** : questions guidees par blocs (jamais tout d'un coup)
- **Quality gates** : regles strictes avec severite (Critical/High/Medium)
- **Error handling** : scenarios d'erreur avec actions correctives
- **Cross-skill integration** : chaque skill sait d'ou il vient et ou il va
- **Fichiers de reference** : exemples concrets, templates de copy, standards SEO

---

## Structure du projet

```
dp-createur/
├── README.md                    Ce fichier
├── INSTALL.md                   Guide d'installation detaille
├── CONTRIBUTING.md              Guide de contribution
├── LICENSE                      Licence MIT
├── CLAUDE.md                    Instructions pour Claude Code
├── business-profile.md          Votre profil (genere par dp-business-profile)
│
└── skills/
    ├── dp-business-profile/
    │   ├── SKILL.md             Definition du skill
    │   └── references/          Exemples et templates
    │       └── profile-example.md
    │
    ├── dp-playbook-create/
    │   ├── SKILL.md
    │   └── references/
    │       ├── design-system.md
    │       ├── voice-guide.md
    │       ├── quality-gates.md
    │       ├── scoring-system.md
    │       ├── product-types.md
    │       ├── error-handling.md
    │       └── example-section.md
    │
    ├── dp-landing-page/
    │   ├── SKILL.md
    │   └── references/
    │       └── copy-templates.md
    │
    └── ... (19 skills au total)
```

---

## Exemples d'outputs

Tous les exemples utilisent un produit fictif : **FitPro Academy** — "Le Playbook du Coach Fitness" a 47 euros.

| Output | Skill | Exemple |
|--------|-------|---------|
| Section ebook (2000 mots) | dp-playbook-create | [example-section.md](skills/dp-playbook-create/references/example-section.md) |
| Copy landing page (3 prix) | dp-landing-page | [copy-templates.md](skills/dp-landing-page/references/copy-templates.md) |
| 7 emails de lancement | dp-email-sequence | [launch-sequence-templates.md](skills/dp-email-sequence/references/launch-sequence-templates.md) |
| 3 angles Meta Ads | dp-ad-angles-meta | [ad-copy-examples.md](skills/dp-ad-angles-meta/references/ad-copy-examples.md) |
| 2 RSA Google Ads | dp-ad-angles-google | [rsa-examples.md](skills/dp-ad-angles-google/references/rsa-examples.md) |
| Strategie 20 articles | dp-blog-strategy | [strategy-example.md](skills/dp-blog-strategy/references/strategy-example.md) |
| 7 briefs Instagram | dp-mediaplan | [post-briefs-example.md](skills/dp-mediaplan/references/post-briefs-example.md) |
| Funnel complet avec math | dp-sales-funnel | [funnel-example.md](skills/dp-sales-funnel/references/funnel-example.md) |

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
