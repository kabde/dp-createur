---
name: dp-playbook-audit
description: "Audit qualité complet pour ebooks et playbooks HTML et PDF : structure, contenu, nombre de pages, cohérence FR/EN, conformité design system, et standards professionnels. Supporte les fichiers HTML (audit structurel complet) et PDF (audit de contenu via extraction de texte). Génère un rapport détaillé avec score 0-100 et plan d'action priorisé. Triggers : audit, vérifier, qualité, check, review playbook, contrôle qualité, PDF, HTML."
user-invokable: true
argument-hint: "[fichier.html|fichier.pdf] [--compare fichier-ref] [--section section-id]"
allowed-tools: Read Bash Glob
metadata:
  author: DP Créateur
  version: "2.1.0"
  category: production
  updated: 2026-04-18
---

# Playbook Audit — Contrôle Qualité Ebook (HTML + PDF)

<!-- v2.1.0 | 2026-04-18 | Ajout du support PDF : extraction texte via pdftotext/pdfinfo, 6 checks adaptés, détection automatique du format -->

Expert en contrôle qualité de produits digitaux pour DP Créateur. Audite n'importe quel ebook **HTML ou PDF** — structure, contenu, nombre de pages, cohérence linguistique, conformité design system — et livre un rapport actionnable.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-playbook-audit [fichier.html]` | Audit complet d'un fichier HTML |
| `/dp-playbook-audit [fichier.pdf]` | Audit complet d'un fichier PDF |
| `/dp-playbook-audit` | Audit du playbook par défaut (cherche HTML puis PDF) |
| `/dp-playbook-audit [fichier] --compare [ref]` | Audit avec comparaison contre un fichier de référence (HTML ou PDF) |
| `/dp-playbook-audit [fichier] --section [id]` | Audit d'une seule section (HTML uniquement) |
| `/dp-playbook-audit --quick [fichier]` | Audit rapide (structure + quality gates uniquement) |

## Output Format

```
LIVRABLE :
├── Rapport d'audit structuré (8 checks HTML / 6 checks PDF)
├── Score qualité 0-100 avec détail par catégorie
├── Issues classées par sévérité (Critical / High / Medium / Low)
├── Plan d'action priorisé
└── Recommandation : PUBLISH / REVISE / REWRITE
```

---

## Process

```
0. Load context       → business-profile.md + références
1. Detect format      → HTML ou PDF ? Adapter les checks
2. Read/Extract       → Lecture HTML ou extraction texte PDF
3. Run checks         → 8 checks (HTML) ou 6 checks (PDF)
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
Read references/audit-checks.md        → Détail des checks
Read references/scoring-criteria.md    → Barème de notation
Read references/audit-report-example.md → Exemple de rapport d'audit
```

---

## Step 1 — Détection du Format & Context Intake

### 1a. Détecter le format du fichier

```
SI le fichier se termine par .html ou .htm :
  → MODE HTML (8 checks complets)

SI le fichier se termine par .pdf :
  → MODE PDF (6 checks adaptés)
  → Vérifier les outils d'extraction (voir Step 1b)

SI aucun fichier spécifié :
  → Chercher dans : ebook en/*.html, ebook fr/*.html, exports/*.pdf
  → Si trouvé → demander confirmation
  → Si rien trouvé → demander le chemin

SI extension inconnue :
  → Demander : "C'est un fichier HTML ou PDF ?"
```

### 1b. Vérifier les outils PDF (mode PDF uniquement)

```bash
# Vérifier pdftotext (poppler-utils)
which pdftotext 2>/dev/null && echo "OK: pdftotext disponible"

# Vérifier pdfinfo (poppler-utils)
which pdfinfo 2>/dev/null && echo "OK: pdfinfo disponible"

# Fallback : Python PyPDF2
python3 -c "import PyPDF2; print('OK: PyPDF2 disponible')" 2>/dev/null
```

**Si aucun outil disponible :**
```
AUCUN OUTIL PDF DÉTECTÉ

Pour auditer un PDF, installe l'un de ces outils :

Option 1 — poppler-utils (recommandé) :
  macOS  : brew install poppler
  Ubuntu : sudo apt-get install poppler-utils
  Windows: télécharger depuis poppler.freedesktop.org

Option 2 — Python PyPDF2 :
  pip install PyPDF2

Option 3 — Convertir le PDF en HTML d'abord :
  → Utilise le fichier HTML source si tu l'as encore
  → L'audit HTML est plus complet que l'audit PDF
```

### 1c. Questions si contexte manquant

| # | Question | Quand |
|---|----------|-------|
| Q1 | Quel fichier veux-tu auditer ? (chemin complet) | Aucun fichier trouvé automatiquement |
| Q2 | Tu veux comparer avec un fichier de référence ? Si oui, lequel ? | Pas de version FR/EN évidente |
| Q3 | Audit complet ou focus sur une section ? | Fichier très long (HTML > 3000 lignes) |

---

## Step 2 — Extraction du Contenu

### Mode HTML

```
Lire le fichier HTML en entier avec le tool Read.
```

### Mode PDF — Extraction de texte

```bash
# Extraire le texte complet du PDF
pdftotext "[fichier.pdf]" "/tmp/audit-extracted.txt"

# Récupérer les métadonnées
pdfinfo "[fichier.pdf]"
```

**Si pdftotext n'est pas disponible, fallback Python :**

```bash
python3 -c "
import PyPDF2
reader = PyPDF2.PdfReader('[fichier.pdf]')
print(f'Pages: {len(reader.pages)}')
for i, page in enumerate(reader.pages):
    text = page.extract_text()
    if text:
        print(f'--- PAGE {i+1} ---')
        print(text)
" > /tmp/audit-extracted.txt
```

**Après extraction, stocker :**
- Nombre de pages (depuis `pdfinfo` ou `len(reader.pages)`)
- Texte brut complet (depuis `pdftotext` ou `extract_text()`)
- Taille du fichier
- Métadonnées (titre, auteur, date de création)

---

## SECTION A — CHECKS HTML (8 checks, si format = HTML)

### CHECK 1 — Intégrité Structurelle (15 points)

Vérifier que le document HTML a :

- [ ] `<!DOCTYPE html>` et `<html lang="...">` avec attribut de langue correct
- [ ] `<head>` avec `<meta charset="UTF-8">`, viewport meta, et `<title>` descriptif
- [ ] Structure racine : `<article class="ebook">` → `<header>` → `<nav class="sommaire">` → `<main>` → sections
- [ ] Propriétés CSS custom définies dans `:root` (`--color-primary`, `--color-accent`)
- [ ] Toutes les classes CSS utilisées sont définies dans le bloc `<style>`
- [ ] Styles d'impression (`@media print`) présents
- [ ] Aucun CSS/JS externe (sauf Google Fonts)

**Scoring** : 15 points max. -3 par élément manquant critique. -1 par élément manquant mineur.

### CHECK 2 — Complétude des Sections (15 points)

Lister toutes les sections `<section class="section" id="...">` trouvées dans le fichier.

Pour chaque section, vérifier :
- [ ] Elle existe (l'ID est présent dans le DOM)
- [ ] Elle n'est pas vide (contient du contenu significatif)
- [ ] Elle a au moins un `<h2>`
- [ ] Elle a au moins 2 sous-sections `<h3>`

Comparer avec le fichier de référence si disponible (même nombre de sections, même IDs).

**Scoring** : 15 points max. -2 par section manquante. -1 par section vide ou incomplète.

### CHECK 3 — Content Blocks (15 points)

Chaque section principale doit contenir ces 3 types de blocs :

1. **Value Block** (ouverture) — `<div class="value-block">`
2. **Tools Block** — `<div class="tools-block">`
3. **Recap Block** (fermeture) — `<div class="recap-block">`

Compter chaque type et comparer avec la référence si disponible.

**Scoring** : 15 points max. -2 par bloc value/recap manquant. -1 par tools-block manquant.

### CHECK 4 — Navigation / Sommaire (10 points)

- [ ] `<nav class="sommaire">` existe
- [ ] Chaque `<a href="#...">` pointe vers un ID existant
- [ ] Aucun lien interne cassé
- [ ] Le sommaire liste toutes les sections principales

```bash
# Vérifier les liens du sommaire
grep -oP 'href="#[^"]*"' fichier.html | sort
grep -oP 'id="[^"]*"' fichier.html | sort
```

**Scoring** : 10 points max. -3 par lien cassé. -1 par section non listée.

### CHECK 5 — Cohérence FR/EN (15 points)

Si deux versions sont disponibles, comparer :

- [ ] Même nombre de `<h2>` et `<h3>`
- [ ] Même ordre de sections
- [ ] Toutes les références d'outils (T1-T10, E1-E3) identiques
- [ ] Formules KPI et métriques identiques
- [ ] Scripts et templates tous traduits
- [ ] Aucun texte non traduit restant

**Scoring** : 15 points max (redistribuer si pas de comparaison). -2 par écart structurel. -1 par contenu non traduit.

### CHECK 6 — Qualité Linguistique (10 points)

- [ ] Ton professionnel et direct (voix DP Créateur)
- [ ] Terminologie cohérente
- [ ] Pas de fautes de grammaire/orthographe
- [ ] Tutoiement (FR) ou "you" (EN) cohérent

**Scoring** : 10 points max. -2 par problème de ton. -1 par erreur linguistique.

### CHECK 7 — Design & Formatting (10 points)

- [ ] Labels `<h4>` corrects dans les blocs
- [ ] Listes `<ul class="single-item">` bien formées
- [ ] Pas de tags HTML orphelins
- [ ] Pas de styles inline (tout dans `<style>`)
- [ ] Couleurs cohérentes avec business-profile.md

**Scoring** : 10 points max. -2 par problème de structure. -1 par incohérence.

### CHECK 8 — Annexes (10 points)

- [ ] Chaque annexe a un ID unique et du contenu
- [ ] Le sommaire référence les annexes

**Scoring** : 10 points max. -2 par annexe vide. -1 par annexe non référencée.

---

## SECTION B — CHECKS PDF (6 checks, si format = PDF)

> **Note** : L'audit PDF est moins granulaire que l'audit HTML car le PDF ne conserve pas la structure du code source (classes CSS, IDs, blocs). L'audit se concentre sur le contenu, le volume, et la qualité.
>
> **Recommandation** : Si le fichier HTML source existe encore, préférer l'audit HTML (plus complet).

### CHECK PDF-1 — Métadonnées & Volume (20 points)

```bash
# Extraire les infos du PDF
pdfinfo "[fichier.pdf]"
```

Vérifier :

| Critère | Seuil Playbook | Seuil Guide | Seuil Lead Magnet |
|---------|---------------|-------------|-------------------|
| Nombre de pages | ≥ 60 | ≥ 30 | ≥ 10 |
| Word count (texte extrait) | ≥ 21000 | ≥ 10000 | ≥ 3500 |
| Taille fichier | < 10 MB | < 5 MB | < 3 MB |

```bash
# Compter les mots du texte extrait
wc -w /tmp/audit-extracted.txt

# Taille du fichier
ls -lh "[fichier.pdf]"
```

Vérifier aussi :
- [ ] Titre du PDF défini (pas "Untitled" ou vide)
- [ ] Auteur défini
- [ ] Date de création présente

**Scoring** :
- 20 points max
- Pages < minimum requis : -8 (Critical)
- Word count < minimum : -6 (Critical)
- Taille > limite : -3 (High)
- Titre/auteur manquant : -1 chaque (Low)

### CHECK PDF-2 — Contenu & Placeholders (20 points)

Analyser le texte extrait pour détecter :

```bash
# Chercher des placeholders
grep -in '\[TODO\]\|\[INSERT\]\|\[EXAMPLE\]\|Lorem ipsum\|\[À COMPLÉTER\]\|\[PLACEHOLDER\]' /tmp/audit-extracted.txt

# Chercher des sections vides (lignes successives vides)
awk '/^$/{blank++; if(blank>5) print NR": 5+ lignes vides consécutives"} /[^ ]/{blank=0}' /tmp/audit-extracted.txt

# Chercher du texte dans l'autre langue (FR dans un doc EN, ou inversement)
# Si doc EN :
grep -in 'À retenir\|Ce que tu vas apprendre\|Outil principal\|Valeur de cette section' /tmp/audit-extracted.txt
# Si doc FR :
grep -in 'Key takeaway\|What you will learn\|Primary tool\|Value of this section' /tmp/audit-extracted.txt
```

Vérifier :
- [ ] Zéro placeholder trouvé
- [ ] Pas de sections vides (5+ lignes blanches consécutives)
- [ ] Pas de texte non traduit
- [ ] Pas de "Lorem ipsum" ou texte de remplissage

**Scoring** :
- 20 points max
- Placeholder trouvé : -5 par occurrence (Critical)
- Section vide : -3 par occurrence (High)
- Texte non traduit : -2 par occurrence (Medium)

### CHECK PDF-3 — Structure du Contenu (15 points)

Analyser le texte extrait pour détecter la structure :

```bash
# Détecter les titres potentiels (lignes courtes en début de section, souvent en majuscules ou avec numérotation)
grep -n '^[0-9]\+\.\|^[A-Z][A-Z ]\{5,\}\|^Section\|^Chapitre\|^Partie\|^ANNEXE' /tmp/audit-extracted.txt

# Compter les sections détectées
grep -c '^[0-9]\+\.' /tmp/audit-extracted.txt

# Détecter la table des matières
grep -n -i 'table des matières\|sommaire\|table of contents' /tmp/audit-extracted.txt
```

Vérifier :
- [ ] Au moins 8 sections principales détectées (playbook) / 5 (guide) / 3 (lead magnet)
- [ ] Table des matières présente
- [ ] Introduction et conclusion détectées
- [ ] Progression logique des sections (numérotation ou thèmes cohérents)

**Heuristiques de détection de structure :**

| Signal | Interprétation |
|--------|---------------|
| Ligne courte (< 60 chars) suivie de paragraphe long | Probablement un titre |
| Numérotation (1., 2., 3. ou I., II., III.) | Structure de sections |
| Lignes "À retenir" / "Key takeaway" | Présence de recap blocks |
| Lignes "Ce que tu vas apprendre" | Présence de value blocks |
| Mots-clés "Outil" / "Template" / "Script" | Contenu actionnable |

**Scoring** :
- 15 points max
- Moins de sections que le minimum : -4 (High)
- Pas de table des matières : -3 (High)
- Pas d'introduction : -3 (High)
- Pas de conclusion : -2 (Medium)
- Structure non détectable : -3 (Medium)

### CHECK PDF-4 — Qualité Linguistique (20 points)

Analyser un échantillon du texte (premières 2000 lignes + dernières 500 lignes) :

```bash
# Échantillonner le texte
head -2000 /tmp/audit-extracted.txt > /tmp/audit-sample.txt
tail -500 /tmp/audit-extracted.txt >> /tmp/audit-sample.txt
```

Vérifier :
- [ ] Ton professionnel et direct (voix DP Créateur)
- [ ] Tutoiement (FR) ou "you" (EN) cohérent tout au long
- [ ] Pas de fluff motivationnel excessif ("Tu peux le faire !", "Crois en toi !")
- [ ] Contenu actionnable (présence de verbes d'action : "Fais", "Envoie", "Crée", "Lance")
- [ ] Caveats honnêtes ("ça dépend", "résultats variables")
- [ ] Pas de promesses de revenus garantis

```bash
# Détecter le fluff motivationnel
grep -ic 'tu peux le faire\|crois en toi\|n.abandonne jamais\|rien n.est impossible' /tmp/audit-sample.txt

# Détecter les promesses de revenus
grep -ic 'gagn.\+ [0-9]\+.*par mois\|revenu garanti\|devenir riche\|liberté financière' /tmp/audit-sample.txt

# Détecter les verbes d'action (signe de contenu actionnable)
grep -ic 'envoie \|crée \|lance \|ouvre \|écris \|fais \|teste \|vérifie ' /tmp/audit-sample.txt

# Vérifier la cohérence du tutoiement
grep -c '\bvous\b' /tmp/audit-sample.txt   # devrait être ~0 si tutoiement
grep -c '\btu\b' /tmp/audit-sample.txt     # devrait être élevé
```

**Scoring** :
- 20 points max
- Promesses de revenus : -5 par occurrence (Critical)
- Fluff motivationnel > 3 occurrences : -3 (High)
- Vouvoiement incohérent : -3 (High)
- Faible ratio verbes d'action (< 1 pour 500 mots) : -3 (Medium)
- Pas de caveats honnêtes : -2 (Medium)

### CHECK PDF-5 — Cohérence FR/EN (15 points)

Si deux PDFs sont fournis (FR + EN), comparer les textes extraits :

```bash
# Compter les mots de chaque version
wc -w /tmp/audit-fr.txt /tmp/audit-en.txt

# Comparer le nombre de sections détectées
grep -c '^[0-9]\+\.' /tmp/audit-fr.txt
grep -c '^[0-9]\+\.' /tmp/audit-en.txt

# Chercher du texte FR dans le doc EN
grep -in 'étape\|chapitre\|outil principal\|à retenir' /tmp/audit-en.txt

# Chercher du texte EN dans le doc FR
grep -in 'step\|chapter\|primary tool\|key takeaway' /tmp/audit-fr.txt
```

Vérifier :
- [ ] Word count similaire (écart < 15%)
- [ ] Même nombre de sections détectées
- [ ] Pas de texte FR dans le doc EN (et vice versa)
- [ ] Même nombre de pages (écart < 3 pages)

**Scoring** :
- 15 points max (redistribuer si pas de comparaison : +5 à PDF-2, +5 à PDF-4, +5 à PDF-6)
- Écart word count > 15% : -4 (High)
- Écart nombre de sections : -3 (High)
- Texte non traduit : -2 par occurrence (Medium)
- Écart pages > 5 : -2 (Medium)

### CHECK PDF-6 — Complétude & Éléments Actionnables (10 points)

Vérifier la présence d'éléments qui font un bon produit digital :

```bash
# Chercher des templates / scripts / checklists
grep -ic 'template\|script\|checklist\|modèle\|exemple\|exercice' /tmp/audit-extracted.txt

# Chercher des chiffres concrets (signe de contenu spécifique)
grep -c '[0-9]\+%\|[0-9]\+ jour\|[0-9]\+ client\|[0-9]\+ €\|[0-9]\+ eur' /tmp/audit-extracted.txt

# Chercher un CTA final (lien vers un produit/service)
tail -100 /tmp/audit-extracted.txt | grep -ic 'prochaine étape\|next step\|pour aller plus loin\|découvr'

# Chercher des exercices
grep -ic 'exercice\|à toi de jouer\|complète\|remplis\|calcule' /tmp/audit-extracted.txt
```

Vérifier :
- [ ] Au moins 1 template/script/checklist par 3 sections détectées
- [ ] Chiffres concrets présents (minimum 10 données chiffrées)
- [ ] CTA final ou "prochaine étape" dans les dernières pages
- [ ] Au moins 3 exercices pratiques (playbook uniquement)
- [ ] Présence de glossaire ou ressources (playbook uniquement)

**Scoring** :
- 10 points max
- Zéro template/script/checklist : -4 (High)
- Moins de 5 données chiffrées : -2 (Medium)
- Pas de CTA final : -2 (Medium)
- Zéro exercice (playbook) : -2 (High)

---

## Step 3 — Calcul du Score

### Score HTML (8 checks)

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
```

### Score PDF (6 checks)

```
QUALITY SCORE : [XX]/100

Métadonnées & Volume    : [XX]/20  (CHECK PDF-1)
Contenu & Placeholders  : [XX]/20  (CHECK PDF-2)
Structure du contenu    : [XX]/15  (CHECK PDF-3)
Qualité linguistique    : [XX]/20  (CHECK PDF-4)
Cohérence FR/EN         : [XX]/15  (CHECK PDF-5)
Complétude & Actionable : [XX]/10  (CHECK PDF-6)
```

### Seuils de publication (identiques HTML et PDF)

| Score | Verdict | Action |
|-------|---------|--------|
| 90-100 | ✅ PRÊT À PUBLIER | Publier tel quel |
| 70-89 | ⚠️ PUBLIABLE AVEC RÉSERVES | Corriger les issues High avant publication |
| 50-69 | ❌ NÉCESSITE DES CORRECTIONS | Corriger les issues Critical et High |
| 0-49 | 🚫 NON PUBLIABLE | Restructuration ou réécriture majeure nécessaire |

**Hard gate** : Score < 70 = NE PAS recommander la publication.

Pour un playbook : vérifier ≥ 60 pages (PDF) ou ≥ 21000 mots (HTML).

Si pas de comparaison FR/EN, redistribuer les 15 points du CHECK 5/PDF-5 sur les autres checks.

---

## Step 4 — Rapport Final

```
# PLAYBOOK AUDIT REPORT
File: [chemin]
Format: [HTML / PDF]
Date: [date]
Auditor: dp-playbook-audit v2.1

## SUMMARY
- Format: [HTML (8 checks) / PDF (6 checks)]
- Pages: [N] (PDF) ou estimé [N] (HTML, ~350 mots/page)
- Word count: [N]
- Overall Score: [XX]/100
- Status: PUBLISH / REVISE / REWRITE
- Critical Issues: [count]
- High Issues: [count]
- Medium Issues: [count]
- Low Issues: [count]

## DETAIL PAR CHECK
[Résultat de chaque check]

## ACTION ITEMS
Priority 1 (Critical) — Bloquer la publication :
  - [item avec localisation]

Priority 2 (High) — Corriger avant publication :
  - [item]

Priority 3 (Medium) — Amélioration recommandée :
  - [item]

Priority 4 (Low) — Nice to have :
  - [item]

## PROCHAINES ÉTAPES
  → /dp-playbook-section   Réécrire une section spécifique
  → /dp-playbook-sync      Corriger les écarts FR/EN
  → /dp-copy-review        Audit du copywriting
  → /dp-export-pdf         Ré-exporter le PDF (si audit HTML → corrections → nouveau PDF)
```

### Note spéciale pour les audits PDF

```
⚠️ LIMITES DE L'AUDIT PDF

L'audit PDF analyse le texte extrait, pas la mise en page visuelle.
Les éléments suivants ne sont PAS vérifiables dans un PDF :
  - Structure HTML (classes CSS, IDs, balises)
  - Liens de navigation internes (sommaire cliquable)
  - Conformité du design system (couleurs, blocs)
  - Styles d'impression

RECOMMANDATION : Si le fichier HTML source existe, préférer
l'audit HTML pour un contrôle qualité plus complet.
Chemin probable : ebook fr/*.html ou ebook en/*.html
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Toujours lire le fichier EN ENTIER avant d'auditer — ne jamais sauter de sections | Critical |
| QG-02 | Utiliser des commandes bash pour compter les éléments — ne jamais estimer | Critical |
| QG-03 | Référencer les numéros de ligne spécifiques pour chaque issue (HTML) ou les pages (PDF) | Critical |
| QG-04 | Le fichier FR est TOUJOURS la référence — si EN diffère de FR, EN est faux | Critical |
| QG-05 | Ne RIEN corriger — seulement rapporter. Les autres skills gèrent les corrections | Critical |
| QG-06 | Chaque issue doit avoir : description + localisation + sévérité + action recommandée | High |
| QG-07 | Le score doit être honnête — un 95/100 est rare. La plupart des premiers drafts sont 60-80. | High |
| QG-08 | Vérifier la cohérence des couleurs avec business-profile.md si disponible (HTML uniquement) | Medium |
| QG-09 | En mode PDF, toujours mentionner les limites de l'audit dans le rapport | High |
| QG-10 | Si pdftotext échoue, tenter PyPDF2 avant de déclarer l'audit impossible | High |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Fichier non trouvé | Message d'erreur clair + `Glob` pour chercher des fichiers HTML/PDF dans le projet |
| Fichier vide | Signaler immédiatement, score 0, recommander `/dp-playbook-create` |
| Fichier non-HTML et non-PDF | Détecter et refuser poliment + suggérer le bon skill |
| PDF protégé par mot de passe | Signaler : "Le PDF est protégé. Déverrouille-le avant de l'auditer." |
| PDF scanné (images, pas de texte) | Signaler : "Le PDF contient des images, pas du texte extractible. L'audit n'est pas possible. Utilise le fichier HTML source." |
| pdftotext non installé | Proposer l'installation (brew install poppler / apt-get install poppler-utils) ou fallback PyPDF2 |
| Extraction de texte partielle | Prévenir : "Seules [N] pages sur [total] ont pu être extraites. Le score peut être sous-estimé." |
| Pas de fichier de référence FR | Auditer sans comparaison, redistribuer les points |
| Fichier trop gros (HTML > 5000 lignes ou PDF > 200 pages) | Proposer audit par section ou mode `--quick` |
| business-profile.md absent | Continuer sans vérification de cohérence business |
| Encoding non-UTF8 | Signaler comme issue Critical |

---

## Commandes Bash — Référence Rapide

### Pour HTML

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

# Chercher des placeholders
grep -n '\[TODO\]\|\[INSERT\]\|\[EXAMPLE\]\|Lorem ipsum' fichier.html
```

### Pour PDF

```bash
# Métadonnées du PDF
pdfinfo fichier.pdf

# Extraire le texte
pdftotext fichier.pdf /tmp/audit-extracted.txt

# Compter les mots
wc -w /tmp/audit-extracted.txt

# Compter les pages
pdfinfo fichier.pdf | grep Pages

# Chercher des placeholders dans le texte extrait
grep -in '\[TODO\]\|\[INSERT\]\|Lorem ipsum' /tmp/audit-extracted.txt

# Détecter les titres/sections
grep -n '^[0-9]\+\.\|^[A-Z][A-Z ]\{5,\}' /tmp/audit-extracted.txt

# Détecter les éléments actionnables
grep -ic 'template\|script\|checklist\|exercice' /tmp/audit-extracted.txt
```

---

## Cross-Skill Integration

| Avant playbook-audit | Skill précédent | Quand |
|----------------------|-----------------|-------|
| Créer un ebook | `/dp-playbook-create` | Avant le premier audit |
| Écrire une section | `/dp-playbook-section` | Avant d'auditer une nouvelle section |
| Exporter en PDF | `/dp-export-pdf` | Si on veut auditer le PDF final |

| Après playbook-audit | Skill suivant | Quand |
|---------------------|---------------|-------|
| Corriger les écarts FR/EN | `/dp-playbook-sync` | Issues de cohérence trouvées |
| Réécrire une section | `/dp-playbook-section` | Section avec score bas |
| Revoir le copywriting | `/dp-copy-review` | Issues de voix ou persuasion |
| Exporter en PDF | `/dp-export-pdf` | Score >= 90, prêt à publier |
