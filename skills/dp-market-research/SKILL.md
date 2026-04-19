---
name: dp-market-research
description: "Valide une idée de produit digital (ebook, guide, playbook) AVANT de le créer. Analyse la demande, la concurrence, le pricing, l'audience, et la faisabilité. Score de validation 0-100 avec verdict GO / TEST / STOP. Utilise uniquement des sources gratuites (Google Trends, Reddit, Amazon, Gumroad, pages de vente publiques). Évite de perdre 30h sur un produit que personne ne veut. Triggers : valider idée, market research, étude de marché, analyse demande, est-ce que ça va se vendre, validation produit, niche research."
user-invokable: true
argument-hint: "[idée de produit] [niche]"
allowed-tools: Read Write Bash Glob WebSearch WebFetch
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: fondation
  updated: 2026-04-18
---

# Market Research — Validation d'Idée de Produit Digital

<!-- v2.0.0 | 2026-04-18 | Création : validation d'idée, scoring 12 dimensions, verdict GO/TEST/STOP, sources gratuites, tests rapides -->

Ce skill répond à LA question que chaque créateur devrait se poser AVANT de passer 30 heures à écrire un ebook : **"Est-ce que quelqu'un va acheter ça ?"**

Analyse la demande réelle, la concurrence existante, le potentiel de pricing, et ta capacité à te différencier — le tout avec des sources gratuites et un verdict clair.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-market-research [idée]` | Validation complète guidée (12 dimensions, verdict) |
| `/dp-market-research express [idée]` | Validation rapide — 5 dimensions clés, verdict en 10 min |
| `/dp-market-research competitors [niche]` | Focus concurrence uniquement (qui vend quoi, à quel prix) |
| `/dp-market-research demand [sujet]` | Focus demande uniquement (volume, questions, douleurs) |
| `/dp-market-research pivot [idée]` | Trouver un meilleur angle si l'idée initiale est faible |

## Output Format

```
LIVRABLE : market-research/validation-[slug].md

├── Fiche idée (synthèse validée)
├── Analyse de la demande (5 signaux)
├── Cartographie concurrentielle (3-5 concurrents analysés)
├── Analyse du pricing (fourchette recommandée)
├── Score de validation 0-100 (12 dimensions pondérées)
├── Verdict : GO / TEST / STOP
├── Risques identifiés + mitigations
├── Tests rapides à lancer (si verdict = TEST)
└── Recommandations concrètes (angle, prix, format, audience)
```

---

## Process

```
1. Context intake        → Ton idée, ton expertise, ton audience actuelle
2. Analyse de la demande → Les gens cherchent-ils ce sujet ? Souffrent-ils de ce problème ?
3. Cartographie concurrence → Qui vend déjà un produit similaire ? À quel prix ? Quelle qualité ?
4. Analyse du pricing    → Combien le marché est-il prêt à payer ?
5. Évaluation de ta position → Peux-tu te différencier ? As-tu la crédibilité ?
6. Scoring 12 dimensions → Score 0-100 avec pondération
7. Verdict               → GO / TEST / STOP avec justification
8. Plan d'action         → Si GO : prochaines étapes. Si TEST : tests à lancer. Si STOP : alternatives.
9. Deliver               → Rapport sauvegardé + résumé
```

---

## Step 1 — Context Intake (Required)

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe :
  → Lire niche, audience, produits existants, voix, budget
  → Adapter l'analyse au contexte existant
  → Ne PAS reposer les questions déjà couvertes

SINON :
  → Poser toutes les questions ci-dessous
```

### 1b. Questions par blocs

#### Bloc 1 — L'idée

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | **C'est quoi ton idée de produit ?** Décris en 2-3 phrases ce que tu veux créer. | Cadre l'analyse |
| Q2 | **Quel format ?** Ebook / playbook / guide / cours vidéo / template pack / autre | Détermine le marché à analyser |
| Q3 | **Quel résultat concret le client obtiendrait ?** Pas "apprendre le marketing" — un résultat mesurable. | Test de la proposition de valeur |

**Après les réponses** : Reformuler : "Tu veux créer un [format] sur [sujet] qui permet à [audience] de [résultat]. Correct ?"

#### Bloc 2 — Toi et ton audience

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | **Quel est ton niveau d'expertise sur ce sujet ?** Expert (5+ ans) / Intermédiaire (2-4 ans) / Débutant (< 2 ans) | Crédibilité et profondeur possible |
| Q5 | **Tu as déjà une audience ?** Si oui : combien d'abonnés email, followers, visiteurs/mois ? Si non : "je pars de zéro". | Capacité de distribution |
| Q6 | **Tu as déjà vendu un produit digital ?** Si oui : lequel, à quel prix, combien de ventes ? | Expérience de vente |

#### Bloc 3 — Le marché

| # | Question | Pourquoi |
|---|----------|----------|
| Q7 | **Tu connais des concurrents directs ?** Noms, URLs, ou "je ne sais pas — cherche pour moi". | Point de départ pour la cartographie |
| Q8 | **Quel prix tu envisages ?** Même approximatif (ex: "entre 27 et 47€"). | Positionne l'analyse de pricing |
| Q9 | **Quel est ton objectif ?** Revenu passif / lancer un business / lead magnet pour du consulting / tester un marché | Influence le verdict |

---

## Step 2 — Analyse de la Demande

> **Objectif** : Déterminer si des VRAIS gens cherchent activement une solution à ce problème.

### 2a. Signaux de demande (5 sources gratuites)

Pour chaque source, chercher le sujet de l'idée et évaluer la demande :

#### Source 1 — Google Trends

```
WebSearch : "[sujet] site:trends.google.com" OU utiliser WebFetch sur Google Trends
```

| Signal | Score |
|--------|-------|
| Tendance en hausse sur 12 mois | 9-10 |
| Stable (ni hausse ni baisse) | 6-7 |
| En baisse | 3-4 |
| Aucune donnée (sujet trop niche) | 2-3 |
| Pics saisonniers (janvier fitness, septembre rentrée) | 7-8 (si timing aligné) |

#### Source 2 — Reddit / Forums / Communautés

```
WebSearch : "[sujet] site:reddit.com" 
WebSearch : "[problème de l'audience] reddit"
```

Compter et analyser :
- Nombre de posts sur le sujet dans les 12 derniers mois
- Nombre de commentaires moyen par post (engagement)
- Questions récurrentes (signal de douleur non résolue)
- Frustrations exprimées (signal fort de demande)

| Signal | Score |
|--------|-------|
| 50+ posts récents avec 10+ commentaires | 9-10 |
| 20-50 posts, engagement modéré | 7-8 |
| 10-20 posts, engagement faible | 5-6 |
| < 10 posts, sujet rare | 2-4 |

**Extraire** : Les 5 questions/frustrations les plus fréquentes (= futur contenu du playbook).

#### Source 3 — YouTube

```
WebSearch : "[sujet] site:youtube.com"
```

| Signal | Score |
|--------|-------|
| Vidéos avec 100K+ vues sur le sujet | 9-10 |
| Vidéos avec 10-100K vues | 7-8 |
| Vidéos avec 1-10K vues | 5-6 |
| Peu/pas de vidéos | 3-4 |

**Ce que ça indique** : Si des YouTubeurs font des vidéos gratuites sur le sujet, il y a de l'intérêt. Si ces vidéos ont beaucoup de vues, la demande est prouvée.

#### Source 4 — Amazon KDP / Livres existants

```
WebSearch : "[sujet] ebook amazon"
WebSearch : "[sujet] kindle"
```

| Signal | Score |
|--------|-------|
| 10+ ebooks sur le sujet, avec reviews | 8-9 (demande prouvée, concurrence à analyser) |
| 5-10 ebooks, peu de reviews | 6-7 (demande modérée) |
| 1-4 ebooks | 5-6 (niche, potentiel si bien différencié) |
| Aucun ebook | 3-4 (soit pas de demande, soit opportunité inexploitée — creuser) |

**Attention** : Beaucoup de concurrence = la demande est réelle. Peu de concurrence peut signifier absence de demande OU opportunité. Il faut croiser avec les autres signaux.

#### Source 5 — Quora / AnswerThePublic / People Also Ask

```
WebSearch : "[sujet] site:quora.com"
WebSearch : "[question du client idéal]"
```

| Signal | Score |
|--------|-------|
| 20+ questions sur Quora avec réponses détaillées | 8-9 |
| 10-20 questions | 6-7 |
| < 10 questions | 4-5 |
| Questions Google "People Also Ask" abondantes | +1 bonus |

### 2b. Synthèse de la demande

```
ANALYSE DE LA DEMANDE — "[Sujet]"
═══════════════════════════════════

Google Trends        : [score /10] — [commentaire : hausse/stable/baisse]
Reddit / Forums      : [score /10] — [X posts, Y commentaires, Z frustrations]
YouTube              : [score /10] — [X vidéos, max Y vues]
Amazon / Ebooks      : [score /10] — [X ebooks existants, Z reviews]
Quora / Q&A          : [score /10] — [X questions]

SCORE DEMANDE MOYEN  : [X/10]

TOP 5 DOULEURS IDENTIFIÉES :
1. "[Douleur extraite des forums/Q&A]"
2. "[Douleur]"
3. "[Douleur]"
4. "[Douleur]"
5. "[Douleur]"

QUESTIONS FRÉQUENTES (futur contenu) :
1. "[Question trouvée]"
2. "[Question]"
3. "[Question]"
```

---

## Step 3 — Cartographie Concurrentielle

> **Objectif** : Savoir exactement qui vend quoi, à quel prix, et où sont les gaps.

### 3a. Trouver 3-5 concurrents

```
WebSearch : "[sujet] ebook"
WebSearch : "[sujet] playbook"
WebSearch : "[sujet] guide [prix]"
WebSearch : "[sujet] course"
WebSearch : "[sujet] gumroad OR lemonscalers OR teachable OR podia"
```

Si l'utilisateur a donné des noms (Q7), les analyser directement.

### 3b. Analyser chaque concurrent

Pour chaque concurrent, visiter la page de vente (WebFetch) et extraire :

```
CONCURRENT [N] : [Nom]
═══════════════════════

URL             : [url de la page de vente]
Produit         : [nom du produit]
Format          : [ebook / cours vidéo / membership / coaching]
Prix            : [prix — ou fourchette si plusieurs tiers]
Pages / Durée   : [nombre de pages ou durée du cours]
Audience cible  : [qui ils ciblent — extrait de leur copy]
Promesse        : [leur headline ou proposition de valeur principale]
Preuve sociale  : [nombre de reviews, témoignages visibles, "X clients"]
Marketing       : [canaux visibles : blog, YouTube, Insta, ads, email]
Forces          : [2-3 points forts observables]
Faiblesses      : [2-3 points faibles ou manques]
```

### 3c. Matrice comparative

| Dimension | Concurrent 1 | Concurrent 2 | Concurrent 3 | TOI (estimé) |
|-----------|-------------|-------------|-------------|--------------|
| Prix | [prix] | [prix] | [prix] | [ton prix envisagé] |
| Format | [ebook/cours] | | | [ton format] |
| Pages/Volume | [X pages] | | | [60+ pages] |
| Profondeur | [/10] | [/10] | [/10] | [/10 estimé] |
| Actionnabilité | [/10] | [/10] | [/10] | [/10] |
| Preuve sociale | [/10] | [/10] | [/10] | [/10] |
| Design/UX | [/10] | [/10] | [/10] | [/10] |
| SEO/Visibilité | [/10] | [/10] | [/10] | [/10] |
| Unicité angle | [/10] | [/10] | [/10] | [/10] |

### 3d. Identifier les gaps

```
GAPS DE MARCHÉ (opportunités)
═════════════════════════════

GAP 1 : [Ce que PERSONNE ne couvre — ex: "aucun concurrent ne propose de templates prêts à copier"]
  → Impact : [élevé/moyen/faible]
  → Action : [comment tu peux combler ce gap]

GAP 2 : [Format manquant — ex: "tous font des cours vidéo, aucun ebook actionnable"]
  → Impact : [élevé/moyen/faible]
  → Action : [comment tu peux combler ce gap]

GAP 3 : [Audience ignorée — ex: "tous ciblent les avancés, personne ne cible les débutants"]
  → Impact : [élevé/moyen/faible]
  → Action : [comment tu peux combler ce gap]
```

---

## Step 4 — Analyse du Pricing

> **Objectif** : Définir une fourchette de prix réaliste basée sur le marché et ta position.

### 4a. Cartographie des prix

```
PRIX DU MARCHÉ — "[Sujet]"
═══════════════════════════

Concurrent 1 : [prix] — [format] — [volume]
Concurrent 2 : [prix] — [format] — [volume]
Concurrent 3 : [prix] — [format] — [volume]
Concurrent 4 : [prix] — [format] — [volume]
Concurrent 5 : [prix] — [format] — [volume]

Prix moyen    : [X€]
Prix médian   : [X€]
Fourchette    : [min€] — [max€]
```

### 4b. Recommandation de prix

| Position | Prix recommandé | Quand |
|----------|----------------|-------|
| **Sous le marché** | Médian × 0.6-0.8 | Tu débutes, pas de preuve sociale, besoin de premiers clients |
| **Au marché** | Médian × 0.9-1.1 | Tu as une audience modérée, produit comparable |
| **Au-dessus du marché** | Médian × 1.3-2.0 | Tu as une forte crédibilité, angle unique, contenu premium |

### 4c. Calcul de rentabilité

```
SIMULATION DE RENTABILITÉ
═════════════════════════

Prix envisagé          : [X€]
Coût de création       : ~30h de travail = [X€ en valeur horaire]
Coût de lancement      : ~[X€] (ads + outils)

SCÉNARIO PESSIMISTE (10 ventes/mois) :
  Revenu mensuel : [X€]
  Break-even     : [N] mois

SCÉNARIO RÉALISTE (30 ventes/mois) :
  Revenu mensuel : [X€]
  Break-even     : [N] mois

SCÉNARIO OPTIMISTE (100 ventes/mois) :
  Revenu mensuel : [X€]
  Break-even     : [N] mois

VERDICT RENTABILITÉ : [Rentable dès le mois X / Nécessite du volume / Non rentable sans upsell]
```

---

## Step 5 — Évaluation de Ta Position

> **Objectif** : Évaluer honnêtement si TU es la bonne personne pour ce produit.

### 5a. Auto-évaluation (guidée)

| Dimension | Question | Score |
|-----------|----------|-------|
| Expertise | Peux-tu parler de ce sujet pendant 2h sans notes ? | /10 |
| Crédibilité | Quelqu'un t'a déjà payé pour ce savoir ? | /10 |
| Résultats | Tu as obtenu les résultats que tu promets ? | /10 |
| Audience | Tu as accès à des gens qui achèteraient ? | /10 |
| Différenciation | Qu'est-ce que tu apportes que les autres ne peuvent pas ? | /10 |
| Passion | Tu seras motivé pour marketer ce produit pendant 6 mois ? | /10 |

### 5b. Analyse de différenciation

```
TON ANGLE UNIQUE
════════════════

Ce que les concurrents font     : [résumé en 2 lignes]
Ce qu'aucun ne fait             : [gap identifié en Step 3]
Ton avantage compétitif         : [expertise + angle + format]
Ta promesse différenciante      : "[En 1 phrase : pourquoi acheter chez toi]"

FORCE DE DIFFÉRENCIATION : [Forte / Moyenne / Faible]

SI FAIBLE : 
  → Option A : Changer l'angle (cibler un sous-segment ignoré)
  → Option B : Changer le format (ebook quand tous font des cours)
  → Option C : Changer le prix (sous-coter agressivement pour les premiers clients)
  → Option D : Pivoter vers un autre sujet
```

---

## Step 6 — Scoring de Validation (12 dimensions)

### Matrice de scoring

| # | Dimension | Poids | Score /10 | Pondéré |
|---|-----------|-------|-----------|---------|
| 1 | Demande Google Trends | 10% | [X] | [X × 0.10] |
| 2 | Demande Reddit/Forums | 10% | [X] | [X × 0.10] |
| 3 | Demande YouTube | 5% | [X] | [X × 0.05] |
| 4 | Demande Amazon/Ebooks | 5% | [X] | [X × 0.05] |
| 5 | Demande Q&A (Quora, PAA) | 5% | [X] | [X × 0.05] |
| 6 | Niveau de concurrence | 10% | [X] | [X × 0.10] |
| 7 | Gaps exploitables | 10% | [X] | [X × 0.10] |
| 8 | Potentiel de pricing | 10% | [X] | [X × 0.10] |
| 9 | Ton expertise | 10% | [X] | [X × 0.10] |
| 10 | Ta capacité de distribution | 10% | [X] | [X × 0.10] |
| 11 | Force de différenciation | 10% | [X] | [X × 0.10] |
| 12 | Rentabilité estimée | 5% | [X] | [X × 0.05] |
| | **TOTAL** | **100%** | | **[XX.X] /10** |

**Score final = Total × 10 = [XX]/100**

### Interprétation des scores par dimension

| Score | Signification |
|-------|--------------|
| 9-10 | Excellent — signal très fort, avantage clair |
| 7-8 | Bon — signal positif, base solide |
| 5-6 | Moyen — potentiel mais nécessite du travail |
| 3-4 | Faible — signal inquiétant, risque élevé |
| 1-2 | Très faible — red flag, problème majeur |

### Scoring du niveau de concurrence (dimension 6)

**Attention** : La concurrence n'est PAS toujours négative. Beaucoup de concurrence = demande prouvée.

| Situation | Score | Explication |
|-----------|-------|-------------|
| 0 concurrents | 4 | Risque : pas de demande prouvée. Mais si demande forte (D1-D5), alors 7. |
| 1-3 concurrents, qualité faible | 9-10 | Demande prouvée + facile à surpasser |
| 1-3 concurrents, qualité forte | 6-7 | Demande prouvée mais différenciation nécessaire |
| 4-10 concurrents, qualité mixte | 7-8 | Marché mature, place pour un bon produit |
| 10+ concurrents, marché saturé | 3-5 | Possible si angle unique fort. Sinon, risqué. |

---

## Step 7 — Verdict

### Logique de verdict

```
SI score >= 75 ET aucune dimension critique < 4 :
  → VERDICT = "🟢 GO — Lance la création"

SI score >= 55 ET score < 75, OU 1 dimension critique < 4 :
  → VERDICT = "🟡 TEST — Valide d'abord avec 3 tests rapides"

SI score < 55 OU 3+ dimensions < 4 :
  → VERDICT = "🔴 STOP — Cette idée a trop de risques. Pivote."
```

**Dimensions critiques** (si l'une d'elles est < 4, ça affecte le verdict) :
- Demande (moyenne des D1-D5)
- Force de différenciation (D11)
- Ton expertise (D9)

### Format du verdict

```
╔══════════════════════════════════════════════════╗
║              VERDICT DE VALIDATION               ║
╠══════════════════════════════════════════════════╣
║                                                  ║
║  Score : [XX] / 100                              ║
║                                                  ║
║  [🟢 GO / 🟡 TEST / 🔴 STOP]                    ║
║                                                  ║
║  "[Justification en 2 phrases]"                  ║
║                                                  ║
╠══════════════════════════════════════════════════╣
║ FORCES (ce qui joue en ta faveur)                ║
║  + [Force 1]                                     ║
║  + [Force 2]                                     ║
║  + [Force 3]                                     ║
╠══════════════════════════════════════════════════╣
║ RISQUES (ce qui pourrait poser problème)         ║
║  - [Risque 1] → Mitigation : [action]            ║
║  - [Risque 2] → Mitigation : [action]            ║
╠══════════════════════════════════════════════════╣
║ RECOMMANDATION                                   ║
║  [Action concrète à faire maintenant]            ║
╚══════════════════════════════════════════════════╝
```

---

## Step 8 — Plan d'Action selon le Verdict

### Si 🟢 GO

```
PROCHAINES ÉTAPES — LANCEMENT
══════════════════════════════

1. /dp-business-profile       → Configurer ton profil (si pas fait)
2. /dp-playbook-create [sujet] → Créer ton ebook (60+ pages)
   Angle recommandé : [angle identifié dans l'analyse]
   Prix recommandé  : [prix identifié dans l'analyse]
   Audience cible   : [audience affinée par l'analyse]
3. /dp-export-pdf              → Convertir en PDF
4. /dp-landing-page            → Créer ta page de vente
5. /dp-email-sequence          → Préparer la séquence de lancement
6. /dp-ad-angles-meta          → Créer tes premières ads

TIMELINE RECOMMANDÉE :
  Semaine 1-2 : Rédaction ebook
  Semaine 3   : Landing page + email sequence
  Semaine 4   : Lancement + ads
```

### Si 🟡 TEST — 5 Tests Rapides à Lancer

```
TESTS DE VALIDATION (avant de créer le produit)
════════════════════════════════════════════════

TEST 1 — Sondage audience (2h)
  Action  : Poste un sondage sur tes réseaux ou dans des groupes ciblés
  Question : "Si je créais un [format] sur [sujet] à [prix], ça t'intéresserait ?"
  Signal GO : 30%+ de "oui" avec au moins 20 réponses
  Signal STOP : < 15% de "oui" ou < 10 réponses

TEST 2 — Lead magnet test (4h)
  Action  : Crée un mini-lead magnet gratuit sur le sujet (3-5 pages)
  Distribue : Posts sociaux + 1 email si tu as une liste
  Signal GO : 50+ téléchargements en 7 jours
  Signal STOP : < 10 téléchargements

TEST 3 — Pré-vente (2h)
  Action  : Crée une landing page simple (avec /dp-landing-page)
  Propose : "Pré-commande à [prix réduit] — livraison dans 4 semaines"
  Signal GO : 5+ pré-ventes
  Signal STOP : 0 pré-vente en 7 jours

TEST 4 — Interview clients (3h)
  Action  : Contacte 5-10 personnes dans ta cible en DM
  Demande : "J'ai une question : [problème du client]. C'est un vrai sujet pour toi ?"
  Signal GO : 7+ répondent et décrivent le problème spontanément
  Signal STOP : < 3 réponses ou "pas vraiment"

TEST 5 — Contenu pilote (4h)
  Action  : Publie 1 article ou 1 vidéo sur LE sujet central du playbook
  Mesure  : Engagement (commentaires, partages, saves, questions)
  Signal GO : 2x ton engagement moyen
  Signal STOP : Engagement inférieur ou égal à la moyenne
```

### Si 🔴 STOP — Alternatives

```
ALTERNATIVES RECOMMANDÉES
══════════════════════════

L'idée "[sujet]" n'est pas assez forte en l'état. Voici 3 pivots possibles :

PIVOT 1 — Changer l'angle
  Au lieu de : "[idée originale]"
  Essayer    : "[angle dérivé ciblant un sous-segment]"
  Pourquoi   : [raison basée sur l'analyse]

PIVOT 2 — Changer l'audience  
  Au lieu de : "[audience originale]"
  Essayer    : "[audience adjacente avec plus de demande]"
  Pourquoi   : [raison basée sur l'analyse]

PIVOT 3 — Changer le format
  Au lieu de : "[format original]"
  Essayer    : "[format alternatif — ex: template pack au lieu d'ebook]"
  Pourquoi   : [raison basée sur l'analyse]

→ Relance /dp-market-research avec le pivot choisi pour revalider.
```

---

## Step 9 — Delivery

Sauvegarder dans `market-research/validation-[slug].md`.

```
✅ VALIDATION TERMINÉE

📄 Rapport    : market-research/validation-[slug].md
📊 Score      : [XX]/100
🎯 Verdict    : [GO / TEST / STOP]
🔎 Concurrents: [N] analysés
💰 Prix rec.  : [fourchette €]
📈 Demande    : [Forte / Modérée / Faible]

SI GO :
  → /dp-playbook-create [sujet]   — Commencer la création
  → Angle recommandé : [angle]
  → Prix recommandé : [prix]

SI TEST :
  → Lance les 5 tests ci-dessus
  → Reviens avec les résultats : /dp-market-research pivot [idée]

SI STOP :
  → /dp-market-research [idée pivot]   — Tester un pivot
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Au moins 3 sources de demande consultées (pas se fier à une seule) | Critical |
| QG-02 | Au moins 3 concurrents analysés (sauf si le marché en a moins) | Critical |
| QG-03 | Chaque score (0-10) est justifié par des données observables, pas du "feeling" | Critical |
| QG-04 | Le verdict est cohérent avec le score (pas de GO à 45/100 ni de STOP à 80/100) | Critical |
| QG-05 | Les prix sont basés sur des prix RÉELS de concurrents, pas inventés | Critical |
| QG-06 | Les risques ont chacun une mitigation concrète | High |
| QG-07 | Le rapport différencie clairement les DONNÉES VÉRIFIÉES des ESTIMATIONS | High |
| QG-08 | Pas de biais de confirmation — si l'idée est faible, le dire honnêtement | Critical |
| QG-09 | Les tests rapides (si TEST) ont des signaux GO/STOP mesurables | High |
| QG-10 | Le plan d'action post-verdict référence les skills DP Créateur pertinents | Medium |
| QG-11 | Si STOP, au moins 2 pivots alternatifs proposés | High |
| QG-12 | Aucune promesse de résultats garantis ("ce marché VA marcher") — honnêteté | Critical |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| L'utilisateur n'a aucune idée | Poser : "Quel problème tu sais résoudre ?", "Quelle question on te pose le plus ?", "Quel sujet te passionne ?" → Générer 3-5 idées à valider |
| L'idée est trop vague ("le marketing") | Proposer 3-5 sous-niches spécifiques. "Marketing pour dentistes" vs "Marketing pour freelances" |
| L'idée est trop niche | Vérifier quand même — parfois les niches sont très rentables. Si demande = 0, suggérer d'élargir |
| Aucun concurrent trouvé | Ce n'est PAS forcément bon signe. Chercher plus largement (cours, livres, vidéos, pas seulement ebooks). Si vraiment rien : soit opportunité rare, soit pas de demande. |
| Trop de concurrents (20+) | Marché saturé mais demande prouvée. Chercher les gaps et les angles non couverts. |
| L'utilisateur conteste le verdict | Présenter les données. "Je comprends, mais voici les chiffres : [données]. Si tu veux quand même y aller, lance les 5 tests d'abord." |
| business-profile.md absent | Poser les questions directement. Recommander de le créer après si GO. |
| WebSearch/WebFetch ne donne pas de résultats | Utiliser des formulations alternatives. Essayer en anglais ET en français. Si vraiment rien : noter "données insuffisantes" et scorer prudemment (5/10). |
| L'utilisateur a déjà un produit et veut valider une V2 | Adapter : analyser les ventes actuelles, les retours clients, les demandes reçues. C'est une validation différente (amélioration vs création). |

---

## Biais à Éviter

| Biais | Description | Comment l'éviter |
|-------|-------------|-----------------|
| **Confirmation** | Chercher uniquement les données qui valident l'idée | Chercher activement les CONTRE-ARGUMENTS |
| **Survivant** | Ne regarder que les succès concurrents | Chercher aussi les échecs (produits retirés, peu de reviews) |
| **Ancrage** | Se fixer sur le premier prix vu | Analyser TOUS les prix avant de recommander |
| **Optimisme** | Surestimer la demande | Utiliser les scénarios PESSIMISTES comme base |
| **Sunk cost** | L'utilisateur a déjà investi du temps et ne veut pas entendre STOP | Être honnête. 5h perdues < 30h perdues. |

---

## Cross-Skill Integration

| Avant market-research | Skill | Quand |
|-----------------------|-------|-------|
| Profil business | `/dp-business-profile` | Si pas encore créé (pour contexte niche + audience) |

| Après market-research | Skill | Quand |
|----------------------|-------|-------|
| Si GO : Créer l'ebook | `/dp-playbook-create` | Verdict = GO |
| Si GO : Page de vente | `/dp-landing-page` | Après création ebook |
| Si GO : Stratégie blog | `/dp-blog-strategy` | Pour le SEO autour du produit |
| Si TEST : Lead magnet | `/dp-lead-magnet-create` | Pour le test #2 (lead magnet test) |
| Si TEST : Landing page | `/dp-landing-page` | Pour le test #3 (pré-vente) |
| Si STOP : Re-valider | `/dp-market-research pivot` | Avec une nouvelle idée |
| Concurrence approfondie | `/dp-competitor-analysis` | Si besoin d'aller plus loin |
| Pricing approfondi | `/dp-pricing-strategy` | Si le pricing est un facteur clé |
