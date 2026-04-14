# Audit Checks — Référence Détaillée

## CHECK 1 — Intégrité Structurelle

### Éléments requis

| Élément | Validation | Sévérité si absent |
|---------|-----------|-------------------|
| `<!DOCTYPE html>` | Première ligne du fichier | Critical |
| `<html lang="fr">` ou `<html lang="en">` | Attribut lang correct | Critical |
| `<meta charset="UTF-8">` | Dans `<head>` | Critical |
| `<meta name="viewport">` | Dans `<head>` | High |
| `<title>` descriptif | Pas vide, pas "Untitled" | High |
| `<article class="ebook">` | Wrapper racine | Critical |
| `<header>` | Dans article | High |
| `<nav class="sommaire">` | Après header | Critical |
| `<main>` | Contient les sections | High |
| CSS `:root` variables | `--text`, `--muted`, `--bg`, `--accent`, `--border` | High |
| `@media print` | Styles d'impression | Medium |
| Pas de CSS/JS externe | Sauf Google Fonts | Critical |

### Commandes bash

```bash
# Vérifier DOCTYPE
head -1 fichier.html | grep -c 'DOCTYPE'

# Vérifier lang
grep -c '<html lang=' fichier.html

# Vérifier CSS variables
grep -c '\-\-text\|\-\-muted\|\-\-bg\|\-\-accent\|\-\-border' fichier.html

# Vérifier print styles
grep -c '@media print' fichier.html

# Chercher CSS/JS externes
grep -n '<link rel="stylesheet"\|<script src=' fichier.html
```

## CHECK 2 — Complétude des Sections

### Validation par section

Pour chaque `<section class="section" id="...">` :

1. L'ID existe dans le DOM
2. La section contient au moins un `<h2>`
3. La section contient au moins 2 `<h3>`
4. La section n'est pas vide (> 100 mots de contenu)

### Commandes bash

```bash
# Lister tous les IDs de section
grep -oP 'class="section" id="\K[^"]*' fichier.html

# Compter les h2
grep -c '<h2>' fichier.html

# Compter les h3
grep -c '<h3>' fichier.html
```

## CHECK 3 — Content Blocks

### Blocs attendus

| Bloc | Classe | Requis dans | h4 FR | h4 EN |
|------|--------|-------------|-------|-------|
| Value | `value-block` | Chaque section principale | Ce que tu vas apprendre | Value of this section |
| Tools | `tools-block` | Si outils pertinents | Outil recommandé | Primary tool |
| Recap | `recap-block` | Chaque section principale | À retenir | Key takeaway |

### Commandes bash

```bash
grep -c 'class="value-block"' fichier.html
grep -c 'class="tools-block"' fichier.html
grep -c 'class="recap-block"' fichier.html
```

## CHECK 4 — Navigation

### Validation des liens

1. Extraire tous les `href="#..."` du sommaire
2. Extraire tous les `id="..."` du document
3. Chaque href doit avoir un ID correspondant
4. Chaque section principale doit avoir un lien dans le sommaire

### Commandes bash

```bash
# Extraire les hrefs du sommaire (entre <nav> et </nav>)
grep -oP 'href="#\K[^"]*' fichier.html

# Extraire tous les IDs
grep -oP 'id="\K[^"]*' fichier.html

# Trouver les hrefs sans ID correspondant (liens cassés)
# Comparer les deux listes
```

## CHECK 5 — Cohérence FR/EN

### Éléments à comparer

| Élément | Commande bash |
|---------|---------------|
| h2 | `grep -c '<h2>' fichier.html` |
| h3 | `grep -c '<h3>' fichier.html` |
| value-block | `grep -c 'class="value-block"' fichier.html` |
| tools-block | `grep -c 'class="tools-block"' fichier.html` |
| recap-block | `grep -c 'class="recap-block"' fichier.html` |
| tool-id spans | `grep -c 'class="tool-id"' fichier.html` |
| Section IDs | `grep -c 'class="section"' fichier.html` |

### Texte non traduit à chercher

```bash
# FR dans EN
grep -n 'À retenir\|Valeur de cette section\|Outil principal\|Sommaire\|Dernière note' fichier-en.html

# EN dans FR
grep -n 'Key takeaway\|Value of this section\|Primary tool\|Table of Contents' fichier-fr.html
```

## CHECK 6 — Qualité Linguistique

### Patterns à détecter

| Pattern | Sévérité | Exemple |
|---------|----------|---------|
| Voix passive | High | "Results can be achieved" → "You'll get results" |
| Filler words | Medium | "It's important to note that" → supprimer |
| Hedge words | Medium | "maybe", "might", "could possibly" |
| Hype words | High | "revolutionary", "game-changing" |
| Mots non traduits | Critical | FR dans EN ou EN dans FR |

## CHECK 7 — Design & Formatting

### Patterns HTML corrects

| Pattern | Correct | Incorrect |
|---------|---------|-----------|
| Listes single-item | `<ul class="single-item">` | `<ul>` pour un seul item |
| Bold | `<strong>` | `<b>` |
| Italic | `<em>` | `<i>` |
| Espacement | `<p>` | `<br>` |
| Styles | Dans `<style>` | Inline `style="..."` |

### Commandes bash

```bash
# Chercher des styles inline
grep -cn 'style="' fichier.html

# Chercher des <b> ou <i>
grep -cn '<b>\|<i>' fichier.html

# Chercher des <br>
grep -cn '<br' fichier.html
```

## CHECK 8 — Annexes

### Validation

Pour chaque annexe :
1. L'ID existe (`annexe-a`, etc.)
2. Du contenu substantiel est présent (pas juste un titre)
3. Le sommaire référence l'annexe
