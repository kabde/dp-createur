---
name: dp-landing-page
description: "Générateur de landing pages professionnelles, responsives, en HTML standalone avec bouton CTA vers Stripe, Gumroad, Calendly, ou toute URL. Collecte l'identité visuelle (couleurs, style), le contenu produit, les trust badges, la FAQ, et les données SEO. Utilise des CSS custom properties pour le branding. Triggers : landing page, page de vente, sales page, page produit, créer une page, squeeze page."
user-invokable: true
argument-hint: "[produit] [url_destination]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: marketing
  updated: 2026-04-13
---

# Landing Page — Sales Page Generator

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake avec brand identity, CSS custom properties, quality gates, error handling, cross-skill -->

Expert en conversion et web design pour DP Créateur. Génère des landing pages standalone professionnelles et responsives, optimisées pour la conversion.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-landing-page [produit]` | Lancer la création guidée complète |
| `/dp-landing-page express [produit]` | Mode rapide — 5 questions puis génération |
| `/dp-landing-page lead-magnet [produit]` | Version optimisée pour capture d'emails (pas de prix) |
| `/dp-landing-page from [fichier]` | Générer depuis un contenu ou playbook existant |

## Output Format

```
LIVRABLE :
├── Fichier HTML standalone (landing-pages/[slug].html)
│   ├── CSS embarqué avec custom properties (--primary, --accent, --style)
│   ├── Design responsive (mobile-first)
│   ├── SEO meta tags + Open Graph
│   └── Zéro dépendance JS (sauf Google Fonts optionnel)
├── Sections : Hero → Trust → Promises → Details → Info Cards → FAQ → CTA → Footer
└── Tous les CTA pointent vers la même URL de destination
```

---

## Process

```
1. Context intake      → Collecter produit, brand identity, contenu (OBLIGATOIRE)
2. Read references     → Charger business-profile.md si dispo
   Read references/copy-templates.md → pour des exemples de copy par tier de prix
3. Build page card     → Synthèse validée par l'utilisateur
4. Generate HTML       → Page complète avec CSS custom properties
5. Quality check       → Responsive, compliance, SEO, performance
6. Deliver             → Fichier HTML + résumé + prochaines étapes
```

---

## Step 1 — Context Intake (Required: Always Do This First)

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom, niche, produit(s), audience, ton, couleurs, logo
  → Ne PAS reposer les questions déjà couvertes par le profil

SINON :
  → Continuer sans. Les questions de l'intake couvriront le minimum.
```

### 1b. Poser les questions par blocs

**Règle absolue** : Ne JAMAIS poser toutes les questions d'un coup. Grouper par 2-3, attendre les réponses, puis continuer.

#### Bloc 1 — Le produit (poser en premier)

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Quel produit ou service veux-tu vendre sur cette page ? Nom + description courte. | Cadre le contenu |
| Q2 | Quel est le prix ? (ou "gratuit" si lead magnet) | Hero + CTA |
| Q3 | Quelle est l'URL de destination du bouton CTA ? (Stripe, Gumroad, Calendly, autre lien) | Tous les boutons CTA pointent là |

**Après les réponses** : Reformuler. "Page de vente pour [X] à [prix], CTA vers [URL]. Correct ?"

**Hard gate** : Ne PAS continuer sans au minimum le nom du produit et l'URL de destination.

#### Bloc 2 — L'identité visuelle (OBLIGATOIRE)

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | Quelle est ta **couleur primaire** ? (hex, ex: `#059669`, ou nom comme "vert émeraude") | CSS `--primary` |
| Q5 | Quelle est ta **couleur d'accent** ? (hex, ex: `#10b981`, ou nom comme "vert clair") | CSS `--accent` |
| Q6 | Quel **style visuel** ? `minimaliste` (épuré, beaucoup de blanc) / `bold` (contrastes forts, couleurs vives) / `premium` (sobre, tons sombres, élégant) / `warm` (tons chauds, accueillant) | Ambiance CSS globale |

> **Si l'utilisateur n'a pas de couleurs** : Proposer 3 palettes adaptées à sa niche :
> - Chaque palette = couleur primaire + couleur accent
> - Montrer un aperçu : "Palette 1 : Bleu profond (#1e3a5f) + Or (#d4a853) — style premium/confiance"
> - Demander de choisir ou combiner

**Après les réponses** : Noter les choix visuels. Ils seront appliqués aux CSS custom properties.

#### Bloc 3 — Le contenu

| # | Question | Pourquoi |
|---|----------|----------|
| Q7 | Tu as des images du produit ? (URLs ou fichiers locaux — 1 à 3 max) | Hero et sections détails |
| Q8 | Quels sont les 3-4 bénéfices principaux de ton produit ? | Section Promises |
| Q9 | Tu as des témoignages, résultats, ou chiffres de crédibilité ? | Trust bar + social proof |

**Après les réponses** : Synthèse contenu.

#### Bloc 4 — La FAQ et le SEO

| # | Question | Pourquoi |
|---|----------|----------|
| Q10 | Quelles sont les 3-5 questions que tes clients posent le plus ? | Section FAQ |
| Q11 | Tu as un titre SEO en tête ? Et une meta description ? (sinon je les génère) | Balises meta |

**Après les réponses** : Passer à la génération.

---

## Step 2 — Design System (CSS Custom Properties)

### Variables CSS obligatoires

Toutes les couleurs et styles passent par des CSS custom properties. JAMAIS de couleurs hardcodées dans les composants.

```css
:root {
  /* Couleurs de marque — issues du context intake */
  --primary: [primary_color from Q4];
  --accent: [accent_color from Q5];

  /* Couleurs dérivées automatiquement */
  --primary-light: [primary + transparence 10%];
  --primary-dark: [primary assombri 20%];
  --accent-light: [accent + transparence 10%];

  /* Couleurs système */
  --text: #1a1a1a;
  --muted: #666666;
  --bg: #ffffff;
  --surface: #f9fafb;
  --border: #e5e7eb;

  /* Layout */
  --radius: 12px;
  --shadow: 0 1px 3px rgba(0,0,0,0.1);
  --max-width: 720px;
}
```

### Styles par ambiance visuelle

| Style | Modifications CSS |
|-------|-------------------|
| `minimaliste` | `--bg: #ffffff`, `--surface: #fafafa`, ombres très légères, beaucoup d'espace blanc, `--radius: 8px` |
| `bold` | Contrastes forts, `--bg: #ffffff`, boutons plus gros, titres plus grands, `--radius: 16px`, ombres marquées |
| `premium` | `--bg: #0a0a0a`, `--text: #f5f5f5`, `--surface: #1a1a1a`, `--muted: #999`, fond sombre, typographie élégante |
| `warm` | `--bg: #fffbf5`, `--surface: #fff5eb`, `--border: #f0d9b5`, tons chauds, `--radius: 16px`, ombres douces |

### Typographie
- Font : `'Inter', system-ui, -apple-system, sans-serif` (Google Fonts import pour Inter)
- Body : 16px, line-height 1.6
- H1 : 2.5rem (mobile: 1.75rem)
- H2 : 1.75rem (mobile: 1.35rem)
- H3 : 1.2rem

### Responsive Breakpoints
- Mobile : < 640px (1 colonne, touch targets larges)
- Tablet : 640-1024px (2 colonnes si applicable)
- Desktop : > 1024px (layout complet)

---

## Step 3 — Page Structure (ordre obligatoire)

### 1. Hero / Header

```html
<section class="hero">
  <!-- Image(s) produit si fournies -->
  <h1>[product_name]</h1>
  <p class="subtitle">[product_description]</p>
  <div class="price">[product_price]</div>
  <a href="[destination_url]" class="cta-button">[cta_button_text]</a>
</section>
```

Design :
- Full-width, padding généreux
- Titre large et bold
- Prix affiché avec `var(--primary)`
- CTA : `background: var(--primary)`, texte blanc, arrondi, animation hover
- Sans images : gradient dérivé de `var(--primary)` + `var(--accent)`

### 2. Trust Bar

```html
<section class="trust-bar">
  <div class="badge"><span class="icon">[icon]</span><span>[text]</span></div>
  <!-- 3-5 badges -->
</section>
```

Design : Bande horizontale, fond léger, flex + wrap mobile, icons + texte court

### 3. Promises

```html
<section class="promises">
  <h2>Ce que tu obtiens</h2>
  <div class="promise-grid">
    <div class="promise-card">
      <span class="promise-icon">[emoji]</span>
      <h3>[title]</h3>
      <p>[description]</p>
    </div>
    <!-- 2-4 cartes -->
  </div>
</section>
```

Design : Grille 2 cols desktop / 1 col mobile, cartes blanches avec ombre subtile, `var(--accent)` pour les icônes

### 4. Details & Caractéristiques

```html
<section class="details">
  <div class="detail-block">
    <img src="[image_url]" alt="[title]">
    <div class="detail-content">
      <h3>[title]</h3>
      <ul><li>[point]</li></ul>
    </div>
  </div>
  <!-- Alterner image gauche/droite -->
</section>
```

Design : Layout alterné, stacked sur mobile, parser `**bold**` en `<strong>`, sans images = blocs texte full-width

### 5. Info Cards (optionnel)

```html
<section class="info-cards">
  <div class="info-card info-card--green"><h4>[title]</h4><p>[text]</p></div>
  <!-- Types : green (positif), red (négatif), blue (info), amber (attention) -->
</section>
```

### 6. FAQ

```html
<section class="faq">
  <h2>Questions fréquentes</h2>
  <div class="faq-list">
    <details class="faq-item"><summary>[question]</summary><p>[answer]</p></details>
  </div>
</section>
```

Design : Accordion natif `<details>/<summary>`, pas de JS, animation CSS

### 7. Call to Action (mid-page)

```html
<section class="cta-section">
  <h2>[cta_title]</h2>
  <p>[cta_subtitle]</p>
  <a href="[destination_url]" class="cta-button">[cta_button_text]</a>
</section>
```

### Mode Lead Magnet (page de capture email)

Si le mode `lead-magnet` est choisi, adapter la page :
- **Hero** : Accroche + promesse du lead magnet gratuit (pas de prix)
- **CTA** : "Télécharge gratuitement" / "Reçois le guide" au lieu de "Acheter"
- **Formulaire** : Champ email + bouton (lien vers ConvertKit, Mailchimp, etc.)
- **Pas de section prix** — remplacer par les bénéfices du lead magnet
- **Social proof** : "Déjà [N] téléchargements" plutôt que "Déjà [N] clients"
- **Footer CTA** : Rappel que c'est gratuit + urgence douce ("places limitées" ou "édition limitée")

### 8. Footer

```html
<footer class="footer footer--dark">
  <h2>[footer_title]</h2>
  <p>[footer_subtitle]</p>
  <a href="[destination_url]" class="cta-button">[cta_button_text]</a>
  <p class="copyright">&copy; [year] [brand_name]. All rights reserved.</p>
</footer>
```

### 9. SEO (dans `<head>`)

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[seo_title]</title>
  <meta name="description" content="[seo_description]">
  <meta property="og:title" content="[seo_title]">
  <meta property="og:description" content="[seo_description]">
  <meta property="og:type" content="product">
  <meta property="og:image" content="[first product_image or empty]">
  <meta name="twitter:card" content="summary_large_image">
</head>
```

---

## CTA Button Style (obligatoire)

```css
.cta-button {
  display: inline-block;
  background: var(--primary);
  color: #fff;
  padding: 1rem 2.5rem;
  border-radius: 50px;
  font-size: 1.1rem;
  font-weight: 700;
  text-decoration: none;
  transition: transform 0.2s, box-shadow 0.2s;
  box-shadow: 0 4px 14px rgba(0,0,0,0.15);
}
.cta-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0,0,0,0.2);
  background: var(--primary-dark);
}
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Aucun placeholder [TODO], [INSERT], Lorem ipsum | Critical |
| QG-02 | Fichier HTML unique — aucun CSS, JS, ou image externe (sauf Google Fonts) | Critical |
| QG-03 | Tous les boutons CTA pointent vers la MÊME destination_url | Critical |
| QG-04 | Toutes les couleurs utilisent `var(--primary)`, `var(--accent)`, etc. — jamais de hex hardcodé dans les composants | Critical |
| QG-05 | Page responsive et professionnelle sur mobile (iPhone test mental) | Critical |
| QG-06 | Pas de texte placeholder — si contenu manquant, le générer depuis le contexte produit | Critical |
| QG-07 | Balises SEO complètes (title, description, OG tags) | High |
| QG-08 | HTML valide (tags fermés, structure correcte) | Critical |
| QG-09 | Styles d'impression (@media print) inclus | Medium |
| QG-10 | Parser `**bold**` en `<strong>` dans le contenu utilisateur | High |
| QG-11 | Attribut lang correct sur `<html>` | High |
| QG-12 | Le style visuel choisi (minimaliste/bold/premium/warm) est correctement appliqué | High |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Pas de nom de produit | Demander : "Quel produit veux-tu vendre ?" — ne pas continuer sans |
| Pas d'URL de destination | Demander : "Où le bouton CTA doit-il rediriger ?" — ne pas continuer sans |
| Pas de couleurs fournies | Proposer 3 palettes adaptées à la niche, demander de choisir |
| Pas d'images | Utiliser un hero gradient (`var(--primary)` → `var(--accent)`), pas de placeholder image |
| Pas de FAQ | Générer 5 FAQ pertinentes depuis le contexte produit |
| Pas de trust badges | Générer 3 badges génériques (ex: "Accès immédiat", "Satisfait ou remboursé", "Support email") |
| Produit gratuit (lead magnet) | Adapter : pas de prix affiché, CTA = "Télécharger gratuitement", ajouter un champ email si possible |
| business-profile.md absent | Continuer avec les réponses du context intake uniquement |
| Contenu trop court | Compléter avec du contenu généré depuis la description produit. Prévenir l'utilisateur |
| Style "premium" demandé | Basculer tout le design en dark mode via les CSS custom properties |

---

## Cross-Skill Integration

| Avant landing-page | Skill précédent | Quand |
|--------------------|-----------------|-------|
| Produit créé | `/dp-playbook-create` | Si on vend un ebook / playbook |
| Funnel conçu | `/dp-sales-funnel` | Pour connaître le rôle de la page dans le funnel |
| Profil business | `business-profile.md` | Pour les couleurs et infos de marque |

| Après landing-page | Skill suivant | Quand |
|--------------------|---------------|-------|
| Publicité Meta | `/dp-ad-angles-meta` | Pour générer les pubs qui pointent vers la page |
| Publicité Google | `/dp-ad-angles-google` | Pour les campagnes Search + Display |
| Séquence email | `/dp-email-sequence` | Pour envoyer du traffic email vers la page |
| Contenu social | `/dp-social-caption` `/dp-mediaplan` | Pour la promotion organique |
| Lead magnet | `/lead-magnet-create` | Si la page est un lead magnet, créer le contenu |
