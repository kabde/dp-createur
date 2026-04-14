---
name: dp-competitor-analysis
description: "Analyse complète d'un concurrent : produit, positionnement, pricing, marketing et funnel. Compare avec votre propre offre et produit un rapport actionnable avec matrice de scoring, opportunités de différenciation, et contre-angles marketing. Triggers : concurrent, competitor, analyse concurrentielle, competitive analysis, benchmark, compare."
user-invokable: true
argument-hint: "[URL ou nom du concurrent] [focus: product|pricing|marketing|all]"
allowed-tools: Read Write Bash Glob WebFetch WebSearch
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: operations
  updated: 2026-04-13
---

# Competitor Analysis — Competitive Intelligence

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake, quality gates, scoring matrix, error handling, cross-skill integration -->

Expert en intelligence concurrentielle. Analyse un concurrent ou produit similaire et produit un rapport actionnable avec matrice de comparaison scorée, opportunités de différenciation, et contre-angles marketing prêts à l'emploi.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-competitor-analysis [URL]` | Analyse complète d'un concurrent via son URL |
| `/dp-competitor-analysis [nom]` | Recherche et analyse d'un concurrent par nom |
| `/dp-competitor-analysis [URL] pricing` | Focus uniquement sur le pricing et le modèle économique |
| `/dp-competitor-analysis [URL] marketing` | Focus sur le marketing, le funnel et le contenu |
| `/dp-competitor-analysis [URL] product` | Focus sur le produit, les features et la proposition de valeur |

## Output Format

```
LIVRABLE :
├── Rapport Markdown structuré (analysis/competitor-[nom]-[date].md)
├── Profil concurrent détaillé
├── Matrice de comparaison scorée (0-10 par dimension)
├── Opportunités de différenciation avec actions concrètes
├── 3-5 contre-angles marketing prêts à utiliser
└── Score global concurrent vs. votre offre
```

---

## Process

```
1. Context intake      → Collecter les infos sur le concurrent ET votre positionnement (OBLIGATOIRE)
2. Gather intelligence → Récupérer les données du concurrent (web, search)
   Read references/analysis-example.md → pour une analyse concurrentielle complète
3. Analyze & score     → Matrice de comparaison scorée dimension par dimension
4. Identify gaps       → Opportunités de différenciation actionnables
5. Counter-angles      → Angles marketing exploitant les faiblesses concurrentes
6. Quality check       → Vérification objectivité + complétude
7. Deliver             → Rapport sauvegardé + résumé + prochaines étapes
```

---

## Step 1 — Context Intake (Required: Always Do This First)

Avant toute analyse, collecter le contexte. Sans lui, la comparaison sera générique et les recommandations inutiles.

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom, niche, produit(s), audience, positionnement, prix
  → Ne PAS reposer les questions déjà couvertes par le profil

SINON :
  → Continuer sans. Les questions de l'intake couvriront le minimum.
```

### 1b. Poser les questions par blocs

**Règle absolue** : Ne JAMAIS poser toutes les questions d'un coup. Grouper par 2-3, attendre les réponses, reformuler pour valider, puis continuer.

#### Bloc 1 — Le concurrent à analyser (poser en premier)

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Quel concurrent veux-tu analyser ? Donne-moi une URL, un nom de produit, ou une description. | Cible de l'analyse |
| Q2 | Sur quoi veux-tu te concentrer ? `product` / `pricing` / `marketing` / `all` (par défaut : all) | Focus de l'analyse |

**Après les réponses** : Confirmer la cible. "OK, je vais analyser [X] en me concentrant sur [Y]. Avant ça, j'ai besoin de comprendre ton positionnement pour que la comparaison soit pertinente."

#### Bloc 2 — Votre positionnement (pour rendre la comparaison pertinente)

| # | Question | Pourquoi |
|---|----------|----------|
| Q3 | Décris ton produit/offre en 2-3 phrases. Qu'est-ce que tu vends, à quel prix, et à qui ? | Base de comparaison |
| Q4 | Quel est ton angle unique ? Ce qui te différencie déjà (ou ce que tu voudrais qui te différencie). | Axe de différenciation |
| Q5 | Qu'est-ce qui t'inquiète spécifiquement chez ce concurrent ? (ex: leur prix, leur audience, leur contenu...) | Focus les recommandations |

**Après les réponses** : Synthèse en 2-3 lignes. "Compris. Tu es [X], tu vends [Y] à [Z], et tu veux comprendre comment [concurrent] se positionne par rapport à toi, surtout sur [point d'inquiétude]. Je lance l'analyse."

**Hard gate** : Ne PAS lancer l'analyse sans connaître le produit de l'utilisateur. Sans cette info, la comparaison n'a pas de référent.

---

## Step 2 — Gather Intelligence

### 2a. Si URL fournie

Utiliser WebFetch pour lire la page du concurrent. Extraire :

| Dimension | Données à collecter |
|-----------|---------------------|
| Produit | Nom, description, format, contenu, features |
| Pricing | Prix, modèle (one-time, subscription, tiers), upsells |
| Audience | Qui ils ciblent (profil, niveau, situation) |
| Promesse | Proposition de valeur principale, résultats promis |
| Preuve sociale | Témoignages, chiffres, logos, études de cas |
| Contenu | Blog, social, email, podcast, fréquence |
| Funnel | CTA, pages de vente, séquence, lead magnets |
| Design | Qualité visuelle, positionnement de marque (premium/budget/authority) |

### 2b. Si nom/description fourni

Utiliser WebSearch pour trouver le concurrent en ligne, puis WebFetch sur les pages clés.

### 2c. Si accès limité

Si la page est derrière un paywall ou inaccessible :
- Noter ce qui n'a pas pu être vérifié
- Utiliser les données publiques disponibles (réseaux sociaux, avis, contenu gratuit)
- Indiquer clairement dans le rapport : "Non vérifié — basé sur les données publiques"

---

## Step 3 — Analyze & Score (Comparison Matrix)

Construire la matrice de comparaison scorée. Chaque dimension reçoit un score de 0 à 10.

### 3a. Profil concurrent

```
╔══════════════════════════════════════════════════╗
║           PROFIL CONCURRENT                      ║
╠══════════════════════════════════════════════════╣
║ Nom          : [nom]                             ║
║ URL          : [url]                             ║
║ Produit      : [description courte]              ║
║ Prix         : [prix + modèle]                   ║
║ Audience     : [qui ils ciblent]                 ║
║ Positionnement : [premium/budget/authority/community] ║
╠══════════════════════════════════════════════════╣
║ FORCES                                           ║
║ • [force 1]                                      ║
║ • [force 2]                                      ║
║ • [force 3]                                      ║
╠══════════════════════════════════════════════════╣
║ FAIBLESSES                                       ║
║ • [faiblesse 1]                                  ║
║ • [faiblesse 2]                                  ║
║ • [faiblesse 3]                                  ║
╚══════════════════════════════════════════════════╝
```

### 3b. Matrice de comparaison scorée

```
MATRICE DE COMPARAISON (0-10 par dimension)
═══════════════════════════════════════════════════
| Dimension        | Concurrent | Vous  | Écart | Avantage    |
|------------------|-----------|-------|-------|-------------|
| Prix             | [X]/10    | [X]/10| [+/-] | [qui]       |
| Valeur perçue    | [X]/10    | [X]/10| [+/-] | [qui]       |
| Profondeur       | [X]/10    | [X]/10| [+/-] | [qui]       |
| Actionnabilité   | [X]/10    | [X]/10| [+/-] | [qui]       |
| Support client   | [X]/10    | [X]/10| [+/-] | [qui]       |
| Identité de marque| [X]/10   | [X]/10| [+/-] | [qui]       |
| Preuve sociale   | [X]/10    | [X]/10| [+/-] | [qui]       |
| Contenu/SEO      | [X]/10    | [X]/10| [+/-] | [qui]       |
| Funnel/Conversion| [X]/10    | [X]/10| [+/-] | [qui]       |
| Innovation       | [X]/10    | [X]/10| [+/-] | [qui]       |
|------------------|-----------|-------|-------|-------------|
| TOTAL            | [XX]/100  | [XX]/100| [+/-]|             |
═══════════════════════════════════════════════════

VERDICT : [Résumé en 1-2 phrases — qui est en avance et sur quoi]
```

### 3c. Scoring Rules

| Score | Signification |
|-------|---------------|
| 0-2 | Absent ou très faible |
| 3-4 | Basique, en dessous de la moyenne du marché |
| 5-6 | Correct, dans la moyenne |
| 7-8 | Bon, au-dessus de la moyenne |
| 9-10 | Excellent, best-in-class |

**Règle d'objectivité** : Si le concurrent est meilleur sur une dimension, le score doit le refléter. Pas de biais favorable envers l'utilisateur.

---

## Step 4 — Identify Differentiation Opportunities

```
OPPORTUNITÉS DE DIFFÉRENCIATION
═══════════════════════════════════

PRIORITÉ HAUTE (écart > 3 points en votre défaveur) :
1. [Dimension] — Écart : [X] points
   Pourquoi : [explication]
   Action : [chose concrète à faire cette semaine]
   Impact estimé : [ce que ça change]

2. [Dimension] — Écart : [X] points
   ...

OPPORTUNITÉS (dimensions où le concurrent est faible) :
1. [Gap identifié]
   Action : [comment l'exploiter]
   
2. [Gap identifié]
   ...
```

Catégories à explorer systématiquement :

| Catégorie | Question clé |
|-----------|-------------|
| Positionnement | Comment vous démarquer dans l'esprit du client ? |
| Pricing | Votre prix est-il un atout ou un handicap face à ce concurrent ? |
| Contenu | Quels sujets le concurrent ne couvre PAS que vous pourriez posséder ? |
| Audience | Y a-t-il des segments mal servis que le concurrent ignore ? |
| Produit | Que propose le concurrent que vous devriez ajouter ? |
| Marketing | Quels messages contreraient directement le positionnement concurrent ? |

---

## Step 5 — Marketing Counter-Angles

Suggérer 3-5 angles marketing qui exploitent les faiblesses du concurrent sans jamais le nommer.

```
CONTRE-ANGLES MARKETING
═══════════════════════════

1. [Nom de l'angle]
   Hook : "[Headline / première phrase accrocheuse]"
   Pourquoi ça marche : [explication — quelle faiblesse ça exploite]
   Où l'utiliser : [post social, ad, page de vente, email]

2. [Nom de l'angle]
   Hook : "[Headline]"
   Pourquoi ça marche : [explication]
   Où l'utiliser : [canal]

3. ...
```

---

## Step 6 — SWOT Synthesis

Après les analyses détaillées, synthétiser en format SWOT :

```
SYNTHÈSE SWOT — VOTRE POSITION VS. [CONCURRENT]
═══════════════════════════════════════════════════

INTERNE (Vous) :
  Forces    : [3 points forts clés]
  Faiblesses : [3 points à améliorer]

EXTERNE (Concurrent / Marché) :
  Opportunités : [3 gaps exploitables]
  Menaces       : [3 risques si le concurrent progresse]

FORCES CONCURRENTIELLES :
  Substituts directs   : [même type de produit]
  Substituts indirects : [même problème, format différent]
  Coût de changement   : [facilité pour leur client de passer chez vous]
```

---

## Step 7 — Quality Check & Delivery

### 7a. Quality Gates

| ID | Gate | Severity |
|----|------|----------|
| QG-01 | Aucun placeholder dans le rapport | Critical |
| QG-02 | Chaque recommandation a une action concrète | Critical |
| QG-03 | Scores justifiés par des données observables | Critical |
| QG-04 | Objectivité respectée — pas de biais favorable | High |
| QG-05 | Distinction claire entre données vérifiées et estimations | High |
| QG-06 | Aucune recommandation de copier le concurrent | Critical |
| QG-07 | Aucune attaque directe ou dénigrement du concurrent | Critical |
| QG-08 | Contre-angles ne nomment jamais le concurrent | High |

### 7b. Sauvegarder le rapport

```
Chemin de sortie : analysis/competitor-[nom]-[YYYY-MM-DD].md
Créer le dossier analysis/ si inexistant.
```

### 7c. Présenter le livrable

```
ANALYSE CONCURRENTIELLE TERMINÉE
═══════════════════════════════════

Concurrent analysé : [nom]
Focus              : [product/pricing/marketing/all]
Fichier            : [chemin complet]

SCORE COMPARATIF :
  Concurrent : [XX]/100
  Vous       : [XX]/100
  Écart      : [+/- XX] points

DIMENSIONS CLÉ :
  Votre avantage  : [dimension 1], [dimension 2]
  Leur avantage   : [dimension 1], [dimension 2]
  À parité        : [dimension 1], [dimension 2]

TOP 3 ACTIONS PRIORITAIRES :
  1. [action concrète]
  2. [action concrète]
  3. [action concrète]

PROCHAINES ÉTAPES :
  → /dp-mediaplan          Créer du contenu exploitant les gaps identifiés
  → /dp-ad-angles-meta     Transformer les contre-angles en publicités
  → /dp-landing-page       Optimiser votre page de vente vs. la concurrence
  → /dp-playbook-create    Créer un produit comblant un gap identifié
```

---

## Error Handling

| Scénario | Action |
|----------|--------|
| URL inaccessible (paywall, 404) | Tenter WebSearch, utiliser données publiques, noter les limites dans le rapport |
| Concurrent inconnu / pas de données | Demander plus de contexte à l'utilisateur, chercher des alternatives similaires |
| L'utilisateur n'a pas encore de produit | Analyser le concurrent comme benchmark, recommandations orientées "ce qu'il faut construire" |
| Focus trop large (trop de concurrents) | Limiter à 1 concurrent par analyse. Proposer de relancer pour les autres |
| L'utilisateur veut copier le concurrent | Rediriger vers la différenciation. "Copier = courir derrière. Différencier = créer ta catégorie." |
| business-profile.md absent | Continuer avec les réponses du context intake |
| Données contradictoires sur le concurrent | Signaler l'incohérence, donner les deux versions, recommander de vérifier |

---

## Cross-Skill Integration

| Avant competitor-analysis | Skill précédent | Quand |
|--------------------------|-----------------|-------|
| Définir son propre positionnement | `business-profile.md` | Recommandé pour enrichir la comparaison |

| Après competitor-analysis | Skill suivant | Quand |
|--------------------------|---------------|-------|
| Créer du contenu différencié | `/dp-mediaplan` | Pour exploiter les gaps de contenu |
| Publicités ciblées | `/dp-ad-angles-meta` `/dp-ad-angles-google` | Pour transformer les contre-angles en ads |
| Page de vente optimisée | `/dp-landing-page` | Pour se positionner vs. la concurrence |
| Nouveau produit | `/dp-playbook-create` | Si un gap produit a été identifié |
| Captions exploitant les angles | `/dp-social-caption` | Pour du contenu de différenciation |
