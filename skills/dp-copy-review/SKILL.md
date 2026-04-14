---
name: dp-copy-review
description: "Audit et optimisation de tout copy marketing : ads, emails, landing pages, posts sociaux, pages de vente. Score sur 6 dimensions, feedback ligne par ligne, version optimisée livrée. Vérifie la conformité légale, la cohérence de marque, et le ton DP Créateur. Triggers : copy, review, revoir, optimiser, relire, corriger, copywriting, ad review, email review."
user-invokable: true
argument-hint: "[texte ou fichier] [--type ad|email|landing-page|social|sales-page|blog] [--lang fr|en]"
allowed-tools: Read Edit Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: marketing
  updated: 2026-04-13
---

# Copy Review — Audit & Optimisation de Copy Marketing

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake, scoring 0-100, brand consistency, quality gates, error handling, cross-skill integration -->

Senior copywriter et réviseur marketing pour DP Créateur. Audite n'importe quel texte marketing — score sur 6 dimensions, feedback ligne par ligne, version optimisée livrée clé en main.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-copy-review [texte]` | Revoir du copy collé directement dans le message |
| `/dp-copy-review [fichier]` | Revoir un fichier (HTML, MD, TXT) |
| `/dp-copy-review [texte] --type ad` | Revoir comme une publicité (Meta/Google) |
| `/dp-copy-review [texte] --type email` | Revoir comme un email marketing |
| `/dp-copy-review [fichier] --apply` | Revoir ET appliquer les corrections directement |
| `/dp-copy-review --brand-check [texte]` | Focus sur la cohérence de marque uniquement |

## Output Format

```
LIVRABLE :
├── Scorecard 6 dimensions (score 0-100)
├── Diagnostic : PUBLISH / REVISE / REWRITE
├── Feedback ligne par ligne (issues + corrections)
├── Version optimisée complète (copier-coller ready)
└── Vérification de conformité légale
```

---

## Process

```
0. Load context        → business-profile.md + identité visuelle
1. Context intake      → Identifier le copy, le type, l'objectif
2. Classify            → Auto-détection du type et de la langue
3. Score 6 dimensions  → Clarté, Persuasion, CTA, Voix, Structure, Conformité
4. Line-by-line        → Feedback détaillé avec corrections
   Read references/before-after-examples.md → pour 2 reviews avant/après
5. Optimize            → Version réécrite complète
6. Brand check         → Cohérence identité visuelle et marque
7. Deliver             → Rapport + version optimisée
```

---

## Step 0 — Context Loading (Silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire :
    - Nom de marque / identité
    - Produit(s) et prix
    - Audience cible
    - Ton et voix
    - Couleurs de marque (primaire, accent)
    - Termes interdits / terminologie préférée
  → Utiliser comme référence pour la cohérence de marque

SINON :
  → Continuer sans. Le review se base sur les bonnes pratiques générales.
```

---

## Step 1 — Context Intake

### 1a. Identifier le copy à revoir

```
SI $ARGUMENTS contient un chemin de fichier :
  → Lire le fichier
SI $ARGUMENTS contient du texte directement :
  → Utiliser ce texte
SI $ARGUMENTS est vide :
  → Demander : "Colle le texte à revoir, ou donne-moi le chemin du fichier."
```

### 1b. Questions si contexte manquant (max 2-3)

| # | Question | Quand |
|---|----------|-------|
| Q1 | C'est quoi ce copy ? (ad, email, landing page, post social, page de vente, blog) | Type non détectable automatiquement |
| Q2 | C'est pour quelle audience ? Et quel est l'objectif ? (acheter, cliquer, s'inscrire) | Pas clair depuis le contenu |
| Q3 | Tu as des contraintes spécifiques ? (limite de caractères, plateforme, ton souhaité) | Pour ads ou posts sociaux |

---

## Step 2 — Classification Automatique

| Signal | Type détecté |
|--------|-------------|
| Mention de "Ad", caractères courts (<300), CTA unique | `ad` |
| Subject line, preview text, P.S. | `email` |
| Hero section, pricing, FAQ, testimonials | `landing-page` |
| Hashtags, @mentions, très court | `social-post` |
| Longue page, objections, garantie, CTA multiples | `sales-page` |
| H1, H2s, paragraphes longs, SEO | `blog` |
| Section HTML avec value-block/recap-block | `playbook-section` |

Détecter aussi la langue : FR (tutoiement, accents) vs EN ("you", structure anglaise).

---

## Step 3 — Scoring sur 6 Dimensions

### Barème de conversion

| Score 1-10 | Score /100 | Interprétation |
|-----------|-----------|----------------|
| 9-10 | 90-100 | Excellent — prêt à publier |
| 7-8 | 70-89 | Bon — corrections mineures |
| 5-6 | 50-69 | Moyen — révision nécessaire |
| 3-4 | 30-49 | Faible — réécriture recommandée |
| 1-2 | 10-29 | Critique — réécriture obligatoire |

### Dimension 1 — CLARTÉ (0-17 points)

Le lecteur comprend-il le message en 5 secondes ?

| Critère | Points |
|---------|--------|
| Message principal identifiable immédiatement | 5 |
| Pas de jargon inexpliqué | 4 |
| Phrases courtes et directes | 4 |
| Une seule idée par paragraphe | 4 |

Issues à chercher : passages flous, phrases complexes, ambiguïtés, jargon.

### Dimension 2 — PERSUASION (0-17 points)

Le copy donne-t-il envie d'agir ?

| Critère | Points |
|---------|--------|
| Problème du lecteur clairement identifié | 4 |
| Solution présentée de manière concrète | 4 |
| Preuves / crédibilité (chiffres, résultats, témoignages) | 5 |
| Urgence ou raison d'agir maintenant | 4 |

Issues à chercher : arguments faibles, preuves manquantes, claims vagues, pas de différenciation.

### Dimension 3 — CTA (0-17 points)

L'appel à l'action est-il clair, convaincant et unique ?

| Critère | Points |
|---------|--------|
| CTA présent et visible | 5 |
| CTA unique (pas de CTAs concurrents) | 4 |
| CTA orienté bénéfice (pas juste "Cliquez ici") | 4 |
| CTA positionné au bon endroit | 4 |

Issues à chercher : CTA absent, CTAs multiples, CTA faible, CTA enterré.

### Dimension 4 — VOIX & MARQUE (0-17 points)

Le copy sonne-t-il comme la marque ? Est-il cohérent avec l'identité ?

| Critère | Points |
|---------|--------|
| Ton DP Créateur respecté (direct, anti-BS, action-first) | 5 |
| Pas de voix passive dans les instructions | 3 |
| Pas de filler words ou fluff motivationnel | 3 |
| Terminologie cohérente avec le reste des contenus | 3 |
| Cohérence avec les couleurs/identité de marque (si applicable) | 3 |

Issues à chercher : trop formel, trop hype, voix passive, mots de remplissage, incohérence de marque.

### Dimension 5 — STRUCTURE (0-16 points)

Le copy est-il scannable, bien formaté, de bonne longueur ?

| Critère | Points |
|---------|--------|
| Scannable (titres, listes, espacement) | 4 |
| Longueur appropriée au format | 4 |
| Hook en première ligne | 4 |
| Progression logique (problème → solution → action) | 4 |

Issues à chercher : longs paragraphes, pas de sauts de ligne, mauvaise longueur, pas de hook.

### Dimension 6 — CONFORMITÉ (0-16 points)

Le copy est-il légalement safe ?

| Critère | Points |
|---------|--------|
| Aucune promesse de revenus garantis | 5 |
| Aucune fausse urgence sans fondement | 3 |
| Disclaimer présent si claims de résultats | 4 |
| Conforme aux policies de la plateforme (Meta, Google, etc.) | 4 |

Issues à chercher : income claims, fausse rareté, testimonials sans disclaimer, mots bannis.

### Calcul du score global

```
COPY REVIEW SCORECARD
======================
Clarté       : [XX]/17
Persuasion   : [XX]/17
CTA          : [XX]/17
Voix/Marque  : [XX]/17
Structure    : [XX]/16
Conformité   : [XX]/16
─────────────────────
TOTAL        : [XX]/100

Statut : PUBLISH (≥85) / REVISE (60-84) / REWRITE (<60)
```

---

## Step 4 — Feedback Ligne par Ligne

Pour chaque issue trouvée :

```
ISSUE [N] — [Sévérité: Critical/Warning/Suggestion]
  Passage  : "[texte original]"
  Problème : [ce qui ne va pas]
  Fix      : "[texte corrigé]"
  Pourquoi : [explication brève]
```

Classer par sévérité :
1. **Critical** — Conformité, erreurs factuelles, CTA absent → bloquer la publication
2. **Warning** — Voix incorrecte, structure faible, persuasion manquante → corriger avant pub
3. **Suggestion** — Optimisations, style, nuances → nice to have

---

## Step 5 — Version Optimisée

Réécrire le copy en entier avec toutes les corrections appliquées.

```
VERSION OPTIMISÉE
==================
[Copy complet réécrit — prêt à copier-coller]
```

**Règle** : La version optimisée est COMPLÈTE. Pas de "garder le reste tel quel".

---

## Step 6 — Brand Consistency Check

Si business-profile.md a été chargé, vérifier :

| Vérification | Détail |
|-------------|--------|
| Nom de marque | Correctement utilisé, orthographe exacte |
| Prix | Cohérent avec le profil (pas de prix inventé) |
| Audience | Le copy s'adresse à la bonne audience |
| Ton | Correspond au ton défini dans le profil |
| Couleurs (si HTML) | Les couleurs utilisées correspondent à l'identité visuelle |
| Terminologie | Utilise les termes préférés, évite les termes interdits |

Signaler toute incohérence comme issue de type Warning.

---

## Step 7 — Livrable Final

```
✅ COPY REVIEW COMPLETE

📄 Type      : [ad/email/dp-landing-page/social/sales-page/blog]
🌐 Langue    : [FR/EN]
📝 Mots      : [count]
📊 Score     : [XX]/100 — [PUBLISH/REVISE/REWRITE]

SCORECARD :
  Clarté       [██████████░░] 14/17
  Persuasion   [████████░░░░] 12/17
  CTA          [████████████] 17/17
  Voix/Marque  [█████████░░░] 13/17
  Structure    [██████████░░] 13/16
  Conformité   [████████████] 16/16

ISSUES : [total]
  Critical   : [N]
  Warning    : [N]
  Suggestion : [N]

PROCHAINES ÉTAPES :
  → Appliquer les corrections (si fichier : "Tu veux que j'applique ?")
  → /dp-playbook-section  Réécrire une section d'ebook
  → /dp-playbook-audit    Audit complet d'un ebook
```

Si le copy venait d'un fichier, proposer : "Tu veux que j'applique les corrections directement dans le fichier ?"

---

## Critères par Type de Contenu

### Ads (Meta / Google)

- [ ] Le hook arrête le scroll (test de la première ligne)
- [ ] Dans les limites de caractères de la plateforme
- [ ] Un seul CTA clair
- [ ] Pas de mots bannis (policies Meta/Google)
- [ ] Prix mentionné (transparence)
- [ ] Aucune promesse de revenus
- [ ] Direction visuelle cohérente

### Emails

- [ ] Objet crée la curiosité (< 50 caractères)
- [ ] Preview text complète l'objet (pas de répétition)
- [ ] Ouvre avec un hook, pas un "Bonjour"
- [ ] Paragraphes courts (lecture mobile)
- [ ] Un CTA, répété 2-3 fois
- [ ] P.S. présent et utile
- [ ] From: personnel (pas corporate)

### Landing Pages

- [ ] Le hero communique la valeur en 5 secondes
- [ ] Prix visible (pas caché)
- [ ] Preuves de confiance présentes
- [ ] FAQ adresse les vraies objections
- [ ] Tous les boutons → même destination
- [ ] Structure responsive mobile
- [ ] Meta tags SEO présents

### Posts Sociaux

- [ ] Hook dans la première ligne (avant le "...plus")
- [ ] Longueur appropriée à la plateforme
- [ ] Hashtags pertinents (pas spam)
- [ ] CTA présent
- [ ] Suggestion visuelle incluse

### Blog

- [ ] H1 contient le mot-clé principal
- [ ] Meta description < 160 caractères
- [ ] Mot-clé dans les 100 premiers mots
- [ ] H2s ciblent des variations du mot-clé
- [ ] CTA intégré naturellement
- [ ] Valeur standalone (pas juste promotionnel)

---

## Checklist Voix DP Créateur

### Red flags à détecter

| Pattern | Problème | Correction |
|---------|----------|------------|
| Voix passive : "Les résultats peuvent être obtenus" | Faible, impersonnel | "Tu vas obtenir des résultats" |
| Filler : "Il est important de noter que" | Inutile | Supprimer |
| Hedge words : "peut-être", "pourrait", "éventuellement" | Manque de conviction | S'engager ou supprimer |
| Hype : "révolutionnaire", "game-changing", "secret" | Crédibilité zéro | Remplacer par un claim spécifique |
| Générique : "apporter de la valeur", "être authentique" | Vide de sens | Remplacer par une action spécifique |
| Buzzwords : "leverager", "synergie", "optimiser" | Corporate speak | Langage simple |
| Fluff motivationnel : "Tu peux le faire !", "Crois en toi" | Pas actionnable | Supprimer ou remplacer par une instruction |

### Must-have

- Chiffres spécifiques quand possible
- Verbes d'action (envoyer, réserver, closer, mesurer)
- Adresse directe ("tu" / "you")
- Caveats honnêtes quand approprié

---

## Règles de Conformité

### Ne doit JAMAIS apparaître

| Interdit | Exemple |
|----------|---------|
| Garantie de revenus | "Gagne 10K€/mois" |
| Promesses irréalistes | "100 clients en une semaine" |
| Fausse urgence | "Plus que 3 places" (si faux) |
| Fausse rareté | "Offre limitée" qui se reset |
| Témoignages atypiques sans disclaimer | Résultats exceptionnels présentés comme normaux |
| Claims non vérifiables | Statistiques inventées |

### Doit TOUJOURS apparaître (si applicable)

- "Les résultats varient" ou disclaimer équivalent
- Transparence sur le prix
- Identification claire du produit (produit digital / ebook / etc.)
- Description honnête du contenu inclus

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Les issues de conformité sont TOUJOURS priorité Critical | Critical |
| QG-02 | La version optimisée est COMPLÈTE — pas de "garder le reste" | Critical |
| QG-03 | Le score est honnête — un 95/100 est rare. La plupart des premiers drafts sont 50-70. | Critical |
| QG-04 | Toujours fournir le fix, pas seulement le diagnostic | Critical |
| QG-05 | Si le copy est déjà bon, le dire. Ne pas inventer des problèmes. | High |
| QG-06 | Vérifier la cohérence avec business-profile.md si disponible | High |
| QG-07 | Vérifier la cohérence de l'identité visuelle (couleurs, typo) pour le contenu HTML | Medium |
| QG-08 | Les prix mentionnés doivent correspondre au profil business | Critical |
| QG-09 | Score >= 80 et zéro issue Critical = PUBLIER. Score 60-79 = RÉVISER. Score < 60 = RÉÉCRIRE. | Critical |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Pas de copy fourni | Demander clairement : "Colle le texte ou donne-moi un chemin de fichier." |
| Fichier non trouvé | `Glob` pour chercher. Proposer les fichiers trouvés. |
| Copy trop court (< 10 mots) | Revoir quand même mais signaler : "Difficile d'évaluer la persuasion sur si peu de texte." |
| Copy trop long (> 5000 mots) | Proposer de découper par section ou de se concentrer sur les zones clés. |
| Type de contenu ambigu | Demander à l'utilisateur. Proposer le type le plus probable. |
| Langue mélangée (FR + EN) | Signaler comme issue. Demander la langue cible. |
| business-profile.md absent | Continuer sans brand check. Mentionner que le check serait plus précis avec un profil. |
| Le copy est excellent | Le dire franchement. Score élevé. Quelques suggestions mineures seulement. |

---

## Cross-Skill Integration

| Avant copy-review | Skill précédent | Quand |
|-------------------|-----------------|-------|
| Créer un ebook | `/dp-playbook-create` | Le copy est une section d'ebook |
| Créer une publicité | `/dp-ad-angles-meta` | Le copy est une ad Meta |
| Écrire un email | `/dp-email-sequence` | Le copy est un email |

| Après copy-review | Skill suivant | Quand |
|-------------------|---------------|-------|
| Réécrire une section | `/dp-playbook-section` | Section d'ebook avec score bas |
| Auditer l'ebook complet | `/dp-playbook-audit` | Pour un check plus large |
| Synchroniser FR/EN | `/dp-playbook-sync` | Le copy corrigé doit être traduit |
| Créer la landing page | `/dp-landing-page` | Le copy est validé, prêt pour la page |
