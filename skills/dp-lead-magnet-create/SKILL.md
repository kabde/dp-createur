---
name: dp-lead-magnet-create
description: "Crée des lead magnets professionnels (10-15 pages) pour capturer des emails : checklists, cheat sheets, mini-guides, templates packs, quiz results. Optimisé pour la conversion et lié au produit payant. Triggers: lead magnet, gratuit, freebie, capture email, opt-in, checklist gratuite, guide gratuit."
user-invokable: true
argument-hint: "[sujet] [type: checklist|cheat-sheet|mini-guide|template-pack|quiz-result]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: creation
  updated: 2026-04-13
---

# Lead Magnet Create — Créateur de Lead Magnets

<!-- v2.0.0 | 2026-04-13 | Création initiale : context intake, types de lead magnets, quality gates, cross-skill -->

Expert en création de lead magnets pour DP Créateur. Guide l'utilisateur pas à pas — du sujet au fichier HTML prêt à convertir en PDF distribuable. Chaque lead magnet est conçu pour capturer des emails et alimenter le funnel vers le produit payant.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-lead-magnet-create [sujet]` | Lancer la création guidée |
| `/dp-lead-magnet-create express [sujet]` | Mode rapide — 4 questions puis rédaction complète |
| `/dp-lead-magnet-create from [fichier]` | Transformer du contenu existant en lead magnet |
| `/dp-lead-magnet-create list` | Afficher les 5 types de lead magnets disponibles avec exemples |

## Output Format

```
LIVRABLE :
├── Fichier HTML standalone (lead-magnets/[slug]-FR.html)
├── CSS embarqué (design system DP Créateur)
├── Prêt pour export PDF (styles d'impression inclus)
├── CTA final vers le produit payant intégré
└── Score qualité 0-100 avec détail par catégorie
```

---

## Process

```
1. Context intake      → Collecter le sujet, le type, le produit payant lié
2. Read references     → Charger quality gates, voice guide, business profile
3. Build lead card     → Synthèse validée par l'utilisateur
4. Design structure    → Plan détaillé selon le type choisi
5. Write content       → Rédaction section par section (80% valeur / 20% transition)
6. Quality check       → Score 0-100, quality gates, pre-delivery review
7. Deliver             → Fichier HTML + résumé + prochaines étapes
```

---

## Step 1 — Context Intake (Required: Always Do This First)

Avant toute rédaction, collecter le contexte. Un lead magnet sans lien clair avec le produit payant est un effort gaspillé.

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom, niche, produit(s), audience, ton, couleurs
  → Ne PAS reposer les questions déjà couvertes par le profil

SINON :
  → Continuer sans. Les questions de l'intake couvriront le minimum.
```

### 1b. Poser les questions par blocs

**Règle absolue** : Ne JAMAIS poser toutes les questions d'un coup. Grouper par 2-3, attendre les réponses, reformuler pour valider, puis continuer.

#### Bloc 1 — Le produit payant et le funnel

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Quel est ton produit PAYANT ? (nom, prix, format) Le lead magnet doit alimenter le funnel vers ce produit. | Le lead magnet n'existe pas seul — il est le premier pas du parcours client |
| Q2 | Quel type de lead magnet tu veux créer ? `checklist` / `cheat-sheet` / `mini-guide` / `template-pack` / `quiz-result` | Détermine la structure et le format |
| Q3 | Quel problème rapide ça résout ? Décris le quick win en 1 phrase — le lecteur doit obtenir un résultat en 15 min. | Un lead magnet qui ne donne pas de résultat immédiat finit à la poubelle |

**Après les réponses** : Reformuler en 2-3 lignes. "Si je comprends bien, tu veux créer un [type] qui donne [quick win] et qui amène vers [produit payant]. C'est correct ?"

#### Bloc 2 — L'audience et le positionnement

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | Pour qui exactement ? Décris ton lecteur idéal (métier, situation, niveau). | Ciblage du contenu et du ton |
| Q5 | Quel titre tu as en tête ? Même provisoire. Sinon je t'en propose 5. | Cadrage éditorial |
| Q6 | Couleurs de marque ? (couleur principale hex + couleur accent hex) Ou je lis ton business-profile.md. | Cohérence visuelle avec ta marque |

**Après les réponses** : Confirmer le format et annoncer le volume attendu.

> **Volume attendu par type :**
> - Checklist : 10-12 pages (~3 000-3 500 mots)
> - Cheat sheet : 8-10 pages (~2 500-3 000 mots)
> - Mini-guide : 12-15 pages (~4 000-5 000 mots)
> - Template pack : 10-15 pages (~3 000-4 500 mots)
> - Quiz result : 10-12 pages (~3 000-3 500 mots)

### Cas particuliers — Context Intake

| Situation | Action |
|-----------|--------|
| Pas de produit payant | Alerter : "Un lead magnet sans produit payant derrière, c'est du contenu gratuit sans ROI. Tu veux qu'on définisse d'abord ton offre payante ?" Proposer `/dp-playbook-create`. |
| Pas d'idée de sujet | Poser : "Quelle question tes prospects te posent le plus souvent ?", "Quel problème tu résous en 15 min pendant tes consultations ?" |
| Sujet trop large | Proposer 3 angles précis. Un lead magnet = 1 problème = 1 solution. |
| L'utilisateur veut un lead magnet de 30+ pages | Rediriger vers `/dp-playbook-create` avec le type `lead-magnet` ou `guide`. Un lead magnet doit rester court — sinon c'est un ebook. |
| L'utilisateur fournit du contenu existant | Lire, identifier le meilleur angle, extraire et restructurer au format lead magnet. |

---

## Step 2 — Lead Card (Synthèse de découverte)

Après toutes les questions, présenter la fiche :

```
╔══════════════════════════════════════════════════╗
║            FICHE LEAD MAGNET — SYNTHÈSE          ║
╠══════════════════════════════════════════════════╣
║ Titre        : [titre ou "à définir"]            ║
║ Type         : [checklist/cheat-sheet/mini-guide/ ║
║                 template-pack/quiz-result]        ║
║ Pages visées : [10-15]                           ║
║ Quick win    : [résultat en 15 min]              ║
╠══════════════════════════════════════════════════╣
║ FUNNEL                                           ║
║ Produit payant : [nom — prix]                    ║
║ Lien logique   : [pourquoi ce lead magnet mène   ║
║                   naturellement au produit]       ║
╠══════════════════════════════════════════════════╣
║ AUDIENCE                                         ║
║ Qui            : [profil lecteur]                ║
║ Problème       : [douleur immédiate]             ║
╠══════════════════════════════════════════════════╣
║ IDENTITÉ VISUELLE                                ║
║ Primaire       : [#hex — nom]                    ║
║ Accent         : [#hex — nom]                    ║
║ Marque         : [nom/logo ou "aucun"]           ║
╚══════════════════════════════════════════════════╝
```

**Demande validation** : "Cette fiche te correspond ? Tu veux modifier quelque chose avant qu'on passe au plan ?"

**Hard gate** : Ne PAS continuer sans validation explicite de l'utilisateur.

---

## Step 3 — Structure par Type

### Type 1 — Checklist

**Format** : 15-25 items organisés en catégories, cases à cocher, 1 page = 1 catégorie.

```
STRUCTURE CHECKLIST (10-12 pages)
═════════════════════════════════

Page 1     : Couverture (titre, sous-titre, marque)
Page 2     : Introduction — pourquoi cette checklist + comment l'utiliser (1 paragraphe)
Pages 3-9  : Les catégories (3-5 catégories, 5-7 items chacune)
  Chaque catégorie :
    - Titre de la catégorie
    - 5-7 items avec case à cocher
    - Pour chaque item : 1 ligne d'action + 1 ligne "pourquoi c'est important"
    - Indicateur de priorité : 🔴 Critique / 🟡 Important / 🟢 Bonus
Page 10    : Récapitulatif — score (X/25 items cochés = quel niveau)
Page 11    : CTA vers le produit payant — "Tu as coché moins de 15 items ? Voici comment corriger ça."
Page 12    : À propos + liens
```

### Type 2 — Cheat Sheet

**Format** : 1-3 pages recto/verso, ultra-condensé, format référence rapide. Conçu pour être imprimé et gardé à portée de main.

```
STRUCTURE CHEAT SHEET (8-10 pages)
══════════════════════════════════

Page 1     : Couverture
Page 2     : Mode d'emploi — "Imprime cette page et garde-la près de toi"
Pages 3-7  : Le contenu condensé
  Organisé en blocs visuels :
    - Tableaux de référence rapide
    - Formules / frameworks en 1 ligne
    - Do / Don't côte à côte
    - Aide-mémoire visuel (schémas simples)
  Règle : ZÉRO paragraphe de plus de 2 lignes. Tout est bullet, tableau ou schéma.
Page 8     : Erreurs fréquentes — les 5 pièges à éviter
Page 9     : CTA vers le produit payant — "Cette cheat sheet couvre les bases. Pour le système complet..."
Page 10    : À propos + liens
```

### Type 3 — Mini-Guide

**Format** : 10-15 pages, 3-5 sections courtes, valeur immédiate. C'est un guide actionnable, pas un ebook complet.

```
STRUCTURE MINI-GUIDE (12-15 pages)
══════════════════════════════════

Page 1       : Couverture
Page 2       : Introduction — Le problème + la promesse (max 200 mots)
Pages 3-4    : Section 1 — [Concept fondamental]
  - Le principe en 2-3 paragraphes
  - 1 exemple concret
  - 1 action immédiate
Pages 5-7    : Section 2 — [La méthode pas à pas]
  - Étapes numérotées (3-5 étapes)
  - Template ou script prêt à copier
Pages 8-10   : Section 3 — [Application concrète]
  - Étude de cas ou scénario type
  - Checklist d'action
Pages 11-12  : Section 4 (optionnel) — [Erreurs à éviter]
Page 13      : Récapitulatif — plan d'action en 1 page
Page 14      : CTA vers le produit payant — transition naturelle
Page 15      : À propos + liens
```

### Type 4 — Template Pack

**Format** : 5-10 templates prêts à copier, chacun avec mode d'emploi court.

```
STRUCTURE TEMPLATE PACK (10-15 pages)
═════════════════════════════════════

Page 1       : Couverture
Page 2       : Comment utiliser ce pack — instructions en 3 étapes
Pages 3-12   : Les templates (1-2 pages par template)
  Chaque template :
    - Titre du template
    - Quand l'utiliser (1-2 lignes)
    - Le template lui-même (prêt à copier)
    - Exemple rempli (pour montrer à quoi ça ressemble)
    - 1 conseil pro (astuce pour maximiser l'impact)
Page 13      : Tableau récapitulatif — quel template pour quelle situation
Page 14      : CTA vers le produit payant — "Ces templates couvrent [X]. Pour le système complet de [Y]..."
Page 15      : À propos + liens
```

### Type 5 — Quiz Result

**Format** : Résultats personnalisés basés sur des réponses, avec recommandations adaptées.

```
STRUCTURE QUIZ RESULT (10-12 pages)
═══════════════════════════════════

Page 1       : Couverture — "Tes résultats personnalisés"
Page 2       : Rappel du quiz — les questions et le système de scoring
Pages 3-4    : Profil A — [Nom du profil] (score X-Y)
  - Description du profil (forces, faiblesses)
  - 3 recommandations spécifiques
  - 1 action prioritaire
Pages 5-6    : Profil B — [Nom du profil] (score X-Y)
  - Même structure
Pages 7-8    : Profil C — [Nom du profil] (score X-Y)
  - Même structure
Pages 9-10   : Profil D (optionnel) — [Nom du profil] (score X-Y)
Page 11      : Plan d'action commun — les 3 étapes que tous les profils doivent suivre
Page 12      : CTA vers le produit payant — personnalisé selon le profil
```

---

## Step 4 — Rédaction

### 4a. Charger les références (silencieux)

```
SI le dossier references/ de dp-playbook-create contient des fichiers utiles :
  Read dp-playbook-create/references/design-system.md   → CSS complet
  Read dp-playbook-create/references/voice-guide.md     → Ton et style
  Read dp-playbook-create/references/quality-gates.md   → Règles de base
```

### 4b. Règles de rédaction spécifiques au lead magnet

| Règle | Détail |
|-------|--------|
| **80/20** | 80 % valeur gratuite actionnable / 20 % transition vers le payant. Le lecteur ne doit JAMAIS se sentir frustré ou arnaqué. |
| **Quick win réel** | Le lecteur DOIT obtenir un résultat concret en 15 minutes de lecture. Pas "apprendre des choses" — un résultat : une checklist remplie, un template copié, un plan d'action défini. |
| **CTA final obligatoire** | La dernière page avant "À propos" est TOUJOURS un CTA vers le produit payant. Transition naturelle, pas de vente agressive. |
| **Teaser sans frustrer** | Mentionner le produit payant 2-3 fois max dans le corps du contenu (pas plus). Toujours dans un contexte de "pour aller plus loin", jamais en remplacement de contenu. |
| **Autonomie du contenu** | Le lead magnet doit avoir de la valeur MÊME si le lecteur n'achète jamais le produit payant. C'est ça qui construit la confiance. |
| **Longueur des sections** | Sections courtes — 300-600 mots max. C'est un lead magnet, pas un ebook. |

### 4c. Pattern de page (obligatoire)

Chaque section suit CE pattern :

```html
<section class="lm-section" id="[kebab-case-unique]">
  <h2>[Titre court et actionnable]</h2>

  <div class="lm-value-block">
    <p>[1-2 phrases — ce que le lecteur va obtenir dans cette section]</p>
  </div>

  <!-- Contenu principal — direct, actionnable -->
  <p>[Contenu court, spécifique, voix DP Créateur]</p>

  <!-- Template / checklist / action si applicable -->
  <div class="lm-action-block">
    <h4>Action</h4>
    <p>[L'action concrète à faire maintenant]</p>
  </div>
</section>
```

### 4d. Pattern de CTA final (obligatoire)

```html
<section class="lm-cta-section" id="next-step">
  <h2>[Titre orienté résultat — pas "Achète mon produit"]</h2>

  <p>[2-3 phrases de transition naturelle. Rappeler le quick win obtenu grâce au lead magnet. Montrer que le produit payant est la suite logique.]</p>

  <div class="lm-cta-block">
    <h3>[Nom du produit payant]</h3>
    <p>[1 phrase = la promesse du produit]</p>
    <p>[Prix + ce qui est inclus en 2-3 bullets]</p>
    <a href="[URL]" class="lm-cta-button">[Texte du bouton — orienté action]</a>
  </div>

  <p class="lm-caveat">[Caveat honnête — "Pas de résultat garanti", "Ça demande du travail", etc. La transparence renforce la confiance.]</p>
</section>
```

### 4e. Règles de voix

| Faire | Ne PAS faire |
|-------|-------------|
| Tutoyer systématiquement | "Nous", "on", voix passive |
| Actions spécifiques ("Coche les 5 premiers items maintenant") | Conseils vagues ("réfléchis à ta situation") |
| Phrases courtes — c'est un lead magnet, pas un roman | Paragraphes de 10 lignes |
| Caveats honnêtes ("ça marche si tu appliques") | Promesses garanties |
| Donner le résultat d'abord, expliquer ensuite | Longues introductions théoriques |
| Templates prêts à copier-coller | "Crée ton propre template" sans exemple |

---

## Step 5 — Quality Check (Pre-Delivery)

### 5a. Quality Gates — Vérification stricte

Avant de livrer, vérifier CHAQUE gate. Si un gate Critical échoue, corriger avant livraison.

| ID | Gate | Severity |
|----|------|----------|
| QG-01 | CTA vers le produit payant présent et clair | Critical |
| QG-02 | Le lecteur obtient un résultat concret en lisant (pas juste "apprendre") | Critical |
| QG-03 | 10-15 pages max (pas plus — c'est un lead magnet, pas un ebook) | Critical |
| QG-04 | Aucun placeholder ([TODO], [INSERT], Lorem ipsum, texte entre crochets) | Critical |
| QG-05 | Lien logique avec le produit payant (le lead magnet = premier pas du parcours) | Critical |
| QG-06 | Couleurs de marque appliquées (--color-primary, --color-accent) | High |
| QG-07 | Ratio 80/20 respecté (80 % valeur gratuite / 20 % transition payant) | High |
| QG-08 | Pas de CSS/JS externe (sauf Google Fonts) | Critical |
| QG-09 | Au moins 1 élément actionnable par section (checklist, template, exercice) | High |
| QG-10 | HTML valide (tags fermés, structure correcte, IDs uniques) | Critical |

### 5b. Pre-Delivery Checklist

```
AVANT DE LIVRER, vérifier :

Structure :
  [ ] HTML valide et bien formé
  [ ] Tous les IDs de section uniques et en kebab-case
  [ ] CSS complet embarqué dans <style>
  [ ] Attribut lang="fr" correct
  [ ] Styles d'impression (@media print) inclus
  [ ] 10-15 pages max

Contenu :
  [ ] ZÉRO placeholder, TODO, ou [texte entre crochets]
  [ ] Quick win réel — le lecteur obtient un résultat concret
  [ ] CTA final vers le produit payant — transition naturelle
  [ ] Ratio 80/20 respecté
  [ ] Produit payant mentionné 2-3 fois max dans le corps (hors CTA final)
  [ ] Voix DP Créateur respectée (direct, action-first, honnête)
  [ ] Pas de fluff motivationnel
  [ ] Caveats honnêtes où nécessaire

Technique :
  [ ] Fichier enregistré au bon chemin
  [ ] Dossier créé si nécessaire
  [ ] Pas de conflit avec un fichier existant
```

### 5c. Score Qualité

Calculer et afficher le score :

```
QUALITY SCORE : [XX]/100

Structure     : [XX]/20  (pages, navigation, HTML)
Contenu       : [XX]/30  (valeur, actionnabilité, quick win)
Funnel        : [XX]/20  (CTA, lien avec produit payant, ratio 80/20)
Voix & Ton    : [XX]/15  (direct, tutoiement, honnête)
Lisibilité    : [XX]/15  (sections courtes, visuels, aération)

Issues :
  Critical : [N] (doivent être corrigés)
  High     : [N] (fortement recommandé)
  Medium   : [N] (à considérer)
  Low      : [N] (nice to have)
```

**Hard gate** : Si score < 75 ou si des issues Critical existent, corriger avant livraison.

---

## Step 6 — Delivery

### 6a. Assembler et sauvegarder

```
Chemin de sortie :
  → lead-magnets/[slug]-FR.html

Slug : titre en minuscules, espaces → tirets, pas de caractères spéciaux.
Créer le dossier si inexistant.
```

### 6b. Présenter le livrable

```
LEAD MAGNET CREE — Score : [XX]/100

Fichier     : [chemin complet]
Titre       : [titre final]
Type        : [checklist/cheat-sheet/mini-guide/template-pack/quiz-result]
Pages       : [N] pages
Mots        : ~[estimation] mots
Quick win   : [résultat que le lecteur obtient]
Produit lié : [nom du produit payant — prix]

SCORE QUALITE :
  Structure  [██████████░░] XX/100
  Contenu    [████████████] XX/100
  Funnel     [█████████░░░] XX/100
  Voix       [██████████░░] XX/100
  Lisibilité [███████████░] XX/100

PROCHAINES ETAPES :
  → /dp-export-pdf          Convertir en PDF distribuable
  → /dp-landing-page        Créer la landing page de capture
  → /dp-email-sequence      Séquence email post-téléchargement
  → /dp-social-caption      Posts pour promouvoir le lead magnet
  → /dp-ad-angles-meta      Publicité pour la landing page
```

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Pas de produit payant défini | Alerter l'utilisateur. Proposer de définir l'offre d'abord avec `/dp-playbook-create` ou `/dp-business-profile`. Un lead magnet sans funnel = effort gaspillé. |
| Sujet trop large pour un lead magnet | Proposer 3 sous-sujets précis. Rappeler : 1 lead magnet = 1 problème = 1 quick win. |
| L'utilisateur veut plus de 15 pages | Rediriger vers `/dp-playbook-create` type `lead-magnet` ou `guide`. Expliquer : un lead magnet trop long n'est pas lu. |
| Le quick win n'est pas assez concret | Poser : "Si ton lecteur suit ton lead magnet pendant 15 min, qu'est-ce qu'il a de concret à la fin ? Un document rempli ? Une décision prise ? Un outil configuré ?" |
| Pas de lien logique entre le lead magnet et le produit payant | Proposer 3 angles qui créent un pont naturel. Le lead magnet résout le problème A, le produit payant résout A+B+C. |
| business-profile.md absent | Continuer avec les réponses du context intake. Poser les questions de couleurs obligatoirement. |
| Fichier existant même slug | Demander confirmation avant écrasement. |
| Le contenu dépasse le ratio 80/20 | Identifier les passages trop promotionnels et les réécrire en mode "valeur d'abord". |

---

## Cross-Skill Integration

### Avant la création du lead magnet

| Skill | Quand |
|-------|-------|
| `/dp-business-profile` | Pour définir le positionnement, l'audience et les couleurs de marque |
| `/dp-playbook-create` | Si le produit payant n'existe pas encore |
| `/dp-competitor-analysis` | Pour identifier les lead magnets des concurrents et se différencier |

### Après la création du lead magnet

| Skill | Quand |
|-------|-------|
| `/dp-export-pdf` | Convertir le HTML en PDF distribuable |
| `/dp-landing-page` | Créer la page de capture email (formulaire + promesse) |
| `/dp-email-sequence` | Séquence de bienvenue post-téléchargement (nurturing → vente) |
| `/dp-social-caption` | Créer les posts pour promouvoir le lead magnet |
| `/dp-mediaplan` | Planifier la promotion organique |
| `/dp-ad-angles-meta` | Créer les publicités Facebook/Instagram pour la landing page |
| `/dp-ad-angles-google` | Créer les publicités Google pour la landing page |
| `/dp-blog-article` | Rédiger un article SEO qui mène vers le lead magnet |
