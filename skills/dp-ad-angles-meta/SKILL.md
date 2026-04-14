---
name: dp-ad-angles-meta
description: "Générateur complet d'angles publicitaires Meta Ads (Facebook + Instagram). Produit 12 angles différenciés avec copies A/B, hooks, textes primaires, headlines, descriptions, CTAs, suggestions d'audiences, et structure de campagne. Inclut les specs Meta, la direction créative, et les paramètres UTM. Triggers : meta ads, facebook ads, instagram ads, pub meta, publicité facebook, angles pub, ad copy."
user-invokable: true
argument-hint: "[produit] [objectif: conversions|leads|traffic|awareness] [budget]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: marketing
  updated: 2026-04-13
---

# Ad Angles Meta — Facebook & Instagram Ad Copy Generator

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake, quality gates, UTM, error handling, cross-skill integration -->

Expert en performance marketing Meta Ads pour DP Créateur. Génère des angles publicitaires différenciés avec copies prêtes à l'emploi, ciblage d'audience, et structure de campagne.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-ad-angles-meta [produit]` | Lancer la génération complète (12 angles + structure) |
| `/dp-ad-angles-meta express [produit]` | Mode rapide — 3 questions puis génération |
| `/dp-ad-angles-meta retarget [produit]` | Angles spécifiques retargeting uniquement |
| `/dp-ad-angles-meta video [produit]` | Focus scripts vidéo (Reels + Stories) |

## Output Format

```
LIVRABLE :
├── 12 angles publicitaires différenciés
│   ├── Version A + Version B (A/B test) par angle
│   ├── Hook, Primary Text, Headline, Description, CTA
│   ├── Direction créative (visuel + style)
│   └── Ciblage d'audience par angle
├── Structure de campagne (Testing → Scaling → Iteration)
├── URLs avec paramètres UTM générés
└── Fichier HTML : ads/meta-angles-[date].html
```

---

## Process

```
1. Context intake      → Collecter les infos produit, audience, budget (OBLIGATOIRE)
2. Read references     → Charger business-profile.md + contenu produit si dispo
   Read references/ad-copy-examples.md → pour 3 exemples d'angles complets
3. Generate angles     → 12 angles psychologiques différenciés
4. Write copies        → Version A + B par angle avec specs Meta
5. Build targeting     → Suggestions d'audience par angle
6. Structure campaign  → Phases Testing → Scaling → Iteration
7. Generate UTMs       → Paramètres de tracking pour chaque URL
8. Quality check       → Vérification specs, compliance, différenciation
9. Deliver             → Fichier HTML + résumé
```

---

## Step 1 — Context Intake (Required: Always Do This First)

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom, niche, produit(s), audience, ton, prix
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
| Q1 | Quel produit ou service veux-tu promouvoir ? Décris-le en 1-2 phrases. | Cadre la campagne |
| Q2 | Quel est le prix ? (ou gratuit si lead magnet) | Copy et positionnement |
| Q3 | Quel objectif de campagne ? `conversions` / `leads` / `traffic` / `awareness` | Structure et optimisation |

**Après les réponses** : Reformuler. "Tu veux promouvoir [X] à [prix] avec un objectif de [Y]. Correct ?"

#### Bloc 2 — L'audience et le budget

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | Qui est ton client idéal ? Décris-le (métier, situation, frustration). | Ciblage et ton des copies |
| Q5 | Quel budget quotidien ou mensuel ? | Recommandations de structure |
| Q6 | Tu as une landing page URL ? (sinon je génère les copies sans lien) | UTM et CTA |

**Après les réponses** : Synthèse audience.

#### Bloc 3 — Contexte existant

| # | Question | Pourquoi |
|---|----------|----------|
| Q7 | Tu as déjà fait des pubs Meta ? Si oui, qu'est-ce qui a marché / pas marché ? | Éviter les erreurs passées |
| Q8 | Tu as du social proof ? (témoignages, chiffres, résultats) | Angles authority et social proof |

**Après les réponses** : Passer à la génération.

---

## Step 2 — Generate 12 Ad Angles

Chaque angle attaque depuis un point d'entrée psychologique différent :

| # | Type d'angle | Psychologie |
|---|-------------|-------------|
| 1 | **Pain Point** | Frustration directe du client idéal |
| 2 | **Contrarian** | Remettre en question une croyance commune |
| 3 | **Résultat spécifique** | Chiffre concret + délai précis |
| 4 | **Social Proof** | Témoignages, résultats, communauté |
| 5 | **FOMO** | Urgence ou peur de rater l'opportunité |
| 6 | **Simplicité** | Facilité d'exécution, peu de temps requis |
| 7 | **Identité** | "Tu n'es pas [défaut]. Tu [vrai problème]." |
| 8 | **Comparaison** | Prix vs alternatives coûteuses |
| 9 | **Curiosité** | Tease un mécanisme ou une méthode |
| 10 | **Autorité** | Crédibilité, expertise, parcours |
| 11 | **Urgence / Rareté** | Offre limitée, bonus temporaire |
| 12 | **Transformation** | Avant / après — le voyage du client |

**Règle** : Chaque angle doit être GENUINEMENT différent. Pas juste une reformulation du même message.

## Step 3 — Write Complete Ad Copies

Pour CHAQUE angle, produire :

```
ANGLE [#]: [Nom de l'angle]
========================
Segment d'audience : [qui cet angle cible le mieux]
Placement : [Feed / Stories / Reels / All]
Format : [Image / Vidéo / Carousel]

--- VERSION A ---

Hook (première ligne — doit arrêter le scroll) :
[Ligne d'ouverture — max 125 caractères]

Primary Text (corps — max 500 caractères pour le Feed) :
[Corps complet de la pub]

Headline (max 40 caractères) :
[Titre bold sous l'image/vidéo]

Description (max 30 caractères) :
[Texte de support sous le headline]

CTA Button : [Shop Now / Learn More / Get Offer / Sign Up]

URL avec UTM :
[landing_page_url]?utm_source=meta&utm_medium=paid&utm_campaign=[product-slug]&utm_content=angle[#]-va

--- VERSION B (variation A/B) ---

Hook : [Alternative]
Primary Text : [Alternative]
Headline : [Alternative]
Description : [Alternative]
CTA Button : [Même ou différent]

URL avec UTM :
[landing_page_url]?utm_source=meta&utm_medium=paid&utm_campaign=[product-slug]&utm_content=angle[#]-vb

--- DIRECTION CRÉATIVE ---

Visuel : [Ce que l'image/vidéo doit montrer]
Texte overlay : [Texte clé sur le créatif — max 20% de l'image]
Style : [Clean / Bold / Raw / Professionnel]
Couleurs : Utiliser les couleurs de marque (primary_color, accent_color) pour la cohérence visuelle
Référence : [Décrire l'ambiance]
```

### Scripts vidéo (minimum 2 angles en format vidéo)

Au moins 2 angles doivent inclure un script Reels/Stories complet :

```
SCRIPT VIDÉO — ANGLE [#]
=========================
Durée : [15-30 secondes]

[0-3s] HOOK VISUEL — [ce qui apparaît à l'écran]
[0-5s] HOOK AUDIO — "[texte parlé qui accroche]"
[5-15s] PROBLÈME + AGITATION — "[développement]"
[15-25s] SOLUTION + PREUVE — "[pitch produit]"
[25-30s] CTA — "[appel à l'action]"

Direction visuelle : [talking head / screen recording / b-roll / text animation]
```

## Step 4 — Audience Targeting

Pour chaque angle :

```
CIBLAGE
-------
Intérêts : [liste d'intérêts Meta pertinents]
Comportements : [achat, utilisation device]
Démographie : [âge, genre si pertinent]
Lookalike : [source — liste email, acheteurs, visiteurs site]
Exclusions : [qui exclure — clients existants, etc.]
```

## Step 5 — Campaign Structure

```
STRUCTURE DE CAMPAGNE
=====================

Phase 1 — Testing (Jours 1-7)
  Budget : [X]€/jour
  Angles à tester : [top 4-5 angles]
  Ad sets : 1 par angle, ciblage large
  Optimisation : Achat / Lead (selon objectif)
  Seuil : Minimum 5× le CPA cible par ad set avant de juger

Phase 2 — Scaling (Jours 8-14)
  Kill : angles avec CPA > [seuil] ou CTR < 1%
  Scale : augmenter budget de 20% sur les gagnants
  Ajouter : audiences lookalike depuis les convertis

Phase 3 — Iteration (Jours 15+)
  Nouveaux créatifs sur les angles gagnants
  Retargeting : visiteurs site, viewers vidéo, engagers
  Refresh : nouvelles versions A/B toutes les 2-3 semaines
```

## Step 6 — UTM Parameters

Générer les UTM pour CHAQUE URL de pub :

```
FORMAT UTM STANDARD :
  utm_source=meta
  utm_medium=paid
  utm_campaign=[product-slug]
  utm_content=angle[N]-v[a|b]
  utm_term=[audience-segment]

EXEMPLE :
  https://example.com/produit?utm_source=meta&utm_medium=paid&utm_campaign=mon-produit&utm_content=angle3-va&utm_term=interet-marketing
```

---

## Meta Ads Specs Reference

| Élément | Limite de caractères |
|---------|---------------------|
| Primary Text | 125 chars (visible), 500 max |
| Headline | 40 chars recommandé, 255 max |
| Description | 30 chars recommandé, 125 max |
| Image | 1080×1080 (Feed), 1080×1920 (Stories) |
| Vidéo | 1:1 ou 9:16, max 240 min, <4GB |
| Carousel | 2-10 cartes, 1080×1080 chaque |
| Texte sur image | <20% de la surface (best practice) |

## Copy Rules

- **Le hook est tout** — Si la première ligne n'arrête pas le scroll, la pub échoue
- **Spécifique > vague** — Des chiffres et délais concrets battent les généralités
- **Phrases courtes** — Surtout pour la lecture mobile
- **Un seul CTA par pub** — Ne pas confondre le lecteur
- **Pas de hype** — "Système" pas "secret", "guide" pas "formule magique"
- **Transparence prix** — Si le prix est un avantage, l'utiliser comme argument
- **Social proof quand disponible** — Même des chiffres d'expérience ou de résultats
- **Compliance** — Aucune garantie de revenus, aucune affirmation fausse

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Aucun placeholder [TODO], [INSERT], Lorem ipsum | Critical |
| QG-02 | Aucune garantie de revenus ou promesse de résultats financiers | Critical |
| QG-03 | Chaque angle est genuinement différent (pas de reformulation) | Critical |
| QG-04 | Tous les character counts respectent les limites Meta | Critical |
| QG-05 | Chaque URL inclut des paramètres UTM complets | High |
| QG-06 | Au moins 2 angles incluent un script vidéo | High |
| QG-07 | Chaque angle a une Version A et une Version B | High |
| QG-08 | Budget minimum ≥ 5× CPA estimé par ad set — ne jamais lancer avec un budget insuffisant pour la phase d'apprentissage | Critical |
| QG-09 | Toujours inclure les paramètres UTM | Critical |
| QG-10 | Ne jamais promettre un ROAS spécifique | Critical |
| QG-11 | Budget de test minimum : ≥5× le CPA cible par ad set | High |
| QG-12 | Copies légalement conformes — pas de fausses déclarations | Critical |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Pas de produit défini | Demander : "Quel produit veux-tu promouvoir ?" Minimum = nom + prix + description courte |
| Pas de landing page URL | Générer les copies sans URL. Ajouter un placeholder `[URL_LANDING_PAGE]` dans les UTM |
| Budget trop faible (<5€/jour) | Recommander 2-3 angles max au lieu de 12. Focus organique + boost de posts |
| Pas de social proof | Omettre l'angle #4 (Social Proof). Remplacer par un angle "Behind the scenes" |
| Produit gratuit (lead magnet) | Adapter les CTA : "Download Free" / "Get Your Copy". Objectif = leads, pas conversions |
| business-profile.md absent | Continuer avec les réponses du context intake uniquement |
| Audience trop large / vague | Proposer 3 segments spécifiques. Demander de choisir ou confirmer |
| L'utilisateur veut un seul angle | Générer l'angle demandé avec 3 versions (A/B/C) au lieu de 12 angles |

---

## Cross-Skill Integration

| Avant ad-angles-meta | Skill précédent | Quand |
|----------------------|-----------------|-------|
| Page de vente prête | `/dp-landing-page` | Toujours recommandé — besoin d'une URL de destination |
| Profil business défini | `business-profile.md` | Pour éviter les questions redondantes |
| Funnel conçu | `/dp-sales-funnel` | Pour aligner les pubs avec la stratégie globale |

| Après ad-angles-meta | Skill suivant | Quand |
|----------------------|---------------|-------|
| Pubs Google en complément | `/dp-ad-angles-google` | Pour couvrir Search + YouTube |
| Contenu organique | `/dp-social-caption` `/dp-mediaplan` | Pour renforcer la présence organique |
| Séquence email | `/dp-email-sequence` | Pour nurturer les leads captés |
| Analyse des résultats | Suivi manuel des UTM | Après 7 jours de diffusion |
