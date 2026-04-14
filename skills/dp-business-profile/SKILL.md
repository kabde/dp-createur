---
name: dp-business-profile
description: "Crée ou met à jour le fichier business-profile.md — la configuration centrale lue par tous les skills DP Créateur. Pose des questions guidées sur l'identité, les produits, l'audience, la voix de marque, l'identité visuelle et le stack technique. Ce fichier élimine les questions répétitives dans les autres skills. Triggers: profil, business profile, configurer, setup, identité, marque, brand, couleurs, config."
user-invokable: true
argument-hint: "[update] — sans argument = création guidée, avec 'update' = mise à jour partielle"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: fondation
  updated: 2026-04-13
---

# Business Profile — Configuration Centrale

<!-- v2.0.0 | 2026-04-13 | Création : profil business lu par tous les skills -->

Ce skill crée le fichier `business-profile.md` à la racine du projet. Ce fichier est **la source de vérité** pour tous les autres skills DP Créateur — il élimine les questions répétitives et assure la cohérence entre tous les outputs (ebooks, landing pages, emails, ads, social…).

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-business-profile` | Création guidée complète (première fois) |
| `/dp-business-profile update` | Mettre à jour des sections spécifiques |
| `/dp-business-profile show` | Afficher le profil actuel |
| `/dp-business-profile express` | Création rapide — 5 questions essentielles puis génération |
| `/dp-business-profile check` | Vérifier la complétude et signaler les manques |

## Output Format

```
LIVRABLE :
├── business-profile.md (à la racine du projet)
│   ├── Identité business
│   ├── Produits / Services
│   ├── Audience cible
│   ├── Voix & Ton
│   ├── Identité visuelle (couleurs, typo, style)
│   ├── Stack technique
│   └── Liens & URLs
└── Lu automatiquement par tous les skills DP Créateur
```

---

## Process

```
1. Vérifier si un profil existe déjà
2. Context intake     → Questions guidées par blocs
3. Synthèse           → Fiche récap pour validation
4. Écrire le fichier  → business-profile.md
5. Confirmation       → Résumé + prochaines étapes
```

---

## Step 0 — Vérifier l'existant

```
SI business-profile.md existe à la racine :
  → Lire le contenu
  → SI commande "update" : demander quelle section modifier
  → SI commande "show" : afficher le profil et s'arrêter
  → SI commande "check" : vérifier la complétude et lister les champs vides
  → SINON : prévenir "Un profil existe déjà. Tu veux le mettre à jour ou repartir de zéro ?"

SINON :
  → Lancer la création guidée
```

---

## Step 1 — Context Intake (Création Guidée)

**Règle** : Poser les questions par blocs de 3-4. Attendre les réponses. Reformuler pour valider. Puis continuer.

### Bloc 1 — Identité

| # | Question | Champ | Obligatoire |
|---|----------|-------|-------------|
| Q1 | Comment s'appelle ton business / ta marque ? | `brand_name` | Oui |
| Q2 | Qui es-tu ? Ton nom et ton rôle. (ex: "Marie, coach fitness" ou "Alex & Sarah, co-fondateurs") | `founder` | Oui |
| Q3 | Décris ton business en 1-2 phrases. Qu'est-ce que tu fais et pour qui ? | `description` | Oui |
| Q4 | Quelle est ta niche / ton secteur ? (ex: coaching, fitness, marketing digital, développement perso, finance…) | `niche` | Oui |

### Bloc 2 — Produits & Services

| # | Question | Champ | Obligatoire |
|---|----------|-------|-------------|
| Q5 | Quel est ton **produit principal** ? (nom, type, prix) | `main_product` | Oui |
| Q6 | Tu as une **URL de vente** ? (Gumroad, Stripe, LemonSqueezy, site web…) | `checkout_url` | Non |
| Q7 | Tu as d'**autres produits** ou services ? Liste-les avec leurs prix. | `other_products` | Non |
| Q8 | Tu as un **lead magnet** (ressource gratuite) ? Si oui, lequel ? | `lead_magnet` | Non |

### Bloc 3 — Audience

| # | Question | Champ | Obligatoire |
|---|----------|-------|-------------|
| Q9 | Décris ton **client idéal** en 2-3 phrases. (métier, situation, niveau, âge approximatif) | `target_audience` | Oui |
| Q10 | Quel est le **problème n°1** de ton audience ? Le truc qui les frustre. | `pain_point` | Oui |
| Q11 | Quel **résultat concret** tu leur promets ? (mesurable si possible) | `promise` | Oui |
| Q12 | Quelles sont les **objections fréquentes** de tes prospects ? (prix, temps, doute…) | `objections` | Non |

### Bloc 4 — Voix & Ton

| # | Question | Champ | Obligatoire |
|---|----------|-------|-------------|
| Q13 | Comment tu décrirais ta **voix de marque** en 3 mots ? (ex: "direct, expert, honnête" ou "chaleureux, pédagogue, inspirant") | `voice` | Oui |
| Q14 | Tu **tutoies** ou tu **vouvoies** ton audience ? | `address` | Oui |
| Q15 | Y a-t-il des **mots ou expressions** que tu utilises souvent ? Ou que tu veux éviter ? | `vocabulary` | Non |

> Si l'utilisateur ne sait pas, proposer des exemples :
> - "Direct et sans bullshit" (style DP Créateur classique)
> - "Chaleureux et pédagogue" (style prof/mentor bienveillant)
> - "Data-driven et professionnel" (style B2B/corporate)
> - "Fun et décalé" (style jeune audience, réseaux sociaux)

### Bloc 5 — Identité Visuelle

| # | Question | Champ | Obligatoire |
|---|----------|-------|-------------|
| Q16 | Tu as une **couleur principale** de marque ? (donne le hex, ex: #1e3a5f, ou le nom, ex: "bleu marine") | `primary_color` | Oui |
| Q17 | Tu as une **couleur d'accent** ? (pour les boutons, highlights, CTAs) | `accent_color` | Oui |
| Q18 | Quel **style visuel** tu préfères ? `minimaliste` / `bold` / `premium` / `warm` | `visual_style` | Oui |
| Q19 | Tu as une **police de marque** ? (ex: Inter, Montserrat, Playfair Display…) Si non, je garderai Inter. | `font` | Non |
| Q20 | Tu as un **logo** ? (URL ou description) | `logo` | Non |

> **Si l'utilisateur n'a pas de couleurs** :
> Proposer 3 palettes adaptées à sa niche :
>
> | Palette | Primaire | Accent | Style | Idéal pour |
> |---------|----------|--------|-------|------------|
> | Confiance | `#1e3a5f` Bleu nuit | `#d4a853` Or | Premium | Coaching, finance, consulting |
> | Énergie | `#1e1e1e` Noir | `#f43f5e` Rose vif | Bold | Fitness, marketing, jeune audience |
> | Nature | `#2d4a3e` Vert forêt | `#22c55e` Vert vif | Minimaliste | Bien-être, développement perso, écologie |
>
> "Laquelle te parle ? Ou donne-moi tes propres couleurs."

### Bloc 6 — Stack Technique & Liens

| # | Question | Champ | Obligatoire |
|---|----------|-------|-------------|
| Q21 | Quelle **plateforme de vente** tu utilises ? (Gumroad, Stripe, LemonSqueezy, Stan Store, Shopify…) | `payment_platform` | Non |
| Q22 | Quel **outil email** ? (ConvertKit, Mailchimp, Brevo, MailerLite…) | `email_tool` | Non |
| Q23 | Tu as un **site WordPress** ? Si oui, quelle URL ? | `wordpress_url` | Non |
| Q24 | Tes **réseaux sociaux** ? (Instagram, LinkedIn, TikTok, YouTube, X — donne les handles) | `social_handles` | Non |
| Q25 | Tu fais de la **pub payante** ? Si oui, quelles plateformes ? (Meta Ads, Google Ads, TikTok Ads…) | `ad_platforms` | Non |

---

## Step 2 — Synthèse pour Validation

```
╔═══════════════════════════════════════════════════╗
║           BUSINESS PROFILE — SYNTHÈSE             ║
╠═══════════════════════════════════════════════════╣
║ IDENTITÉ                                          ║
║  Marque      : [brand_name]                       ║
║  Fondateur   : [founder]                          ║
║  Description : [description]                      ║
║  Niche       : [niche]                            ║
╠═══════════════════════════════════════════════════╣
║ PRODUIT PRINCIPAL                                 ║
║  Nom         : [main_product.name]                ║
║  Type        : [main_product.type]                ║
║  Prix        : [main_product.price]               ║
║  URL         : [checkout_url]                     ║
╠═══════════════════════════════════════════════════╣
║ AUDIENCE                                          ║
║  Cible       : [target_audience]                  ║
║  Problème    : [pain_point]                       ║
║  Promesse    : [promise]                          ║
╠═══════════════════════════════════════════════════╣
║ VOIX                                              ║
║  Style       : [voice]                            ║
║  Adresse     : [tu/vous]                          ║
╠═══════════════════════════════════════════════════╣
║ IDENTITÉ VISUELLE                                 ║
║  Primaire    : [primary_color] ████               ║
║  Accent      : [accent_color]  ████               ║
║  Style       : [visual_style]                     ║
║  Font        : [font]                             ║
╠═══════════════════════════════════════════════════╣
║ STACK                                             ║
║  Vente       : [payment_platform]                 ║
║  Email       : [email_tool]                       ║
║  WordPress   : [wordpress_url]                    ║
║  Pub         : [ad_platforms]                     ║
╚═══════════════════════════════════════════════════╝
```

**Demander** : "C'est bon pour toi ? Tu veux modifier quelque chose avant que je crée le fichier ?"

**Hard gate** : Ne PAS créer le fichier sans validation.

---

## Step 3 — Écrire business-profile.md

Créer le fichier à la racine du projet : `/Users/abder/Documents/dp-skills/business-profile.md`

### Format du fichier

```markdown
# Business Profile — [brand_name]

> Ce fichier est lu automatiquement par tous les skills DP Créateur.
> Dernière mise à jour : [date]

## Identité

- **Marque** : [brand_name]
- **Fondateur(s)** : [founder]
- **Description** : [description]
- **Niche** : [niche]
- **Localisation** : [location si mentionné]

## Produit Principal

- **Nom** : [main_product.name]
- **Type** : [ebook / playbook / guide / cours / service / coaching]
- **Prix** : [main_product.price]
- **URL de vente** : [checkout_url]
- **Proposition de valeur** : [1 phrase — le bénéfice principal]

## Autres Produits

| Nom | Type | Prix | URL |
|-----|------|------|-----|
| [nom] | [type] | [prix] | [url] |
| ... | ... | ... | ... |

## Lead Magnet

- **Nom** : [lead_magnet.name]
- **Type** : [checklist / cheat-sheet / mini-guide / template]
- **URL** : [lead_magnet.url]

## Audience Cible

- **Qui** : [target_audience]
- **Problème n°1** : [pain_point]
- **Résultat promis** : [promise]
- **Objections fréquentes** : [objections]

## Voix & Ton

- **Style** : [voice — 3 mots]
- **Adresse** : [tu / vous]
- **Mots à utiliser** : [vocabulary.use]
- **Mots à éviter** : [vocabulary.avoid]
- **Exemples de ton** :
  - ✅ "[exemple de phrase dans le bon ton]"
  - ❌ "[exemple de phrase à éviter]"

## Identité Visuelle

- **Couleur primaire** : [primary_color] — [nom de la couleur]
- **Couleur accent** : [accent_color] — [nom de la couleur]
- **Style visuel** : [minimaliste / bold / premium / warm]
- **Police** : [font]
- **Logo** : [logo_url ou description]

### Palette CSS

```css
:root {
  --color-primary: [primary_color];
  --color-accent: [accent_color];
  --color-accent-light: [accent à 5% opacité];
  --color-accent-dark: [accent assombri 40%];
  --color-accent-mid: [accent assombri 20%];
}
```

## Stack Technique

| Outil | Usage | URL |
|-------|-------|-----|
| [payment_platform] | Paiements | [url] |
| [email_tool] | Email marketing | [url] |
| WordPress | Site / Blog | [wordpress_url] |
| [ad_platform_1] | Publicité | — |
| [ad_platform_2] | Publicité | — |

## Réseaux Sociaux

| Plateforme | Handle | URL |
|------------|--------|-----|
| Instagram | @[handle] | https://instagram.com/[handle] |
| LinkedIn | [handle] | [url] |
| TikTok | @[handle] | https://tiktok.com/@[handle] |
| YouTube | [handle] | [url] |
| X/Twitter | @[handle] | https://x.com/[handle] |
```

---

## Step 4 — Confirmation

```
✅ BUSINESS PROFILE CRÉÉ

📄 Fichier : business-profile.md (racine du projet)
📊 Champs remplis : [N]/25
⚠️ Champs manquants : [liste si applicable]

CE QUI CHANGE MAINTENANT :
  Tous les skills DP Créateur vont lire ce fichier automatiquement.
  Plus besoin de répondre aux questions sur ton business, tes couleurs,
  ton audience ou ton ton à chaque fois. Tout est centralisé ici.

PROCHAINES ÉTAPES :
  → /dp-playbook-create    — Créer ton premier ebook
  → /dp-landing-page       — Créer ta page de vente
  → /dp-blog-strategy      — Planifier ta stratégie de contenu
  → /dp-email-sequence     — Créer tes séquences email
  → /dp-business-profile update — Modifier ton profil plus tard
```

---

## Mode Update

Quand l'utilisateur lance `/dp-business-profile update` :

1. Lire le fichier existant
2. Demander : "Quelle section tu veux modifier ?" avec la liste :
   - Identité
   - Produits
   - Audience
   - Voix & Ton
   - Identité visuelle
   - Stack technique
   - Réseaux sociaux
3. Poser les questions uniquement pour la section choisie
4. Mettre à jour le fichier sans toucher aux autres sections
5. Confirmer les changements

---

## Mode Check

Quand l'utilisateur lance `/dp-business-profile check` :

1. Lire le fichier existant
2. Vérifier chaque champ :
   - ✅ Rempli
   - ⚠️ Vide ou placeholder
   - ❌ Manquant
3. Afficher le rapport de complétude
4. Recommander les champs à remplir en priorité

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | brand_name rempli | Critical |
| QG-02 | Au moins 1 produit défini | Critical |
| QG-03 | target_audience rempli | Critical |
| QG-04 | voice définie (3 mots) | High |
| QG-05 | primary_color et accent_color définis | High |
| QG-06 | Couleurs au format hex valide (#XXXXXX) | High |
| QG-07 | Pas de placeholder dans le fichier final | Critical |
| QG-08 | Validation explicite avant écriture | Critical |
| QG-09 | Le fichier est à la racine du projet (pas dans skills/) | Critical |
| QG-10 | Le fichier inclut la date de dernière mise à jour | Medium |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| L'utilisateur ne connaît pas ses couleurs | Proposer 3 palettes adaptées à la niche |
| L'utilisateur n'a pas encore de produit | Remplir les autres sections, laisser produit en "à définir" |
| Le fichier existe déjà | Proposer update ou remplacement |
| L'utilisateur veut aller vite | Mode express : 5 questions essentielles seulement — Q1 (Nom du business), Q5 (Produit principal : nom + prix), Q9 (Client idéal en 1 phrase), Q13 (Voix en 3 mots), Q16 (Couleur principale + accent). Générer le profil avec ces réponses et laisser le reste en "à compléter". |
| Niche difficile à identifier | Poser "Qu'est-ce que tu vends et à qui ?" et déduire |
| Couleur hex invalide | Convertir le nom en hex ("bleu marine" → #1e3a5f) |
| L'utilisateur n'a pas de site/blog | Laisser les champs WordPress et URL vides |
