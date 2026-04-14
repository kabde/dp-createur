---
name: dp-playbook-section
description: "Rédige ou réécrit une section complète pour un ebook DP Créateur existant. Produit du HTML prêt à insérer, respecte le design system, et valide la cohérence avec les sections voisines. Supporte ajout et réécriture. Triggers : section, ajouter section, réécrire, rewrite, add section, insert."
user-invokable: true
argument-hint: "[sujet] [--target fichier.html] [--action add|rewrite] [--after section-id]"
allowed-tools: Read Write Edit Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: creation
  updated: 2026-04-13
---

# Playbook Section — Rédacteur de Section Ebook

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake, quality gates, scoring, error handling, cross-skill integration -->

Expert en rédaction de contenu pour DP Créateur. Écrit ou réécrit une seule section d'un ebook existant — HTML prêt à insérer, cohérent avec le style et la structure du document cible.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-playbook-section [sujet]` | Rédiger une nouvelle section (demande le fichier cible) |
| `/dp-playbook-section [sujet] --target [fichier.html]` | Rédiger pour un ebook spécifique |
| `/dp-playbook-section rewrite [section-id] --target [fichier.html]` | Réécrire une section existante |
| `/dp-playbook-section [sujet] --after [section-id]` | Insérer après une section spécifique |
| `/dp-playbook-section [sujet] --before [section-id]` | Insérer avant une section spécifique |

## Output Format

```
LIVRABLE :
├── Section HTML complète (value-block + h3s + tools-block + recap-block)
├── Insérée directement dans le fichier cible
├── Sommaire mis à jour automatiquement
└── Vérification de cohérence post-insertion
```

---

## Process

```
0. Load context         → business-profile.md + références
1. Context intake       → Collecter sujet, fichier cible, action, position
2. Read target          → Analyser le playbook cible
3. Analyze neighbors    → Vérifier les sections adjacentes
4. Plan section         → Outline validé par l'utilisateur
5. Write section        → Rédaction HTML complète
6. Insert / Replace     → Modification du fichier + sommaire
7. Verify               → Comptages post-insertion
```

---

## Step 0 — Context Loading (Silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom de marque, niche, audience, ton, couleurs
  → Adapter le contenu de la section au contexte business

SINON :
  → Continuer sans. Le contenu s'adaptera au playbook cible.
```

---

## Step 1 — Context Intake

### 1a. Identifier les paramètres

| Paramètre | Source | Obligatoire |
|-----------|--------|-------------|
| Sujet de la section | `$ARGUMENTS` ou message utilisateur | Oui |
| Fichier cible | `$ARGUMENTS` (--target) ou question | Oui |
| Action | `add` (défaut) ou `rewrite` | Non |
| Position | `--after:[id]` ou `--before:[id]` | Non (pour `add`) |
| Brief / instructions | Message utilisateur | Non |
| Langue | Auto-détectée depuis le playbook | Non |

### 1b. Questions si contexte manquant (blocs de 2-3)

#### Bloc 1 — L'essentiel

| # | Question | Quand |
|---|----------|-------|
| Q1 | Quel est le sujet de la section ? Décris en 1-2 phrases. | Pas de sujet dans $ARGUMENTS |
| Q2 | Dans quel fichier on insère ? (chemin complet de l'ebook HTML) | Pas de --target, pas de fichier évident |
| Q3 | Nouvelle section (`add`) ou réécriture d'une section existante (`rewrite`) ? | Pas clair depuis le contexte |

#### Bloc 2 — Le positionnement (si `add`)

| # | Question | Quand |
|---|----------|-------|
| Q4 | Après quelle section ? (je te liste les sections existantes) | Position non spécifiée |
| Q5 | Tu as des instructions spécifiques, des points clés à couvrir, un angle ? | Toujours utile — optionnel |

---

## Step 2 — Lire le Playbook Cible

Lire le fichier cible en entier pour comprendre :

| Élément | Ce qu'on cherche |
|---------|-----------------|
| Langue | `<html lang="...">` → détermine les labels h4 et le tutoiement/you |
| Sections existantes | Liste des IDs + titres h2 |
| Ton et profondeur | Nombre moyen de h3 par h2, densité de paragraphes |
| Outils référencés | Quels T1-T10 / E1-E3 sont déjà utilisés |
| CSS | Confirmer le design system (classes disponibles) |
| Identité visuelle | Couleurs dans `:root`, polices, style général |

---

## Step 3 — Analyser les Sections Voisines

**Si `add`** : Lire les sections avant et après le point d'insertion :
- Flux logique — la nouvelle section doit s'insérer naturellement
- Pas de chevauchement — ne pas répéter ce que les voisines couvrent
- Profondeur cohérente — nombre de h3 et densité similaires

**Si `rewrite`** : Lire la section existante :
- Identifier ce qui doit changer et ce qui doit rester
- Conserver le même ID de section (ne pas casser les ancres)
- Vérifier les références depuis d'autres sections

---

## Step 4 — Planifier la Section

Présenter cet outline à l'utilisateur **AVANT** de rédiger :

```
╔══════════════════════════════════════════════════╗
║           PLAN DE SECTION — VALIDATION           ║
╠══════════════════════════════════════════════════╣
║ Section ID   : [kebab-case-id]                   ║
║ Titre (h2)   : [Titre de la section]             ║
║ Action       : add / rewrite                     ║
║ Position     : après [section-id]                ║
║ Langue       : FR / EN                           ║
╠══════════════════════════════════════════════════╣
║ VALUE BLOCK                                      ║
║ [1-2 phrases — ce que le lecteur gagne]          ║
╠══════════════════════════════════════════════════╣
║ SOUS-SECTIONS (h3)                               ║
║ 1. [Titre] — [brief]                             ║
║ 2. [Titre] — [brief]                             ║
║ 3. [Titre] — [brief]                             ║
║ 4. [Titre] — [brief] (si applicable)             ║
╠══════════════════════════════════════════════════╣
║ TOOLS BLOCK : [outil(s) pertinent(s) ou "N/A"]  ║
║ RECAP BLOCK : [3 takeaways prévus]              ║
╚══════════════════════════════════════════════════╝
```

**Hard gate** : Ne PAS rédiger sans validation explicite de l'outline.

---

## Step 5 — Rédiger la Section

### 5a. Structure HTML obligatoire

```html
<section class="section" id="[section-id]">
  <h2>[Titre]</h2>

  <div class="value-block">
    <h4>[Label selon langue]</h4>
    <p>[Ce que le lecteur va apprendre — concret, pas de "dans cette section..."]</p>
  </div>

  <h3>[Sous-section 1]</h3>
  <p>[Contenu — actionnable, spécifique, voix DP Créateur]</p>

  <h3>[Sous-section 2]</h3>
  <p>[Contenu avec template/script/checklist si applicable]</p>

  <!-- 2-7 sous-sections h3 max -->

  <div class="tools-block">
    <h4>[Label selon langue]</h4>
    <ul>
      <li><span class="tool-id">T[N]</span> - <strong>[Nom]</strong> — [pourquoi]</li>
    </ul>
  </div>

  <div class="recap-block">
    <h4>[Label selon langue]</h4>
    <ul class="single-item">
      <li>[Point clé 1 — action principale]</li>
      <li>[Point clé 2 — insight clé]</li>
      <li>[Point clé 3 — erreur à éviter]</li>
    </ul>
  </div>
</section>
```

### 5b. Labels selon la langue

| Élément | FR | EN |
|---------|----|----|
| Value block h4 | Ce que tu vas apprendre | Value of this section |
| Recap block h4 | À retenir | Key takeaway |
| Tools block h4 | Outil recommandé | Primary tool |

### 5c. Règles de contenu

| Critère | Règle |
|---------|-------|
| Spécificité | "Envoie 20 DMs par jour" pas "contacte des gens régulièrement" |
| Frameworks | Donner un script, une checklist, un step-by-step — pas un essai |
| Honnêteté | "Les résultats varient" est OK. "Tu vas 10x ton revenu" ne l'est pas. |
| Paragraphes | Max 4-5 lignes. Casser les murs de texte. |
| Verbes d'action | Commencer les sous-sections par ce que le lecteur FAIT |
| Pas de fluff | Zéro "Vous avez compris l'importance de...", zéro motivation vide |

### 5d. 4 critères par sous-section

| Critère | Question de vérification |
|---------|-------------------------|
| **QUOI** | Qu'est-ce que le lecteur va faire concrètement ? |
| **POURQUOI** | Pourquoi c'est important ? Que se passe-t-il s'il saute cette étape ? |
| **COMMENT** | Les étapes exactes, le script, le template, la checklist |
| **MESURE** | Comment savoir si ça marche ? Quel KPI ou signal ? |

---

## Step 6 — Insérer ou Remplacer

### Si `add` :
1. Utiliser `Edit` pour insérer le HTML à la position correcte
2. Mettre à jour `<nav class="sommaire">` avec un lien vers la nouvelle section
3. Vérifier que le nouvel ID ne conflicte pas avec les IDs existants

### Si `rewrite` :
1. Utiliser `Edit` pour remplacer le contenu de la section existante
2. Garder le même ID de section (ne pas casser les ancres)
3. Mettre à jour le titre dans le sommaire si le titre a changé

---

## Step 7 — Vérification Post-Insertion

Après insertion, confirmer :
- [ ] Le lien du sommaire fonctionne (href matche le nouvel ID)
- [ ] La section s'intègre correctement dans le flux du document
- [ ] Comptage total h2/h3 pour confirmer que rien n'est cassé
- [ ] Pas de conflit d'ID

Afficher le résumé :

```
✅ SECTION [AJOUTÉE/RÉÉCRITE]

📄 Fichier    : [chemin du playbook]
📌 Section ID : [id]
📌 Titre      : [titre]
📊 Sous-sections : [count] h3
📍 Position   : après [section-id]
📝 Mots       : ~[count]
🧱 Blocs      : value ✓ | tools ✓/— | recap ✓
📋 Sommaire   : mis à jour ✓

PROCHAINES ÉTAPES :
  → /dp-playbook-audit    Vérifier la qualité globale
  → /dp-playbook-sync     Synchroniser FR/EN
  → /dp-copy-review       Revoir le copywriting
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Chaque section DOIT avoir : `<section>` wrapper + `<h2>` + value-block + recap-block | Critical |
| QG-02 | Minimum 2, maximum 7 sous-sections `<h3>` par section | Critical |
| QG-03 | Le recap-block a exactement 3 takeaways | High |
| QG-04 | Zéro placeholder (`[TODO]`, `[INSERT]`, `Lorem ipsum`) | Critical |
| QG-05 | Zéro promesse de revenus garantis | Critical |
| QG-06 | L'ID de section est en kebab-case, unique dans le document | Critical |
| QG-07 | Pas de styles inline — tout vient du CSS du playbook | High |
| QG-08 | Pas de `<br>`, `<b>`, `<i>` — utiliser `<p>`, `<strong>`, `<em>` | High |
| QG-09 | Pas de JavaScript, pas d'images, pas de liens externes | Critical |
| QG-10 | Le sommaire est mis à jour après insertion | Critical |
| QG-11 | La voix DP Créateur est respectée (directe, action-first, honnête) | High |
| QG-12 | Si business-profile.md existe, le contenu est cohérent avec le profil | Medium |
| QG-13 | Chaque h3 sous-section : minimum 200 mots pour playbook, 150 pour guide. Vérifier que chaque h3 contient du contenu actionnable, pas du remplissage. | High |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Fichier cible non trouvé | `Glob` pour chercher des .html dans le projet. Demander le chemin. |
| Fichier cible n'est pas un ebook HTML | Détecter (pas de `<article class="ebook">`) et refuser poliment. Suggérer `/dp-playbook-create`. |
| Section ID déjà existant (pour `add`) | Proposer un ID alternatif. Ne jamais écraser sans confirmation. |
| Section ID non trouvé (pour `rewrite`) | Lister les IDs disponibles, demander lequel réécrire. |
| Sujet trop vague | Proposer 3 angles spécifiques. Demander de choisir. |
| Sujet qui chevauche une section existante | Signaler le chevauchement + proposer de fusionner ou différencier. |
| Langue non détectable | Demander explicitement FR ou EN. |
| business-profile.md absent | Continuer sans. Adapter au contenu existant du playbook. |

---

## Référence des Outils

| ID | Outil | Usage typique |
|----|-------|---------------|
| T1 | ClickUp | Gestion de projet |
| T2 | Tally | Formulaires, sondages |
| T3 | Calendly | Prise de RDV |
| T4 | Miro | Brainstorming visuel |
| T5 | Notion | Documentation, CRM |
| T6 | Apollo | Prospection |
| T7 | Loom | Messages vidéo |
| T8 | Canva | Design graphique |
| T9 | Stripe | Paiements |
| T10 | Slack | Communication |
| E1 | Meta Ads | Publicité Facebook/Instagram |
| E2 | Zoom | Visioconférence |
| E3 | Google Workspace | Email, docs |

Format HTML : `<span class="tool-id">T1</span> - <strong>ClickUp</strong> — [pourquoi]`

---

## Cross-Skill Integration

| Avant playbook-section | Skill précédent | Quand |
|------------------------|-----------------|-------|
| Créer l'ebook | `/dp-playbook-create` | Le fichier cible n'existe pas encore |
| Auditer l'ebook | `/dp-playbook-audit` | Pour identifier les sections à réécrire |

| Après playbook-section | Skill suivant | Quand |
|-----------------------|---------------|-------|
| Vérifier la qualité | `/dp-playbook-audit` | Toujours recommandé après insertion |
| Synchroniser FR/EN | `/dp-playbook-sync` | Si l'ebook existe en deux langues |
| Revoir le copywriting | `/dp-copy-review` | Pour affiner le ton et la persuasion |
