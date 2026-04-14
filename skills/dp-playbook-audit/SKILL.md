---
name: dp-playbook-audit
description: "Audit qualité complet pour ebooks et playbooks HTML : structure, contenu, cohérence FR/EN, conformité design system, et standards professionnels. Génère un rapport détaillé avec score 0-100 et plan d'action priorisé. Triggers : audit, vérifier, qualité, check, review playbook, contrôle qualité."
user-invokable: true
argument-hint: "[fichier.html] [--compare fichier-ref.html] [--section section-id]"
allowed-tools: Read Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: production
  updated: 2026-04-13
---

# Playbook Audit — Contrôle Qualité Ebook

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake, quality gates, scoring 0-100, error handling, cross-skill integration -->

Expert en contrôle qualité de produits digitaux pour DP Créateur. Audite n'importe quel ebook HTML — structure, contenu, cohérence linguistique, conformité design system — et livre un rapport actionnable.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-playbook-audit [fichier.html]` | Audit complet du fichier spécifié |
| `/dp-playbook-audit` | Audit du playbook EN par défaut + comparaison FR |
| `/dp-playbook-audit [fichier] --compare [ref.html]` | Audit avec comparaison contre un fichier de référence |
| `/dp-playbook-audit [fichier] --section [id]` | Audit d'une seule section |
| `/dp-playbook-audit --quick [fichier]` | Audit rapide (structure + quality gates uniquement) |

## Output Format

```
LIVRABLE :
├── Rapport d'audit structuré (8 checks)
├── Score qualité 0-100 avec détail par catégorie
├── Issues classées par sévérité (Critical / High / Medium / Low)
├── Plan d'action priorisé
└── Recommandation : PUBLISH / REVISE / REWRITE
```

---

## Process

```
0. Load context       → business-profile.md + références
1. Read target file   → Lecture complète du fichier cible
2. Read reference     → Lecture du fichier FR de référence (si comparaison)
3. Run 8 checks       → Structural, Sections, Blocks, Nav, Consistency, Language, Design, Annexes
4. Score              → Calcul du score 0-100
5. Report             → Rapport structuré + plan d'action
```

---

## Step 0 — Context Loading (Silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom de marque, produit(s), prix, audience, couleurs
  → Utiliser ces infos pour vérifier la cohérence du contenu audité

SINON :
  → Continuer sans. L'audit se base sur le contenu tel quel.
```

Charger aussi les références internes si disponibles :
```
Read references/audit-checks.md      → Détail des 8 checks
Read references/scoring-criteria.md  → Barème de notation
Read references/audit-report-example.md → pour un exemple de rapport d'audit
```

---

## Step 1 — Context Intake

### 1a. Identifier le fichier cible

```
SI $ARGUMENTS contient un chemin de fichier :
  → Utiliser ce fichier comme cible
SI $ARGUMENTS est vide :
  → Chercher ebook en/*.html ou ebook fr/*.html
  → Si un seul fichier trouvé → l'utiliser
  → Si plusieurs → demander lequel auditer
SI le fichier n'existe pas :
  → Erreur claire + suggestion de chemin
```

### 1b. Questions si contexte manquant (max 2-3)

| # | Question | Quand |
|---|----------|-------|
| Q1 | Quel fichier veux-tu auditer ? (chemin complet) | Aucun fichier trouvé automatiquement |
| Q2 | Tu veux comparer avec un fichier de référence ? Si oui, lequel ? | Pas de version FR/EN évidente |
| Q3 | Audit complet ou focus sur une section ? | Fichier très long (>3000 lignes) |

---

## Step 2 — Les 8 Checks

### CHECK 1 — Intégrité Structurelle

Vérifier que le document HTML a :

- [ ] `<!DOCTYPE html>` et `<html lang="...">` avec attribut de langue correct
- [ ] `<head>` avec `<meta charset="UTF-8">`, viewport meta, et `<title>` descriptif
- [ ] Structure racine : `<article class="ebook">` → `<header>` → `<nav class="sommaire">` → `<main>` → sections
- [ ] Propriétés CSS custom définies dans `:root` (`--text`, `--muted`, `--bg`, `--accent`, `--border`)
- [ ] Toutes les classes CSS utilisées sont définies dans le bloc `<style>`
- [ ] Styles d'impression (`@media print`) présents
- [ ] Aucun CSS/JS externe (sauf Google Fonts)

**Scoring** : 15 points max. -3 par élément manquant critique. -1 par élément manquant mineur.

### CHECK 2 — Complétude des Sections

Lister toutes les sections `<section class="section" id="...">` trouvées dans le fichier.

Pour chaque section, vérifier :
- [ ] Elle existe (l'ID est présent dans le DOM)
- [ ] Elle n'est pas vide (contient du contenu significatif)
- [ ] Elle a au moins un `<h2>`
- [ ] Elle a au moins 2 sous-sections `<h3>`

Comparer avec le fichier de référence si disponible (même nombre de sections, même IDs).

**Scoring** : 15 points max. -2 par section manquante. -1 par section vide ou incomplète.

### CHECK 3 — Content Blocks

Chaque section principale doit contenir ces 3 types de blocs :

1. **Value Block** (ouverture) — `<div class="value-block">` — Ce que le lecteur va apprendre
2. **Tools Block** — `<div class="tools-block">` — Outils recommandés avec IDs (T1-T10, E1-E3)
3. **Recap Block** (fermeture) — `<div class="recap-block">` — Points clés (idéalement 3)

Compter chaque type et comparer avec la référence si disponible.

| Type de bloc | Attendu | Trouvé | Statut |
|-------------|---------|--------|--------|
| value-block | ? | ? | ? |
| tools-block | ? | ? | ? |
| recap-block | ? | ? | ? |

**Scoring** : 15 points max. -2 par bloc value/recap manquant. -1 par tools-block manquant.

### CHECK 4 — Navigation / Sommaire

- [ ] `<nav class="sommaire">` existe
- [ ] Chaque `<a href="#...">` pointe vers un ID existant
- [ ] Aucun lien interne cassé (tous les `href="#xxx"` ont un `id="xxx"` correspondant)
- [ ] Le sommaire liste toutes les sections principales

**Utiliser des commandes bash** pour vérifier les liens :
```bash
# Extraire tous les href="#xxx" du sommaire
# Extraire tous les id="xxx" du document
# Comparer les deux listes
```

**Scoring** : 10 points max. -3 par lien cassé. -1 par section non listée dans le sommaire.

### CHECK 5 — Cohérence de Contenu (FR vs EN)

Si deux versions sont disponibles, comparer :

- [ ] Même nombre de `<h2>`
- [ ] Même nombre de `<h3>`
- [ ] Même ordre de sections
- [ ] Toutes les références d'outils (T1-T10, E1-E3) présentes et identiques
- [ ] Formules KPI et métriques identiques (chiffres, pourcentages, taux)
- [ ] Scripts et templates tous traduits (pas laissés dans l'autre langue)
- [ ] Aucun texte non traduit restant
- [ ] Structure du plan d'action identique (mêmes jours, mêmes actions)

**Scoring** : 15 points max (0 si pas de comparaison — redistribuer sur les autres checks). -2 par écart structurel. -1 par contenu non traduit.

### CHECK 6 — Qualité Linguistique

- [ ] Ton professionnel et direct (voix DP Créateur)
- [ ] Pas de traductions maladroites ou calques littéraux
- [ ] Terminologie cohérente (même terme pour même concept)
- [ ] Pas de fautes de grammaire ou d'orthographe
- [ ] Pas de mots dans l'autre langue restés non traduits (sauf termes intentionnels)
- [ ] Tutoiement (FR) ou "you" (EN) cohérent

**Scoring** : 10 points max. -2 par problème de ton. -1 par erreur linguistique.

### CHECK 7 — Design & Formatting

- [ ] Labels `<h4>` corrects dans les blocs :
  - Value blocks : "Ce que tu vas apprendre" (FR) / "Value of this section" (EN)
  - Recap blocks : "À retenir" (FR) / "Key takeaway" (EN)
  - Tools blocks : "Outil principal" / "Primary tool" ou similaire
- [ ] Listes single-item : `<ul class="single-item">`
- [ ] Pas de tags HTML orphelins ou non fermés
- [ ] Indentation cohérente
- [ ] Pas de styles inline (tout dans `<style>`)
- [ ] Couleurs cohérentes avec l'identité visuelle (si business-profile.md spécifie des couleurs)

**Scoring** : 10 points max. -2 par problème de structure HTML. -1 par incohérence de formatting.

### CHECK 8 — Annexes / Contenu Complémentaire

Vérifier que les annexes (si présentes) contiennent du contenu substantiel :

- [ ] Chaque annexe a un ID unique
- [ ] Chaque annexe a du contenu (pas de placeholder)
- [ ] Le sommaire référence les annexes

**Scoring** : 10 points max. -2 par annexe vide. -1 par annexe manquante du sommaire.

---

## Step 3 — Calcul du Score

```
QUALITY SCORE : [XX]/100

Intégrité structurelle  : [XX]/15  (CHECK 1)
Complétude sections     : [XX]/15  (CHECK 2)
Content blocks          : [XX]/15  (CHECK 3)
Navigation              : [XX]/10  (CHECK 4)
Cohérence FR/EN         : [XX]/15  (CHECK 5)
Qualité linguistique    : [XX]/10  (CHECK 6)
Design & Formatting     : [XX]/10  (CHECK 7)
Annexes                 : [XX]/10  (CHECK 8)

Statut :
  90-100 → PUBLISH    (prêt à publier)
  70-89  → REVISE     (corrections nécessaires)
  <70    → REWRITE    (problèmes majeurs)
```

### Seuils de publication

| Score | Verdict | Action |
|-------|---------|--------|
| 90-100 | ✅ PRÊT À PUBLIER | Publier tel quel |
| 70-89 | ⚠️ PUBLIABLE AVEC RÉSERVES | Corriger les issues High avant publication |
| 50-69 | ❌ NÉCESSITE DES CORRECTIONS | Corriger les issues Critical et High |
| 0-49 | 🚫 NON PUBLIABLE | Restructuration ou réécriture majeure nécessaire |

**Hard gate** : Score < 70 = NE PAS recommander la publication. Orienter vers `/dp-playbook-section` pour corrections.

Pour un playbook, vérifier que le total estimé atteint 60+ pages (≥ 21000 mots).

Si pas de comparaison FR/EN, redistribuer les 15 points du CHECK 5 :
- +5 à CHECK 2, +5 à CHECK 3, +5 à CHECK 6.

---

## Step 4 — Rapport Final

```
# PLAYBOOK AUDIT REPORT
File: [chemin]
Date: [date]
Auditor: playbook-audit v2.0

## SUMMARY
- Overall Score: [XX]/100
- Status: PUBLISH / REVISE / REWRITE
- Critical Issues: [count]
- High Issues: [count]
- Medium Issues: [count]
- Low Issues: [count]

## DETAIL PAR CHECK
[Résultat de chaque check avec numéros de ligne]

## ACTION ITEMS
Priority 1 (Critical) — Bloquer la publication :
  - [item avec numéro de ligne]

Priority 2 (High) — Corriger avant publication :
  - [item avec numéro de ligne]

Priority 3 (Medium) — Amélioration recommandée :
  - [item]

Priority 4 (Low) — Nice to have :
  - [item]

## PROCHAINES ÉTAPES
  → /dp-playbook-sync    Corriger les écarts FR/EN
  → /dp-playbook-section Réécrire une section spécifique
  → /dp-copy-review      Audit du copywriting
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Toujours lire le fichier EN ENTIER avant d'auditer — ne jamais sauter de sections | Critical |
| QG-02 | Utiliser des commandes bash pour compter les éléments — ne jamais estimer | Critical |
| QG-03 | Référencer les numéros de ligne spécifiques pour chaque issue | Critical |
| QG-04 | Le fichier FR est TOUJOURS la référence — si EN diffère de FR, EN est faux (sauf localisation volontaire) | Critical |
| QG-05 | Ne RIEN corriger — seulement rapporter. Le skill `/dp-playbook-sync` gère les corrections | Critical |
| QG-06 | Chaque issue doit avoir : description + localisation + sévérité + action recommandée | High |
| QG-07 | Le score doit être honnête — un 95/100 est rare. La plupart des premiers drafts sont 60-80. | High |
| QG-08 | Vérifier la cohérence des couleurs avec business-profile.md si disponible | Medium |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Fichier non trouvé | Message d'erreur clair + `Glob` pour chercher des fichiers HTML dans le projet |
| Fichier vide | Signaler immédiatement, score 0, recommander `/dp-playbook-create` |
| Fichier non-HTML | Détecter et refuser poliment + suggérer le bon skill |
| Pas de fichier de référence FR | Auditer sans comparaison, redistribuer les points |
| Fichier trop gros (>5000 lignes) | Proposer audit par section ou mode `--quick` |
| business-profile.md absent | Continuer sans vérification de cohérence business |
| Encoding non-UTF8 | Signaler comme issue Critical dans CHECK 1 |
| CSS manquant | Signaler comme issue Critical, recommander de charger `references/design-system.md` |

---

## Méthode d'Exécution — Commandes Bash

Pour des comptages précis, utiliser ces commandes :

```bash
# Compter les éléments structurels
grep -c '<h2>' fichier.html
grep -c '<h3>' fichier.html
grep -c 'class="value-block"' fichier.html
grep -c 'class="tools-block"' fichier.html
grep -c 'class="recap-block"' fichier.html

# Extraire les IDs de section
grep -oP 'id="[^"]*"' fichier.html

# Vérifier les liens du sommaire
grep -oP 'href="#[^"]*"' fichier.html

# Chercher du texte non traduit (FR dans EN)
grep -n 'À retenir\|Valeur de cette section\|Outil principal' fichier-en.html

# Chercher des placeholders
grep -n '\[TODO\]\|\[INSERT\]\|\[EXAMPLE\]\|Lorem ipsum' fichier.html
```

---

## Cross-Skill Integration

| Avant playbook-audit | Skill précédent | Quand |
|----------------------|-----------------|-------|
| Créer un ebook | `/dp-playbook-create` | Avant le premier audit |
| Écrire une section | `/dp-playbook-section` | Avant d'auditer une nouvelle section |

| Après playbook-audit | Skill suivant | Quand |
|---------------------|---------------|-------|
| Corriger les écarts FR/EN | `/dp-playbook-sync` | Issues de cohérence trouvées |
| Réécrire une section | `/dp-playbook-section` | Section avec score bas |
| Revoir le copywriting | `/dp-copy-review` | Issues de voix ou persuasion |
| Exporter en PDF | `/export-pdf` | Score >= 90, prêt à publier |
