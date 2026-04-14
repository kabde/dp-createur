---
name: dp-mediaplan
description: "Génère un calendrier de contenu social media complet (1-8 semaines) avec briefs détaillés par post, direction visuelle, et stratégie de funnel TOFU/MOFU/BOFU. Produit un fichier HTML standalone avec design dark-theme. Supporte Instagram, LinkedIn, Facebook, TikTok. Triggers : media plan, calendrier contenu, content calendar, social media, planning, publication, réseaux sociaux."
user-invokable: true
argument-hint: "[plateforme(s)] [durée: 2w|4w|8w] [objectif: awareness|leads|launch|evergreen]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: operations
  updated: 2026-04-13
---

# Media Plan — Social Media Content Calendar

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake, brand identity, visual direction, quality gates, error handling, cross-skill integration -->

Expert en stratégie social media. Génère un calendrier de contenu complet avec briefs détaillés, direction visuelle par post, et stratégie de funnel — livré en fichier HTML standalone prêt à utiliser.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-mediaplan [plateforme]` | Calendrier 4 semaines pour une plateforme |
| `/dp-mediaplan [plateforme] 2w` | Calendrier 2 semaines (format court) |
| `/dp-mediaplan [plateforme] 8w` | Calendrier 8 semaines (format long) |
| `/dp-mediaplan [plateforme] launch` | Calendrier orienté lancement de produit |
| `/dp-mediaplan multi [IG+LI]` | Calendrier multi-plateformes |
| `/dp-mediaplan launch [produit]` | Calendrier de lancement J-14 → J+7 |

## Output Format

```
LIVRABLE :
├── Fichier HTML standalone (mediaplan-[platform]-[mois].html)
├── CSS dark-theme embarqué (design system media plan)
├── Stratégie funnel TOFU/MOFU/BOFU
├── Briefs détaillés par post (hook, copy, CTA, visual direction)
├── Direction visuelle par post (couleurs, style, notes designer)
├── KPIs et métriques de suivi
└── Guidelines de production
```

---

## Process

```
1. Context intake      → Collecter plateforme, produit, identité visuelle, messages (OBLIGATOIRE)
2. Read references     → Charger le design system HTML du media plan de référence
3. Design strategy     → Funnel mix, content mix, fréquence adaptée
4. Build calendar      → Thème par semaine + briefs détaillés par post
5. Assemble HTML       → Fichier standalone avec le design system dark-theme
6. Quality check       → Vérification funnel balance, complétude, règles
7. Deliver             → Fichier HTML + résumé + prochaines étapes
```

---

## Step 1 — Context Intake (Required: Always Do This First)

Avant toute planification, collecter le contexte. Sans lui, le contenu sera générique et les visuels incohérents.

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom, niche, produit(s), audience, ton, couleurs, style visuel
  → Ne PAS reposer les questions déjà couvertes par le profil

SINON :
  → Continuer sans. Les questions de l'intake couvriront le minimum.
```

### 1b. Poser les questions par blocs

**Règle absolue** : Ne JAMAIS poser toutes les questions d'un coup. Grouper par 2-3, attendre les réponses, reformuler pour valider, puis continuer.

#### Bloc 1 — Plateforme et durée (poser en premier)

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Sur quelle(s) plateforme(s) tu publies ? `Instagram` / `LinkedIn` / `Facebook` / `TikTok` (ou plusieurs) | Adapte le format et le ton |
| Q2 | Sur combien de semaines ? (par défaut : 4 semaines) | Scope du calendrier |
| Q3 | Quel est ton objectif principal ? `awareness` (notoriété) / `leads` (génération de prospects) / `launch` (lancement produit) / `evergreen` (contenu pérenne) | Oriente la stratégie de funnel |
| Q4 | Tu as une couleur principale de marque ? (hex ou nom) | Identité visuelle des contenus |
| Q5 | Et une couleur d'accent ? (pour les CTAs, highlights) | Cohérence visuelle |
| Q6 | Quel style visuel ? `minimaliste` / `bold` / `premium` / `warm` | Direction artistique |

**Après les réponses** : Confirmer. "OK, je vais créer un calendrier [plateforme] sur [X] semaines, orienté [objectif]."

#### Bloc 2 — Produit et messages

| # | Question | Pourquoi |
|---|----------|----------|
| Q7 | Quel produit ou offre veux-tu promouvoir ? Décris en 2-3 phrases (nom, prix, pour qui, quel résultat). | Contenu des posts BOFU |
| Q8 | Quels sont tes 3-5 messages clés ? Les idées que tu veux que ton audience retienne. | Fil rouge éditorial |
| Q9 | Tu as des sujets ou angles spécifiques à couvrir ? Ou des sujets à éviter ? | Cadrage éditorial |

**Après les réponses** : Reformuler les messages clés. "Tes messages centraux sont : [1], [2], [3]. C'est bien ça ?"

#### Bloc 3 — Identité visuelle et brand (CRITIQUE pour la direction visuelle)

| # | Question | Pourquoi |
|---|----------|----------|
| Q10 | Quelles sont tes couleurs de marque ? Donne-moi ta couleur primaire et ta couleur d'accent (hex, nom, ou description). *(Confirmer/affiner les réponses Q4-Q6)* | Cohérence visuelle des briefs |
| Q11 | Tu as un nom de marque / logo / polices spécifiques à utiliser dans les visuels ? | Branding cohérent |

> **Si l'utilisateur n'a pas de couleurs** : Proposer 3 palettes adaptées à sa niche :
> - Chaque palette = couleur primaire + couleur accent + couleur texte
> - Montrer un aperçu : "Palette 1 : Bleu profond (#1e3a5f) + Or (#d4a853) — style premium/confiance"
> - Demander de choisir ou combiner

**Après les réponses** : Résumer les choix visuels. Ces informations seront utilisées dans CHAQUE brief de post pour la direction visuelle.

### 1c. Synthèse de découverte

Après tous les blocs, présenter :

```
╔══════════════════════════════════════════════════╗
║           MEDIA PLAN — SYNTHÈSE                  ║
╠══════════════════════════════════════════════════╣
║ Plateforme(s) : [plateforme(s)]                  ║
║ Durée         : [X] semaines                     ║
║ Objectif      : [objectif]                       ║
║ Fréquence     : [X] posts/semaine                ║
╠══════════════════════════════════════════════════╣
║ PRODUIT                                          ║
║ Nom           : [nom du produit]                 ║
║ Prix          : [prix]                           ║
║ Audience      : [pour qui]                       ║
╠══════════════════════════════════════════════════╣
║ MESSAGES CLÉS                                    ║
║ 1. [message 1]                                   ║
║ 2. [message 2]                                   ║
║ 3. [message 3]                                   ║
╠══════════════════════════════════════════════════╣
║ IDENTITÉ VISUELLE                                ║
║ Primaire       : [#hex — nom]                    ║
║ Accent         : [#hex — nom]                    ║
║ Style          : [minimaliste/bold/premium/warm/raw] ║
║ Marque/Police  : [infos branding]                ║
╠══════════════════════════════════════════════════╣
║ EXCLUSIONS     : [sujets à ne pas aborder]       ║
╚══════════════════════════════════════════════════╝
```

**Demande validation** : "Cette synthèse est correcte ? Tu veux modifier quelque chose avant que je construise le calendrier ?"

**Hard gate** : Ne PAS continuer sans validation explicite.

---

## Step 2 — Read Reference Design

### 2a. Charger le media plan de référence (silencieux)

```
Read references/post-briefs-example.md → pour 1 semaine de briefs Instagram
Read mediaplan.html (ou presentation.html) à la racine du projet pour extraire :
  → Le CSS complet du design system dark-theme
  → La structure HTML (header, strategy grid, week sections, content cards)
  → Le format des briefs de contenu dans chaque card

SI le fichier de référence est absent :
  → Utiliser le design system par défaut décrit dans Step 5
```

---

## Step 3 — Design the Strategy

### 3a. Funnel Mix — TOFU / MOFU / BOFU

| Stage | Objectif | Proportion | Couleur tag | Types de contenu |
|-------|----------|-----------|-------------|------------------|
| **TOFU** (Awareness) | Attirer, éduquer | 40% | Vert (`#3ecf8e`) | Tips éducatifs, mythes débunkés, pain points, formats tendance |
| **MOFU** (Consideration) | Crédibiliser, démontrer | 35% | Bleu (`#6E8BFF`) | Cas d'étude, frameworks, coulisses, how-to, processus |
| **BOFU** (Decision) | Convertir | 25% | Rouge (`#ff6b6b`) | Témoignages, offres, urgence, CTA direct vers le produit |

### 3b. Content Mix par plateforme

| Plateforme | Reels/Vidéo | Carrousels/Docs | Statique/Texte | Stories |
|-----------|-------------|-----------------|----------------|---------|
| Instagram | 40% | 35% | 15% | 10% |
| LinkedIn | 10% | 30% | 50% | 10% |
| Facebook | 30% | 20% | 40% | 10% |
| TikTok | 80% | 0% | 10% | 10% |

### 3c. Fréquence de publication

| Plateforme | Fréquence | Meilleurs jours |
|-----------|-----------|-----------------|
| Instagram | 5-6x/semaine | Mardi, Mercredi, Jeudi, Samedi |
| LinkedIn | 4-5x/semaine | Mardi, Mercredi, Jeudi |
| Facebook | 4-5x/semaine | Mercredi, Jeudi, Vendredi |
| TikTok | 7x/semaine (1-2x/jour) | Tous les jours |

### Calendrier de Lancement (mode launch)

Si l'objectif est un lancement de produit, utiliser ce calendrier spécial :

| Phase | Jours | Posts/jour | Objectif | Mix |
|-------|-------|-----------|----------|-----|
| Teasing | J-14 → J-7 | 1 | Créer la curiosité | 100% TOFU |
| Warm-up | J-7 → J-1 | 1-2 | Révéler le produit, éduquer | 60% MOFU / 40% TOFU |
| Launch | J-0 → J+2 | 2-3 | Convertir | 80% BOFU / 20% MOFU |
| Social proof | J+3 → J+5 | 1-2 | Renforcer avec des témoignages | 60% BOFU / 40% MOFU |
| Last call | J+6 → J+7 | 1-2 | Urgence finale | 100% BOFU |

### 3d. Ajustements selon l'objectif

| Objectif | Ajustement funnel | Ajustement contenu |
|----------|------------------|--------------------|
| Awareness | TOFU 55% / MOFU 30% / BOFU 15% | Plus de contenu viral, formats tendance |
| Leads | TOFU 35% / MOFU 40% / BOFU 25% | Plus de lead magnets, DM triggers |
| Launch | TOFU 20% / MOFU 30% / BOFU 50% | Séquence de lancement structurée (teasing → reveal → offre → urgence) |
| Evergreen | TOFU 45% / MOFU 40% / BOFU 15% | Contenu intemporel, recyclable |

---

## Step 4 — Build the Weekly Calendar

Pour chaque semaine, créer :

### 4a. Thème de la semaine

Chaque semaine a un thème narratif qui relie les posts entre eux. Les semaines se construisent les unes sur les autres.

```
SEMAINE [N] — "[Thème]"
Narratif : [ce qu'on construit dans la tête de l'audience cette semaine]
Mix funnel : [X] TOFU / [X] MOFU / [X] BOFU
```

### 4b. Brief détaillé par post (OBLIGATOIRE pour chaque post)

Chaque post reçoit un brief complet. Aucun placeholder.

```
POST BRIEF
═══════════════════════════════
Jour         : [Lundi-Dimanche]
Funnel       : [TOFU ■ / MOFU ■ / BOFU ■]
Format       : [Reel / Carrousel / Statique / Story / Texte]
Sujet        : [sujet spécifique]

HOOK (première ligne — le scroll-stopper) :
"[Hook percutant — court, spécifique, émotionnel ou contrarian]"

COPY (texte complet du post) :
[Caption complète, prête à copier-coller. Formatée pour la plateforme.]

CTA :
[Call to action clair — soft/medium/hard selon le funnel stage]

DIRECTION VISUELLE :
  Couleur dominante : [couleur primaire ou accent de la marque]
  Ambiance          : [description de l'ambiance visuelle]
  Éléments          : [ce qui doit apparaître — texte overlay, photo, illustration]
  Style             : [cohérent avec le style visuel choisi au Q8]
  Format/Specs      : [dimensions, durée si vidéo, nombre de slides si carrousel]

NOTES DESIGNER :
  [Instructions spécifiques pour la personne qui créera le visuel]

HASHTAGS (si applicable) :
  [5-10 hashtags — mix broad + niche + long-tail]
```

### 4c. Hooks — Patterns qui marchent

| Pattern | Exemple |
|---------|---------|
| Contrarian | "La plupart des [pros] échouent parce qu'ils font exactement ce que les gourous conseillent." |
| Chiffre spécifique | "J'ai [résultat concret] en [durée]. Voici la méthode exacte." |
| Pain point | "Tu n'as pas un problème de [X]. Tu as un problème de [Y]." |
| Question | "Et si ton [business/projet] pouvait tourner en [temps] par jour ?" |
| Affirmation forte | "Tu n'as pas besoin de [chose considérée nécessaire] pour [résultat]." |

### 4d. Patterns CTA

| Intensité | Exemples |
|-----------|----------|
| **Soft** | "Sauvegarde ce post." / "Suis pour plus de contenu comme ça." |
| **Medium** | "DM moi '[mot-clé]' pour recevoir [lead magnet]." / "Commente [emoji] si ça te parle." |
| **Hard** | "Lien en bio. [Prix]. Pas de blabla. Que de l'action." / "Offre valable jusqu'à [date]." |

### 4e. Règles de copy

| Faire | Ne PAS faire |
|-------|-------------|
| Phrases courtes. Impact maximal. | Paragraphes-murs |
| Une idée par paragraphe | Multiples idées enchevêtrées |
| Line breaks pour la lisibilité | Blocs de texte compacts (surtout Instagram) |
| Chiffres et résultats spécifiques | "Résultats incroyables" sans détails |
| Adapter le ton à la plateforme | Même texte copié sur toutes les plateformes |
| Terminer par un CTA clair | Fin en queue de poisson |
| Caveats honnêtes quand nécessaire | Promesses garanties ou exagérées |

### 4f. Hashtag Strategy (Instagram / TikTok)

Mix recommandé :
- 3 broad : #[niche large] #[industrie] #[thème général]
- 3 niche : #[spécialité] #[méthode] #[sous-niche]
- 3 long-tail : #[phrase spécifique recherchée] #[question fréquente]

---

## Step 5 — Assemble the HTML

Construire le fichier HTML en utilisant le design system du media plan de référence.

### 5a. Structure HTML requise

```html
<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Marque] — Media Plan [Plateforme] ([Durée])</title>
  <style>
    /* CSS dark-theme copié du fichier de référence */
    /* Couleurs de marque intégrées en CSS custom properties */
    :root {
      --brand-primary: [couleur primaire du Q7];
      --brand-accent: [couleur accent du Q7];
      --tofu: #3ecf8e;
      --mofu: #6E8BFF;
      --bofu: #ff6b6b;
    }
  </style>
</head>
<body>
  <div class="container">
    <!-- Header avec branding -->
    <div class="header">
      <div class="label">[Plateforme] Content Strategy</div>
      <h1>[Titre du media plan]</h1>
      <p>[Durée] • [Objectif] • [Produit]</p>
    </div>

    <!-- Strategy Overview (3 cards: TOFU, MOFU, BOFU) -->
    <div class="strategy-grid">
      <!-- Cards avec proportions et exemples de contenu -->
    </div>

    <!-- Content Mix chart -->

    <!-- Semaine 1 -->
    <div class="week-section">
      <h2>Semaine 1 — [Thème]</h2>
      <!-- Calendar grid + Content Cards avec briefs complets -->
    </div>

    <!-- Semaines 2, 3, 4... -->

    <!-- KPI Section -->
    <!-- Production Guidelines -->
  </div>
</body>
</html>
```

### 5b. Les couleurs de marque dans les visuels

Les couleurs collectées au Bloc 1 du context intake (Q4-Q6) doivent être :
- Intégrées en CSS custom properties (`--brand-primary`, `--brand-accent`)
- Référencées dans chaque brief de direction visuelle
- Cohérentes avec le style visuel choisi (minimaliste, bold, premium, warm, raw)

### 5c. Sauvegarde

```
Chemin de sortie : mediaplan-[platform]-[mois].html (à la racine du projet)
Si multi-plateformes : mediaplan-multi-[mois].html
```

---

## Step 6 — Quality Check

### 6a. Quality Gates

| ID | Gate | Severity |
|----|------|----------|
| QG-01 | Aucun placeholder dans les briefs (pas de [TODO], [INSERT]) | Critical |
| QG-02 | Chaque post a un brief COMPLET (hook, copy, CTA, visual direction) | Critical |
| QG-03 | Jamais >1 post BOFU consécutif | Critical |
| QG-04 | Chaque post a une direction visuelle avec les couleurs de marque | Critical |
| QG-05 | Proportions funnel respectées : Awareness: TOFU 50-60%, MOFU 25-30%, BOFU 15-20%. Launch: TOFU 30%, MOFU 30%, BOFU 40% | High |
| QG-06 | Chaque semaine a un thème narratif cohérent | High |
| QG-07 | Les semaines progressent narrativement (pas de répétition) | High |
| QG-08 | Minimum 3 formats différents par semaine (ex: carousel + reel + image) | Medium |
| QG-09 | CTA adaptés au funnel stage (pas de hard sell en TOFU) | High |
| QG-10 | CSS complet et embarqué, pas de dépendances externes | Critical |
| QG-11 | Hooks non répétitifs — chaque post a un hook unique | High |
| QG-12 | Copy adaptée à la plateforme (longueur, ton, format) | High |

### 6b. Funnel Balance Check

```
VÉRIFICATION FUNNEL
═══════════════════

Objectif : [objectif choisi]
Target   : TOFU [X]% / MOFU [X]% / BOFU [X]%

Actual   : TOFU [X]% ([N] posts) / MOFU [X]% ([N] posts) / BOFU [X]% ([N] posts)
Status   : [OK / AJUSTER]

Consécutivité BOFU : [OK — aucun back-to-back / VIOLATION — jour X et X+1]
```

---

## Step 7 — KPI Section (inclure en fin de media plan HTML)

| Métrique | Cible | Comment mesurer |
|----------|-------|-----------------|
| Portée | +20% semaine après semaine | Analytics de la plateforme |
| Taux d'engagement | >3% (IG), >5% (LinkedIn) | (Likes+Commentaires+Saves) / Portée |
| Visites profil | +15% semaine après semaine | Analytics de la plateforme |
| Clics lien | Suivre chaque semaine | Outil link-in-bio |
| DMs reçus | 5+/semaine | Comptage manuel |
| Leads générés | 3+/semaine | CRM ou spreadsheet |

---

## Step 8 — Delivery

### 8a. Présenter le livrable

```
MEDIA PLAN CRÉÉ
═══════════════════

Fichier       : [chemin complet]
Plateforme(s) : [plateforme(s)]
Durée         : [X] semaines
Posts totaux  : [N] posts
Objectif      : [objectif]

RÉPARTITION FUNNEL :
  TOFU ■ [XX]% — [N] posts (awareness)
  MOFU ■ [XX]% — [N] posts (considération)
  BOFU ■ [XX]% — [N] posts (conversion)

FORMATS :
  Reels/Vidéo    : [N] posts
  Carrousels     : [N] posts
  Statique/Texte : [N] posts
  Stories        : [N] posts

IDENTITÉ VISUELLE APPLIQUÉE :
  Primaire : [#hex]
  Accent   : [#hex]
  Style    : [style choisi]

PROCHAINES ÉTAPES :
  → /dp-social-caption     Rédiger les captions en détail
  → /dp-ad-angles-meta     Transformer les BOFU en publicités
  → /dp-competitor-analysis Analyser la stratégie social du concurrent
  → /dp-playbook-create     Créer le lead magnet mentionné dans les posts
  → /ebook-cover         Créer les visuels de couverture
```

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Plateforme non spécifiée | Demander avant de continuer — c'est obligatoire |
| Produit non défini | Créer un calendrier awareness/éducation sans BOFU. Prévenir l'utilisateur. |
| Pas de couleurs de marque | Proposer 3 palettes adaptées à la niche, demander de choisir |
| Durée trop longue (>8 semaines) | Découper en phases de 4 semaines. Livrer la première, proposer la suite. |
| Trop de plateformes (>3) | Recommander de prioriser 2 plateformes. Adapter le contenu plutôt que dupliquer. |
| Le fichier mediaplan.html de référence n'existe pas | Utiliser le design system dark-theme par défaut |
| business-profile.md absent | Continuer avec les réponses du context intake |
| L'utilisateur veut le même contenu sur toutes les plateformes | Déconseiller. Adapter au minimum le format et le ton. Expliquer pourquoi. |

---

## Cross-Skill Integration

| Avant mediaplan | Skill précédent | Quand |
|----------------|-----------------|-------|
| Analyser la concurrence | `/dp-competitor-analysis` | Pour identifier les gaps de contenu à exploiter |
| Définir le positionnement | `business-profile.md` | Recommandé pour cohérence |
| Créer le produit à promouvoir | `/dp-playbook-create` | Si le produit n'existe pas encore |

| Après mediaplan | Skill suivant | Quand |
|----------------|---------------|-------|
| Rédiger les captions détaillées | `/dp-social-caption` | Pour développer certains posts |
| Créer les publicités | `/dp-ad-angles-meta` `/dp-ad-angles-google` | Pour amplifier les meilleurs posts |
| Créer le lead magnet | `/dp-playbook-create` `/lead-magnet-create` | Si un lead magnet est mentionné dans le plan |
| Séquence email | `/dp-email-sequence` | Pour la conversion post-DM |
| Page de vente | `/dp-landing-page` | Si le lien en bio mène à une page |
