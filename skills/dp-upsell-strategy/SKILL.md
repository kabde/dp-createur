---
name: dp-upsell-strategy
description: "Conçoit une stratégie d'upsell complète autour du produit principal : échelle de valeur (gratuit → premium), order bumps, upsells post-achat, downsells, cross-sells, séquences email d'ascension. Calcule le LTV (lifetime value) par client et optimise le revenu par transaction. S'appuie sur le contexte du projet (business-profile.md, produit principal, audience). Triggers : upsell, cross-sell, order bump, downsell, échelle de valeur, value ladder, augmenter le panier, LTV, lifetime value, maximiser revenu."
user-invokable: true
argument-hint: "[produit principal] [prix] [--full pour stratégie complète avec copies]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: marketing
  updated: 2026-04-18
---

# Upsell Strategy — Maximiser le Revenu par Client

<!-- v2.0.0 | 2026-04-18 | Création : échelle de valeur, order bump, upsell/downsell, LTV, séquences d'ascension, copies complètes -->

Un client à 47€ peut devenir un client à 500€+ avec la bonne stratégie. Ce skill conçoit l'intégralité du parcours de monétisation autour de ton produit principal — de l'entrée gratuite jusqu'à l'offre premium — avec les copies, les timings, et les calculs de rentabilité.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-upsell-strategy` | Stratégie complète guidée (échelle + copies + calculs) |
| `/dp-upsell-strategy express` | Version rapide — échelle de valeur + calcul LTV uniquement |
| `/dp-upsell-strategy bump [produit]` | Concevoir uniquement l'order bump pour un produit |
| `/dp-upsell-strategy email [produit]` | Séquence email d'ascension uniquement |
| `/dp-upsell-strategy ltv` | Calculer le LTV actuel et identifier les leviers |

## Output Format

```
LIVRABLE : upsell-strategy/strategy-[slug].md

├── Échelle de valeur complète (4-6 niveaux)
│   ├── Chaque niveau : produit, prix, format, promesse, marge
│   └── Logique de progression (pourquoi ce produit APRÈS celui-là)
│
├── Order Bump
│   ├── Produit recommandé + prix + copy complète
│   └── Design de la checkbox (HTML + texte)
│
├── Upsell Post-Achat (1-click)
│   ├── Produit + prix + copy de la page upsell
│   └── Timing (immédiatement après / 24h / 7j)
│
├── Downsell (si refus de l'upsell)
│   ├── Offre alternative + prix réduit + copy
│   └── Logique : "Pas prêt pour X ? Voici Y à moitié prix"
│
├── Cross-Sell
│   ├── Produits complémentaires (non concurrents)
│   └── Quand et comment les proposer
│
├── Séquence Email d'Ascension (5-7 emails)
│   ├── Chaque email : timing, sujet, body, CTA
│   └── Logique de progression vers le next level
│
├── Calculs LTV
│   ├── Revenu moyen par client (sans upsell vs avec)
│   ├── Take rate estimé par offre
│   └── Impact sur le budget d'acquisition (CPA max)
│
└── Plan d'implémentation
    ├── Outils nécessaires
    ├── Ordre de mise en place
    └── Timeline recommandée
```

---

## Process

```
1. Context intake         → Produit principal, prix, audience, stack
2. Read context           → business-profile.md, produits existants, funnel existant
3. Analyser la position   → Où en est l'utilisateur dans sa monétisation
4. Concevoir l'échelle    → 4-6 niveaux de valeur cohérents
5. Créer l'order bump     → Produit + copy + design
6. Créer l'upsell         → Offre post-achat + page + timing
7. Créer le downsell      → Alternative si refus
8. Créer les cross-sells  → Produits complémentaires
9. Séquence d'ascension   → Emails qui font monter dans l'échelle
10. Calculer le LTV       → Impact financier chiffré
11. Plan d'implémentation → Quoi mettre en place, dans quel ordre
12. Deliver               → Rapport + copies + calculs
```

---

## Step 1 — Context Intake (Required)

### 1a. Charger le contexte projet (silencieux)

```
SI business-profile.md existe :
  → Lire : nom business, produit principal (nom, prix, format), 
    autres produits, audience, niche, stack technique (plateforme de vente, email)
  → Pré-remplir l'analyse — NE PAS reposer ces questions

SI un funnel existe (funnel/sales-funnel-*.md) :
  → Lire : architecture actuelle, étapes, produits dans le funnel
  → Identifier les trous dans l'échelle de valeur

SI des séquences email existent (emails/) :
  → Lire : types de séquences déjà créées
  → Ne pas recréer ce qui existe

SINON :
  → Poser toutes les questions
```

### 1b. Questions par blocs

#### Bloc 1 — Ton produit principal

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | **Quel est ton produit principal ?** Nom, format (ebook, cours, coaching), prix. | Centre de l'échelle de valeur |
| Q2 | **Combien de ventes par mois ?** (même approximatif : 0, 5, 20, 100+) | Détermine la maturité et les priorités |
| Q3 | **Quel est ton panier moyen actuel ?** (= prix du produit si un seul produit) | Baseline pour mesurer l'amélioration |

**Après les réponses** : "Ton produit principal est [X] à [prix], avec environ [N] ventes/mois. Ton panier moyen est [Y€]. On va travailler à augmenter ça."

#### Bloc 2 — Ton écosystème actuel

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | **Tu as d'autres produits ou services ?** (même en projet) Liste-les avec leurs prix. | Construire sur l'existant |
| Q5 | **Tu as un lead magnet gratuit ?** Si oui, lequel ? | Entrée de l'échelle |
| Q6 | **Tu offres du coaching, du consulting ou du service 1:1 ?** Si oui, à quel prix ? | Haut de l'échelle |

#### Bloc 3 — Tes capacités

| # | Question | Pourquoi |
|---|----------|----------|
| Q7 | **Quelle plateforme de vente ?** (Gumroad, Stripe, LemonSqueezy, Shopify, Systeme.io) | Détermine ce qui est techniquement possible (order bump, upsell 1-click) |
| Q8 | **Quel outil email ?** (ConvertKit, Mailchimp, Brevo, ActiveCampaign) | Pour les séquences d'ascension et le tagging |
| Q9 | **Combien de temps tu peux investir pour créer les produits manquants ?** (1 semaine / 1 mois / j'ai le temps) | Priorisation réaliste |

---

## Step 2 — Analyser la Position Actuelle

### Diagnostic express

```
DIAGNOSTIC — MONÉTISATION ACTUELLE
════════════════════════════════════

Produit principal      : [nom] — [prix]
Ventes/mois            : [N]
Panier moyen           : [X€]
Revenu mensuel estimé  : [X€]
LTV actuel (sans upsell): [≈ prix du produit]

Autres produits        : [liste ou "aucun"]
Lead magnet            : [oui/non — lequel]
Offre premium          : [oui/non — laquelle]

MANQUES IDENTIFIÉS :
  [✅/❌] Lead magnet gratuit (entrée de funnel)
  [✅/❌] Order bump au checkout (panier +)
  [✅/❌] Upsell post-achat (next level)
  [✅/❌] Downsell si refus (récupérer les hésitants)
  [✅/❌] Cross-sell (produits complémentaires)
  [✅/❌] Offre premium/high-ticket (maximiser les meilleurs clients)
  [✅/❌] Séquence email d'ascension (automatiser la montée)

POTENTIEL D'AMÉLIORATION :
  Panier moyen actuel : [X€]
  Panier moyen cible  : [X€] (+[Y%])
  LTV actuel          : [X€]
  LTV cible           : [X€] (+[Y%])
```

---

## Step 3 — Concevoir l'Échelle de Valeur

### Principe fondamental

```
L'échelle de valeur = chaque étape résout un problème PLUS PROFOND
                      et demande un engagement PLUS FORT.

Le client monte naturellement quand :
  1. Il a obtenu un résultat au niveau précédent ✓
  2. Il veut aller plus loin ✓
  3. L'offre suivante est la suite logique ✓
  4. Le prix est proportionnel à la valeur ✓
```

### Structure de l'échelle (6 niveaux)

```
ÉCHELLE DE VALEUR — [Business Name]
════════════════════════════════════

NIVEAU 0 — GRATUIT (Lead Magnet)
  ┌─────────────────────────────────────────┐
  │ Produit   : [Lead magnet]               │
  │ Prix      : Gratuit                     │
  │ Format    : [PDF / Checklist / Quiz]    │
  │ Promesse  : [Quick win en 15 min]       │
  │ Objectif  : Capturer l'email            │
  │ Transition: → "Tu veux le système      │
  │              complet ? Voici [Produit]" │
  │ Skill DP  : /dp-lead-magnet-create      │
  └─────────────────────────────────────────┘
          │
          ▼ Email séquence welcome (J1-J7)
          
NIVEAU 1 — PRODUIT D'ENTRÉE (Order Bump)
  ┌─────────────────────────────────────────┐
  │ Produit   : [Petit produit]             │
  │ Prix      : [7-27€]                     │
  │ Format    : [Template pack / Checklist] │
  │ Promesse  : [Accélérateur du produit    │
  │              principal]                 │
  │ Moment    : Au checkout (case à cocher) │
  │ Take rate : 25-40% des acheteurs        │
  └─────────────────────────────────────────┘
          │
          ▼ Achat du produit principal
          
NIVEAU 2 — PRODUIT PRINCIPAL ← TON PRODUIT ACTUEL
  ┌─────────────────────────────────────────┐
  │ Produit   : [Nom du produit principal]  │
  │ Prix      : [Prix actuel]              │
  │ Format    : [Ebook / Playbook / Cours]  │
  │ Promesse  : [Résultat principal]        │
  │ C'est ici : Le cœur de ton business    │
  └─────────────────────────────────────────┘
          │
          ▼ Page upsell (immédiatement après l'achat)
          
NIVEAU 3 — UPSELL (Offre Premium)
  ┌─────────────────────────────────────────┐
  │ Produit   : [Produit avancé]            │
  │ Prix      : [2-5× le produit principal] │
  │ Format    : [Cours vidéo / Workshop /   │
  │              Pack avancé]               │
  │ Promesse  : [Aller plus loin, plus vite]│
  │ Moment    : Immédiatement post-achat    │
  │ Take rate : 10-20% des acheteurs        │
  │ SI REFUS  : → Downsell (Niveau 3b)     │
  └─────────────────────────────────────────┘
          │
          ├── SI REFUS ──▶ NIVEAU 3b — DOWNSELL
          │   ┌────────────────────────────────┐
          │   │ Produit : [Version allégée]    │
          │   │ Prix    : [50% du Niveau 3]    │
          │   │ Format  : [Mini-cours / Pack]  │
          │   │ Message : "Pas prêt pour [X] ? │
          │   │  Voici [Y] pour commencer."    │
          │   │ Take rate: 15-25% des refus    │
          │   └────────────────────────────────┘
          │
          ▼ Email séquence ascension (J+7 à J+30)
          
NIVEAU 4 — HIGH-TICKET
  ┌─────────────────────────────────────────┐
  │ Produit   : [Coaching / Programme /     │
  │              Mastermind]                │
  │ Prix      : [10-20× le produit         │
  │              principal]                │
  │ Format    : [Coaching 1:1 / Groupe /    │
  │              Done-for-you]             │
  │ Promesse  : [Résultat garanti avec      │
  │              accompagnement]            │
  │ Moment    : Email J+14 à J+30          │
  │ Take rate : 2-5% des acheteurs         │
  │ Qualification: Appel découverte         │
  └─────────────────────────────────────────┘
          │
          ▼ Clients satisfaits
          
NIVEAU 5 — RÉCURRENCE / COMMUNAUTÉ
  ┌─────────────────────────────────────────┐
  │ Produit   : [Membership / Communauté /  │
  │              Abonnement]               │
  │ Prix      : [19-99€/mois]              │
  │ Format    : [Communauté privée / Accès  │
  │              contenu mensuel / Q&A]     │
  │ Promesse  : [Support continu + mises    │
  │              à jour + communauté]       │
  │ Moment    : Email J+30+                │
  │ Take rate : 5-10% des acheteurs        │
  └─────────────────────────────────────────┘
```

### Règles de conception de l'échelle

| Règle | Détail |
|-------|--------|
| Chaque niveau est la SUITE LOGIQUE du précédent | Pas un produit random — le client doit se dire "c'est exactement ce dont j'ai besoin maintenant" |
| Le prix monte PAR PALIERS | ×2 à ×5 entre chaque niveau, jamais ×20 d'un coup |
| Le format change avec le prix | Ebook → Cours vidéo → Coaching. Plus le prix monte, plus c'est personnalisé |
| Le lead magnet est un ÉCHANTILLON du produit principal | Pas un sujet différent — un aperçu de ce que contient le produit |
| L'order bump ACCÉLÈRE le produit principal | Templates, scripts, checklists qui rendent le produit principal plus efficace |
| L'upsell APPROFONDIT | Même sujet mais niveau supérieur, ou accompagnement |
| Le downsell SIMPLIFIE | Version allégée, prix réduit, sans engagement |

### Demander validation

"Voici l'échelle de valeur proposée. Est-ce que ça correspond à ta vision ? Tu veux ajuster un niveau ?"

**Hard gate** : Valider avant de passer aux copies.

---

## Step 4 — Order Bump (copy + design)

### Conception

```
ORDER BUMP — [Nom du produit bump]
════════════════════════════════════

Prix : [X€] (ajouté au panier, pas séparé)
Format : [Template pack / Checklist avancée / Ressource complémentaire]
```

### Copy de la checkbox

```
╔══════════════════════════════════════════════════════╗
║ ☐  OUI, AJOUTE [Nom du bump] — [prix]               ║
║                                                      ║
║  [1 phrase d'accroche qui décrit le bénéfice]        ║
║                                                      ║
║  [2-3 bullet points de ce qui est inclus]            ║
║  • [Item 1 — résultat concret]                       ║
║  • [Item 2 — gain de temps]                          ║
║  • [Item 3 — bonus exclusif]                         ║
║                                                      ║
║  Offre réservée aux acheteurs — pas disponible après.║
╚══════════════════════════════════════════════════════╝
```

### Règles de l'order bump

| Règle | Pourquoi |
|-------|----------|
| Prix = 30-60% du produit principal | Si le produit est à 47€, le bump est à 17-27€ |
| 1 seule phrase d'accroche | Pas de paragraphe — c'est une décision en 3 secondes |
| 3 bullet points max | Le client est DÉJÀ en train de payer, pas le moment de lire |
| Scarcité vraie | "Réservé aux acheteurs" — pas de faux countdown |
| Le bump COMPLÈTE, ne REMPLACE PAS | Il rend le produit principal meilleur, pas optionnel |

---

## Step 5 — Upsell Post-Achat

### Page d'upsell (affichée immédiatement après l'achat)

```
PAGE UPSELL — [Nom de l'offre upsell]
══════════════════════════════════════

TIMING : Immédiatement après le paiement (before thank you page)
         OU email J+1 (si la plateforme ne supporte pas le 1-click upsell)

PRIX : [X€] (ou [X€/mois] si récurrent)
```

### Copy de la page upsell

```
[HEADLINE]
Tu viens d'acheter [Produit Principal]. Bravo — tu es dans les [X%] qui passent à l'action.

Mais soyons honnêtes : [Produit Principal] te donne le SYSTÈME.
Ce qui te manque encore, c'est [le problème suivant].

[SECTION — CE QUE TU OBTIENS]

Avec [Nom Upsell], tu obtiens :

• [Bénéfice 1 — résultat concret] → [pourquoi c'est important]
• [Bénéfice 2 — gain de temps] → "[avec le playbook seul, ça prend X. Avec ça, Y]"
• [Bénéfice 3 — exclusivité] → [ce qui n'est pas dans le produit principal]
• [Bénéfice 4 — support] → [accès à toi, communauté, Q&A]

[SECTION — PREUVE]

[Témoignage ou résultat concret de quelqu'un qui a combiné les deux]
"[Citation]" — [Prénom], [Résultat]

[SECTION — OFFRE]

Prix normal : [prix barré]
Ton prix aujourd'hui (acheteur [Produit]) : [prix réduit]

Cette offre est réservée aux acheteurs de [Produit]. 
Elle ne sera plus disponible après cette page.

[CTA — BOUTON]
"Oui, j'ajoute [Nom Upsell] pour [prix]"

[LIEN REFUS]
"Non merci, je continue avec [Produit] seul" → redirige vers thank you page
```

### Timings recommandés par type

| Type d'upsell | Quand le proposer | Take rate moyen |
|---------------|-------------------|-----------------|
| Page 1-click post-achat | Immédiatement après paiement | 10-20% |
| Email J+1 | Le lendemain (after onboarding) | 5-10% |
| Email J+7 | Après premiers résultats | 8-15% |
| Email J+14 | Après engagement prouvé | 5-10% |
| Appel découverte (high-ticket) | J+14 à J+30 | 2-5% |

---

## Step 6 — Downsell (si refus de l'upsell)

### Logique du downsell

```
Le client a dit NON à l'upsell.
Ne PAS insister. Proposer une ALTERNATIVE plus accessible.

RÈGLE : Le downsell est 40-60% du prix de l'upsell.
        Il offre MOINS mais reste utile seul.
```

### Copy du downsell

```
[HEADLINE]
Pas de souci — [Produit Upsell] n'est peut-être pas pour toi maintenant.

Mais avant de continuer, j'ai une autre option :

[Nom Downsell] — seulement [prix réduit]

C'est [description en 1 phrase — version simplifiée de l'upsell].

Tu obtiens :
• [Item 1 — le plus important de l'upsell]
• [Item 2 — la partie la plus actionnable]
(Sans [Item 3 et 4 qui étaient dans l'upsell])

C'est un investissement de [prix] pour [résultat].

[CTA]
"Oui, j'ajoute [Downsell] pour [prix]"

[LIEN REFUS]
"Non merci, je continue sans" → thank you page
```

---

## Step 7 — Cross-Sells

### Produits complémentaires (ne concurrencent PAS le produit principal)

```
CROSS-SELLS RECOMMANDÉS
════════════════════════

CROSS-SELL 1 :
  Produit   : [Produit complémentaire]
  Prix      : [prix]
  Relation  : "[Produit Principal] t'apprend QUOI faire. [Cross-sell] t'aide à le faire PLUS VITE."
  Moment    : Email post-achat J+3 ou J+5
  Copy      : "Tu avances bien avec [Produit]. Pour aller encore plus vite, [Cross-sell] est fait pour toi."

CROSS-SELL 2 :
  Produit   : [Outil / Template / Ressource]
  Prix      : [prix]
  Relation  : "[Produit Principal] donne la méthode. [Cross-sell] donne les outils."
  Moment    : In-app ou email J+7

CROSS-SELL 3 :
  Produit   : [Produit d'un partenaire — affiliation]
  Prix      : [prix]
  Commission: [30-50%]
  Relation  : "Pour [sujet connexe], je recommande [Partenaire]."
  Moment    : Email J+14 ou dans les annexes du playbook
```

### Règle absolue du cross-sell

| Faire | Ne PAS faire |
|-------|-------------|
| Compléter le produit principal | Concurrencer ton propre produit |
| Proposer au bon moment (après résultats) | Proposer immédiatement (too soon) |
| Donner une raison spécifique | "Tu pourrais aussi aimer..." (trop vague) |
| Limiter à 2-3 cross-sells max | Bombarder de 10 offres |

### Cross-Sell Affilié — Monétiser sans créer de produit

Le cross-sell affilié est un levier puissant : tu recommandes un outil ou un produit d'un partenaire via ton lien d'affiliation et tu touches une commission sur chaque vente — sans créer quoi que ce soit.

#### Comment choisir un produit affilié

| Critère | Seuil | Pourquoi |
|---------|-------|----------|
| Pertinence pour ton audience | Le client en a BESOIN pour appliquer ton produit | Sinon c'est du spam, pas une recommandation |
| Tu l'as utilisé toi-même | Oui, obligatoire | Tu ne recommandes que ce que tu connais. Ta crédibilité en dépend. |
| Commission | ≥ 20% (one-time) ou ≥ 15% (récurrent/mois) | En dessous, pas assez rentable pour l'effort |
| Page de vente du partenaire | Professionnelle, bien convertie | Si sa page est mauvaise, tu perds des commissions ET de la crédibilité |
| Support client du partenaire | Bon | Si le client a un problème, il viendra se plaindre chez TOI |
| Cookie duration | ≥ 30 jours | Le client n'achète pas toujours immédiatement |

#### Types de produits affiliés par niche

| Type | Exemples | Commission typique | Revenu moyen/client |
|------|----------|-------------------|---------------------|
| Outils SaaS (récurrent) | ConvertKit, Systeme.io, Canva Pro, Notion | 20-30% récurrent/mois | 3-10€/mois passif |
| Formations en ligne | Cours complémentaire d'un partenaire | 30-50% one-time | 15-50€ par vente |
| Livres / Ebooks | Amazon, Gumroad d'un partenaire | 4-50% selon plateforme | 2-15€ par vente |
| Équipement / Matériel | Amazon, boutiques spécialisées | 3-10% | 2-20€ par vente |
| Services (hosting, design) | Hébergeurs, freelances, agences | 50-200€ flat ou 20-30% | 20-100€ par referral |

#### Où placer les liens affiliés

```
PLACEMENTS RECOMMANDÉS (par efficacité décroissante)
════════════════════════════════════════════════════

1. DANS L'EBOOK — Section "Outils recommandés" ou Annexes
   Placement  : tools-block ou annexe "Ressources"
   Exemple    : "Pour créer ta landing page, j'utilise Systeme.io [lien affilié].
                C'est l'outil que je recommande pour les coachs qui débutent."
   Disclosure : Ajouter en bas de la section : "Ce lien est un lien affilié.
                Si tu t'inscris via ce lien, je touche une commission — sans
                surcoût pour toi. Je ne recommande que des outils que j'utilise."
   Efficacité : ★★★★★ (le lecteur est engagé et applique)

2. DANS LA THANK YOU PAGE — Section "Outils pour démarrer"
   Placement  : Après le message de remerciement, avant le footer
   Exemple    : "Pour appliquer le Playbook, tu auras besoin de :
                ☐ Un outil email → Je recommande ConvertKit [lien]
                ☐ Un outil de design → Canva Pro [lien]"
   Efficacité : ★★★★☆ (le client vient de payer, il est en mode action)

3. DANS LES EMAILS POST-ACHAT — Email J+3 ou J+5
   Placement  : Email dédié "Les outils que j'utilise" ou P.S. dans un email de contenu
   Exemple    : "P.S. — Tu me demandes souvent quel outil j'utilise pour [X].
                C'est [Outil] — voici mon lien [lien affilié]. Tu as 30 jours
                d'essai gratuit."
   Efficacité : ★★★★☆ (contextualisé, pas agressif)

4. DANS LES ARTICLES DE BLOG — Recommandations contextuelles
   Placement  : Dans un article où l'outil est mentionné naturellement
   Exemple    : "Pour cette étape, tu peux utiliser [Outil] [lien affilié]
                (c'est celui que j'utilise depuis 3 ans)."
   Efficacité : ★★★☆☆ (trafic froid, conversion plus faible)

5. DANS UNE PAGE RESSOURCES DÉDIÉE — /outils ou /ressources
   Placement  : Page standalone listant tous tes outils recommandés
   Exemple    : Page avec 5-10 outils, chacun avec : nom, description, pourquoi
                tu l'utilises, lien affilié, alternative gratuite
   Efficacité : ★★★☆☆ (SEO long-terme, conversion modérée)
```

#### Disclosure légale (OBLIGATOIRE)

```
RÈGLES DE DISCLOSURE
════════════════════

RGPD / FTC / Loi française :
Tu DOIS informer le lecteur que le lien est un lien d'affiliation.

OÙ :
  - À côté du lien (même paragraphe) OU
  - En note de bas de section OU
  - En footer de l'email

FORMULATIONS ACCEPTÉES :
  FR : "Ce lien est un lien affilié. Je touche une commission si tu
       t'inscris via ce lien, sans surcoût pour toi."
  FR court : "(lien affilié)"
  EN : "This is an affiliate link. I earn a commission if you sign up
       through this link, at no extra cost to you."
  EN court : "(affiliate link)"

FORMULATIONS INTERDITES :
  ❌ Aucune mention (illégal)
  ❌ Lien caché derrière un shortener sans disclosure
  ❌ "Je ne gagne rien sur ce lien" (mensonge)
```

#### Tracking des liens affiliés

```
STRUCTURE D'URL RECOMMANDÉE
════════════════════════════

[URL_PARTENAIRE]?ref=[ton_id]&utm_source=[source]&utm_medium=affiliate&utm_campaign=[produit]

Exemples :
  Dans l'ebook    : ?ref=fitpro&utm_source=playbook&utm_medium=affiliate&utm_campaign=outils
  Thank you page  : ?ref=fitpro&utm_source=thankyou&utm_medium=affiliate&utm_campaign=outils
  Email J+3       : ?ref=fitpro&utm_source=email&utm_medium=affiliate&utm_campaign=post-achat
  Blog            : ?ref=fitpro&utm_source=blog&utm_medium=affiliate&utm_campaign=[slug-article]
  Page ressources : ?ref=fitpro&utm_source=ressources&utm_medium=affiliate&utm_campaign=outils

→ Tu sauras EXACTEMENT d'où viennent tes commissions.
```

#### Impact sur le LTV

```
SIMULATION REVENU AFFILIÉ
══════════════════════════

Exemple : Tu recommandes ConvertKit (commission récurrente 30%)

  Prix ConvertKit        : 29€/mois
  Ta commission          : 8.70€/mois par inscription
  Take rate estimé       : 12% de tes acheteurs s'inscrivent
  Durée moyenne client   : 8 mois

  Revenu par client affilié : 8.70€ × 8 mois = 69.60€
  Pondéré par take rate     : 69.60€ × 12% = 8.35€/client

  → +8.35€ de LTV par client, SANS créer de produit.
  → Sur 100 clients/mois = 835€/mois en revenu passif affilié.

AJOUT AU LTV TOTAL :
  LTV sans affiliation  : 91.97€
  LTV avec affiliation  : 100.32€ (+9%)
```

---

## Step 8 — Séquence Email d'Ascension

### Objectif

Faire monter le client dans l'échelle de valeur de manière naturelle, basée sur son engagement et ses résultats.

### Séquence (7 emails, J+7 à J+30)

```
SÉQUENCE D'ASCENSION — [Business Name]
═══════════════════════════════════════

Pré-requis : Le client a acheté [Produit Principal].
             La séquence post-achat (onboarding) est terminée (J+1 à J+5).
             Cette séquence commence à J+7.

EMAIL 1 — J+7 : Check-in + Quick Win
  Sujet A : "Où en es-tu avec [Produit] ?"
  Sujet B : "Ta première semaine — feedback ?"
  Body    : Demander comment ça avance. Partager un tip bonus
            pas dans le playbook. Montrer que tu t'intéresses.
  CTA     : "Réponds à cet email — je lis tout."
  Objectif: Engagement + identifier les clients actifs

EMAIL 2 — J+10 : Témoignage + Preuve
  Sujet A : "[Prénom] a signé [N] clients en [durée] avec [Produit]"
  Sujet B : "Ce que les meilleurs font différemment"
  Body    : Partager un cas client. Montrer le résultat obtenu
            avec [Produit]. Teaser subtil : "il a ensuite rejoint
            [Offre Premium] pour aller encore plus loin."
  CTA     : Lien vers le témoignage complet
  Objectif: Social proof + planter la graine de l'upsell

EMAIL 3 — J+14 : Identifier le plafond
  Sujet A : "Tu as probablement atteint ce point"
  Sujet B : "Le mur que 80% des [audience] rencontrent à ce stade"
  Body    : Nommer le PROCHAIN problème que le client va rencontrer
            (celui que l'upsell résout). Pas de pitch — juste nommer.
            "Si tu en es là, c'est normal. Voici pourquoi."
  CTA     : "Tu te reconnais ? Réponds oui."
  Objectif: Qualification — les "oui" reçoivent l'email 4 en priorité

EMAIL 4 — J+17 : La solution (soft pitch)
  Sujet A : "Comment dépasser le plafond de [problème]"
  Sujet B : "La prochaine étape (si tu es prêt)"
  Body    : Présenter [Offre Premium] comme la suite logique.
            Pas un pitch agressif — une proposition. "J'ai créé
            [Offre] exactement pour ce moment. Voici ce que c'est."
            Détailler 3-4 bénéfices.
  CTA     : Lien vers la page de l'offre ou formulaire d'intérêt
  Objectif: Conversion douce

EMAIL 5 — J+21 : Objections
  Sujet A : "Les 3 raisons de ne PAS rejoindre [Offre Premium]"
  Sujet B : "C'est peut-être pas pour toi (et c'est OK)"
  Body    : Lister honnêtement les 3 raisons de ne PAS acheter.
            Puis pour chaque raison, expliquer pourquoi elle ne
            s'applique peut-être pas à eux. Anti-manipulation :
            honnêteté → confiance → conversion.
  CTA     : Lien vers l'offre + "Si tu as des questions, réponds."
  Objectif: Lever les objections avec honnêteté

EMAIL 6 — J+25 : Urgence (vraie)
  Sujet A : "[Offre] — les inscriptions ferment le [date]"
  Sujet B : "Dernière fenêtre pour [Offre Premium]"
  Body    : Si urgence réelle (places limitées, cohorte, prix early).
            SI PAS d'urgence réelle : ne PAS fabriquer de fausse urgence.
            Plutôt : "Je voulais te rappeler que [Offre] existe.
            Voici ce que les premiers participants ont obtenu."
  CTA     : Lien direct vers l'offre
  Objectif: Conversion avec urgence honnête

EMAIL 7 — J+30 : Fermeture douce + porte ouverte
  Sujet A : "Dernière chose sur [Offre Premium]"
  Sujet B : "On en reparle quand tu es prêt"
  Body    : Pas de pression. "Je ne vais plus t'en parler.
            Si c'est le bon moment, [lien]. Sinon, je continue
            à t'envoyer du contenu gratuit. No hard feelings."
  CTA     : Lien vers l'offre (dernière fois)
  Objectif: Clore avec élégance — pas de spam post-séquence
```

### Tags et automations (ConvertKit / ActiveCampaign)

```
AUTOMATIONS
═══════════

Achat [Produit Principal] :
  → Tag : "buyer-[product-slug]"
  → Déclenche : séquence onboarding (J+1 à J+5)
  → Puis : séquence ascension (J+7 à J+30)

Achat [Order Bump] :
  → Tag : "bump-[product-slug]"
  → Note : client engagé → candidat prioritaire pour upsell

Achat [Upsell] :
  → Tag : "premium-[product-slug]"
  → Retirer de la séquence d'ascension
  → Déclenche : séquence onboarding premium

Refus [Upsell] + Achat [Downsell] :
  → Tag : "downsell-[product-slug]"
  → Continuer la séquence d'ascension (ils peuvent encore upgrader)

Clic "oui je me reconnais" (Email 3) :
  → Tag : "qualified-for-upsell"
  → Envoyer Email 4 immédiatement (pas attendre J+17)

Aucun achat après J+30 :
  → Tag : "nurture-only"
  → Basculer vers la séquence de contenu gratuit (blog, tips)
  → Reproposer l'offre dans 60-90 jours
```

---

## Step 9 — Calcul LTV (Lifetime Value)

### Formule

```
LTV = Prix Produit × 1
    + (Prix Bump × Taux Bump)
    + (Prix Upsell × Taux Upsell)
    + (Prix Downsell × Taux Downsell × Taux Refus Upsell)
    + (Prix Cross-sell × Taux Cross-sell)
    + (Prix High-Ticket × Taux High-Ticket)
```

### Tableau de calcul

```
CALCUL LTV — [Business Name]
═════════════════════════════

                          Prix    Take Rate    Revenu/client
Produit Principal        [X€]      100%         [X€]
Order Bump               [X€]      [30%]        [X€]
Upsell                   [X€]      [15%]        [X€]
Downsell (si refus)      [X€]      [20%×85%]    [X€]
Cross-sell 1             [X€]      [10%]        [X€]
High-Ticket              [X€]      [3%]         [X€]
                         ─────────────────────────────
LTV PAR CLIENT                                  [TOTAL €]

COMPARAISON :
  AVANT (sans upsell) : [prix produit]€ par client
  APRÈS (avec échelle): [LTV]€ par client
  AUGMENTATION        : +[X%]

IMPACT SUR L'ACQUISITION :
  Avant : CPA max = [prix produit × marge]€
  Après : CPA max = [LTV × marge]€
  → Tu peux dépenser [X€] DE PLUS par client en ads
    et rester rentable.
```

### Benchmarks de take rate par niche

| Offre | Take Rate Pessimiste | Réaliste | Optimiste |
|-------|---------------------|----------|-----------|
| Order Bump | 20% | 30% | 45% |
| Upsell 1-click | 8% | 15% | 25% |
| Downsell (parmi les refus) | 10% | 20% | 30% |
| Cross-sell email | 5% | 10% | 15% |
| High-ticket (appel) | 1% | 3% | 5% |
| Membership mensuel | 3% | 7% | 12% |

---

## Step 10 — Plan d'Implémentation

```
PLAN D'IMPLÉMENTATION
══════════════════════

PHASE 1 — Quick Wins (Semaine 1)
  [ ] Créer l'order bump → /dp-lead-magnet-create ou /dp-playbook-create
  [ ] Configurer la checkbox order bump sur [plateforme]
  [ ] Rédiger la copy de la checkbox (3 bullet points)
  Impact estimé : +[X€]/mois

PHASE 2 — Upsell (Semaine 2)
  [ ] Créer le produit upsell (ou identifier un existant)
  [ ] Créer la page upsell → /dp-landing-page
  [ ] Configurer le redirect post-achat vers la page upsell
  [ ] Créer le downsell (version allégée)
  Impact estimé : +[X€]/mois

PHASE 3 — Séquence d'ascension (Semaine 3)
  [ ] Rédiger les 7 emails → /dp-email-sequence
  [ ] Configurer les automations et tags dans [outil email]
  [ ] Tester le flow complet (achat → bump → upsell → emails)
  Impact estimé : +[X€]/mois

PHASE 4 — High-Ticket (Mois 2)
  [ ] Définir l'offre high-ticket (coaching, programme, done-for-you)
  [ ] Créer la page de candidature → /dp-landing-page
  [ ] Ajouter le pitch dans l'email 4 de la séquence
  Impact estimé : +[X€]/mois

TIMELINE :
  Semaine 1 : Order bump live → panier moyen +[X%]
  Semaine 2 : Upsell live → LTV +[X%]
  Semaine 3 : Séquence email active → ascension automatique
  Mois 2    : High-ticket → les meilleurs clients montent
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | L'échelle de valeur est une PROGRESSION LOGIQUE (chaque niveau est la suite du précédent) | Critical |
| QG-02 | Le prix monte PAR PALIERS (×2 à ×5, jamais ×20 entre deux niveaux) | Critical |
| QG-03 | L'order bump coûte 30-60% du produit principal (pas plus, pas moins) | High |
| QG-04 | L'upsell est proposé au BON MOMENT (post-achat immédiat ou J+7, pas J+0 et J+1 et J+2) | Critical |
| QG-05 | Le downsell est proposé SEULEMENT si l'upsell est refusé (pas en parallèle) | Critical |
| QG-06 | JAMAIS de fausse urgence — si la deadline n'est pas réelle, ne pas en inventer | Critical |
| QG-07 | Chaque offre a une copy COMPLÈTE (pas de placeholders [titre], [prix]) | Critical |
| QG-08 | Le calcul LTV utilise des take rates RÉALISTES (pas 50% upsell) | High |
| QG-09 | La séquence email d'ascension ne commence PAS avant J+7 (laisser le temps d'utiliser le produit) | High |
| QG-10 | L'email 7 FERME la séquence avec respect — pas de harcèlement post-séquence | Critical |
| QG-11 | Les automations et tags sont documentés (pas juste "envoyer un email") | High |
| QG-12 | Le plan d'implémentation est réaliste par rapport au temps disponible (Q9) | High |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| L'utilisateur n'a qu'un seul produit | Concevoir l'échelle complète en partant de ce produit. Les niveaux manquants = produits à CRÉER. |
| L'utilisateur n'a aucune vente | Prioriser le produit principal d'abord. L'upsell vient APRÈS les premières ventes (au moins 10). |
| La plateforme ne supporte pas le 1-click upsell | Alternative : email J+1 avec offre spéciale 24h. Proposer Systeme.io ou ThriveCart comme alternative. |
| L'utilisateur ne veut pas faire de coaching/high-ticket | Ne pas forcer. L'échelle peut s'arrêter au Niveau 3. Proposer un membership ou des produits digitaux avancés. |
| Le produit principal est gratuit (lead magnet) | L'échelle commence au Niveau 0. Le "produit principal" est le premier produit PAYANT. Adapter l'échelle en décalant tout d'un niveau. |
| L'utilisateur a déjà un upsell qui ne convertit pas | Diagnostiquer : mauvais timing ? mauvais prix ? mauvaise copy ? mauvais produit ? Proposer des corrections spécifiques. |
| business-profile.md absent | Poser les questions du Bloc 1 pour comprendre le contexte minimum. |
| Take rate inférieur aux benchmarks | C'est normal au début. Proposer des A/B tests sur la copy et le timing avant de changer l'offre. |

---

## Cross-Skill Integration

| Avant | Skill | Quand |
|-------|-------|-------|
| Produit principal créé | `/dp-playbook-create` | Le produit doit exister |
| Profil business | `/dp-business-profile` | Pour le contexte complet |
| Validation marché | `/dp-market-research` | Pour valider le pricing et le positionnement |
| Funnel existant | `/dp-sales-funnel` | Pour s'intégrer dans le funnel actuel |

| Après | Skill | Quand |
|-------|-------|-------|
| Créer l'order bump | `/dp-lead-magnet-create` ou `/dp-playbook-create` | Si le bump est un nouveau produit |
| Page upsell | `/dp-landing-page` | Pour la page de l'offre premium |
| Séquence email | `/dp-email-sequence` | Pour la séquence d'ascension |
| Couverture produit upsell | `/dp-ebook-cover` | Si l'upsell est un ebook/guide |
| Ads pour le funnel | `/dp-ad-angles-meta` | Pour l'acquisition vers l'échelle complète |
