---
name: dp-blog-strategy
description: "Génère une stratégie de contenu blog complète : liste d'articles priorisés, topic clusters, maillage interne planifié, calendrier éditorial SEO. Analyse le contexte business pour proposer les articles à plus fort impact. Triggers: stratégie blog, plan articles, content strategy, topic cluster, idées articles, calendrier éditorial, plan SEO."
user-invokable: true
argument-hint: "[niche ou sujet] [nombre d'articles: 10|20|30]"
allowed-tools: Read Write Bash Glob WebSearch WebFetch
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: content
  updated: 2026-04-13
---

# Blog Strategy — Stratégie de Contenu & Topic Clusters

<!-- v2.0.0 | 2026-04-13 | Création : stratégie editoriale, topic clusters, maillage, calendrier -->

Stratège de contenu SEO pour DP Créateur. Analyse le contexte business, la niche et l'audience pour proposer une **liste d'articles priorisés** organisés en topic clusters avec un plan de maillage interne. Le but : ne pas écrire au hasard, mais construire une stratégie qui positionne le site comme autorité sur sa thématique.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-blog-strategy [niche]` | Stratégie complète guidée (10-30 articles) |
| `/dp-blog-strategy express [niche]` | Mode rapide — 5 questions puis stratégie |
| `/dp-blog-strategy cluster [sujet pilier]` | Générer un topic cluster autour d'un sujet |
| `/dp-blog-strategy audit` | Analyser les articles existants et trouver les gaps |
| `/dp-blog-strategy calendar [mois]` | Calendrier éditorial mensuel basé sur la stratégie |

## Output Format

```
LIVRABLES :
├── blog-strategy/strategy-[slug].md
│   ├── Liste de 10-30 articles priorisés
│   ├── Topic clusters (pilier + satellites)
│   ├── Carte de maillage interne
│   ├── Calendrier éditorial
│   └── Score de priorité par article
└── Prêt à alimenter /dp-blog-article pour chaque article
```

---

## Process

```
1. Context intake        → Business, niche, audience, produits, objectifs
2. Analyse de territoire → Identifier les thématiques porteuses
3. Topic clusters        → Organiser en piliers + articles satellites
4. Priorisation          → Scorer chaque article (impact × faisabilité)
5. Maillage interne      → Planifier les liens entre articles
6. Calendrier            → Séquencer la publication
7. Deliver               → Fichier stratégie + résumé
```

---

## Step 1 — Context Intake (Required)

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe → lire et extraire contexte
SI des articles existent dans blog/ → les lister pour éviter les doublons
Read references/strategy-example.md → pour une stratégie de 20 articles en 3 clusters
SINON → poser les questions
```

### 1b. Questions par blocs

#### Bloc 1 — Le business et la niche

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | C'est quoi ton business ? Décris en 2-3 phrases ce que tu vends et à qui. | Cadre la stratégie |
| Q2 | Quelle est ta niche principale ? (ex: coaching, fitness, marketing digital, productivité, finance perso…) | Territoire de contenu |
| Q3 | Quels sont tes produits/services ? (noms, prix, URLs si disponibles) | Les CTAs dans les articles |

#### Bloc 2 — L'audience et les objectifs

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | Décris ton lecteur idéal. Quel est son niveau ? Ses questions fréquentes ? | Angle et profondeur du contenu |
| Q5 | Quel est ton objectif principal avec le blog ? `trafic SEO` / `autorité/crédibilité` / `génération de leads` / `ventes directes` | Priorisation des types d'articles |
| Q6 | Tu as déjà un blog ou des articles ? Si oui, combien et sur quels sujets ? | Éviter les doublons, identifier les gaps |

#### Bloc 3 — Le cadrage

| # | Question | Pourquoi |
|---|----------|----------|
| Q7 | Combien d'articles tu veux planifier ? (`10` / `20` / `30`) | Taille de la stratégie |
| Q8 | Quel rythme de publication tu peux tenir ? (1/semaine, 2/mois, 1/mois…) | Calendrier réaliste |
| Q9 | Tu as des sujets que tu veux absolument couvrir ? Ou des sujets à éviter ? | Contraintes éditoriales |

---

## Step 2 — Analyse de Territoire

### 2a. Identifier les thématiques porteuses

À partir du contexte, générer une **carte thématique** :

```
CARTE THÉMATIQUE — [Niche]
═══════════════════════════

THÈME 1 : [Thème principal]
  Potentiel : ████████░░ 8/10
  Questions fréquentes : [3-5 questions que l'audience se pose]
  Mots-clés : [5-10 keywords associés]

THÈME 2 : [Thème secondaire]
  Potentiel : ██████░░░░ 6/10
  Questions fréquentes : [...]
  Mots-clés : [...]

THÈME 3 : [...]
  ...

THÈMES IDENTIFIÉS : [N]
```

### 2b. Analyser les articles existants (si applicable)

```
SI des articles existent dans blog/ :
  → Lister les sujets déjà couverts
  → Identifier les gaps (thèmes non couverts)
  → Identifier les opportunités de mise à jour
  → Identifier les articles orphelins (pas de liens entrants/sortants)
```

---

## Step 3 — Topic Clusters

Organiser les articles en **clusters** (modèle hub & spoke) :

### Structure d'un cluster

```
CLUSTER : [Nom du thème pilier]
════════════════════════════════

  ┌─────────────────────┐
  │   ARTICLE PILIER    │  ← Article long (3000+ mots), couvre le sujet en profondeur
  │   "[Titre pilier]"  │     Intent : informational/commercial
  │   Score priorité: X │     Cible : mot-clé principal du cluster
  └────────┬────────────┘
           │
     ┌─────┼─────┬──────────┬──────────┐
     │     │     │          │          │
     ▼     ▼     ▼          ▼          ▼
  [Sat.1] [Sat.2] [Sat.3] [Sat.4]  [Sat.5]   ← Articles satellites
  Intent  Intent   Intent  Intent   Intent       Chacun cible un sous-sujet
  Score   Score    Score   Score    Score         Tous linkent vers le pilier
                                                  Le pilier linke vers tous
```

### Règles de clustering

| Règle | Détail |
|-------|--------|
| 1 pilier = 4-8 satellites | Plus n'apporte pas de valeur marginale |
| Le pilier couvre le sujet large | Intent informational ou commercial |
| Les satellites traitent un sous-sujet précis | Intent plus spécifique |
| Chaque satellite linke vers le pilier | Maillage ascendant |
| Le pilier linke vers chaque satellite | Maillage descendant |
| Les satellites peuvent se linker entre eux | Maillage horizontal (si pertinent) |
| Pas de cannibalisation | Chaque article cible un mot-clé DIFFÉRENT |

### Format de sortie par cluster

```
CLUSTER 1 : [Thème]
─────────────────────

PILIER : "[Titre de l'article pilier]"
  Keyword    : [mot-clé principal]
  Intent     : [informational/commercial/transactional]
  Mots       : 3000+
  Priorité   : ██████████ 10/10
  CTA        : [produit à promouvoir]
  Statut     : [à écrire / existe / à mettre à jour]

  SATELLITE 1 : "[Titre]"
    Keyword  : [sous-mot-clé]
    Intent   : [intent]
    Mots     : 1500-2000
    Priorité : ████████░░ 8/10
    Lien     : → vers pilier + satellite 3
    Statut   : [à écrire]

  SATELLITE 2 : "[Titre]"
    ...
```

---

## Step 4 — Priorisation (Score par article)

Scorer chaque article sur 2 axes pour prioriser l'ordre de création :

### Matrice de priorisation

```
          IMPACT ÉLEVÉ
               │
     ┌─────────┼─────────┐
     │  QUICK  │  PRIO   │
     │  WINS   │  MAX    │   ← Écrire en PREMIER
     │         │         │
─────┼─────────┼─────────┼─────
     │         │         │
     │  BACK-  │  PROJETS│
     │  LOG    │  LONGS  │   ← Écrire plus tard
     │         │         │
     └─────────┼─────────┘
          IMPACT FAIBLE
    EFFORT FAIBLE   EFFORT ÉLEVÉ
```

### Critères de scoring

| Critère | Poids | Mesure |
|---------|-------|--------|
| Volume de recherche estimé | 25% | Élevé (8-10) / Moyen (5-7) / Faible (1-4) |
| Pertinence produit | 25% | Lien direct avec un produit (10) / Indirect (5) / Aucun (2) |
| Difficulté concurrentielle | 20% | Faible concurrence (10) / Moyenne (5) / Forte (2) |
| Position dans le funnel | 15% | BOFU (10) / MOFU (7) / TOFU (4). Cible globale : TOFU 35-45%, MOFU 30-40%, BOFU 20-30% |
| Faisabilité (expertise dispo) | 15% | Expert (10) / Intermédiaire (6) / Recherche nécessaire (3) |

**Score = Σ (critère × poids)**

### Tableau de priorisation final

```
# │ Titre                          │ Cluster │ Intent │ Score │ Priorité │ Effort
──┼────────────────────────────────┼─────────┼────────┼───────┼──────────┼────────
1 │ [Titre article]                │ Cl. 1   │ BOFU   │ 9.2   │ ★★★★★    │ Moyen
2 │ [Titre article]                │ Cl. 1   │ MOFU   │ 8.7   │ ★★★★★    │ Faible
3 │ [Titre article]                │ Cl. 2   │ TOFU   │ 8.1   │ ★★★★☆    │ Élevé
...
```

---

## Step 5 — Plan de Maillage Interne

### Carte de liens

Pour chaque article, définir :

```
MAILLAGE INTERNE
═════════════════

[Article A] ──→ [Article B] (ancre: "découvre comment [X]")
[Article A] ──→ [Article C] (ancre: "[sujet spécifique]")
[Article B] ──→ [Article A] (ancre: "guide complet sur [Y]")
[Article C] ──→ [Article A] (ancre: "[sujet parent]")
...

PILIER 1 ←→ Satellite 1, 2, 3, 4, 5 (liens bidirectionnels)
PILIER 2 ←→ Satellite 6, 7, 8, 9 (liens bidirectionnels)
PILIER 1 ──→ PILIER 2 (cross-link si thèmes liés)
```

### Règles de maillage

| Règle | Détail |
|-------|--------|
| 3-5 liens internes par article de 1500+ mots | Minimum pour le SEO |
| Ancres descriptives et variées | Jamais "cliquez ici", jamais la même ancre partout |
| Liens contextuels | Dans le corps du texte, pas dans un bloc "À lire aussi" isolé |
| Pas d'orphelins | Chaque article doit avoir au moins 1 lien entrant |
| Pilier ↔ Satellites | Toujours bidirectionnel |
| Cross-cluster | Lier les piliers entre eux si thèmes liés |
| Profondeur max 3 clics | Tout article accessible en 3 clics depuis l'accueil |

---

## Step 6 — Calendrier Éditorial

### Format

```
CALENDRIER ÉDITORIAL — [Mois Année]
═════════════════════════════════════

SEMAINE 1 (DD/MM - DD/MM)
  ├── [Jour] : "[Titre article]" — Cluster [N], [intent], ~[mots] mots
  │             Priorité: ★★★★★ │ Mot-clé: [keyword]
  │             Maillage: → [article X], [article Y]
  │             CTA: [produit]
  └── Dérivés à produire après : carousel IG, post LinkedIn

SEMAINE 2 (DD/MM - DD/MM)
  ├── [Jour] : "[Titre article]" — ...
  └── ...

NOTES :
  - Publier les piliers AVANT les satellites
  - Alterner TOFU / MOFU / BOFU
  - Mettre à jour le maillage après chaque publication
```

### Règles de séquençage

| Règle | Pourquoi |
|-------|----------|
| Pilier avant satellites | Les satellites ont besoin du pilier pour le maillage |
| Alterner les types d'intent | Varier le contenu pour l'audience |
| Articles BOFU en priorité | Impact business direct, conversion rapide |
| Pas de 2 piliers la même semaine | Trop lourd à produire |
| Prévoir la mise à jour du maillage | Ajouter les liens aux articles existants quand un nouveau sort |

---

## Step 7 — Delivery

Sauvegarder dans `blog-strategy/strategy-[slug].md`.

```
✅ STRATÉGIE BLOG CRÉÉE

📄 Fichier     : blog-strategy/strategy-[slug].md
📊 Articles    : [N] articles planifiés
🏗️ Clusters    : [N] topic clusters
🔗 Liens       : [N] liens internes planifiés
📅 Calendrier  : [N] semaines

PROCHAINES ÉTAPES :
  → /dp-blog-article [titre article #1]  — Écrire le premier article (commencer par le pilier)
  → /dp-blog-publish                      — Publier sur WordPress
  → /dp-social-caption                    — Promouvoir sur les réseaux
  → /dp-email-sequence                    — Envoyer à la liste email
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Chaque article a un mot-clé unique (pas de cannibalisation) | Critical |
| QG-02 | Chaque cluster a 1 pilier + 4-8 satellites | High |
| QG-03 | Chaque article a un score de priorité calculé | High |
| QG-04 | Le maillage ne laisse aucun article orphelin | Critical |
| QG-05 | Le calendrier est réaliste (respecte le rythme annoncé par l'utilisateur) | High |
| QG-06 | Les piliers sont planifiés AVANT leurs satellites | Critical |
| QG-07 | Mix d'intents équilibré : TOFU 35-45%, MOFU 30-40%, BOFU 20-30%. Ajuster selon l'objectif (plus de BOFU si objectif = ventes) | Medium |
| QG-08 | Chaque article a un CTA défini vers un produit/service | Medium |
| QG-09 | Pas de sujet en doublon avec les articles existants | Critical |
| QG-10 | Les ancres de maillage sont toutes différentes et descriptives | High |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| L'utilisateur ne connaît pas sa niche | Poser : "Qu'est-ce que tu vends ?", "À qui ?", "Quel problème tu résous ?" et dériver la niche |
| Niche trop large | Proposer 3 sous-niches plus ciblées |
| Niche trop petite (pas assez d'articles possibles) | Élargir en proposant des thèmes adjacents |
| Articles existants non organisés | Auditer, classer dans les clusters, identifier les gaps |
| business-profile.md absent | Poser les questions de contexte directement |
| L'utilisateur veut plus de 30 articles | Accepter mais prévenir : "Je recommande de commencer par 20, exécuter, puis itérer." |
| Pas de produit à promouvoir (blog personnel) | Adapter les CTAs vers newsletter, lead magnet, ou engagement |
| Concurrence très forte sur tous les keywords | Prioriser les long-tail et les angles différenciants |

---

## Cross-Skill Integration

| Avant | Skill | Quand |
|-------|-------|-------|
| Définir le business | `business-profile.md` | Si pas encore créé |
| Analyser la concurrence | `/dp-competitor-analysis` | Pour identifier les gaps |

| Après | Skill | Quand |
|-------|-------|-------|
| Écrire chaque article | `/dp-blog-article` | Pour chaque article de la stratégie |
| Publier sur WordPress | `/dp-blog-publish` | Après rédaction |
| Promouvoir | `/dp-social-caption` `/dp-mediaplan` | Après publication |
