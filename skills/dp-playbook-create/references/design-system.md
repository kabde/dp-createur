# Design System — DP Créateur Ebook CSS

## Usage

Ce CSS utilise des **CSS custom properties** (variables) pour permettre la personnalisation des couleurs de marque. Remplacer les valeurs dans `:root` par les couleurs choisies par l'utilisateur.

## Personnalisation des couleurs

Les variables à adapter selon l'identité visuelle du client :

```css
:root {
  /* --- COULEURS DE MARQUE (à personnaliser) --- */
  --color-primary: #111111;         /* Couleur principale — titres, accents forts */
  --color-accent: #22c55e;          /* Couleur d'accent — CTA, highlights */
  --color-accent-light: #f0fdf4;    /* Accent très clair — fond des blocks */
  --color-accent-dark: #166534;     /* Accent foncé — texte sur fond accent */
  --color-accent-mid: #16a34a;      /* Accent moyen — labels, h4 dans blocks */
  
  /* --- COULEURS SYSTÈME (rarement modifiées) --- */
  --color-text: #1a1a1a;            /* Texte principal */
  --color-text-secondary: #333;     /* Texte secondaire */
  --color-text-muted: #888;         /* Texte discret (labels h4) */
  --color-bg: #ffffff;              /* Fond de page */
  --color-bg-subtle: #fafafa;       /* Fond subtil (sommaire, hover) */
  --color-border: #e5e5e5;          /* Bordures */
  --color-border-light: #f0f0f0;    /* Bordures légères */
  
  /* --- BLOCKS — Couleurs secondaires --- */
  --color-tools-bg: #eff6ff;        /* Fond tools-block */
  --color-tools-border: #3b82f6;    /* Bordure tools-block */
  --color-tools-text: #1e40af;      /* Texte tools-block */
  --color-tools-label: #2563eb;     /* Label tools-block */
  
  --color-recap-bg: #fefce8;        /* Fond recap-block */
  --color-recap-border: #eab308;    /* Bordure recap-block */
  --color-recap-text: #854d0e;      /* Texte recap-block */
  --color-recap-label: #a16207;     /* Label recap-block */
}
```

### Palettes pré-configurées par style

**Minimaliste (défaut)** — Noir + vert, épuré
```css
--color-primary: #111111;
--color-accent: #22c55e;
```

**Bold** — Contrastes forts, énergie
```css
--color-primary: #1e1e1e;
--color-accent: #f43f5e;         /* Rose vif */
--color-accent-light: #fff1f2;
--color-accent-dark: #9f1239;
--color-accent-mid: #e11d48;
```

**Premium** — Sobre, élégant, confiance
```css
--color-primary: #1e3a5f;        /* Bleu nuit */
--color-accent: #d4a853;         /* Or */
--color-accent-light: #fdf8ef;
--color-accent-dark: #92722a;
--color-accent-mid: #b8942e;
```

**Warm** — Tons chauds, accueillant
```css
--color-primary: #3d2c1e;        /* Brun profond */
--color-accent: #e07a3a;         /* Orange terre */
--color-accent-light: #fef3ec;
--color-accent-dark: #9a4a14;
--color-accent-mid: #c4621e;
```

### Générer une palette custom à partir de 2 couleurs

Si l'utilisateur donne seulement une couleur primaire et un accent :

```
--color-primary:      [couleur donnée]
--color-accent:       [accent donné]
--color-accent-light: [accent à 5% opacité sur blanc]
--color-accent-dark:  [accent assombri de 40%]
--color-accent-mid:   [accent assombri de 20%]
```

## CSS Complet (avec variables)

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');

:root {
  --color-primary: #111111;
  --color-accent: #22c55e;
  --color-accent-light: #f0fdf4;
  --color-accent-dark: #166534;
  --color-accent-mid: #16a34a;
  --color-text: #1a1a1a;
  --color-text-secondary: #333;
  --color-text-muted: #888;
  --color-bg: #ffffff;
  --color-bg-subtle: #fafafa;
  --color-border: #e5e5e5;
  --color-border-light: #f0f0f0;
  --color-tools-bg: #eff6ff;
  --color-tools-border: #3b82f6;
  --color-tools-text: #1e40af;
  --color-tools-label: #2563eb;
  --color-recap-bg: #fefce8;
  --color-recap-border: #eab308;
  --color-recap-text: #854d0e;
  --color-recap-label: #a16207;
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: 'Inter', system-ui, -apple-system, sans-serif;
  line-height: 1.7;
  color: var(--color-text);
  background: var(--color-bg);
  max-width: 780px;
  margin: 0 auto;
  padding: 2rem 1.5rem;
}

/* --- Typography --- */
h1 { font-size: 2.2rem; font-weight: 800; line-height: 1.15; margin-bottom: 0.5rem; letter-spacing: -0.03em; color: var(--color-primary); }
h2 { font-size: 1.6rem; font-weight: 700; margin-top: 3rem; margin-bottom: 1rem; letter-spacing: -0.02em; color: var(--color-primary); border-bottom: 2px solid var(--color-primary); padding-bottom: 0.5rem; }
h3 { font-size: 1.15rem; font-weight: 600; margin-top: 2rem; margin-bottom: 0.5rem; color: var(--color-primary); }
h4 { font-size: 0.8rem; font-weight: 700; text-transform: uppercase; letter-spacing: 0.08em; color: var(--color-text-muted); margin-bottom: 0.5rem; }

p { margin-bottom: 1rem; color: var(--color-text-secondary); }
strong { font-weight: 600; color: var(--color-primary); }

/* --- Lists --- */
ul, ol { margin: 0.75rem 0 1.25rem 1.5rem; }
li { margin-bottom: 0.4rem; color: var(--color-text-secondary); }
ul.single-item { list-style: none; margin-left: 0; }
ul.single-item li { padding: 0.5rem 0; border-bottom: 1px solid var(--color-border-light); }
ul.single-item li:last-child { border-bottom: none; }

/* --- Value Block (ouverture de section) --- */
.value-block {
  background: var(--color-accent-light);
  border-left: 4px solid var(--color-accent);
  padding: 1rem 1.25rem;
  margin: 1.5rem 0;
  border-radius: 0 8px 8px 0;
}
.value-block h4 { color: var(--color-accent-mid); }
.value-block p { color: var(--color-accent-dark); margin-bottom: 0; }

/* --- Tools Block (recommandation d'outils) --- */
.tools-block {
  background: var(--color-tools-bg);
  border-left: 4px solid var(--color-tools-border);
  padding: 1rem 1.25rem;
  margin: 1.5rem 0;
  border-radius: 0 8px 8px 0;
}
.tools-block h4 { color: var(--color-tools-label); }
.tools-block p, .tools-block li { color: var(--color-tools-text); }

/* --- Recap Block (fermeture de section) --- */
.recap-block {
  background: var(--color-recap-bg);
  border-left: 4px solid var(--color-recap-border);
  padding: 1rem 1.25rem;
  margin: 1.5rem 0;
  border-radius: 0 8px 8px 0;
}
.recap-block h4 { color: var(--color-recap-label); }
.recap-block li { color: var(--color-recap-text); }

/* --- Layout --- */
.section { margin-bottom: 3rem; }
.ebook { max-width: 100%; }

/* --- Table of Contents --- */
.sommaire {
  background: var(--color-bg-subtle);
  border: 1px solid var(--color-border);
  border-radius: 10px;
  padding: 1.5rem 2rem;
  margin: 2rem 0 3rem;
}
.sommaire-title { font-size: 1.1rem; border-bottom: none; margin-top: 0; padding-bottom: 0; }
.sommaire ol { counter-reset: sommaire; list-style: none; margin-left: 0; }
.sommaire li { counter-increment: sommaire; padding: 0.3rem 0; }
.sommaire li::before { content: counter(sommaire, decimal-leading-zero); font-weight: 700; color: var(--color-text-muted); margin-right: 0.75rem; font-size: 0.85rem; }
.sommaire a { color: var(--color-primary); text-decoration: none; border-bottom: 1px solid var(--color-border); transition: border-color 0.2s; }
.sommaire a:hover { border-color: var(--color-primary); }

/* --- Tool Reference Badges --- */
.tool-id {
  display: inline-block;
  background: var(--color-primary);
  color: var(--color-bg);
  font-size: 0.7rem;
  font-weight: 700;
  padding: 0.15rem 0.45rem;
  border-radius: 4px;
  margin-right: 0.25rem;
  vertical-align: middle;
}

/* --- Print / PDF Optimization --- */
@media print {
  body { max-width: 100%; padding: 0; font-size: 11pt; }
  .section { page-break-inside: avoid; }
  h2 { page-break-after: avoid; }
  .value-block, .tools-block, .recap-block { page-break-inside: avoid; }
  .sommaire { page-break-after: always; }
  .sommaire a { border-bottom: none; }
  .tool-id { background: #333; }
}

/* --- Responsive --- */
@media (max-width: 640px) {
  body { padding: 1rem; }
  h1 { font-size: 1.7rem; }
  h2 { font-size: 1.3rem; }
  .sommaire { padding: 1rem 1.25rem; }
}
```

## Block Usage Guide

| Block | Variable couleur | Quand l'utiliser |
|-------|-----------------|-----------------|
| `.value-block` | `--color-accent-*` | Ouverture de chaque section h2 — ce que le lecteur va apprendre |
| `.tools-block` | `--color-tools-*` | Quand un outil/logiciel est recommandé pour la section |
| `.recap-block` | `--color-recap-*` | Fermeture de chaque section h2 — 3 points clés à retenir |

## HTML Template Minimal

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Titre] — [Marque]</title>
  <style>
    /* COLLER LE CSS CI-DESSUS */
    /* REMPLACER LES VARIABLES :root PAR LES COULEURS DU CLIENT */
  </style>
</head>
<body>
  <article class="ebook">
    <header class="section" id="debut">
      <h1>[Titre]</h1>
      <p>[Sous-titre]</p>
    </header>
    <nav class="sommaire" aria-label="Table des matières">
      <h2 class="sommaire-title">Table des matières</h2>
      <ol>
        <li><a href="#debut">Lis ça d'abord</a></li>
        <!-- ... -->
      </ol>
    </nav>
    <main>
      <!-- Sections -->
    </main>
  </article>
</body>
</html>
```
