---
name: dp-export-pdf
description: "Convertit tout fichier HTML en PDF professionnel prêt à vendre. Gère la pagination, les headers/footers, la couverture, la table des matières cliquable, et les liens actifs. Supporte les ebooks, guides, lead magnets, checklists et landing pages. Triggers: export, pdf, convertir, print, télécharger, ebook pdf, exporter."
user-invokable: true
argument-hint: "[fichier.html] [format: a4|letter|ebook] [--cover] [--toc]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: production
  updated: 2026-04-13
---

# Export PDF — Convertisseur HTML → PDF Professionnel

<!-- v2.0.0 | 2026-04-13 | Création : conversion HTML→PDF, pagination, couverture, TOC, liens actifs -->

Convertit les fichiers HTML générés par les skills DP Créateur en PDF professionnels prêts à vendre ou distribuer. Le PDF est le format standard pour les ebooks, guides et lead magnets vendus en ligne.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-export-pdf browser [fichier.html]` | Conversion via navigateur (aucune installation) |
| `/dp-export-pdf [fichier.html]` | Conversion simple avec options par défaut |
| `/dp-export-pdf [fichier.html] --cover` | Avec page de couverture |
| `/dp-export-pdf [fichier.html] --toc` | Avec table des matières cliquable |
| `/dp-export-pdf [fichier.html] --full` | Cover + TOC + headers/footers + numéros de page |
| `/dp-export-pdf batch [dossier]` | Convertir tous les HTML d'un dossier |
| `/dp-export-pdf check` | Vérifier que les outils de conversion sont installés |

## Output Format

```
LIVRABLE :
├── exports/[slug].pdf
│   ├── PDF professionnel haute qualité
│   ├── Pagination automatique
│   ├── Liens internes et externes cliquables
│   ├── Table des matières cliquable (optionnel)
│   ├── Page de couverture (optionnel)
│   ├── Headers/footers avec numéros de page (optionnel)
│   └── Optimisé pour impression et écran
└── Prêt à uploader sur Gumroad, LemonSqueezy, etc.
```

---

## Process

```
1. Vérifier les outils     → Puppeteer, Playwright, ou wkhtmltopdf installé ?
2. Context intake           → Fichier source, options de format
3. Préparer le HTML         → Injecter les styles print, cover, TOC
4. Convertir                → HTML → PDF via le moteur disponible
5. Vérifier                 → Pages, taille, liens, qualité
6. Deliver                  → Fichier PDF + résumé
```

---

## Step 1 — Vérifier les Outils de Conversion

Tester dans cet ordre de préférence :

```bash
# Option 1 (recommandée) : Puppeteer / Playwright (via Node.js)
which node && npm list -g puppeteer 2>/dev/null || npm list puppeteer 2>/dev/null

# Option 2 : Playwright
which playwright 2>/dev/null

# Option 3 : wkhtmltopdf
which wkhtmltopdf 2>/dev/null

# Option 4 : Prince (commercial, haute qualité)
which prince 2>/dev/null

# Option 5 : Chrome/Chromium headless
which google-chrome 2>/dev/null || which chromium 2>/dev/null || which chromium-browser 2>/dev/null
```

### Si aucun outil n'est installé

Guider l'installation :

```
AUCUN OUTIL PDF DÉTECTÉ

Options d'installation (choisis celle qui te convient) :

Option 1 — Puppeteer (recommandé, Node.js requis) :
  npm install -g puppeteer

Option 2 — wkhtmltopdf (standalone, pas de Node requis) :
  macOS  : brew install wkhtmltopdf
  Ubuntu : sudo apt-get install wkhtmltopdf
  Windows: télécharger depuis wkhtmltopdf.org

Option 3 — Chrome headless (si Chrome est déjà installé) :
  Aucune installation supplémentaire nécessaire.

Quel outil tu veux installer ? Ou tu en as déjà un ?
```

---

## Step 2 — Context Intake

### 2a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : couleurs (primary_color, accent_color), nom de marque, logo, style visuel
  → Pré-remplir les options de couverture (couleurs, nom auteur, marque)
  → Ne PAS reposer les questions déjà couvertes par le profil
Read references/print-css-troubleshooting.md → pour résoudre les problèmes d'impression courants

SINON :
  → Continuer sans. Les questions de l'intake couvriront le minimum.
```

### 2b. Questions

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Quel **fichier HTML** veux-tu convertir ? (chemin ou "je vais le chercher") | Source |
| Q2 | Quel **format de page** ? `a4` (210×297mm, standard EU) / `letter` (8.5×11in, standard US) / `ebook` (6×9in, format livre) | Mise en page |
| Q3 | Tu veux une **page de couverture** ? (oui/non) | Apparence pro |
| Q4 | Tu veux des **numéros de page** et headers/footers ? (oui/non) | Navigation |

#### Si couverture = oui

| # | Question | Pourquoi |
|---|----------|----------|
| Q5 | **Titre** à afficher sur la couverture ? (défaut : titre du HTML) | Personnalisation |
| Q6 | **Sous-titre** ? | Contexte |
| Q7 | **Nom de l'auteur** ? (défaut : depuis business-profile.md) | Attribution |
| Q8 | Tu as une **image de couverture** ? (chemin ou URL) Si non, je fais une couverture avec tes couleurs de marque. | Visuel |

---

## Step 3 — Préparer le HTML pour le PDF

### 3a. Injecter les styles print optimisés

Ajouter ou remplacer le `@media print` dans le HTML :

```css
@media print {
  /* --- Reset pour PDF --- */
  body {
    max-width: 100%;
    padding: 0;
    margin: 0;
    font-size: 11pt;
    line-height: 1.6;
    color: #000;
  }

  /* --- Pagination --- */
  .section {
    page-break-inside: avoid;
  }
  h2 {
    page-break-after: avoid;
    page-break-before: always;
  }
  h2:first-of-type {
    page-break-before: avoid;
  }
  h3 {
    page-break-after: avoid;
  }
  .value-block, .tools-block, .recap-block {
    page-break-inside: avoid;
  }

  /* --- Blocks pour impression --- */
  .value-block {
    border-left: 3pt solid [accent_color];
    background: #f8f8f8 !important;
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }
  .tools-block {
    border-left: 3pt solid #3b82f6;
    background: #f0f4ff !important;
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }
  .recap-block {
    border-left: 3pt solid #eab308;
    background: #fffef0 !important;
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
  }

  /* --- Liens --- */
  a {
    color: [primary_color] !important;
    text-decoration: underline;
  }

  /* --- TOC --- */
  .sommaire {
    page-break-after: always;
  }

  /* --- Images --- */
  img {
    max-width: 100%;
    page-break-inside: avoid;
  }
}
```

### 3b. Générer la page de couverture (si demandée)

Insérer avant le contenu principal :

```html
<div class="cover-page" style="
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  text-align: center;
  background: [primary_color];
  color: white;
  padding: 3rem;
  page-break-after: always;
">
  <!-- Logo si disponible -->
  <img src="[logo_url]" alt="[brand_name]" style="max-width: 120px; margin-bottom: 2rem;" />

  <h1 style="
    font-size: 2.8rem;
    font-weight: 800;
    line-height: 1.1;
    margin-bottom: 1rem;
    color: white;
    border: none;
  ">[Titre]</h1>

  <p style="
    font-size: 1.2rem;
    opacity: 0.9;
    margin-bottom: 3rem;
    max-width: 500px;
  ">[Sous-titre]</p>

  <div style="
    width: 60px;
    height: 3px;
    background: [accent_color];
    margin-bottom: 3rem;
  "></div>

  <p style="
    font-size: 1rem;
    font-weight: 600;
  ">[Auteur]</p>

  <p style="
    font-size: 0.8rem;
    opacity: 0.6;
    margin-top: auto;
  ">[brand_name] — [année]</p>
</div>
```

### 3c. Numéros de page et headers/footers

Pour Puppeteer/Playwright, passer en option :

```javascript
{
  displayHeaderFooter: true,
  headerTemplate: `
    <div style="font-size: 8pt; color: #999; width: 100%; text-align: center; padding: 5px 0;">
      [brand_name] — [Titre du document]
    </div>
  `,
  footerTemplate: `
    <div style="font-size: 8pt; color: #999; width: 100%; text-align: center; padding: 5px 0;">
      Page <span class="pageNumber"></span> / <span class="totalPages"></span>
    </div>
  `,
  margin: {
    top: '20mm',
    bottom: '20mm',
    left: '15mm',
    right: '15mm'
  }
}
```

---

## Step 4 — Convertir

### Méthode 0 : Navigateur (aucune installation requise)

La méthode la plus simple — fonctionne sur tout ordinateur avec Chrome, Edge, ou Firefox.

**Étapes :**
1. Ouvrir le fichier HTML dans Chrome (double-clic sur le fichier)
2. Vérifier que le rendu est correct (couleurs, mise en page, blocs)
3. `Ctrl+P` (ou `Cmd+P` sur Mac) → Imprimer
4. Destination : **"Enregistrer au format PDF"**
5. Paramètres recommandés :
   - Format : A4
   - Marges : Par défaut ou Aucune (selon le CSS)
   - Cocher **"Graphiques d'arrière-plan"** (CRITIQUE — sinon les blocs colorés disparaissent)
   - Décocher "En-têtes et pieds de page" (le CSS gère les numéros de page)
6. Cliquer "Enregistrer" → choisir le dossier `exports/`

**Avantages :** Aucune installation, fonctionne partout, résultat immédiat
**Limites :** Pas de numéros de page automatiques, pas de header/footer custom, pas de batch

> **C'est la méthode recommandée si tu n'es pas développeur.** Les méthodes ci-dessous (Puppeteer, wkhtmltopdf) offrent plus de contrôle mais nécessitent des installations techniques.

### Méthode 1 : Puppeteer (Node.js)

```javascript
// Créer le script de conversion
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch({ headless: true });
  const page = await browser.newPage();

  await page.goto(`file://${__dirname}/[fichier.html]`, {
    waitUntil: 'networkidle0'
  });

  await page.pdf({
    path: 'exports/[slug].pdf',
    format: '[A4|Letter]',
    printBackground: true,
    displayHeaderFooter: [true|false],
    headerTemplate: '[header]',
    footerTemplate: '[footer]',
    margin: { top: '20mm', bottom: '20mm', left: '15mm', right: '15mm' },
    preferCSSPageSize: false
  });

  await browser.close();
})();
```

```bash
node convert-pdf.js
```

### Méthode 2 : Chrome/Chromium Headless

```bash
google-chrome --headless --disable-gpu \
  --print-to-pdf="exports/[slug].pdf" \
  --no-pdf-header-footer \
  --print-to-pdf-no-header \
  "file://$(pwd)/[fichier.html]"
```

### Méthode 3 : wkhtmltopdf

```bash
wkhtmltopdf \
  --page-size [A4|Letter] \
  --margin-top 20mm \
  --margin-bottom 20mm \
  --margin-left 15mm \
  --margin-right 15mm \
  --enable-local-file-access \
  --print-media-type \
  --encoding UTF-8 \
  --footer-center "Page [page] / [topage]" \
  --footer-font-size 8 \
  --footer-spacing 10 \
  "[fichier.html]" \
  "exports/[slug].pdf"
```

### Méthode 4 : Playwright

```bash
npx playwright pdf "[fichier.html]" "exports/[slug].pdf" \
  --format A4 \
  --margin "20mm 15mm 20mm 15mm"
```

---

## Step 5 — Vérification

Après conversion, vérifier :

```bash
# Taille du fichier
ls -lh exports/[slug].pdf

# Nombre de pages (si pdfinfo disponible)
pdfinfo exports/[slug].pdf 2>/dev/null | grep Pages

# Ou via Python
python3 -c "
import subprocess
result = subprocess.run(['file', 'exports/[slug].pdf'], capture_output=True, text=True)
print(result.stdout)
"
```

### Checklist de vérification

```
[ ] Le PDF s'ouvre correctement
[ ] La couverture est présente (si demandée)
[ ] Les numéros de pages sont corrects
[ ] La table des matières est cliquable
[ ] Les liens internes fonctionnent
[ ] Les liens externes fonctionnent
[ ] Les blocs colorés (value/tools/recap) sont visibles
[ ] Le texte est lisible (pas coupé, pas trop petit)
[ ] Les images sont nettes (si applicable)
[ ] Le fichier pèse < 10 MB (sinon optimiser)
```

---

## Step 6 — Delivery

```
✅ PDF EXPORTÉ

📄 Fichier   : exports/[slug].pdf
📐 Format    : [A4 / Letter / Ebook 6×9]
📖 Pages     : [N]
📦 Taille    : [X] MB
🎨 Couverture: [Oui/Non]
📑 TOC       : [Cliquable/Non]
🔢 Numéros   : [Oui/Non]

PROCHAINES ÉTAPES :
  → Upload sur [Gumroad/LemonSqueezy/Stripe] pour la vente
  → /dp-landing-page      — Créer la page de vente
  → /dp-ebook-cover       — Créer un mockup 3D de la couverture
  → /dp-email-sequence    — Séquence de lancement
  → /dp-ad-angles-meta    — Campagne pub Facebook/Instagram
```

---

## Optimisation du PDF

### Si le fichier est trop lourd (> 5 MB)

```bash
# Compresser avec Ghostscript (si installé)
gs -sDEVICE=pdfwrite -dCompatibilityLevel=1.4 \
   -dPDFSETTINGS=/ebook \
   -dNOPAUSE -dBATCH -dQUIET \
   -sOutputFile="exports/[slug]-optimized.pdf" \
   "exports/[slug].pdf"
```

| Setting | Qualité | Taille | Usage |
|---------|---------|--------|-------|
| `/screen` | 72 dpi | Minimale | Preview en ligne |
| `/ebook` | 150 dpi | Bonne | Distribution digitale (recommandé) |
| `/printer` | 300 dpi | Élevée | Impression professionnelle |
| `/prepress` | 300+ dpi | Maximale | Impression haute qualité |

### Format Ebook (6×9 pouces)

Pour un format livre classique :

```javascript
// Puppeteer
await page.pdf({
  width: '6in',
  height: '9in',
  margin: { top: '0.75in', bottom: '0.75in', left: '0.75in', right: '0.75in' }
});
```

```bash
# wkhtmltopdf
wkhtmltopdf --page-width 152.4mm --page-height 228.6mm ...
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Au moins 1 outil de conversion installé et fonctionnel | Critical |
| QG-02 | Le fichier HTML source existe et est valide | Critical |
| QG-03 | Le PDF s'ouvre sans erreur | Critical |
| QG-04 | La taille du PDF est < 10 MB | High |
| QG-05 | Les liens de la TOC sont cliquables | High |
| QG-06 | Les couleurs des blocks sont préservées (print-color-adjust) | Medium |
| QG-07 | Pas de texte coupé en bas de page | High |
| QG-08 | Les numéros de page sont corrects | Medium |
| QG-09 | La couverture utilise les couleurs de marque | Medium |
| QG-10 | Le dossier exports/ est créé si inexistant | Low |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Aucun outil PDF installé | Guider l'installation (Puppeteer ou wkhtmltopdf) |
| Aucun outil technique installé | Utiliser la Méthode 0 (navigateur). Ouvrir le HTML dans Chrome → Ctrl+P → Enregistrer en PDF. Cocher "Graphiques d'arrière-plan". |
| Fichier HTML introuvable | Lister les fichiers disponibles dans ebook en/, ebook fr/, blog/ |
| Fonts Google non chargées | Ajouter un délai (`waitUntil: 'networkidle0'`) ou télécharger la font localement |
| Couleurs non imprimées | Vérifier `-webkit-print-color-adjust: exact` dans le CSS |
| PDF trop lourd | Proposer la compression Ghostscript |
| Liens cassés dans le PDF | Vérifier les ancres HTML (# → IDs existants) |
| Page blanche en trop | Vérifier les `page-break-before: always` en trop dans le CSS |
| Texte coupé entre pages | Ajouter `page-break-inside: avoid` aux sections concernées |
| Timeout de conversion | Augmenter le timeout, vérifier que le HTML n'est pas trop lourd |
| wkhtmltopdf ne supporte pas les CSS modernes | Passer à Puppeteer/Playwright |

---

## Cross-Skill Integration

| Avant | Skill | Quand |
|-------|-------|-------|
| Créer l'ebook | `/dp-playbook-create` | Le HTML doit exister |
| Auditer l'ebook | `/dp-playbook-audit` | Vérifier la qualité avant export |
| Créer la couverture | `/dp-ebook-cover` | Pour avoir l'image de cover |
| Profil business | `/dp-business-profile` | Pour les couleurs et le logo |

| Après | Skill | Quand |
|-------|-------|-------|
| Page de vente | `/dp-landing-page` | Pour vendre le PDF |
| Mise en ligne | `/dp-product-listing` | Pour préparer l'upload |
| Séquence email | `/dp-email-sequence` | Pour le lancement |
| Publicité | `/dp-ad-angles-meta` `/dp-ad-angles-google` | Pour l'acquisition |
