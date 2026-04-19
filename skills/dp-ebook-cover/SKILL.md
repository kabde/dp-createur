---
name: dp-ebook-cover
description: "Génère des couvertures professionnelles pour ebooks et produits digitaux. Produit un brief créatif complet, une couverture HTML/CSS standalone, des prompts pour IA image (Midjourney, DALL-E, Ideogram), un mockup 3D en CSS, et les specs pour Canva/Figma. Adapté à l'identité visuelle du projet. Triggers : couverture, cover, ebook cover, visuel produit, mockup, thumbnail, image produit."
user-invokable: true
argument-hint: "[titre ebook] [style: minimaliste|bold|premium|warm|editorial]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: creation
  updated: 2026-04-18
---

# Ebook Cover — Générateur de Couvertures Produit

<!-- v2.0.0 | 2026-04-18 | Création : briefs créatifs, couverture HTML/CSS, prompts IA image, mockup 3D CSS, specs Canva -->

Génère des couvertures professionnelles pour les produits digitaux DP Créateur. Pas besoin de designer — le skill produit soit une couverture HTML prête à screenshoter, soit un brief complet pour Canva, Figma ou IA image.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-ebook-cover [titre]` | Création guidée complète (brief + HTML + prompts IA) |
| `/dp-ebook-cover html [titre]` | Couverture HTML/CSS standalone uniquement |
| `/dp-ebook-cover mockup [titre]` | Mockup 3D CSS (pour landing page) |
| `/dp-ebook-cover prompt [titre]` | Prompts IA image uniquement (Midjourney, DALL-E, Ideogram) |
| `/dp-ebook-cover canva [titre]` | Brief détaillé pour Canva/Figma |
| `/dp-ebook-cover batch [dossier]` | Couvertures pour tous les ebooks d'un dossier |

## Output Format

```
LIVRABLES (jusqu'à 5 fichiers) :
│
├── 1. Brief créatif : covers/brief-[slug].md
│   ├── Direction artistique complète
│   ├── Specs techniques (dimensions, résolutions)
│   └── Variantes de composition (3 options)
│
├── 2. Couverture HTML : covers/[slug]-cover.html
│   ├── HTML/CSS standalone — ouvrir dans le navigateur et screenshoter
│   ├── Design responsive (s'adapte à la taille de la fenêtre)
│   └── Couleurs de marque via CSS custom properties
│
├── 3. Mockup 3D CSS : covers/[slug]-mockup.html
│   ├── Rendu 3D en pur CSS (perspective + transform)
│   ├── Intégrable dans une landing page
│   └── Pas de JS, pas d'image externe
│
├── 4. Prompts IA image : dans le brief
│   ├── Prompt Midjourney (optimisé v6)
│   ├── Prompt DALL-E 3 (optimisé ChatGPT)
│   └── Prompt Ideogram (optimisé texte sur image)
│
└── 5. Brief Canva/Figma : dans le brief
    ├── Dimensions et marges
    ├── Éléments à placer (titre, sous-titre, auteur, logo)
    └── Fonts, couleurs, effets recommandés
```

---

## Process

```
1. Context intake      → Titre, sous-titre, auteur, style, couleurs
2. Read references     → business-profile.md, design-system
3. Direction artistique → 3 options de composition
4. Générer la couverture HTML/CSS
5. Générer le mockup 3D CSS
6. Générer les prompts IA image
7. Générer le brief Canva/Figma
8. Quality check       → Lisibilité, hiérarchie, cohérence marque
9. Deliver             → Fichiers + résumé
```

---

## Step 1 — Context Intake (Required)

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe :
  → Extraire : couleurs (primary, accent), nom de marque, logo, style visuel, police
  → Pré-remplir les options de design
  → Ne PAS reposer les questions déjà couvertes

SINON :
  → Poser toutes les questions
```

### 1b. Questions par blocs

#### Bloc 1 — Le contenu de la couverture

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | **Titre de l'ebook** (tel qu'il doit apparaître sur la couverture) | Typographie principale |
| Q2 | **Sous-titre** ? (optionnel — 1 ligne max) | Contexte sous le titre |
| Q3 | **Nom de l'auteur** (tel qu'il doit apparaître) | Attribution |
| Q4 | **Nom de marque / logo** à afficher ? | Branding |

#### Bloc 2 — Le style visuel

| # | Question | Pourquoi |
|---|----------|----------|
| Q5 | **Quel style ?** `minimaliste` (épuré, typo forte) / `bold` (contrastes, couleurs vives) / `premium` (sombre, élégant, doré) / `warm` (tons chauds, accueillant) / `editorial` (style magazine, moderne) | Direction artistique |
| Q6 | **Couleur primaire** (hex ou nom) — ou "utiliser celles de mon profil" | Fond / accents |
| Q7 | **Couleur d'accent** (hex ou nom) | Highlights / éléments décoratifs |
| Q8 | Tu veux une **image de fond** ou un **design typographique pur** ? Si image : décris le type d'image souhaitée. | Composition |

---

## Step 2 — Direction Artistique

### 2a. Dimensions et formats

| Usage | Dimensions | Ratio | Résolution |
|-------|-----------|-------|------------|
| Couverture ebook (standard) | 1600 × 2560 px | 1:1.6 | 300 dpi |
| Thumbnail Gumroad/LemonSqueezy | 1280 × 720 px | 16:9 | 72 dpi |
| Mockup landing page | 600 × 800 px | 3:4 | 72 dpi |
| Story Instagram | 1080 × 1920 px | 9:16 | 72 dpi |
| Post Instagram | 1080 × 1080 px | 1:1 | 72 dpi |

### 2b. Proposer 3 compositions

Pour chaque style, proposer 3 options de layout :

```
OPTION A — Typographique centré
══════════════════════════════

┌──────────────────────────┐
│                          │
│         [MARQUE]         │  ← Petit, haut, discret
│                          │
│                          │
│      ████████████        │  ← Élément décoratif (ligne, forme)
│                          │
│     TITRE PRINCIPAL      │  ← Grande typo, bold, centré
│     EN DEUX LIGNES       │
│                          │
│      sous-titre ici      │  ← Plus petit, regular
│                          │
│      ████████████        │  ← Séparateur
│                          │
│       Par [Auteur]       │  ← Bas, centré
│                          │
└──────────────────────────┘


OPTION B — Bande latérale
══════════════════════════

┌─────────┬────────────────┐
│         │                │
│  BANDE  │                │
│ COULEUR │  TITRE         │  ← Aligné à gauche
│ PRIMARY │  PRINCIPAL     │
│         │                │
│  logo   │  sous-titre    │
│         │                │
│         │  ─────────     │
│         │  Par [Auteur]  │
│         │                │
└─────────┴────────────────┘


OPTION C — Image de fond + overlay
═══════════════════════════════════

┌──────────────────────────┐
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│  ← Image de fond (assombrie)
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓▓▓▓  TITRE PRINCIPAL ▓▓▓│  ← Texte blanc sur overlay sombre
│▓▓▓▓  EN BLANC BOLD   ▓▓▓│
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓▓▓▓  sous-titre       ▓▓│
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
│▓▓  Par [Auteur]  [logo] ▓│
└──────────────────────────┘
```

Demander : "Laquelle préfères-tu ? A, B, C, ou un mix ?"

### 2c. Règles de composition par style

| Style | Fond | Typo titre | Typo sous-titre | Accents | Effet |
|-------|------|-----------|----------------|---------|-------|
| `minimaliste` | Blanc ou très clair | Bold, noir, grande taille | Light, gris | Ligne fine couleur accent | Beaucoup d'espace blanc |
| `bold` | Couleur primaire saturée | Extra-bold, blanc, très grande | Semi-bold, blanc 80% | Formes géométriques, contraste | Ombres marquées |
| `premium` | Noir ou bleu très foncé | Serif ou sans-serif élégant, or/blanc | Thin, or 60% | Or, cuivre, ligne fine | Subtil, luxueux |
| `warm` | Beige, crème, terre | Bold, brun foncé | Regular, brun moyen | Orange, terracotta | Coins arrondis, doux |
| `editorial` | Blanc cassé | Mix serif (titre) + sans-serif (sous-titre) | Italique, gris | Ligne éditoriale, grille | Style magazine, asymétrique |

---

## Step 3 — Couverture HTML/CSS

Générer un fichier HTML standalone qui rend la couverture dans le navigateur.

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cover — [Titre]</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;800&display=swap');

    * { margin: 0; padding: 0; box-sizing: border-box; }

    :root {
      --primary: [primary_color];
      --accent: [accent_color];
      --text: #ffffff;
      --bg: [primary_color];
    }

    body {
      display: flex;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
      background: #f0f0f0;
      font-family: 'Inter', system-ui, sans-serif;
    }

    .cover {
      width: 800px;
      height: 1280px;
      background: var(--bg);
      position: relative;
      overflow: hidden;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      padding: 60px;
      box-shadow: 0 20px 60px rgba(0,0,0,0.3);
    }

    /* === ADAPTER SELON LE STYLE CHOISI === */

    .cover-brand {
      position: absolute;
      top: 40px;
      font-size: 0.85rem;
      font-weight: 600;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      color: var(--accent);
    }

    .cover-decorator {
      width: 80px;
      height: 4px;
      background: var(--accent);
      margin-bottom: 2rem;
      border-radius: 2px;
    }

    .cover-title {
      font-size: 3.2rem;
      font-weight: 800;
      line-height: 1.1;
      color: var(--text);
      letter-spacing: -0.03em;
      margin-bottom: 1rem;
      max-width: 90%;
    }

    .cover-subtitle {
      font-size: 1.15rem;
      font-weight: 300;
      color: rgba(255,255,255,0.8);
      max-width: 80%;
      line-height: 1.6;
      margin-bottom: 2rem;
    }

    .cover-decorator-bottom {
      width: 60px;
      height: 3px;
      background: var(--accent);
      margin-bottom: 2rem;
      border-radius: 2px;
    }

    .cover-author {
      font-size: 1rem;
      font-weight: 600;
      color: rgba(255,255,255,0.9);
      letter-spacing: 0.05em;
    }

    .cover-badge {
      position: absolute;
      bottom: 40px;
      font-size: 0.75rem;
      color: rgba(255,255,255,0.4);
    }

    /* Responsive pour preview */
    @media (max-width: 850px) {
      .cover {
        width: 100vw;
        height: 160vw;
        padding: 40px;
      }
      .cover-title { font-size: 2.2rem; }
    }
  </style>
</head>
<body>
  <div class="cover">
    <span class="cover-brand">[MARQUE]</span>
    <div class="cover-decorator"></div>
    <h1 class="cover-title">[TITRE]</h1>
    <p class="cover-subtitle">[SOUS-TITRE]</p>
    <div class="cover-decorator-bottom"></div>
    <p class="cover-author">Par [AUTEUR]</p>
    <span class="cover-badge">[MARQUE] — [ANNÉE]</span>
  </div>
</body>
</html>
```

**Adapter le CSS** selon le style choisi (Q5) :
- `minimaliste` : `--bg: #ffffff; --text: #111111;` fond blanc, titre noir
- `bold` : `--bg: var(--primary);` fond couleur saturée, titre blanc, éléments gros
- `premium` : `--bg: #0a0a0a;` fond noir, titre blanc/or, accents dorés
- `warm` : `--bg: #fef6ee;` fond crème, titre brun, accents terre
- `editorial` : `--bg: #fafaf8;` fond cassé, titre noir, mix serif/sans-serif

**Instruction à l'utilisateur** :
```
Pour capturer ta couverture en image :
1. Ouvre le fichier covers/[slug]-cover.html dans Chrome
2. Ajuste la taille de la fenêtre au ratio souhaité
3. Clic droit → "Inspecter" → sélectionne l'élément .cover
4. Clic droit sur l'élément → "Capture node screenshot"
5. → Image PNG haute qualité sauvegardée

Ou : screenshot avec Cmd+Shift+4 (Mac) / Win+Shift+S (Windows)
```

---

## Step 4 — Mockup 3D CSS

Générer un rendu 3D de l'ebook en pur CSS, intégrable dans une landing page.

```html
<div class="mockup-3d" style="perspective: 1200px; display: flex; justify-content: center; padding: 3rem 0;">
  <div style="
    width: 280px;
    height: 400px;
    background: var(--primary, [primary_color]);
    border-radius: 4px 16px 16px 4px;
    transform: rotateY(-20deg) rotateX(5deg);
    box-shadow:
      -15px 15px 30px rgba(0,0,0,0.3),
      inset -3px 0 8px rgba(0,0,0,0.15),
      inset 0 0 0 1px rgba(255,255,255,0.05);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    padding: 2rem 1.5rem;
    position: relative;
    transition: transform 0.4s ease;
  ">
    <!-- Effet tranche du livre -->
    <div style="
      position: absolute;
      left: -12px;
      top: 4px;
      bottom: 4px;
      width: 12px;
      background: linear-gradient(
        to right,
        rgba(0,0,0,0.15),
        rgba(0,0,0,0.05) 40%,
        rgba(255,255,255,0.05) 60%,
        rgba(0,0,0,0.1)
      );
      border-radius: 4px 0 0 4px;
      transform: skewY(-2deg);
    "></div>

    <!-- Contenu de la couverture -->
    <p style="font-size: 0.55rem; font-weight: 600; letter-spacing: 0.15em; text-transform: uppercase; color: [accent_color]; margin-bottom: 1.5rem;">[MARQUE]</p>
    <div style="width: 40px; height: 2px; background: [accent_color]; margin-bottom: 1rem;"></div>
    <h3 style="font-size: 1.25rem; font-weight: 800; color: #fff; line-height: 1.15; margin-bottom: 0.5rem; letter-spacing: -0.02em;">[TITRE]</h3>
    <p style="font-size: 0.65rem; color: rgba(255,255,255,0.7); margin-bottom: 1.5rem;">[SOUS-TITRE]</p>
    <div style="width: 30px; height: 2px; background: [accent_color]; margin-bottom: 1rem;"></div>
    <p style="font-size: 0.6rem; font-weight: 600; color: rgba(255,255,255,0.8);">Par [AUTEUR]</p>
  </div>
</div>
```

**Instructions** : "Copie ce bloc HTML dans ta landing page, dans la section Hero. Remplace les couleurs et le texte. Le mockup 3D est en pur CSS — pas de JS, pas d'image."

---

## Step 5 — Prompts IA Image

### Prompt Midjourney (v6)

```
[STYLE] ebook cover design, "[TITRE]" written in bold typography, 
[COMPOSITION: centered | left-aligned | overlay on image],
[FOND: solid [primary_color] background | dark background | gradient],
[ACCENTS: [accent_color] decorative lines | geometric shapes | subtle glow],
professional digital product cover, clean modern design,
[FONT STYLE: sans-serif bold | serif elegant | mixed typography],
high contrast, sharp text, no people, no photos,
--ar 5:8 --v 6 --style raw --s 250
```

**Exemple concret (style bold) :**
```
Bold modern ebook cover design, "LE PLAYBOOK DU COACH FITNESS" written in 
extra-bold white sans-serif typography on solid emerald green #059669 background, 
centered composition, thin gold accent line separating title and subtitle, 
subtitle "10 clients en 30 jours" in lighter weight, author name at bottom, 
professional digital product aesthetic, clean geometric design, high contrast, 
sharp readable text --ar 5:8 --v 6 --style raw --s 250
```

### Prompt DALL-E 3 (ChatGPT)

```
Create a professional ebook cover design with the following specifications:
- Title: "[TITRE]" in bold [font style] typography
- Subtitle: "[SOUS-TITRE]" in lighter weight below
- Author: "Par [AUTEUR]" at the bottom
- Background: [description — solid color / gradient / dark / image]
- Primary color: [primary_color]
- Accent color: [accent_color] for decorative elements
- Style: [minimaliste / bold / premium / warm / editorial]
- Aspect ratio: 5:8 (portrait, ebook format)
- The text must be clearly readable and properly spelled
- No photographs of people
- Clean, modern, professional design
```

### Prompt Ideogram (optimisé pour texte lisible)

```
"[TITRE]" ebook cover, [style] design, [primary_color] background, 
[accent_color] accent elements, "[SOUS-TITRE]" subtitle, 
"Par [AUTEUR]" author credit, professional typography, 
sharp text rendering, digital product cover, 5:8 aspect ratio
```

> **Note importante** : Les IA image ont du mal avec le texte long. Si le titre fait plus de 5 mots, recommander de générer l'image de fond SANS texte puis d'ajouter le texte dans Canva/Figma.

---

## Step 6 — Brief Canva/Figma

```
BRIEF CANVA / FIGMA
════════════════════

DOCUMENT
  Dimensions : 1600 × 2560 px (ebook cover standard)
  Marges     : 80px de chaque côté (zone de sécurité texte)
  Fond       : [couleur ou image — selon le style]

ÉLÉMENTS À PLACER (de haut en bas) :
  1. Logo/Marque — haut centre, 80px du bord
     Font  : [police] Semi-Bold, 14px, letter-spacing 3px, uppercase
     Color : [accent_color]

  2. Décorateur — ligne horizontale
     Size  : 80px × 4px
     Color : [accent_color]
     Pos   : Centre, ~35% du haut

  3. Titre — centre
     Font  : [police] Extra-Bold, 72-96px
     Color : [text_color]
     Align : Centre
     Max   : 3 lignes
     Tip   : Réduire la taille si > 3 lignes

  4. Sous-titre — sous le titre
     Font  : [police] Light, 24-28px
     Color : [text_color 70% opacité]
     Align : Centre
     Max   : 2 lignes

  5. Décorateur bas — ligne horizontale
     Size  : 60px × 3px
     Color : [accent_color]

  6. Auteur — bas centre, 100px du bord
     Font  : [police] Semi-Bold, 20px
     Color : [text_color 90% opacité]

EFFETS OPTIONNELS :
  - Ombre portée sur le texte (si fond image)
  - Gradient overlay (si image : noir 60% → transparent)
  - Forme décorative géométrique (cercle, triangle, lignes)

EXPORT :
  - PNG haute qualité (300 dpi si impression, 72 dpi si digital)
  - Aussi exporter en 1280×720 (thumbnail) et 1080×1080 (Instagram)
```

---

## Step 7 — Quality Check

### Checklist avant livraison

```
LISIBILITÉ :
  [ ] Le titre est lisible en 1 seconde (même en thumbnail)
  [ ] Le contraste texte/fond est suffisant (ratio ≥ 4.5:1)
  [ ] Le titre n'est pas coupé ni trop petit
  [ ] Pas plus de 3 lignes pour le titre

HIÉRARCHIE :
  [ ] L'œil va : Titre → Sous-titre → Auteur (dans cet ordre)
  [ ] Le titre domine visuellement (2-3x plus gros que le reste)
  [ ] L'auteur est visible mais discret

COHÉRENCE MARQUE :
  [ ] Les couleurs matchent le business-profile.md
  [ ] Le style est cohérent avec la landing page
  [ ] La police est la même que dans les autres supports

TECHNIQUE :
  [ ] Le HTML est valide et s'ouvre dans un navigateur
  [ ] Le mockup 3D rend correctement (perspective + ombre)
  [ ] Les prompts IA sont spécifiques et réalistes
  [ ] Le brief Canva a les dimensions exactes
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Le titre est lisible en thumbnail (< 100px de large) | Critical |
| QG-02 | Le contraste texte/fond respecte le ratio WCAG 4.5:1 | Critical |
| QG-03 | Les couleurs correspondent au business-profile.md | High |
| QG-04 | Le fichier HTML est standalone (aucune dépendance externe sauf Google Fonts) | Critical |
| QG-05 | Le mockup 3D utilise les mêmes couleurs que la couverture | High |
| QG-06 | Les prompts IA ne demandent PAS de texte long (> 5 mots) sur l'image | High |
| QG-07 | Le brief Canva a les dimensions exactes en pixels | High |
| QG-08 | Au moins 3 compositions proposées avant de générer | Medium |
| QG-09 | Le titre fait maximum 3 lignes sur la couverture | High |
| QG-10 | Pas de faute d'orthographe dans le titre/sous-titre | Critical |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Titre trop long (> 8 mots) | Proposer de raccourcir. "Un bon titre de couverture fait 3-6 mots. Le sous-titre prend le reste." |
| Pas de couleurs | Charger business-profile.md ou proposer 3 palettes |
| L'utilisateur veut une photo sur la couverture | Recommander un prompt IA sans texte + ajout texte dans Canva |
| Style non précisé | Proposer les 5 styles avec aperçu ASCII et demander de choisir |
| L'utilisateur veut plusieurs couvertures | Générer les 3 compositions en HTML (A, B, C) |
| Titre en 2 langues | Choisir une seule langue pour la couverture. L'autre version = un autre fichier. |
| Le mockup 3D ne rend pas bien sur Safari | Le CSS perspective est standard — si problème, proposer le fallback flat |
| business-profile.md absent | Poser Q5-Q7 obligatoirement |
| L'utilisateur a déjà une image de couverture | Proposer le mockup 3D intégrant son image comme texture |

---

## Delivery

```
✅ COUVERTURE CRÉÉE

📄 Brief créatif   : covers/brief-[slug].md
🎨 Couverture HTML : covers/[slug]-cover.html
📦 Mockup 3D CSS   : covers/[slug]-mockup.html
🤖 Prompts IA      : dans le brief (Midjourney, DALL-E, Ideogram)
🎯 Brief Canva     : dans le brief (dimensions, éléments, effets)

POUR UTILISER :
  → Ouvrir [slug]-cover.html dans Chrome → screenshot
  → OU copier le mockup 3D dans ta landing page (section Hero)
  → OU utiliser le prompt IA pour générer une image
  → OU suivre le brief Canva pour créer manuellement

PROCHAINES ÉTAPES :
  → /dp-landing-page        Intégrer le mockup dans la page de vente
  → /dp-export-pdf --cover  Utiliser comme couverture du PDF
  → /dp-social-caption      Utiliser comme visuel pour les posts
  → /dp-ad-angles-meta      Utiliser comme créatif publicitaire
```

---

## Cross-Skill Integration

| Avant | Skill | Quand |
|-------|-------|-------|
| Créer l'ebook | `/dp-playbook-create` | Le contenu doit exister pour avoir le titre final |
| Profil business | `/dp-business-profile` | Pour les couleurs et la marque |
| Validation marché | `/dp-market-research` | Le titre peut changer après validation |

| Après | Skill | Quand |
|-------|-------|-------|
| Landing page | `/dp-landing-page` | Intégrer le mockup 3D dans le Hero |
| Export PDF | `/dp-export-pdf --cover` | Utiliser comme page de couverture |
| Ads Meta | `/dp-ad-angles-meta` | Utiliser comme créatif publicitaire |
| Social | `/dp-social-caption` | Visuel pour les posts de promotion |
| Mediaplan | `/dp-mediaplan` | Visuel pour les briefs de contenu |
