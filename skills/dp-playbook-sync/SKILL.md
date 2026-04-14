---
name: dp-playbook-sync
description: "Synchronise un ebook DP Créateur entre FR et EN. Détecte les écarts structurels et de contenu, traduit les sections manquantes ou modifiées, et met à jour le fichier cible en préservant le design et le ton. FR est toujours la source de vérité. Triggers : sync, synchroniser, traduire, translate, fr-en, en-fr, mise à jour traduction."
user-invokable: true
argument-hint: "[--source fichier-fr.html] [--target fichier-en.html] [--section section-id]"
allowed-tools: Read Bash Edit Write Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: production
  updated: 2026-04-13
---

# Playbook Sync — Synchronisation FR ↔ EN

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake, quality gates, guide de traduction, error handling, cross-skill integration -->

Expert en traduction et synchronisation de contenu pour DP Créateur. Maintient la cohérence entre les versions FR et EN de n'importe quel ebook — le FR est toujours la source de vérité.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-playbook-sync` | Sync complète — cherche automatiquement les fichiers FR/EN |
| `/dp-playbook-sync --source [fr.html] --target [en.html]` | Sync avec fichiers spécifiques |
| `/dp-playbook-sync --section [id]` | Sync d'une seule section |
| `/dp-playbook-sync --diff-only` | Afficher les différences sans appliquer de changements |
| `/dp-playbook-sync --full-translate [fichier-fr.html]` | Créer la version EN from scratch |

## Output Format

```
LIVRABLE :
├── Fichier EN mis à jour (ou créé)
├── Rapport de synchronisation (éléments modifiés)
├── Tableau de vérification (comptages FR vs EN)
└── Confirmation de cohérence post-sync
```

---

## Process

```
0. Load context       → business-profile.md + références
1. Context intake     → Identifier source et cible
2. Read both files    → Lecture complète FR + EN
3. Structural diff    → Comparaison élément par élément
4. Present diff       → Résumé pour validation utilisateur
5. Apply changes      → Traduction + insertion
6. Verify             → Re-comptage pour confirmer la cohérence
7. Report             → Résumé final
```

---

## Step 0 — Context Loading (Silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom de marque, produit(s), prix, audience, terminologie
  → Utiliser pour la cohérence des traductions (noms propres, termes métier)
Read references/sync-example.md → pour un exemple de synchronisation FR→EN

SINON :
  → Continuer sans. Les traductions se basent sur le contenu FR.
```

---

## Step 1 — Context Intake

### 1a. Identifier les fichiers

```
SI $ARGUMENTS contient --source et --target :
  → Utiliser ces fichiers

SI $ARGUMENTS contient un seul fichier :
  → C'est la source FR. Chercher le fichier EN correspondant.

SI $ARGUMENTS est vide :
  → Chercher dans ebook fr/*.html et ebook en/*.html
  → Si trouvé : utiliser automatiquement
  → Sinon : demander
```

### 1b. Questions si contexte manquant (max 2-3)

| # | Question | Quand |
|---|----------|-------|
| Q1 | Quel est le fichier FR (source) ? | Pas trouvé automatiquement |
| Q2 | Quel est le fichier EN (cible) ? Ou je le crée de zéro ? | Pas de fichier EN trouvé |
| Q3 | Sync complète ou juste une section spécifique ? | Fichier très long |

---

## Step 2 — Lire les Deux Fichiers

**Règle absolue** : Lire les deux fichiers EN ENTIER avant de commencer. Ne pas sauter de sections. Le diff nécessite une vision complète.

Si le fichier EN n'existe pas → passer directement au mode "full translate".

---

## Step 3 — Diff Structurel

### 3a. Comptages avec bash

Compter dans les deux fichiers :

```bash
# Éléments à compter (FR et EN)
grep -c '<h2>' fichier.html
grep -c '<h3>' fichier.html
grep -c 'class="value-block"' fichier.html
grep -c 'class="tools-block"' fichier.html
grep -c 'class="recap-block"' fichier.html
grep -c 'class="section"' fichier.html
grep -c 'class="tool-id"' fichier.html
```

### 3b. Comparaison section par section

Pour chaque section FR (par ID), vérifier si la version EN :

| Vérification | Détail |
|-------------|--------|
| Existe | L'ID est présent dans le document EN |
| Même structure | Même nombre de h3 |
| Mêmes blocs | value-block, tools-block, recap-block présents |
| Même densité | Nombre de paragraphes comparable |
| Même contenu | KPIs, chiffres, scripts, templates identiques |
| Traduit | Aucun texte FR restant dans la version EN |

### 3c. Résumé du diff

```
╔══════════════════════════════════════════════════╗
║              SYNC DIFF SUMMARY                   ║
╠══════════════════════════════════════════════════╣
║ Sections à ajouter   : [liste ou "aucune"]       ║
║ Sections à mettre à jour : [liste ou "aucune"]   ║
║ Blocs à ajouter      : [liste ou "aucun"]        ║
║ Contenu à traduire   : [liste avec lignes FR]    ║
║ Contenu à corriger   : [liste avec lignes EN]    ║
║ Labels h4 à corriger : [liste ou "aucun"]        ║
╠══════════════════════════════════════════════════╣
║ COMPTAGES                                        ║
║                     FR    EN    Match             ║
║ h2 headings       : ?     ?     ✓/✗              ║
║ h3 headings       : ?     ?     ✓/✗              ║
║ value-block       : ?     ?     ✓/✗              ║
║ tools-block       : ?     ?     ✓/✗              ║
║ recap-block       : ?     ?     ✓/✗              ║
║ Section IDs       : ?     ?     ✓/✗              ║
║ Tool refs         : ?     ?     ✓/✗              ║
╚══════════════════════════════════════════════════╝
```

---

## Step 4 — Validation Utilisateur

**Hard gate** : Toujours montrer le diff et obtenir confirmation AVANT de modifier le fichier EN.

Proposer les options :
1. "Sync tout" — appliquer toutes les modifications
2. "Sync sélective" — choisir les sections à synchroniser
3. "Annuler" — ne rien faire

---

## Step 5 — Appliquer les Changements

### 5a. Types de modifications

| Type | Action |
|------|--------|
| Section manquante | Traduire la section FR complète et insérer à la bonne position |
| Bloc manquant | Traduire le bloc et insérer à la bonne position dans la section |
| Contenu divergent | Mettre à jour le contenu EN pour refléter le FR |
| Structure cassée | Corriger le HTML pour correspondre au FR (IDs, classes, nesting) |
| Labels h4 incorrects | Corriger les labels selon la table de traduction |

### 5b. Outil à utiliser

```
Modification ciblée (1-2 sections)    → Edit tool
Réécriture majeure (>50% du fichier)  → Write tool
Fichier EN inexistant                 → Write tool (création complète)
```

---

## Step 6 — Guide de Traduction

### Ton & Voix

La voix DP Créateur en anglais est :
- **Directe** — Pas de remplissage, pas de fluff. Aller droit au but.
- **Action-oriented** — Impératif : "Send X", "Book Y", "Measure Z"
- **Anti-bullshit** — Honnête sur les limites, pas de promesses magiques.
- **Professional informal** — Comme un mentor intelligent, pas un manuel.

### Règles de traduction clés

#### Termes à garder en anglais dans les DEUX versions

```
Lead, Follow-up, Close/Closing, Call, DM, CTA, KPI
Pipeline, Funnel, Landing page
Show rate, Reply rate, Close rate
Onboarding, Wow moment, A/B test, ROI, CAC
```

#### Table de traduction des labels

| FR | EN |
|----|----|
| À retenir | Key takeaway |
| Ce que tu vas apprendre / Valeur de cette section | Value of this section |
| Outil principal / Outil recommandé | Primary tool |
| Erreur classique | Common mistake |
| Règle | Rule |
| Sommaire | Table of Contents |
| Dernière note | Final note |

#### Adaptations marché US

| Aspect | Règle |
|--------|-------|
| Devise | Utiliser `$` pas `€` |
| Exemples | Niches US (life coaching, executive coaching, health & wellness) |
| Références culturelles | Adapter si nécessaire (LinkedIn US vs FR) |

#### Éléments à préserver exactement

- Tous les tags HTML, classes et IDs
- Tout le CSS (ne pas toucher le bloc `<style>` sauf bug)
- Tous les IDs d'outils (T1-T10, E1-E3) et leurs wrappers `<span class="tool-id">`
- La structure et l'ordre des sections
- Tous les liens `<nav class="sommaire">`
- Les couleurs et l'identité visuelle

#### Interdictions

| Ne jamais... | Pourquoi |
|-------------|----------|
| Ajouter du contenu absent du FR | La source FR fait autorité |
| Supprimer du contenu présent en FR | Idem |
| Changer l'ordre des sections | Cohérence structurelle |
| Modifier les classes CSS ou IDs | Casse le design system |
| Ajouter du JavaScript | Standalone = zéro JS |
| Traduire les noms de marques/outils | ClickUp reste ClickUp |

---

## Step 7 — Vérification Post-Sync

### 7a. Re-comptage

Relancer les comptages bash pour confirmer que FR et EN correspondent maintenant.

### 7b. Vérifications supplémentaires

Pour chaque section traduite :
- [ ] Aucun texte français restant
- [ ] Labels h4 correctement traduits
- [ ] Chiffres et métriques identiques au FR
- [ ] Scripts et templates entièrement traduits
- [ ] La section se lit naturellement en anglais (pas de traduction littérale)

### 7c. Rapport final

```
✅ SYNC COMPLETE

📄 Source  : [chemin FR]
📄 Cible   : [chemin EN]
📊 Sections mises à jour : [liste]
🧱 Blocs ajoutés : [liste]
📝 Lignes modifiées : [count]

VÉRIFICATION :
| Élément       | FR  | EN  | Match |
|---------------|-----|-----|-------|
| h2 headings   | ?   | ?   | ✓/✗   |
| h3 headings   | ?   | ?   | ✓/✗   |
| value-block   | ?   | ?   | ✓/✗   |
| tools-block   | ?   | ?   | ✓/✗   |
| recap-block   | ?   | ?   | ✓/✗   |
| Section IDs   | ?   | ?   | ✓/✗   |
| Tool refs     | ?   | ?   | ✓/✗   |

PROCHAINES ÉTAPES :
  → /dp-playbook-audit    Audit qualité du fichier EN
  → /dp-copy-review       Revoir le copywriting EN
  → /export-pdf        Convertir en PDF
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | TOUJOURS lire les deux fichiers en entier avant de commencer | Critical |
| QG-02 | TOUJOURS montrer le diff et obtenir validation avant de modifier | Critical |
| QG-03 | TOUJOURS vérifier les comptages après modification | Critical |
| QG-04 | Le FR est TOUJOURS la source de vérité — ne jamais modifier le FR pour matcher le EN | Critical |
| QG-05 | Aucun texte français ne doit rester dans la version EN | Critical |
| QG-06 | Les chiffres, KPIs et métriques doivent être identiques FR/EN | Critical |
| QG-07 | Les IDs, classes CSS et structure HTML doivent être préservés | Critical |
| QG-08 | La traduction doit sonner naturelle — pas de calques littéraux | High |
| QG-09 | Les prix doivent être cohérents avec business-profile.md si disponible | High |
| QG-10 | Les couleurs et l'identité visuelle doivent être identiques dans les deux versions | Medium |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Fichier FR non trouvé | `Glob` pour chercher des .html dans `ebook fr/`. Demander le chemin. |
| Fichier EN non trouvé | Proposer de créer la version EN from scratch (full translate) |
| Fichier EN existe mais est vide | Traiter comme "non trouvé" — full translate |
| Les deux fichiers sont identiques | Rien à faire. Le signaler clairement. |
| Fichier FR a des problèmes structurels | Recommander `/dp-playbook-audit` sur le FR d'abord |
| Section ID demandée n'existe pas dans le FR | Lister les IDs disponibles, demander lequel synchroniser |
| Conflit de structure (EN a des sections que FR n'a pas) | Signaler les sections orphelines. Demander : supprimer ou garder ? |
| business-profile.md absent | Continuer sans vérification de cohérence des prix/noms |
| Fichier trop gros pour un seul Edit | Découper en modifications section par section |

---

## Cross-Skill Integration

| Avant playbook-sync | Skill précédent | Quand |
|---------------------|-----------------|-------|
| Créer l'ebook FR | `/dp-playbook-create` | Le FR n'existe pas encore |
| Auditer le FR | `/dp-playbook-audit` | Avant de synchroniser, s'assurer que le FR est propre |
| Modifier une section FR | `/dp-playbook-section` | Une section FR a changé, déclenchant le besoin de sync |

| Après playbook-sync | Skill suivant | Quand |
|--------------------|---------------|-------|
| Auditer le EN | `/dp-playbook-audit` | Toujours recommandé après sync |
| Revoir le copywriting EN | `/dp-copy-review` | Vérifier que la traduction est persuasive |
| Exporter en PDF | `/export-pdf` | Les deux versions sont prêtes |
