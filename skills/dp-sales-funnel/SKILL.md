---
name: dp-sales-funnel
description: "Architecte de funnel de vente complet, du trafic à l'upsell. Définit chaque étape avec outils, KPIs, besoins en contenu, automations, et math de conversion. Intègre l'identité visuelle pour cohérence avec les landing pages. Adaptable à tout budget et stack technique. Triggers : funnel, tunnel de vente, sales funnel, stratégie de vente, parcours client, conversion, entonnoir."
user-invokable: true
argument-hint: "[produit] [type: simple|ladder|webinar|challenge] [budget]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: marketing
  updated: 2026-04-13
---

# Sales Funnel — Funnel Architect

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake avec brand identity, quality gates, error handling, cross-skill integration -->

Stratège funnel de vente pour DP Créateur. Conçoit des funnels complets et praticables avec chaque étape définie : source de trafic, landing page, capture de leads, nurturing, vente, et post-achat.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-sales-funnel [produit]` | Lancer la conception complète du funnel |
| `/dp-sales-funnel simple [produit]` | Funnel simple (1 produit, trafic → vente) |
| `/dp-sales-funnel ladder [produit]` | Funnel échelle (lead magnet → produit → upsell) |
| `/dp-sales-funnel webinar [produit]` | Funnel webinaire (inscription → live → offre) |
| `/dp-sales-funnel challenge [produit]` | Funnel challenge (inscription → X jours → offre) |

## Output Format

```
LIVRABLE :
├── Architecture du funnel (schéma visuel)
│   └── [TRAFFIC] → [CAPTURE] → [NURTURE] → [VENTE] → [DELIVERY] → [UPSELL]
├── Détail de chaque étape
│   ├── Outils recommandés + coûts
│   ├── KPIs cibles
│   ├── Contenu à créer
│   └── Automations
├── Math de conversion (budget → ROI)
├── Identité visuelle pour cohérence cross-pages
├── Checklist de contenu à créer
└── Fichier : funnel/dp-sales-funnel-[slug].md
```

---

## Process

```
1. Context intake      → Collecter produit, budget, outils, objectifs, brand (OBLIGATOIRE)
2. Read references     → Charger business-profile.md + contenu produit si dispo
   Read references/funnel-example.md → pour un funnel complet avec math
3. Design funnel       → Architecture 6 étapes
4. Define tech stack   → Outils + coûts + intégrations
5. Conversion math     → Budget → Clicks → Leads → Ventes → ROI
6. Automation map      → Triggers → Actions → Outils
7. Content checklist   → Tout ce qu'il faut créer
8. Brand integration   → Couleurs et identité pour les pages du funnel
9. Quality check       → Cohérence, réalisme, complétude
10. Deliver            → Fichier markdown + résumé
```

---

## Step 1 — Context Intake (Required: Always Do This First)

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom, niche, produit(s), audience, ton, prix, couleurs, outils
  → Ne PAS reposer les questions déjà couvertes par le profil

SINON :
  → Continuer sans. Les questions de l'intake couvriront le minimum.
```

### 1b. Poser les questions par blocs

**Règle absolue** : Ne JAMAIS poser toutes les questions d'un coup. Grouper par 2-3, attendre les réponses, puis continuer.

#### Bloc 1 — Le produit et l'objectif

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Quel produit ou service veux-tu vendre ? Nom + prix + description courte. | Centre du funnel |
| Q2 | Quel est ton objectif ? (revenu mensuel cible, nombre de ventes, ou nombre de leads) | Dimensionner le funnel |
| Q3 | Quel type de funnel ? `simple` (trafic → vente) / `ladder` (lead magnet → produit → upsell) / `webinar` / `challenge` | Architecture globale |

**Après les réponses** : Reformuler. "Tu veux un funnel [type] pour vendre [produit] à [prix] avec un objectif de [X]. Correct ?"

#### Bloc 2 — Le budget et les outils

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | Quel est ton budget marketing mensuel ? (pub + outils) | Dimensionner les recommandations |
| Q5 | Quels outils utilises-tu déjà ? (email, paiement, site, CRM, pub) | Éviter de recommander des outils inutiles |
| Q6 | Tu es seul(e) ou en équipe ? Combien de temps par semaine pour le marketing ? | Réalisme des recommandations |

**Après les réponses** : Synthèse contraintes.

#### Bloc 3 — L'audience et la marque

| # | Question | Pourquoi |
|---|----------|----------|
| Q7 | Qui est ton client idéal ? (métier, situation, frustration principale) | Contenu et ciblage |
| Q8 | Tu as déjà une identité visuelle ? Couleur primaire (hex) + couleur accent (hex) + style (`minimaliste` / `bold` / `premium` / `warm`) | Cohérence des landing pages dans le funnel |
| Q9 | Tu as un lead magnet existant ou il faut en créer un ? | Étape capture du funnel |

> **Si pas de couleurs** : Proposer 3 palettes adaptées à la niche.
> Ces couleurs seront transmises à `/dp-landing-page` pour chaque page du funnel.

**Après les réponses** : Synthèse audience + brand.

#### Bloc 4 — Le contexte existant

| # | Question | Pourquoi |
|---|----------|----------|
| Q10 | Tu as déjà une liste email ? Si oui, combien d'abonnés ? | Stratégie nurturing |
| Q11 | Tu as déjà du trafic organique ? (réseaux sociaux, blog, YouTube) | Sources de trafic |
| Q12 | Qu'est-ce que tu as déjà essayé pour vendre ? Qu'est-ce qui a marché / pas marché ? | Éviter les erreurs passées |

**Après les réponses** : Passer à la conception.

---

## Step 2 — Funnel Architecture

Mapper chaque étape du parcours client :

```
ARCHITECTURE DU FUNNEL
======================

[TRAFFIC] → [CAPTURE] → [NURTURE] → [VENTE] → [DELIVERY] → [UPSELL/REFERRAL]

Étape 1 : TRAFFIC
  Sources :
    - Organique : [réseaux sociaux, SEO blog, YouTube, podcast]
    - Payant : [Meta Ads, Google Ads]
    - Référence : [affiliés, bouche-à-oreille, partenariats]
  Contenu nécessaire : [liste]
  KPIs : [reach, CTR, CPC, volume de trafic]
  Budget alloué : [% du total]

Étape 2 : CAPTURE DE LEADS
  Lead Magnet : [ressource gratuite — décrire précisément]
  Landing Page : [description + éléments clés]
    → Couleurs : var(--primary) = [hex], var(--accent) = [hex]
    → Style : [minimaliste/bold/premium/warm]
  Outil formulaire : [Tally, ConvertKit, etc.]
  KPIs : [taux d'opt-in cible : 25-40%]

Étape 3 : NURTURE
  Séquence email : [séquence de bienvenue — X emails sur Y jours]
  Progression : [valeur → preuve → pitch]
  Outil : [ConvertKit, Mailchimp, etc.]
  KPIs : [taux d'ouverture >35%, taux de clic >3%]

Étape 4 : VENTE
  Page de vente : [landing page pour le produit]
    → Mêmes couleurs et style que la page capture
  Checkout : [Stripe, Gumroad, LemonSqueezy]
  Options de paiement : [unique, échelonné si applicable]
  KPIs : [taux de conversion cible : 2-5% depuis email, 1-3% depuis trafic froid]

Étape 5 : DELIVERY
  Accès : [téléchargement immédiat / envoi email]
  Onboarding : [séquence post-achat — X emails]
  Support : [email, communauté, aucun]
  KPIs : [taux de complétion, tickets support]

Étape 6 : UPSELL / RÉTENTION
  Options d'upsell :
    - [Accompagnement individuel]
    - [Produit avancé / module 2]
    - [Communauté / membership]
  Programme de référence : [structure si applicable]
  KPIs : [taux d'upsell, LTV, taux de référence]
```

---

## Step 3 — Tech Stack

```
STACK TECHNIQUE RECOMMANDÉ
===========================
| Étape | Outil | Coût | Pourquoi |
|-------|-------|------|----------|
| Landing pages | [outil] | [X]€/mois | [raison] |
| Email | [outil] | [X]€/mois | [raison] |
| Checkout | [outil] | [X]€/mois + frais | [raison] |
| Analytics | [outil] | gratuit | [raison] |
| CRM | [outil] | [X]€/mois | [raison] |
| Pub | [plateforme] | budget variable | [raison] |

Coût fixe total : [X]€/mois (hors budget pub)
```

Priorités :
- Outils gratuits ou peu chers (adapté à une petite structure)
- Outils qui s'intègrent entre eux
- Simplicité > fonctionnalités

---

## Step 4 — Conversion Math

```
MATH DE CONVERSION
==================
Budget mensuel : [X]€
Coût par clic (estimé) : [X]€
Clics : [budget / CPC]
Taux d'opt-in : [X]%
Leads : [clics × taux opt-in]
Taux email → vente : [X]%
Ventes depuis email : [leads × taux conversion]
Taux vente directe : [X]%
Ventes directes : [clics × (1 - opt-in) × taux direct]
Total ventes : [somme]
Revenu : [ventes × prix]
ROI : [(revenu - budget) / budget × 100]%

Seuil de rentabilité : [X]€ de pub par vente
CPA cible : [X]€ (pour profitabilité)
```

---

## Step 5 — Automation Map

```
AUTOMATIONS
============
Trigger → Action → Outil

1. Nouvel opt-in → Envoyer email de bienvenue #1 → [outil email]
2. Email ouvert → Attendre 1 jour → Envoyer email #2 → [outil email]
3. Lien cliqué dans email → Taguer "intéressé" → [outil email]
4. Achat complété → Envoyer email de livraison → [webhook checkout + email]
5. Achat complété → Démarrer séquence post-achat → [outil email]
6. Jour 14 post-achat → Envoyer email d'upsell → [outil email]
7. Pas d'achat après 7 jours → Envoyer email de relance → [outil email]
8. Page de vente visitée sans achat → Retargeter avec Meta Ad → [Meta pixel]
```

---

## Step 6 — Content Checklist

```
CONTENU À CRÉER
================
[ ] Lead magnet (PDF/vidéo/template)          → /lead-magnet-create
[ ] Landing page capture d'emails              → /dp-landing-page lead-magnet
[ ] Séquence email de bienvenue (X emails)     → /dp-email-sequence
[ ] Page de vente du produit                   → /dp-landing-page
[ ] Séquence email post-achat (X emails)       → /dp-email-sequence
[ ] Page d'upsell (si applicable)              → /dp-landing-page
[ ] 4 semaines de contenu social               → /dp-mediaplan
[ ] 4-6 articles de blog                       → /dp-blog-article
[ ] Angles pubs Meta — 12 angles               → /dp-ad-angles-meta
[ ] Copies Google Ads                          → /dp-ad-angles-google
```

---

## Step 7 — Brand Integration

Pour assurer la cohérence visuelle à travers tout le funnel :

```
IDENTITÉ VISUELLE DU FUNNEL
============================
Couleur primaire : [hex] → CSS var(--primary)
Couleur accent : [hex] → CSS var(--accent)
Style visuel : [minimaliste/bold/premium/warm]
Nom de marque : [nom]
Logo : [oui/non — emplacement]

PAGES DU FUNNEL — TOUTES UTILISENT CES COULEURS :
  1. Landing page lead magnet    → /dp-landing-page (passer couleurs)
  2. Page de vente produit       → /dp-landing-page (passer couleurs)
  3. Page d'upsell               → /dp-landing-page (passer couleurs)
  4. Emails                      → /dp-email-sequence (header couleur primaire)
  5. Pubs Meta                   → /dp-ad-angles-meta (direction créative cohérente)
```

---

## Funnel Design Principles

1. **Simple gagne** — Moins d'étapes = plus de conversions. Ne pas sur-ingénierer.
2. **Lead magnet irrésistible** — Résout UN problème spécifique. Rapide à consommer (<10 min).
3. **L'email fait la vente** — Ne pas espérer que le trafic froid achète à la première visite.
4. **Un seul CTA par page** — Ne jamais donner 3 choix au visiteur. Une seule action.
5. **Vitesse vers la valeur** — Plus vite quelqu'un obtient un résultat (même petit), plus vite il achète plus.
6. **Retargeter tout** — Pixeler la landing page, la page de vente, la page de remerciement.
7. **Mesurer à chaque étape** — Si tu ne peux pas le mesurer, tu ne peux pas l'optimiser.

---

### Seuils de budget

| Budget mensuel | Stratégie recommandée | Répartition |
|----------------|----------------------|-------------|
| 0€ (gratuit) | 100% organique | Blog SEO + social + email |
| < 500€/mois | Organique prioritaire + tests paid | 70% organique / 30% paid (test 1 plateforme) |
| 500-2000€/mois | Mixte | 40% organique / 60% paid (2 plateformes) |
| > 2000€/mois | Paid-first | 20% organique / 80% paid (scaling) |

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Aucun placeholder [TODO], [INSERT] | Critical |
| QG-02 | Chaque étape du funnel a des KPIs cibles chiffrés | Critical |
| QG-03 | La math de conversion est réaliste : Landing page conversion: 15-40%. Email opt-in: 20-50%. Sales from email: 2-10%. Aucun taux > 50% pour une étape payante. | Critical |
| QG-04 | Le budget recommandé est adapté aux contraintes de l'utilisateur (voir seuils de budget ci-dessous) | High |
| QG-05 | Chaque outil recommandé est nommé avec un coût estimé | High |
| QG-06 | Le funnel fonctionne avec 0€ de pub (organique seul) ET est scalable avec du payant | Critical |
| QG-07 | Les couleurs de marque sont définies et transmises à toutes les pages | High |
| QG-08 | La checklist de contenu référence les skills DP Créateur appropriés | High |
| QG-09 | Les automations sont réalistes avec les outils recommandés | High |
| QG-10 | Aucune promesse de revenus garantis | Critical |
| QG-11 | Recommandations adaptées à la taille de l'équipe | High |
| QG-12 | Le seuil de rentabilité (break-even) est calculé et affiché | Critical |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Pas de produit défini | Demander : "Quel produit veux-tu vendre ?" — ne pas continuer sans |
| Budget très faible (<100€/mois) | Recommander funnel 100% organique. Pas de pub payante. Focus contenu + email |
| Pas d'outils existants | Recommander un stack minimaliste : Tally (formulaires) + ConvertKit gratuit + Stripe/Gumroad |
| Pas de lead magnet | Proposer 3 idées de lead magnet adaptées à la niche. Référencer `/lead-magnet-create` |
| Produit pas encore créé | Recommander de créer le produit d'abord. Référencer `/dp-playbook-create` |
| business-profile.md absent | Continuer avec les réponses du context intake uniquement |
| Funnel trop complexe pour l'utilisateur | Simplifier : recommander le type "simple" d'abord. Évoluer vers "ladder" après les premières ventes |
| Pas de trafic existant | Recommander 30 jours de contenu organique avant d'investir en pub payante |
| Pas de couleurs de marque | Proposer 3 palettes, faire choisir, et les propager à toutes les pages du funnel |

---

## Cross-Skill Integration

| Avant sales-funnel | Skill précédent | Quand |
|--------------------|-----------------|-------|
| Produit créé | `/dp-playbook-create` | Si le produit est un ebook/playbook |
| Profil business | `business-profile.md` | Pour les infos de base et couleurs |

| Après sales-funnel | Skill suivant | Quand |
|--------------------|---------------|-------|
| Créer le lead magnet | `/lead-magnet-create` | Si pas de lead magnet existant |
| Page capture emails | `/dp-landing-page lead-magnet` | Première page du funnel |
| Page de vente | `/dp-landing-page` | Page centrale du funnel |
| Séquence email | `/dp-email-sequence` | Nurturing + post-achat |
| Pubs Meta | `/dp-ad-angles-meta` | Source de trafic payant |
| Pubs Google | `/dp-ad-angles-google` | Source de trafic payant complémentaire |
| Plan de contenu | `/dp-mediaplan` | Source de trafic organique |
| Articles blog | `/dp-blog-article` | SEO + trafic organique |
