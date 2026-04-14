---
name: dp-playbook-create
description: "Guide interactif pour créer un ebook professionnel de A à Z. Pose des questions stratégiques par phases, construit le plan, rédige section par section, et livre un HTML standalone prêt pour export PDF. Supporte 6 formats : playbook, guide, lead-magnet, worksheet, checklist, toolkit. Triggers : ebook, playbook, guide, créer, create, nouveau produit, lead magnet."
user-invokable: true
argument-hint: "[sujet] [type: playbook|guide|lead-magnet|worksheet|checklist|toolkit]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: creation
  updated: 2026-04-13
---

# Playbook Create — Créateur d'Ebook Guidé

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake, quality gates, scoring, references, error handling -->

Expert en création de produits digitaux pour DP Créateur. Guide l'utilisateur pas à pas — de l'idée floue au fichier HTML prêt à convertir en PDF vendable.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-playbook-create [sujet]` | Lancer la création guidée |
| `/dp-playbook-create outline [sujet]` | Générer seulement le plan (pas de rédaction) |
| `/dp-playbook-create express [sujet]` | Mode rapide — 5 questions puis rédaction complète |
| `/dp-playbook-create from [fichier]` | Restructurer du contenu existant au format DP Créateur |

## Output Format

```
LIVRABLE :
├── Fichier HTML standalone (ebook fr/[slug]-FR.html ou ebook en/[slug]-EN.html)
├── CSS embarqué (design system DP Créateur)
├── Prêt pour export PDF (styles d'impression inclus)
└── Score qualité 0-100 avec détail par catégorie
```

---

## Process

```
1. Context intake      → Collecter les infos essentielles (OBLIGATOIRE)
2. Read references     → Charger quality gates, product types, voice guide
3. Build product card  → Synthèse validée par l'utilisateur
4. Design structure    → Plan détaillé avec validation
5. Write content       → Rédaction section par section
6. Quality check       → Score 0-100, quality gates, pre-delivery review
7. Deliver             → Fichier HTML + résumé + prochaines étapes
```

---

## Step 1 — Context Intake (Required: Always Do This First)

Avant toute rédaction, collecter le contexte. Sans lui, le contenu sera générique et les recommandations fausses.

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom, niche, produit(s), audience, ton, couleurs
  → Ne PAS reposer les questions déjà couvertes par le profil

SINON :
  → Continuer sans. Les questions de l'intake couvriront le minimum.
```

### 1b. Poser les questions par blocs

**Règle absolue** : Ne JAMAIS poser toutes les questions d'un coup. Grouper par 2-3, attendre les réponses, reformuler pour valider, puis continuer.

#### Bloc 1 — Le sujet et l'expertise (poser en premier)

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | C'est quoi le sujet de ton ebook ? Décris en 1-2 phrases, même si c'est flou. | Cadre le projet |
| Q2 | Quel est ton niveau d'expertise sur ce sujet ? (expert / intermédiaire / j'apprends) | Détermine la profondeur et le ton |
| Q3 | Pourquoi toi ? Qu'est-ce qui te rend légitime ? (expérience, résultats, méthode unique, parcours) | Différenciation et crédibilité |

**Après les réponses** : Reformuler en 2-3 lignes. "Si je comprends bien, tu es [X] et tu veux créer un ebook sur [Y] parce que [Z]. C'est correct ?"

#### Bloc 2 — L'audience cible

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | C'est pour qui exactement ? Décris ton lecteur idéal (métier, situation, niveau). | Ciblage du contenu |
| Q5 | Quel est son problème n°1 ? Le truc qui le frustre au quotidien. | Hook et pertinence |
| Q6 | Qu'est-ce qu'il a déjà essayé ? Et pourquoi ça n'a pas marché ? | Positionnement vs alternatives |
| Q7 | Quel résultat concret il veut ? Pas "être meilleur" — un résultat mesurable. | Promesse et CTA |

**Après les réponses** : Synthèse du profil lecteur en 3-4 lignes. Valider.

#### Bloc 3 — Le produit

| # | Question | Pourquoi |
|---|----------|----------|
| Q8 | Quel type de produit ? `playbook` (60+ pages, produit premium) / `guide` (30-50 pages) / `lead-magnet` (10-15 pages, gratuit) / `worksheet` / `checklist` / `toolkit` | Structure et profondeur |
| Q9 | Quelle langue ? Français ou anglais. | Fichier de sortie |
| Q10 | Tu as un titre en tête ? Même provisoire. Sinon je t'en propose 5. | Cadrage éditorial |
| Q11 | À quel prix tu veux le vendre ? (ou gratuit si lead magnet) | Valeur perçue attendue |

**Après les réponses** : Confirmer le format. Annoncer le volume attendu :
- Playbook : **60+ pages** (~21000+ mots, 10-14 sections de 1500-2500 mots chacune)
- Guide : **30-50 pages** (~10000-17000 mots, 6-8 sections)
- Lead-magnet : **10-15 pages** (~3000-5000 mots, 3-5 sections)

> Read `references/product-types.md` pour les specs détaillées du type choisi.

#### Bloc 4 — L'identité visuelle

| # | Question | Pourquoi |
|---|----------|----------|
| Q12 | Tu as déjà une identité visuelle / des couleurs de marque ? Si oui, donne-moi ta couleur principale (hex ou nom) et ta couleur d'accent. | Cohérence visuelle avec ta marque existante |
| Q13 | Quel style visuel tu veux pour l'ebook ? `minimaliste` (épuré, beaucoup de blanc) / `bold` (contrastes forts, couleurs vives) / `premium` (sobre, tons sombres, élégant) / `warm` (tons chauds, accueillant) | Ambiance générale du design |
| Q14 | Tu as un logo ou un nom de marque à afficher dans l'ebook ? | Branding dans le header |

> **Si l'utilisateur n'a pas de couleurs** : Proposer 3 palettes adaptées à sa niche :
> - Chaque palette = couleur primaire + couleur accent + couleur texte + couleur fond
> - Montrer un aperçu : "Palette 1 : Bleu profond (#1e3a5f) + Or (#d4a853) — style premium/confiance"
> - Demander de choisir ou combiner

**Après les réponses** : Noter les choix visuels. Ils seront appliqués au CSS.

#### Bloc 5 — Le positionnement

| # | Question | Pourquoi |
|---|----------|----------|
| Q15 | En quoi ton ebook est différent de ce qui existe ? | Angle unique |
| Q16 | Quelle est ta PROMESSE principale ? "Après avoir lu ce guide, tu sauras/auras/pourras..." | Message central |
| Q17 | Y a-t-il des sujets à NE PAS aborder ? | Garde-fous |

**Après les réponses** : Passer au Step 2.

### Cas particuliers — Context Intake

> Read `references/error-handling.md` pour la gestion complète des cas limites.

| Situation | Action |
|-----------|--------|
| L'utilisateur n'a pas d'idée | Poser : "Quelle question te pose-t-on le plus souvent ?", "Qu'est-ce que tu sais faire que la plupart trouvent difficile ?", "Quel problème tu as résolu pour toi-même ?" |
| L'idée est vague | Ne jamais refuser. Reformuler en plus précis, demander confirmation. |
| L'utilisateur veut aller vite | Proposer le **mode express** : 7 questions (Q1, Q4, Q5, Q8, Q12, Q13, Q16), puis rédaction complète sans pauses. Les couleurs (Q12, Q13) sont OBLIGATOIRES même en express. Prévenir : "Résultat bon mais moins de contrôle." |
| L'utilisateur fournit du contenu existant | Lire, analyser, identifier forces/lacunes. Intégrer plutôt que repartir de zéro. |

---

## Step 2 — Product Card (Synthèse de découverte)

Après toutes les questions, présenter la fiche produit :

```
╔══════════════════════════════════════════════════╗
║              FICHE PRODUIT — SYNTHÈSE            ║
╠══════════════════════════════════════════════════╣
║ Titre        : [titre ou "à définir"]            ║
║ Type         : [playbook/guide/lead-magnet/...]  ║
║ Langue       : [FR/EN]                           ║
║ Prix visé    : [prix ou gratuit]                 ║
║ Pages visées : [60+ / 30-50 / 10-15]            ║
║ Mots estimés : [21000+ / 10000-17000 / 3000-5000]║
╠══════════════════════════════════════════════════╣
║ AUTEUR                                           ║
║ Expertise    : [niveau + domaine]                ║
║ Légitimité   : [angle unique]                    ║
╠══════════════════════════════════════════════════╣
║ AUDIENCE                                         ║
║ Qui          : [profil lecteur]                  ║
║ Problème     : [douleur principale]              ║
║ Déjà essayé  : [alternatives échouées]           ║
║ Résultat     : [transformation promise]          ║
╠══════════════════════════════════════════════════╣
║ IDENTITÉ VISUELLE                                ║
║ Primaire     : [#hex — nom]                      ║
║ Accent       : [#hex — nom]                      ║
║ Style        : [minimaliste/bold/premium/warm]   ║
║ Marque       : [nom/logo ou "aucun"]             ║
╠══════════════════════════════════════════════════╣
║ POSITIONNEMENT                                   ║
║ Différence   : [angle unique]                    ║
║ Promesse     : [1 phrase]                        ║
║ Exclusions   : [sujets à éviter]                 ║
╚══════════════════════════════════════════════════╝
```

**Demande validation** : "Cette fiche te correspond ? Tu veux modifier quelque chose avant qu'on passe au plan ?"

**Hard gate** : Ne PAS continuer sans validation explicite de l'utilisateur.

---

## Step 3 — Structure & Plan

### 3a. Proposer des titres (si pas encore défini)

Générer **5 options** avec des angles différents :

| # | Angle | Format |
|---|-------|--------|
| 1 | Résultat direct | "[Résultat concret] en [durée]" |
| 2 | Méthode/système | "La méthode [nom] pour [objectif]" |
| 3 | Contrarian | "Pourquoi [croyance commune] est faux (et quoi faire à la place)" |
| 4 | Spécifique + chiffre | "[N] [actions] pour [résultat] — Le guide [type]" |
| 5 | Transformation | "De [situation A] à [situation B] : [méthode]" |

Chaque titre a un sous-titre. Demander à l'utilisateur de choisir ou combiner.

**Hard gate** : Ne PAS continuer sans titre validé.

### 3b. Construire le plan détaillé

> Read `references/product-types.md` pour la structure requise selon le type.

Pour chaque section du plan, indiquer :

```
SECTION [N] — [Titre]
  Objectif      : [ce que le lecteur saura/fera après]
  Sous-sections :
    - [h3 #1]
    - [h3 #2]
    - [h3 #3]
    - [h3 #4]
    - [h3 #5]
  Enrichissements prévus :
    - [ ] Exercice pratique
    - [ ] Template prêt à copier
    - [ ] Étude de cas concrète
    - [ ] Checklist d'action
  Mots estimés  : [1500-2500]
  Pages estimées : [~4-7 pages]
```

**Demande validation** : "Ce plan te convient ? Tu veux ajouter, supprimer ou réorganiser ?"

### 3c. Compteur de pages

Après le plan, afficher un récapitulatif de volume :

```
ESTIMATION DE VOLUME
════════════════════

Sections principales  : [N] × ~2000 mots     = [total] mots
Header + Intro        :                       ~1500 mots
Troubleshooting       :                       ~1500 mots
Plan d'action         :                       ~1500 mots
Annexes (templates)   :                       ~3000 mots
Mot de fin            :                       ~500 mots
──────────────────────────────────────────────────────────
TOTAL ESTIMÉ          : ~[total] mots ≈ [pages] pages
                        (1 page ≈ 350 mots)

OBJECTIF  : [60 pages min pour playbook / 30+ pour guide / 10+ pour lead-magnet]
STATUT    : [✅ OK / ⚠️ Insuffisant — il manque ~X pages]
```

**Si le volume est insuffisant** : proposer d'ajouter des sections ou d'enrichir, jamais de gonfler artificiellement.

**Hard gate playbook** : Le plan DOIT atteindre ≥ 60 pages estimées. Si < 60, lancer l'étape d'enrichissement.

### 3d. Enrichissement (playbook uniquement)

Si le type est `playbook` et le volume < 60 pages, proposer systématiquement :

```
ENRICHISSEMENT — Ton playbook fait ~[N] pages. Pour un produit premium
de 60+ pages, je te propose d'ajouter :

□ Exercices pratiques (1-2 par section)          → +[N] pages
  Instruction + exemple + espace réponse + critères de réussite

□ Templates prêts à copier-coller                → +[N] pages
  Scripts, emails, messages, frameworks visuels

□ Études de cas concrètes                        → +[N] pages
  Cas réels avec résultats, étapes, et leçons

□ Checklists d'action par section                → +[N] pages
  Résumé actionnable en format checklist

□ FAQ étendue (20-30 questions)                  → +[N] pages

□ Glossaire des termes clés                      → +[N] pages

□ Ressources recommandées                        → +[N] pages
  Livres, outils, formations, communautés

Lesquels tu veux ajouter ?
```

Mettre à jour le plan et le compteur après les choix.

### 3e. Validation section par section (optionnel)

Proposer : "Tu veux qu'on détaille chaque section avant que je commence, ou le plan général te suffit ?"

Si oui, pour chaque section demander :
- "Qu'est-ce que tu veux absolument inclure ?"
- "Tu as des exemples, anecdotes, ou données ?"
- "Des frameworks ou méthodes spécifiques ?"

---

## Step 4 — Rédaction

### 4a. Charger les références (silencieux)

```
Read references/design-system.md   → CSS complet
Read references/voice-guide.md     → Ton et style
Read references/quality-gates.md   → Règles à ne jamais violer
Read references/example-section.md → pour voir un exemple de section complète
```

Si un playbook de référence existe dans `ebook en/` ou `ebook fr/`, le lire aussi pour absorber le style.

### 4b. Écrire par groupes

**Règle** : Ne PAS écrire tout d'un coup (sauf mode express ou demande explicite).

```
Groupe 1 : Header + Table des matières + Introduction
  → Présenter à l'utilisateur → Validation

Groupe 2 : Sections 1-3
  → Présenter → "On continue ? Tu veux ajuster ?"

Groupe 3 : Sections 4-6 (si applicable)
  → Présenter → Validation

Groupe 4 : Sections restantes + Troubleshooting + Plan d'action

Groupe 5 : Annexes + Mot de fin

EXCEPTION : Si l'utilisateur dit "écris tout" / "je te fais confiance" / mode express
  → Écrire l'intégralité sans pauses
```

### 4c. Pattern de section (obligatoire)

Chaque section h2 suit CE pattern dans CET ordre :

```html
<section class="section" id="[kebab-case-unique]">
  <h2>[Titre]</h2>

  <div class="value-block">
    <h4>Ce que tu vas apprendre</h4>
    <p>[1-2 phrases concrètes — PAS de "dans cette section nous allons..."]</p>
  </div>

  <h3>[Sous-section 1]</h3>
  <p>[Contenu — actionnable, spécifique, voix DP Créateur]</p>

  <h3>[Sous-section 2]</h3>
  <p>[Contenu avec template/script/checklist si applicable]</p>

  <!-- ... 3-7 sous-sections h3 max ... -->

  <!-- Si un outil est pertinent (PAS obligatoire) -->
  <div class="tools-block">
    <h4>Outil recommandé</h4>
    <ul>
      <li><span class="tool-id">T[N]</span> - <strong>[Nom]</strong> — [pourquoi]</li>
    </ul>
  </div>

  <div class="recap-block">
    <h4>À retenir</h4>
    <ul class="single-item">
      <li>[Point clé 1 — actionnable]</li>
      <li>[Point clé 2 — actionnable]</li>
      <li>[Point clé 3 — actionnable]</li>
    </ul>
  </div>
</section>
```

### 4d. Qualité du contenu — 4 critères par section

| Critère | Question à se poser | Manquement = |
|---------|---------------------|-------------|
| **QUOI** | Qu'est-ce que le lecteur va faire concrètement ? | Contenu théorique inutile |
| **POURQUOI** | Pourquoi c'est important ? Que se passe-t-il s'il saute cette étape ? | Motivation manquante |
| **COMMENT** | Les étapes exactes, le script, le template, la checklist | Conseil vague inapplicable |
| **MESURE** | Comment savoir si ça marche ? Quel KPI ou signal ? | Pas de feedback loop |

### 4e. Règles de voix

> Read `references/voice-guide.md` pour le guide complet.

**Résumé rapide :**

| Faire | Ne PAS faire |
|-------|-------------|
| Tutoyer systématiquement | "Nous", "on", voix passive |
| Actions spécifiques ("Envoie 20 DMs/jour") | Conseils vagues ("contacte des gens") |
| Caveats honnêtes ("ça dépend de ta niche") | Promesses garanties |
| Phrases courtes pour l'impact | Paragraphes-murs |
| Templates prêts à copier | "Crée ton propre template" sans exemple |
| Transitions d'action ("Maintenant, passe à...") | "Dans la section suivante, nous allons..." |

---

## Step 5 — Quality Check (Pre-Delivery)

> Read `references/scoring-system.md` pour la méthodologie complète.
> Read `references/quality-gates.md` pour les hard rules.

### 5a. Quality Gates — Vérification stricte

Avant de livrer, vérifier CHAQUE gate. Si un gate Critical échoue, corriger avant livraison.

| ID | Gate | Severity |
|----|------|----------|
| QG-01 | Aucun placeholder ([TODO], [INSERT], Lorem ipsum) | Critical |
| QG-02 | Aucune promesse de revenus garantis | Critical |
| QG-03 | Chaque h2 a un value-block ET un recap-block | Critical |
| QG-04 | Minimum de mots par section respecté : playbook ≥ 1500 mots/section, guide ≥ 800, lead-magnet ≥ 400 | High |
| QG-05 | Aucune section > 3000 mots sans h3 de découpage | High |
| QG-06 | **Playbook : 60+ pages (≥ 21000 mots). Guide : 30+ pages (≥ 10000 mots). Lead-magnet : 10+ pages (≥ 3500 mots).** | Critical |
| QG-07 | Chaque action a un COMMENT (étapes, script, template) | Critical |
| QG-08 | Pas de CSS/JS externe (sauf Google Fonts) | Critical |
| QG-09 | Tous les liens sommaire pointent vers des IDs existants | Critical |
| QG-10 | Playbook : au moins 1 exercice, template ou checklist par 2 sections | High |
| QG-11 | Les couleurs de marque sont appliquées au CSS (--color-primary, --color-accent) | High |
| QG-12 | HTML valide (tags fermés, structure correcte) | Critical |

### 5b. Pre-Delivery Checklist

```
AVANT DE LIVRER, vérifier :

Structure :
  [ ] HTML valide et bien formé
  [ ] Tous les IDs de section uniques et en kebab-case
  [ ] Sommaire avec liens fonctionnels vers chaque section
  [ ] CSS complet embarqué dans <style>
  [ ] Attribut lang="fr" ou lang="en" correct
  [ ] <title> descriptif
  [ ] Styles d'impression (@media print) inclus

Contenu :
  [ ] ZÉRO placeholder, TODO, ou [texte entre crochets]
  [ ] Chaque section a value-block + recap-block
  [ ] Chaque section répond à QUOI / POURQUOI / COMMENT / MESURE
  [ ] Au moins 1 template/script/checklist par 3 sections
  [ ] Voix DP Créateur respectée (direct, action-first, honnête)
  [ ] Pas de fluff motivationnel
  [ ] Caveats honnêtes où nécessaire
  [ ] Word count atteint pour le type de produit

Technique :
  [ ] Fichier enregistré au bon chemin avec le bon nom
  [ ] Dossier créé si nécessaire
  [ ] Pas de conflit avec un fichier existant
```

### 5c. Score Qualité

Calculer et afficher le score :

```
QUALITY SCORE : [XX]/100

Structure     : [XX]/20  (S01-S09)
Contenu       : [XX]/30  (C01-C08)
Voix & Ton    : [XX]/15  (V01-V06)
Complétude    : [XX]/20  (K01-K08)
Lisibilité    : [XX]/15  (R01-R05)

Issues :
  Critical : [N] (doivent être corrigés)
  High     : [N] (fortement recommandé)
  Medium   : [N] (à considérer)
  Low      : [N] (nice to have)
```

**Hard gate** : Si score < 75 ou si des issues Critical existent, corriger avant livraison.

---

## Step 6 — Delivery

### 6a. Assembler et sauvegarder

```
Chemin de sortie :
  FR → ebook fr/[slug]-FR.html
  EN → ebook en/[slug]-EN.html

Slug : titre en minuscules, espaces → tirets, pas de caractères spéciaux.
Créer le dossier si inexistant.
```

### 6b. Présenter le livrable

```
✅ EBOOK CRÉÉ — Score : [XX]/100

📄 Fichier  : [chemin complet]
📌 Titre    : [titre final]
📦 Type     : [playbook/guide/lead-magnet/...]
📊 Sections : [N] sections principales
📝 Mots     : ~[estimation] mots
📖 Pages    : ~[estimation] pages (≥ 60 pour playbook)
🌐 Langue   : [FR/EN]

SCORE QUALITÉ :
  Structure  [██████████░░] 85/100
  Contenu    [████████████] 92/100
  Voix       [█████████░░░] 78/100
  Complétude [███████████░] 88/100
  Lisibilité [██████████░░] 82/100

PROCHAINES ÉTAPES :
  → /dp-playbook-audit    Audit qualité approfondi
  → /export-pdf        Convertir en PDF vendable
  → /ebook-cover       Créer la couverture
  → /dp-landing-page      Créer la page de vente
  → /lead-magnet-create Créer un lead magnet associé
  → /dp-email-sequence    Séquence email de lancement
```

---

## Error Handling

> Read `references/error-handling.md` pour la table complète.

| Scénario | Action |
|----------|--------|
| Pas d'idée de sujet | Questions de découverte guidées |
| Sujet trop large | Proposer 3-5 sous-niches |
| Expertise insuffisante | Suggérer guide/checklist plutôt que playbook |
| Changement en cours de route | Mettre à jour fiche + plan, réécrire les sections impactées |
| Fichier existant même slug | Demander confirmation avant écrasement |
| business-profile.md absent | Continuer avec les réponses du context intake |
| Playbook de référence absent | Utiliser le CSS de references/design-system.md |

---

## Tool Reference System

Utiliser quand pertinent dans les tools-blocks. Ne PAS forcer un tools-block dans chaque section.

| ID | Outil | Usage |
|----|-------|-------|
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

| Après playbook-create | Skill suivant | Quand |
|-----------------------|---------------|-------|
| Vérifier la qualité | `/dp-playbook-audit` | Toujours recommandé |
| Traduire FR↔EN | `/dp-playbook-sync` | Si bilingue |
| Convertir en PDF | `/dp-export-pdf` | Avant mise en vente |
| Couverture | `/dp-ebook-cover` | Avant mise en vente |
| Page de vente | `/dp-landing-page` | Pour vendre |
| Lead magnet associé | `/dp-lead-magnet-create` | Pour capturer des emails |
| Séquence email | `/dp-email-sequence` | Pour le lancement |
| Publicité | `/dp-ad-angles-meta` `/dp-ad-angles-google` | Pour l'acquisition |
| Contenu social | `/dp-social-caption` `/dp-mediaplan` | Pour la promotion organique |
