---
name: dp-ad-angles-google
description: "Générateur complet de campagnes Google Ads (Search, YouTube, Display). Produit des groupes de mots-clés, des Responsive Search Ads, des scripts YouTube, des créatifs Display, et des recommandations de bidding. Inclut les specs Google, les paramètres UTM, et la structure de campagne. Triggers : google ads, search ads, youtube ads, display ads, pub google, adwords, ppc, sem."
user-invokable: true
argument-hint: "[produit] [type: search|youtube|display|all] [budget]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: marketing
  updated: 2026-04-13
---

# Ad Angles Google — Search, YouTube & Display Ad Generator

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake, quality gates, UTM, Quality Score, error handling, cross-skill -->

Expert Google Ads pour DP Créateur. Génère des campagnes complètes : recherche de mots-clés, copies RSA, scripts YouTube, direction Display, et structure de campagne avec bidding.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-ad-angles-google [produit]` | Lancer la génération complète (Search + YouTube + Display) |
| `/dp-ad-angles-google search [produit]` | Search Ads uniquement (mots-clés + RSA) |
| `/dp-ad-angles-google youtube [produit]` | Scripts YouTube uniquement (In-Stream + Bumper + Discovery) |
| `/dp-ad-angles-google display [produit]` | Display / Discovery Ads uniquement |

## Output Format

```
LIVRABLE :
├── Recherche de mots-clés (4 groupes × 15-25 mots-clés)
│   ├── High Intent (Bottom of Funnel)
│   ├── Medium Intent (Middle of Funnel)
│   ├── Low Intent / Awareness (Top of Funnel)
│   └── Competitor / Alternative
├── Responsive Search Ads (1 RSA par groupe)
│   ├── 15 Headlines (max 30 chars)
│   ├── 4 Descriptions (max 90 chars)
│   └── Extensions (Sitelinks, Callouts, Snippets)
├── Scripts YouTube (3 formats)
├── Display / Discovery Ads
├── Structure de campagne + Bidding
├── URLs avec paramètres UTM
└── Fichier HTML : ads/google-angles-[date].html
```

---

## Process

```
1. Context intake       → Collecter produit, audience, budget, types de campagne (OBLIGATOIRE)
2. Read references      → Charger business-profile.md + contenu produit si dispo
   Read references/rsa-examples.md → pour des RSA et scripts YouTube complets
3. Keyword research     → 4 groupes de mots-clés par intent
4. Write RSA copies     → Headlines + Descriptions + Extensions
5. YouTube scripts      → 3 scripts (In-Stream, Bumper, Discovery)
6. Display ads          → Headlines + Descriptions + Direction visuelle
7. Campaign structure   → Bidding + Budget + Timeline
8. Generate UTMs        → Paramètres de tracking pour chaque URL
9. Quality check        → Quality Score, compliance, specs
10. Deliver             → Fichier HTML + résumé
```

---

## Step 1 — Context Intake (Required: Always Do This First)

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom, niche, produit(s), audience, ton, prix, URL site
  → Extraire aussi : couleurs de marque (primary_color, accent_color, style visuel)
  → Intégrer les couleurs dans les briefs créatifs (direction artistique, spécifications visuelles)
  → Ne PAS reposer les questions déjà couvertes par le profil

SINON :
  → Continuer sans. Les questions de l'intake couvriront le minimum.
```

### 1b. Poser les questions par blocs

**Règle absolue** : Ne JAMAIS poser toutes les questions d'un coup. Grouper par 2-3, attendre les réponses, puis continuer.

#### Bloc 1 — Le produit et l'objectif

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Quel produit ou service veux-tu promouvoir ? Décris-le en 1-2 phrases. | Cadre les mots-clés et copies |
| Q2 | Quel est le prix ? | Argument de vente et bidding |
| Q3 | Quels types de campagne ? `search` / `youtube` / `display` / `all` | Scope de la génération |

**Après les réponses** : Reformuler. "Tu veux des campagnes [types] pour [produit] à [prix]. Correct ?"

#### Bloc 2 — L'audience et le budget

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | Qui est ton client idéal ? Décris son profil (métier, situation, frustration). | Mots-clés et ton des copies |
| Q5 | Quel budget mensuel pour Google Ads ? | Structure et recommandations de bidding |
| Q6 | Tu as une landing page URL ? | Destination des pubs + UTM |

**Après les réponses** : Synthèse audience + budget.

#### Bloc 3 — Contexte existant

| # | Question | Pourquoi |
|---|----------|----------|
| Q7 | Tu as déjà un compte Google Ads ? Si oui, qu'est-ce qui a marché / pas marché ? | Éviter les erreurs passées |
| Q8 | Quels sont tes concurrents principaux ? (noms ou URLs) | Groupe mots-clés "Competitor" |
| Q9 | Tu as un niche focus spécifique ? (ex: coaching fitness vs coaching business) | Précision des mots-clés |

**Après les réponses** : Passer à la génération.

---

## Step 2 — Keyword Research

Générer des groupes de mots-clés organisés par intent de recherche :

### Groupe 1 — High Intent (Bottom of Funnel)
Chercheurs prêts à acheter une solution.
```
Mots-clés : 15-25 termes
Match type : Phrase + Exact
CPC estimé : [fourchette haute]
Exemples de format :
  - [acheter/commander] + [type de produit]
  - [type de produit] + [résultat souhaité]
  - [solution] + [problème spécifique]
```

### Groupe 2 — Medium Intent (Middle of Funnel)
Chercheurs explorant des solutions.
```
Mots-clés : 15-25 termes
Match type : Phrase + Broad (modifié)
CPC estimé : [fourchette moyenne]
Exemples de format :
  - comment + [objectif]
  - [activité] + marketing / stratégie
  - meilleur(e) + [catégorie]
```

### Groupe 3 — Low Intent / Awareness (Top of Funnel)
Chercheurs avec des intérêts adjacents.
```
Mots-clés : 15-25 termes
Match type : Broad
CPC estimé : [fourchette basse]
```

### Groupe 4 — Competitor / Alternative
```
Mots-clés : 10-15 termes
Match type : Phrase
CPC estimé : [fourchette variable]
Format : [concurrent] + alternative / avis / comparaison
```

### Mots-clés négatifs (obligatoire)
```
Toujours inclure :
  - gratuit / free (si on cible des acheteurs)
  - emploi / job / salaire / hiring
  - formation diplômante / certification / degree
  - [termes non pertinents selon la niche]
```

---

## Step 3 — Responsive Search Ads (RSA)

Pour chaque groupe de mots-clés, créer un RSA :

```
RESPONSIVE SEARCH AD — [Nom du groupe]
========================================

Headlines (15 — max 30 caractères chacun) :
H1: [headline]  ← épingler en position 1 (inclure le mot-clé principal)
H2: [headline]  ← épingler en position 2
H3-H15: [headlines variés — bénéfices, prix, CTA, urgence]

Descriptions (4 — max 90 caractères chacune) :
D1: [description]  ← épingler en position 1 (inclure mot-clé + CTA)
D2-D4: [descriptions variées]

Display URL path : [domaine]/[path1]/[path2]

Extensions Sitelink :
- [Titre] → [URL] | [Ligne description 1] | [Ligne description 2]
- [Titre] → [URL] | [Ligne description 1] | [Ligne description 2]
- [Titre] → [URL] | [Ligne description 1] | [Ligne description 2]
- [Titre] → [URL] | [Ligne description 1] | [Ligne description 2]

Extensions Callout :
- [callout 1 — max 25 chars]
- [callout 2]
- [callout 3]
- [callout 4]

Structured Snippets :
- Type : [Types / Fonctionnalités / Marques]
- Valeurs : [valeur1], [valeur2], [valeur3]
```

---

## Step 4 — YouTube Ad Scripts

Créer 3 scripts YouTube :

### Script 1 — Skippable In-Stream (15-30 secondes)
```
[0-5s] HOOK — Doit accrocher AVANT le bouton "Ignorer"
"[Question ou déclaration percutante liée au problème]"

[5-15s] PROBLÈME + AGITATION
"[Développer la frustration — montrer qu'on comprend]"

[15-25s] SOLUTION + PREUVE
"[Présenter le produit + résultat concret]"

[25-30s] CTA
"[Prix]. [Promesse courte]. Lien dans la description."

DIRECTION VISUELLE :
- [talking head / screen recording / texte animé]
- Texte overlay pour les phrases clés
- Coupes rapides, pas d'intro lente
- Couleurs de marque (primary_color, accent_color) pour les overlays et éléments graphiques
```

### Script 2 — Bumper Ad (6 secondes)
```
"[Problème en 1 phrase] ?
[Solution] — [Prix].
[Nom de marque / URL]."
```

### Script 3 — Discovery Ad (60-90 secondes)
```
[Format long pour YouTube Discovery / In-feed]
[Script détaillé avec approche storytelling]
[Structure : Hook → Contexte → Problème → Solution → Preuve → CTA]
```

---

## Step 5 — Display / Discovery Ads

```
DISPLAY AD SET
==============
Headline 1 (30 chars) : [headline]
Headline 2 (30 chars) : [headline]
Headline 3 (30 chars) : [headline]
Long Headline (90 chars) : [headline]
Description 1 (90 chars) : [description]
Description 2 (90 chars) : [description]
Nom de l'entreprise : [depuis business-profile ou intake]

Specs image :
- Paysage : 1200×628
- Carré : 1200×1200
- Portrait : 960×1200

Direction visuelle : [description de ce que les images doivent montrer]
Couleurs : Utiliser les couleurs de marque (primary_color, accent_color) pour la cohérence visuelle

Ciblage d'audience :
- In-market : [segments]
- Affinité : [segments]
- Custom intent : [mots-clés]
- Remarketing : [visiteurs site, viewers YouTube]
```

---

## Step 6 — Campaign Structure & Bidding

```
STRUCTURE DE CAMPAGNE
=====================

Campagne 1 : Search — High Intent
  Budget : [X]€/jour
  Bidding : Target CPA ou Maximize Conversions
  Ad Groups : [par groupe de mots-clés]
  Objectif Quality Score : ≥7 pour chaque mot-clé

Campagne 2 : Search — Medium Intent
  Budget : [X]€/jour
  Bidding : Maximize Clicks → basculer en tCPA après 30 conversions

Campagne 3 : YouTube — In-Stream
  Budget : [X]€/jour
  Bidding : Maximum CPV
  Ciblage : Custom intent + In-market audiences

Campagne 4 : Display — Remarketing
  Budget : [X]€/jour
  Bidding : Target CPA
  Ciblage : Visiteurs site (30 jours), viewers YouTube

TIMELINE DE TEST :
Semaine 1 : Lancer campagnes 1 & 3
Semaine 2 : Ajouter campagne 2, analyser les termes de recherche
Semaine 3 : Ajouter campagne 4 (remarketing), optimiser les enchères
Semaine 4 : Couper les sous-performants, scaler les gagnants
```

## Step 7 — UTM Parameters

Générer les UTM pour CHAQUE URL de pub :

```
FORMAT UTM STANDARD :
  utm_source=google
  utm_medium=cpc (search) | video (youtube) | display
  utm_campaign=[product-slug]-[campaign-type]
  utm_content=[ad-group-name]
  utm_term=[keyword] (search uniquement)

EXEMPLE SEARCH :
  https://example.com/produit?utm_source=google&utm_medium=cpc&utm_campaign=mon-produit-search&utm_content=high-intent&utm_term=acheter+guide

EXEMPLE YOUTUBE :
  https://example.com/produit?utm_source=google&utm_medium=video&utm_campaign=mon-produit-youtube&utm_content=instream-script1
```

---

## Google Ads Specs Reference

| Élément | Limite |
|---------|--------|
| RSA Headline | 30 caractères |
| RSA Description | 90 caractères |
| Display Headline | 30 caractères |
| Display Long Headline | 90 caractères |
| Display Description | 90 caractères |
| Sitelink Title | 25 caractères |
| Sitelink Description | 35 caractères par ligne |
| Callout | 25 caractères |
| URL Path | 15 caractères par segment |

## Copy Rules

- **Matcher l'intent de recherche** — La copy doit répondre à ce que le chercheur recherche
- **Inclure le mot-clé** dans au moins le Headline 1 et la Description 1
- **Utiliser des chiffres** — prix, durée, quantités concrètes
- **CTA dans chaque description** — Action claire et directe
- **Pas de superlatifs sans preuve** — Ne pas dire "meilleur" sans justification
- **Transparence prix** — Si le prix est compétitif, c'est un avantage
- **Aucune déclaration de revenus** — Résultats concrets, pas de promesses financières

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Aucun placeholder [TODO], [INSERT], Lorem ipsum | Critical |
| QG-02 | Aucune garantie de revenus ou promesse de résultats financiers | Critical |
| QG-03 | Tous les character counts respectent les limites Google exactes | Critical |
| QG-04 | Chaque RSA a exactement 15 headlines et 4 descriptions | Critical |
| QG-05 | H1 et D1 incluent le mot-clé principal du groupe | High |
| QG-06 | Chaque URL inclut des paramètres UTM complets | High |
| QG-07 | Mots-clés négatifs définis pour chaque campagne | High |
| QG-08 | Ne jamais recommander du broad match sans smart bidding | Critical |
| QG-09 | Toujours inclure les paramètres UTM | Critical |
| QG-10 | Ne jamais promettre un ROAS spécifique | Critical |
| QG-11 | Objectif Quality Score ≥7 pour les mots-clés high intent | High |
| QG-12 | Hooks YouTube accrochent dans les 5 premières secondes (avant skip) | High |
| QG-13 | Budgets réalistes pour une petite entreprise | High |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Pas de produit défini | Demander : "Quel produit veux-tu promouvoir ?" Minimum = nom + prix + description |
| Pas de landing page URL | Générer les copies avec placeholder `[URL_LANDING_PAGE]`. Suggérer `/dp-landing-page` |
| Budget très faible (<300€/mois) | Recommander Search High Intent uniquement. Pas de Display ni YouTube |
| Pas de concurrents identifiés | Omettre le Groupe 4 (Competitor). Renforcer les groupes 1-3 |
| Niche très spécifique / faible volume | Élargir les mots-clés avec des termes adjacents. Recommander Broad + Smart Bidding |
| business-profile.md absent | Continuer avec les réponses du context intake uniquement |
| L'utilisateur veut un seul type de campagne | Générer uniquement le type demandé. Mentionner les autres comme recommandation |
| Quality Score prévu <7 | Recommander d'améliorer la landing page (pertinence, vitesse, contenu) |

---

## Cross-Skill Integration

| Avant ad-angles-google | Skill précédent | Quand |
|------------------------|-----------------|-------|
| Page de vente prête | `/dp-landing-page` | Toujours recommandé — la landing page impacte le Quality Score |
| Profil business défini | `business-profile.md` | Pour éviter les questions redondantes |
| Funnel conçu | `/dp-sales-funnel` | Pour aligner la stratégie globale |

| Après ad-angles-google | Skill suivant | Quand |
|------------------------|---------------|-------|
| Pubs Meta en complément | `/dp-ad-angles-meta` | Pour couvrir Social Ads en plus de Search |
| Contenu organique | `/dp-social-caption` `/dp-mediaplan` | Pour renforcer la présence organique |
| Séquence email | `/dp-email-sequence` | Pour nurturer les leads captés |
| Blog SEO | `/dp-blog-article` | Pour améliorer le Quality Score via le contenu du site |
