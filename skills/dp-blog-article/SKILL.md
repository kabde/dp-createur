---
name: dp-blog-article
description: "Rédige des articles de blog SEO complets avec optimisation on-page, maillage interne stratégique, E-E-A-T signals, CTAs naturels, schema JSON-LD, et AI search readiness. Intègre le maillage avec les articles existants. Peut publier directement sur WordPress. Triggers: blog, article, SEO, écrire article, rédaction blog, content marketing, post SEO, maillage."
user-invokable: true
argument-hint: "[keyword] [type: standard|pilier|satellite] [format: html|markdown|wordpress]"
allowed-tools: Read Write Bash Glob WebSearch WebFetch
metadata:
  author: DP Créateur
  version: "2.1.0"
  category: content
  updated: 2026-04-13
---

# Blog Article — Rédacteur SEO avec Maillage Interne

<!-- v2.1.0 | 2026-04-13 | Refonte majeure : maillage interne, topic clusters, E-E-A-T, schema JSON-LD, publication WordPress, scoring SEO -->

Rédacteur SEO expert pour DP Créateur. Ne se contente pas d'écrire un article — construit un contenu qui s'intègre dans un écosystème de maillage interne, optimisé pour Google ET pour les moteurs AI (ChatGPT, Perplexity, Google AI Overviews).

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-blog-article [keyword]` | Création guidée complète |
| `/dp-blog-article express [keyword]` | Mode rapide — 3 questions puis rédaction |
| `/dp-blog-article pilier [keyword]` | Article pilier (3000+ mots, hub du cluster) |
| `/dp-blog-article satellite [keyword]` | Article satellite (1500 mots, lié au pilier) |
| `/dp-blog-article outline [keyword]` | Plan seul, pas de rédaction |
| `/dp-blog-article update [fichier]` | Mettre à jour un article existant |
| `/dp-blog-article from [fichier]` | Restructurer du contenu existant en article SEO |

## Output Format

```
LIVRABLES :
├── Article (blog/[slug].[html|md])
│   ├── Contenu SEO-optimisé (H1 > H2 > H3)
│   ├── Meta tags (title, description, OG, Twitter Card)
│   ├── Schema JSON-LD (BlogPosting)
│   ├── Maillage interne (3-10 liens contextuels)
│   ├── CTAs naturels (soft, medium, hard)
│   ├── Section FAQ (People Also Ask)
│   └── CSS embarqué si HTML :
│       /* Si business-profile.md existe, utiliser ses couleurs : */
│       :root {
│         --color-primary: [primary_color depuis business-profile.md ou context intake];
│         --color-accent: [accent_color depuis business-profile.md ou context intake];
│       }
│       /* Appliquer aux liens, titres, et CTAs */
├── Score SEO 0-100
├── Carte de maillage (liens entrants/sortants)
├── 5 suggestions de contenu dérivé
└── Option : publication directe sur WordPress
```

---

## Process

```
1. Context intake       → Business, keyword, audience, produit, voix
2. Read references      → SEO standards, maillage, articles existants
   Read references/article-example.md → pour une structure d'article annotée
3. Keyword analysis     → Variations, questions, intent mapping
4. Plan article         → Structure validée par l'utilisateur
5. Write content        → Rédaction SEO avec maillage intégré
6. SEO quality check    → Score 0-100, quality gates
7. Content derivatives  → 5 contenus dérivés suggérés
8. Deliver / Publish    → Fichier local OU publication WordPress
```

---

## Step 1 — Context Intake (Required)

### 1a. Charger le contexte (silencieux)

```
1. SI business-profile.md existe → lire (nom, niche, produits, audience, voix, couleurs)
2. SI blog-strategy/ existe → lire la stratégie pour identifier :
   - Le cluster auquel cet article appartient
   - Les articles déjà planifiés (maillage)
   - Le pilier du cluster (lien obligatoire)
3. SI blog/ contient des articles → lister les fichiers existants pour le maillage
4. Charger references/seo-standards.md et references/internal-linking.md
```

### 1b. Questions par blocs

#### Bloc 1 — Le sujet et le SEO

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Quel est le **mot-clé principal** que tu veux cibler ? | Optimisation SEO |
| Q2 | Quelle est l'**intention de recherche** ? `informational` (apprendre) / `commercial` (comparer) / `transactional` (acheter) | Structure et ton |
| Q3 | Quel **type d'article** ? `pilier` (3000+ mots, hub du cluster) / `standard` (2000 mots) / `satellite` (1500 mots, lié à un pilier) | Profondeur et maillage |

#### Bloc 2 — L'audience et le produit

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | C'est pour **qui** cet article ? Décris ton lecteur idéal. | Ton et profondeur |
| Q5 | Quel **produit/service** promouvoir dans les CTAs ? (nom, prix, URL) | CTAs contextuels |
| Q6 | Tu as un **angle unique** ? (expérience perso, données, méthode, opinion contrarian) | E-E-A-T + différenciation |

#### Bloc 3 — Le format et le maillage

| # | Question | Pourquoi |
|---|----------|----------|
| Q7 | Quel **format de sortie** ? `html` / `markdown` / `wordpress` (publication directe) | Livrable |
| Q8 | Tu as d'**autres articles** sur ton blog ? Si oui, liste les sujets ou les URLs pour que je crée le maillage. | Maillage interne |
| Q9 | **Voix de marque** en 2-3 mots ? (ex: "direct et expert", "chaleureux et pédagogue") | Cohérence |

#### Si format = wordpress (questions supplémentaires)

| # | Question | Pourquoi |
|---|----------|----------|
| Q10 | Quelle est l'**URL de ton site WordPress** ? (ex: https://monsite.com) | Endpoint API |
| Q11 | Quel est ton **nom d'utilisateur WordPress** ? | Authentification |
| Q12 | Quel est ton **mot de passe d'application WordPress** ? (Généré dans Utilisateurs > Profil > Mots de passe d'application) | Authentification API REST |

> **Note sécurité** : Le mot de passe d'application est différent de ton mot de passe WordPress. Il est généré spécifiquement pour les accès API. Ne partage JAMAIS ton mot de passe WordPress principal.

---

## Step 2 — Keyword Analysis

Avant de planifier l'article, analyser le mot-clé :

```
ANALYSE MOT-CLÉ : "[keyword]"
════════════════════════════════

Mot-clé principal     : [keyword]
Intent                : [informational/commercial/transactional]
Type article          : [pilier/standard/satellite]

Variations et LSI :
  - [variation 1]
  - [variation 2]
  - [variation 3]
  - [variation longue traîne 1]
  - [variation longue traîne 2]

Questions "People Also Ask" :
  - [Question 1] ?
  - [Question 2] ?
  - [Question 3] ?
  - [Question 4] ?
  - [Question 5] ?

Sous-sujets à couvrir :
  - [Sous-sujet 1] → H2 ou H3
  - [Sous-sujet 2] → H2 ou H3
  - [Sous-sujet 3] → H2 ou H3
```

---

## Step 3 — Plan Article (validation obligatoire)

> Read `references/seo-standards.md` pour les critères SEO.

```
PLAN D'ARTICLE
══════════════

SEO :
  Title tag    : [< 60 car, mot-clé au début]
  Meta desc    : [< 160 car, mot-clé + CTA]
  Slug         : /blog/[keyword-slug]
  Mots cible   : [N]
  Langue       : [FR/EN]

Structure :
  INTRO (200-300 mots)
    - Hook : [accroche qui nomme le problème]
    - Promesse : [ce que le lecteur va apprendre]
    - Contexte : [pourquoi c'est important maintenant]
    → 1 lien interne vers le pilier ou article fondamental

  H2: [Section 1 — cible variation keyword]
    H3: [Sous-sujet A]
    H3: [Sous-sujet B]
    → Lien interne vers [article existant]
    → [Soft CTA après cette section]

  H2: [Section 2 — répond à question PAA]
    H3: [Sous-sujet C]
    H3: [Sous-sujet D — template/script inclus]
    → Lien interne vers [article existant]

  H2: [Section 3 — framework actionnable]
    H3: [Étape 1]
    H3: [Étape 2]
    H3: [Étape 3]
    → [Medium CTA après cette section]

  H2: [Section 4 — cible variation longue traîne]
    → Lien interne vers [article existant]

  H2: FAQ — [3-5 questions People Also Ask]
    → 1 lien interne dans une réponse

  CONCLUSION (200-300 mots)
    - Récap des 3 points clés
    - [Hard CTA]
    → 2 liens vers articles complémentaires

Maillage planifié :
  SORTANTS : → [Article A] (ancre: "[texte]")
             → [Article B] (ancre: "[texte]")
             → [Article C] (ancre: "[texte]")
  ENTRANTS : ← Mettre à jour [Article X] pour lier vers celui-ci
             ← Mettre à jour [Article Y] pour lier vers celui-ci
```

**Hard gate** : Ne PAS rédiger sans validation du plan.

---

## Step 4 — Write Content

### 4a. Règles SEO on-page

> Read `references/seo-standards.md` pour les specs complètes.

| Élément | Règle |
|---------|-------|
| H1 | 1 seul. Contient le mot-clé. < 60 caractères. |
| H2 | Chaque H2 cible une variation ou une question. Pas de "Introduction" ou "Conclusion" comme H2. |
| H3 | Découpe en blocs scannables. 3-5 par H2. |
| Premier paragraphe | Mot-clé dans les 100 premiers mots. |
| Densité | 1-2% naturelle. Si ça sonne forcé, c'est trop. |
| Paragraphes | 2-4 phrases max. |
| Listes | Au moins 1 liste par section H2. |

### 4b. Maillage interne

> Read `references/internal-linking.md` pour le guide complet.

```
RÈGLES DE MAILLAGE PENDANT LA RÉDACTION :

1. Premier lien interne dans les 200 premiers mots
2. 3-10 liens internes selon la longueur
3. Ancres TOUJOURS descriptives et variées
4. Liens dans le FLUX du texte (pas isolés)
5. Lien vers le pilier du cluster obligatoire
6. Les liens apportent de la VALEUR au lecteur
7. Vérifier que les articles cibles existent
```

### 4c. E-E-A-T Signals

Intégrer dans l'article :

| Signal | Implémentation |
|--------|---------------|
| **Experience** | Anecdotes perso : "Quand j'ai testé...", "En 3 ans de pratique..." |
| **Expertise** | Données chiffrées, frameworks structurés, sources citées |
| **Authority** | Byline auteur avec bio, date de publication, sources reconnues |
| **Trust** | Disclaimers, caveats honnêtes, pas de promesses, date de mise à jour |

### 4d. AI Search Readiness

Pour maximiser les chances d'être cité par les AI :

| Technique | Implémentation |
|-----------|---------------|
| Passage citable | 134-167 mots auto-suffisants par section clé |
| Réponse directe | Répondre à la question dans les 40-60 premiers mots de la section |
| Données structurées | Tableaux, listes, comparaisons |
| Définitions | Format "X est..." pour les concepts clés |
| Questions en H2/H3 | "Comment [X] ?", "Pourquoi [Y] ?" |

### 4e. CTAs intégrés

**Soft CTA** (après la 1ère section) :
```
💡 Ce [concept] fait partie de [Produit]. [Bénéfice principal en 1 phrase] — [prix]. [Lien]
```

**Medium CTA** (mi-article, après avoir prouvé l'expertise) :
```
Ce [framework/méthode] est tiré de [Produit], qui contient [N] autres [outils/templates] pour [résultat]. [Lien]
```

**Hard CTA** (conclusion) :
```
Si tu veux [résultat concret], [Produit] est fait pour toi. C'est un [type] à [prix] avec [bénéfice #1] + [bénéfice #2]. [Lien]
```

### 4f. Schema JSON-LD

Inclure dans le `<head>` de l'article HTML :

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "[Titre H1]",
  "description": "[Meta description]",
  "author": {
    "@type": "Person",
    "name": "[Auteur]",
    "url": "[URL profil auteur]"
  },
  "datePublished": "[YYYY-MM-DD]",
  "dateModified": "[YYYY-MM-DD]",
  "publisher": {
    "@type": "Organization",
    "name": "[Nom business]"
  },
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "[URL article]"
  },
  "keywords": ["[keyword1]", "[keyword2]", "[keyword3]"]
}
</script>
```

### 4g. Section FAQ avec schema

```html
<section class="faq">
  <h2>Questions fréquentes</h2>

  <div class="faq-item">
    <h3>[Question 1] ?</h3>
    <p>[Réponse directe, 40-100 mots, auto-suffisante]</p>
  </div>
  <!-- ... 3-5 questions ... -->
</section>

<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "[Question 1]",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "[Réponse]"
      }
    }
  ]
}
</script>
```

> **Note** : FAQPage ne génère plus de résultats enrichis Google (depuis août 2023, réservé aux sites gov/santé), mais reste utile pour les citations AI et la structure du contenu.

---

## Step 5 — SEO Quality Score

### Score SEO (0-100)

| Catégorie | Poids | Checks |
|-----------|-------|--------|
| On-Page SEO | 30% | Title, meta, H1, keyword density, slug, premier paragraphe |
| Contenu | 25% | Profondeur, actionabilité, QUOI/POURQUOI/COMMENT/MESURE, templates |
| Maillage | 20% | Liens internes (quantité, ancres, placement), pas d'orphelin |
| E-E-A-T | 15% | Experience, expertise, authority, trust signals |
| Structure | 10% | Hiérarchie H, paragraphes courts, listes, FAQ, schema |

### Affichage

```
SEO SCORE : [XX]/100

On-Page    [████████░░] 82/100
Contenu    [██████████] 95/100
Maillage   [███████░░░] 73/100
E-E-A-T    [████████░░] 80/100
Structure  [█████████░] 88/100

Issues :
  ⚠️ [Issue 1]
  ⚠️ [Issue 2]
```

---

## Step 6 — Deliver or Publish

### Option A : Fichier local

Sauvegarder dans `blog/[slug].[html|md]`.

### Option B : Publication WordPress

Si l'utilisateur a choisi le format `wordpress` et fourni les credentials :

```bash
# Publication via WordPress REST API
curl -X POST "[WP_URL]/wp-json/wp/v2/posts" \
  -u "[username]:[app_password]" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "[Titre]",
    "content": "[Contenu HTML]",
    "status": "draft",
    "slug": "[slug]",
    "excerpt": "[Meta description]",
    "meta": {
      "_yoast_wpseo_title": "[SEO Title]",
      "_yoast_wpseo_metadesc": "[Meta description]"
    }
  }'
```

**Règles de publication WordPress :**

| Règle | Détail |
|-------|--------|
| Toujours publier en **draft** d'abord | L'utilisateur review et publie manuellement |
| Tester la connexion avant de publier | `curl -u user:pass [WP_URL]/wp-json/wp/v2/posts?per_page=1` |
| Vérifier que l'API REST est activée | Tester `[WP_URL]/wp-json/` |
| Ne JAMAIS stocker le mot de passe | Utilisé en mémoire uniquement, pas écrit dans un fichier |
| Confirmer avec l'utilisateur avant d'envoyer | "Je vais publier en brouillon sur [URL]. Tu confirmes ?" |
| Gérer les catégories et tags | Demander si l'utilisateur veut assigner des catégories |

**Après publication :**
```
✅ ARTICLE PUBLIÉ EN BROUILLON

📄 URL admin  : [WP_URL]/wp-admin/post.php?post=[ID]&action=edit
📌 Titre      : [titre]
📊 Status     : Brouillon (à publier manuellement)
🔗 Slug       : /blog/[slug]

CHECKLIST AVANT PUBLICATION :
  [ ] Relire l'article
  [ ] Ajouter les images (alt text inclus dans le contenu)
  [ ] Vérifier le maillage (liens internes actifs)
  [ ] Configurer le SEO (Yoast/RankMath)
  [ ] Planifier ou publier
```

### Résumé de livraison (tous formats)

```
✅ ARTICLE CRÉÉ — Score SEO : [XX]/100

📄 Fichier  : [chemin ou URL]
📌 Titre    : [titre]
📦 Type     : [pilier/standard/satellite]
📊 Mots     : ~[N]
🔗 Maillage : [N] liens internes sortants, [N] articles à mettre à jour

MAILLAGE — ACTIONS POST-PUBLICATION :
  → Ajouter un lien vers cet article dans : [Article A], [Article B]
  → Avec les ancres : "[ancre 1]", "[ancre 2]"

CONTENU DÉRIVÉ :
  1. Carousel Instagram — [sujet] (5-7 slides)
  2. Post LinkedIn — Accroche + lien
  3. Email — Objet : "[objet]" + teaser
  4. Thread X — 5 tweets clés
  5. Reel/TikTok — 30s sur le takeaway #1

PROCHAINES ÉTAPES :
  → /dp-blog-publish          — Publier sur WordPress (si pas déjà fait)
  → /dp-social-caption        — Captions pour promouvoir
  → /dp-email-sequence        — Envoyer à la liste
  → /dp-ad-angles-meta        — Amplifier en pub
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Aucun placeholder ([TODO], [INSERT], Lorem ipsum) | Critical |
| QG-02 | H1 unique, contient le mot-clé principal, < 60 caractères | Critical |
| QG-03 | Meta description < 160 caractères, inclut le mot-clé | Critical |
| QG-04 | Mot-clé dans les 100 premiers mots | High |
| QG-05 | Aucune promesse de revenus ou résultats garantis | Critical |
| QG-06 | Liens internes : ≥ 3 pour article < 1500 mots, ≥ 5 pour 1500-2500 mots, ≥ 8 pour 2500+ mots | Critical |
| QG-07 | Toutes les ancres internes sont descriptives et différentes | High |
| QG-08 | Lien vers le pilier du cluster si article satellite | Critical |
| QG-09 | Section FAQ présente avec 3-5 questions | High |
| QG-10 | Schema JSON-LD BlogPosting inclus | High |
| QG-11 | Hiérarchie H1 > H2 > H3 respectée (pas de saut) | Critical |
| QG-12 | Pas de keyword stuffing — densité 1-2% max | Critical |
| QG-13 | Au moins 2 liens externes vers sources autoritaires | Medium |
| QG-14 | Paragraphes < 4 phrases | Medium |
| QG-15 | Au moins 1 framework, script, ou template actionnable | High |
| QG-16 | Byline auteur avec date de publication | High |
| QG-17 | Si WordPress : publié en DRAFT uniquement, jamais en publish direct | Critical |
| QG-18 | Si WordPress : mot de passe non écrit dans un fichier | Critical |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Pas de mot-clé | Demander : "Quel sujet intéresse ton audience ?" puis aider à trouver le keyword |
| Mot-clé trop large ("marketing") | Proposer 3-5 long-tail plus ciblés |
| Mot-clé trop niche | Prévenir honnêtement, suggérer un keyword parent |
| Pas de produit à promouvoir | CTAs vers newsletter, lead magnet, ou engagement |
| Pas d'articles existants pour le maillage | Créer l'article comme premier contenu, noter les futurs liens |
| Stratégie blog inexistante | Recommander `/dp-blog-strategy` d'abord, mais ne pas bloquer |
| business-profile.md absent | Poser les questions de contexte directement |
| WordPress API inaccessible | Diagnostiquer : URL incorrecte ? API REST désactivée ? Plugin de sécurité ? Proposer de sauvegarder en local. |
| Mot de passe WordPress incorrect | Guider : "Va dans WordPress > Utilisateurs > Profil > Mots de passe d'application. Crée un nouveau mot de passe." |
| Sujet YMYL (santé, finance, juridique) | Ajouter des disclaimers, recommander une relecture par un expert |
| L'utilisateur veut aller vite | Mode express : Q1, Q4, Q7 seulement |
| Contenu existant à restructurer | Lire, analyser, réorganiser pour le SEO plutôt que repartir de zéro |

---

## Cross-Skill Integration

| Avant | Skill | Quand |
|-------|-------|-------|
| Stratégie de contenu | `/dp-blog-strategy` | Pour avoir la liste d'articles et le maillage planifié |
| Profil business | `business-profile.md` | Si pas encore créé |
| Analyse concurrence | `/dp-competitor-analysis` | Pour identifier les angles différenciants |

| Après | Skill | Quand |
|-------|-------|-------|
| Publier sur WordPress | `/dp-blog-publish` | Si pas fait en direct |
| Captions sociales | `/dp-social-caption` | Pour promouvoir |
| Séquence email | `/dp-email-sequence` | Pour envoyer à la liste |
| Calendrier éditorial | `/dp-mediaplan` | Pour planifier la promo |
| Publicité | `/dp-ad-angles-meta` `/dp-ad-angles-google` | Pour amplifier |
| Audit SEO | `/dp-playbook-audit` | Pour vérifier la qualité |
