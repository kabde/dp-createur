# Print CSS Troubleshooting — Problèmes Courants et Solutions

> Référence pour le skill `/dp-export-pdf`
> Produit exemple : **Le Playbook du Coach Fitness** (47 EUR)

---

## Problème 1 — Les couleurs de fond ne s'impriment pas

**Symptôme** : Les blocs `value-block`, `tools-block` et `recap-block` apparaissent blancs dans le PDF alors qu'ils ont un background coloré à l'écran.

**Cause** : Par défaut, les navigateurs n'impriment pas les couleurs de fond pour économiser l'encre.

**Solution** :

```css
@media print {
  .value-block,
  .tools-block,
  .recap-block {
    -webkit-print-color-adjust: exact;
    print-color-adjust: exact;
    background: #f8f8f8 !important; /* Forcer le fond */
  }

  .value-block { border-left: 3pt solid var(--accent); }
  .tools-block { border-left: 3pt solid #3b82f6; }
  .recap-block { border-left: 3pt solid #eab308; }
}
```

**Note Puppeteer** : Ajouter `printBackground: true` dans les options `page.pdf()`.

---

## Problème 2 — Les sauts de page coupent les blocs en deux

**Symptôme** : Un `recap-block` ou un `value-block` commence en bas d'une page et se termine en haut de la suivante. Le contenu est illisible.

**Cause** : Pas de règles `page-break` définies pour les blocs importants.

**Solution** :

```css
@media print {
  /* Empêcher les coupures dans les blocs */
  .value-block,
  .tools-block,
  .recap-block,
  .section {
    page-break-inside: avoid;
  }

  /* Chaque section h2 commence sur une nouvelle page */
  h2 {
    page-break-before: always;
    page-break-after: avoid;
  }

  /* Sauf le premier h2 (pas de page blanche au début) */
  h2:first-of-type {
    page-break-before: avoid;
  }

  /* Un h3 ne doit pas être seul en bas de page */
  h3 {
    page-break-after: avoid;
  }
}
```

---

## Problème 3 — Les images débordent de la page

**Symptôme** : Une image large dépasse la marge droite ou est coupée dans le PDF.

**Cause** : L'image a une largeur fixe en pixels qui excède la largeur de la zone imprimable.

**Solution** :

```css
@media print {
  img {
    max-width: 100%;
    height: auto;
    page-break-inside: avoid;
  }

  /* Si l'image est dans un conteneur flex ou grid */
  figure,
  .image-container {
    max-width: 100%;
    page-break-inside: avoid;
  }
}
```

**Conseil** : Pour les ebooks DP Créateur, éviter les images (le design repose sur le texte et les blocs CSS). Si une image est indispensable, la dimensionner en `%` ou en `rem`, jamais en `px` fixes.

---

## Problème 4 — Le rendu des polices est différent dans le PDF

**Symptôme** : Le texte apparaît plus fin, plus épais, ou dans une police différente de ce qu'on voit à l'écran.

**Cause** : La Google Font n'est pas chargée au moment de la conversion PDF (problème de timing réseau) ou la police de fallback est utilisée.

**Solution** :

```css
/* Utiliser une stack de polices fiable avec fallback */
body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont,
               'Segoe UI', Roboto, sans-serif;
}
```

**Solution Puppeteer** :

```javascript
// Attendre que toutes les ressources (dont les fonts) soient chargées
await page.goto(`file://${filePath}`, {
  waitUntil: 'networkidle0',
  timeout: 30000
});

// Attendre un court délai supplémentaire pour le rendu des fonts
await page.evaluate(() => document.fonts.ready);
```

**Alternative** : Télécharger le fichier `.woff2` de la police et l'embarquer en base64 dans le CSS pour éliminer toute dépendance réseau.

---

## Problème 5 — Le header/footer chevauche le contenu

**Symptôme** : Le header ("FitPro Academy -- Le Playbook du Coach Fitness") ou le footer ("Page 3 / 47") chevauche la première ou dernière ligne de contenu d'une page.

**Cause** : Les marges ne sont pas assez grandes pour accueillir le header/footer.

**Solution Puppeteer** :

```javascript
await page.pdf({
  displayHeaderFooter: true,
  headerTemplate: `
    <div style="font-size: 8pt; color: #999;
                width: 100%; text-align: center;
                padding: 5px 0;">
      FitPro Academy — Le Playbook du Coach Fitness
    </div>
  `,
  footerTemplate: `
    <div style="font-size: 8pt; color: #999;
                width: 100%; text-align: center;
                padding: 5px 0;">
      Page <span class="pageNumber"></span> / <span class="totalPages"></span>
    </div>
  `,
  margin: {
    top: '25mm',    /* Augmenter si le header chevauche */
    bottom: '25mm', /* Augmenter si le footer chevauche */
    left: '15mm',
    right: '15mm'
  }
});
```

**Solution wkhtmltopdf** :

```bash
wkhtmltopdf \
  --margin-top 25mm \
  --margin-bottom 25mm \
  --header-spacing 10 \
  --footer-spacing 10 \
  ...
```

---

## Problème 6 — Les tableaux sont coupés entre deux pages

**Symptôme** : Un tableau commence sur une page et la ligne suivante apparaît sur la page suivante, sans en-têtes de colonnes répétés. Le lecteur perd le contexte.

**Cause** : Les navigateurs ne gèrent pas bien la pagination des tableaux par défaut.

**Solution** :

```css
@media print {
  /* Empêcher la coupure dans les petits tableaux */
  table {
    page-break-inside: avoid;
  }

  /* Pour les grands tableaux : répéter les en-têtes */
  thead {
    display: table-header-group;
  }

  /* Empêcher la coupure au milieu d'une ligne */
  tr {
    page-break-inside: avoid;
  }

  /* Si le tableau est trop grand pour une page,
     accepter la coupure mais forcer la répétition du thead */
  table.allow-split {
    page-break-inside: auto;
  }
}
```

**Conseil** : Dans les playbooks DP Créateur, préférer les listes (`<ul>`, `<ol>`) aux tableaux quand c'est possible. Les listes se paginent beaucoup mieux.

---

## Checklist Rapide Avant Export

```
[ ] @media print est présent dans le <style>
[ ] print-color-adjust: exact sur tous les blocs colorés
[ ] page-break-inside: avoid sur value-block, tools-block, recap-block
[ ] page-break-before: always sur les h2 (sauf le premier)
[ ] page-break-after: avoid sur les h2 et h3
[ ] img { max-width: 100%; height: auto; }
[ ] Marges suffisantes pour header/footer (25mm min si activés)
[ ] Font stack avec fallback (pas de dépendance unique à Google Fonts)
[ ] thead { display: table-header-group; } pour les grands tableaux
[ ] Tester avec Puppeteer printBackground: true
```

---

## Commande de Test Rapide

```bash
# Vérifier que le CSS print est présent dans le fichier HTML
grep -c '@media print' fichier.html
# Attendu : >= 1

# Vérifier que print-color-adjust est défini
grep -c 'print-color-adjust' fichier.html
# Attendu : >= 1

# Vérifier que page-break est défini
grep -c 'page-break' fichier.html
# Attendu : >= 3
```
