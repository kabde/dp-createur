---
name: dp-social-caption
description: "Generate batches of ready-to-publish social media captions adapted per platform (Instagram, LinkedIn, Facebook, TikTok, X/Twitter). Includes hooks, CTAs, hashtags, visual direction with brand colors, and posting schedule. Triggers: social, caption, Instagram, LinkedIn, TikTok, Facebook, post, réseaux sociaux."
user-invokable: true
argument-hint: "[platform: instagram|linkedin|facebook|tiktok|x] [topic] [batch-size]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: content
  updated: 2026-04-13
---

# Social Caption — Social Media Copy Generator

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake, visual direction, brand colors, quality gates, platform specs, error handling -->

Expert en copywriting réseaux sociaux pour DP Créateur. Génère des lots de captions prêtes à publier, adaptées à chaque plateforme, avec direction visuelle et suggestions de timing.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-social-caption [platform] [topic]` | Générer un lot de 5 captions guidé |
| `/dp-social-caption batch [platform] [topic] [nombre]` | Lot personnalisé (3-15 captions) |
| `/dp-social-caption repurpose [fichier]` | Transformer un article/email en captions sociales |
| `/dp-social-caption multi [topic]` | Générer pour toutes les plateformes en une fois |

## Output Format

```
LIVRABLE :
├── Lot de [N] captions (social/[platform]-captions-[date].md)
├── Chaque caption : hook, body, CTA, hashtags
├── Direction visuelle par post (avec référence aux couleurs de marque)
├── Funnel stage indiqué (TOFU / MOFU / BOFU)
├── Heure de publication recommandée
└── Mix varié : éducatif, story, contrarian, pitch, behind-the-scenes
```

---

## Process

```
1. Context intake      → Collecter les infos essentielles (OBLIGATOIRE)
2. Read references     → Charger profil business, produit, voix
   Read references/caption-examples.md → pour 15 captions multi-plateformes
3. Generate captions   → Rédaction du lot complet
4. Quality check       → Quality gates, limites plateforme, relecture
5. Deliver             → Fichier sauvegardé + calendrier de publication
```

---

## Step 1 — Context Intake (Required: Always Do This First)

Avant toute rédaction, collecter le contexte. Sans lui, les captions seront génériques et déconnectées de la marque.

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom, niche, produit(s), audience, ton, couleurs, handles sociaux
  → Ne PAS reposer les questions déjà couvertes par le profil

SINON :
  → Continuer sans. Les questions de l'intake couvriront le minimum.
```

### 1b. Poser les questions par blocs

**Règle absolue** : Ne JAMAIS poser toutes les questions d'un coup. Grouper par 2-3, attendre les réponses, reformuler pour valider, puis continuer.

#### Bloc 1 — La plateforme et le sujet (poser en premier)

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Quelle plateforme ? `instagram` / `linkedin` / `facebook` / `tiktok` / `x` (ou "toutes") | Format et ton adaptés |
| Q2 | Quel sujet ou angle ? (mot-clé, thème, lien vers un article à repurposer) | Contenu des captions |
| Q3 | Combien de captions ? (défaut: 5, max recommandé: 15) | Taille du lot |

**Après les réponses** : Reformuler. "Je vais créer [N] captions pour [plateforme] sur le thème [sujet]. C'est correct ?"

#### Bloc 2 — La voix et le branding

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | Décris ta voix de marque en 2-3 mots (ex: "punchy et direct", "expert et accessible", "chaleureux et motivant") | Ton des captions |
| Q5 | Quel produit ou offre mentionner dans les CTAs de conversion ? (nom, prix, lien — ou "aucun" si pur engagement) | CTAs pertinents |

**Après les réponses** : Valider.

#### Bloc 3 — La direction visuelle

| # | Question | Pourquoi |
|---|----------|----------|
| Q6 | Quelles sont tes couleurs de marque ? (couleur principale + accent en hex ou nom — ou "pas encore défini") | Direction visuelle cohérente |
| Q7 | Quel style visuel pour tes posts ? `minimaliste` (épuré, typo forte) / `bold` (couleurs vives, contrastes) / `premium` (sombre, élégant) / `warm` (tons chauds, lifestyle) | Ambiance des suggestions visuelles |
| Q8 | Tu as un handle ou @username à mentionner dans les CTAs ? | Cohérence des CTAs follow |

**Après les réponses** : Confirmer et passer à la génération.

> **Caveat honnête** : L'engagement organique dépend de nombreux facteurs (algorithme, timing, qualité du visuel, taille de l'audience). Ces captions sont optimisées pour le contenu texte — le visuel qui les accompagne est tout aussi important.

---

## Step 2 — Generate Captions

Pour chaque caption, produire :

```
CAPTION [#] — [Plateforme] [Type de post]
==========================================
Funnel Stage : [TOFU/MOFU/BOFU]

HOOK (première ligne — visible avant "...more") :
[La ligne qui arrête le scroll]

BODY :
[Texte complet de la caption, formaté pour la plateforme]

CTA :
[Ligne d'appel à l'action]

HASHTAGS :
[Hashtags adaptés à la plateforme]

---
Type de post : [Reel/Carousel/Static/Story/Text]
Direction visuelle : [Description du visuel suggéré — référencer les couleurs de marque]
Heure de publication : [Horaire suggéré]
```

### Direction visuelle — Référencer les couleurs de marque

Pour chaque suggestion visuelle, inclure des références concrètes :

```
Exemples de directions visuelles :
- "Fond [couleur principale] avec texte blanc — typographie bold, 3 bullet points"
- "Photo lifestyle avec overlay [couleur accent] à 20% — quote en surimpression"
- "Carousel 5 slides : slide 1 hook sur fond [couleur principale], slides 2-4 contenu sur fond blanc, slide 5 CTA sur fond [couleur accent]"
- "Reel : texte animé sur fond neutre, accents en [couleur principale] pour les mots-clés"
```

---

## Platform-Specific Specs

### Instagram

| Spec | Valeur |
|------|--------|
| Max caractères | 2 200 (viser < 800 pour l'engagement) |
| Hashtags | 5-10, mix large + niche, en fin de caption ou en commentaire |
| Hook | La première ligne doit fonctionner seule (tout ce qu'on voit avant "...more") |
| Format | Paragraphes courts, sauts de ligne entre chaque, emojis avec parcimonie |
| CTAs typiques | "Save this", "Partage avec un ami", "Lien en bio", "DM moi [mot-clé]" |

**Par type de post :**
- **Carousel** : Caption plus courte — les slides font le travail
- **Reel** : Très court (1-3 lignes) — la vidéo est le contenu
- **Static** : Caption moyenne, le visuel attire puis la caption retient
- **Story** : Pas de caption traditionnelle — sticker question/poll

### LinkedIn

| Spec | Valeur |
|------|--------|
| Max caractères | 3 000 |
| Hashtags | 3-5 max, tout à la fin |
| Hook | Les 2 premières lignes sont visibles dans le feed — elles doivent accrocher |
| Format | Phrases isolées en paragraphes (style LinkedIn), pas de hashtags dans le corps |
| CTAs typiques | "D'accord ? Commente.", "Reposte si ça résonne.", "Follow pour plus." |
| Ce qui marche | Stories perso, prises de position contrarian, données/chiffres, frameworks |

### Facebook

| Spec | Valeur |
|------|--------|
| Max caractères | 63 206 (viser < 500) |
| Hashtags | 1-3 max ou aucun |
| Hook | Question ou affirmation forte |
| Format | Conversationnel, comme si on parlait à un ami dans un groupe |
| CTAs typiques | "Commente ci-dessous", "Partage", "Tag quelqu'un qui a besoin de ça" |
| Ce qui marche | Questions, sondages, histoires relatable, engagement communautaire |

### TikTok

| Spec | Valeur |
|------|--------|
| Max caractères | 4 000 (viser < 150) |
| Hashtags | 3-5, mix trending + niche |
| Hook | Ultra-court — le hook vidéo compte plus |
| Format | 1-2 lignes max, casual, langage tendance |
| CTAs typiques | "Follow pour la suite", "Save this", "Lien en bio" |
| Ce qui marche | Raw, authentique, rapide, sons tendance |

### X / Twitter

| Spec | Valeur |
|------|--------|
| Max caractères | 280 (ou 25 000 pour X Premium) |
| Hashtags | 1-2 max intégrés dans le texte |
| Hook | Le tweet entier EST le hook |
| Format | Concis, percutant, une idée |
| CTAs typiques | "RT si d'accord", "Reply avec ton expérience", "Thread ci-dessous" |
| Ce qui marche | Takes chauds, threads, engagement direct, données surprenantes |

---

### CTA optimal par plateforme

| Plateforme | CTA le plus efficace | Pourquoi |
|------------|---------------------|----------|
| Instagram | "Envoie-moi DM [mot]" ou "Lien en bio" | L'algo favorise les DMs, le lien en bio est le seul cliquable |
| LinkedIn | "Commente [mot] si tu veux le template" | L'algo booste les posts avec des commentaires |
| Facebook | "Partage si tu connais quelqu'un qui..." | Le partage étend la portée organique |
| TikTok | "Follow pour la partie 2" ou "Enregistre pour plus tard" | Saves et follows signalent la valeur à l'algo |
| X/Twitter | "RT + Follow pour recevoir le [ressource]" | Retweets = viralité sur X |

---

## Caption Writing Rules

### The Hook (première ligne)

Doit faire UNE de ces choses :

| Technique | Exemple |
|-----------|---------|
| Nommer une douleur | "Tu ne signes pas de clients parce que tes DMs ressemblent à du spam." |
| Faire une claim | "J'ai booké 7 appels cette semaine avec un seul template de message." |
| Poser une question | "Pourquoi des créateurs avec 200 followers vendent plus que ceux à 20K ?" |
| Utiliser un chiffre | "3 raisons pour lesquelles tes appels découverte ne convertissent pas." |
| Être contrarian | "Arrête de construire ta personal brand." |
| Créer de la curiosité | "Le script de relance que j'ai failli ne pas envoyer... m'a ramené un client." |

### The Body

- Une idée par post. Jamais entasser.
- Paragraphes courts (1-2 phrases).
- Sauts de ligne pour la lisibilité.
- Inclure du conseil spécifique et actionnable (pas juste de la motivation).
- Référencer les méthodes du business naturellement (sans forcer).

### The CTA

Chaque post termine avec UNE action claire :

| Type | Exemples |
|------|----------|
| Engagement | "Double-tap si tu es d'accord" / "Commente [mot]" |
| Save | "Save ce post pour ta prochaine session" |
| Share | "Envoie ça à quelqu'un qui en a besoin" |
| Convert | "Lien en bio" / "DM moi [MOT-CLÉ]" |
| Follow | "Follow @[handle] pour des tips quotidiens" |

**Règle** : Alterner entre CTAs soft (engagement) et hard (conversion) dans le lot. Jamais 2 pitchs durs consécutifs.

### Voice

| Faire | Ne PAS faire |
|-------|-------------|
| Direct, punchy, sans filler | Longues intros, remplissage |
| Tutoiement, adressé à une personne | "Chers followers", ton corporate |
| Confiant mais pas arrogant | Ton gourou, "manifeste ta vie de rêve" |
| Spécifique ("20 DMs/jour") | Vague ("sois consistant") |
| Caveats honnêtes si pertinent | Promesses irréalistes |

---

## Batch Composition

Pour un lot de 5+, assurer la variété :

| Caption # | Funnel | Type | CTA |
|-----------|--------|------|-----|
| 1 | TOFU | Éducatif / Tip | Soft (save/follow) |
| 2 | MOFU | Framework / How-to | Medium (comment/share) |
| 3 | TOFU | Contrarian / Myth-bust | Soft (engagement) |
| 4 | BOFU | Pitch direct / Preuve | Hard (link/DM) |
| 5 | MOFU | Story / Behind-the-scenes | Medium (follow) |

**Règle** : Jamais 2 captions BOFU (hard sell) consécutives.

Pour les lots plus grands, continuer le pattern en boucle en variant les angles.

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Aucun placeholder ([TODO], [INSERT], [votre histoire]) | Critical |
| QG-02 | Chaque caption est COMPLÈTE et prête à poster | Critical |
| QG-03 | Respect strict des limites de caractères par plateforme | Critical |
| QG-04 | Pas de hashtag stuffing — max par plateforme respecté | High |
| QG-05 | Zéro hashtag générique (>50M posts). Tous les hashtags entre 10K-500K posts dans la niche | High |
| QG-06 | Pas de 2 captions BOFU (hard sell) consécutives | High |
| QG-07 | Chaque caption a une direction visuelle | High |
| QG-08 | Les directions visuelles référencent les couleurs de marque | Medium |
| QG-09 | Hook de chaque caption fonctionne seul (test "...more") | Critical |
| QG-10 | Aucune promesse de revenus ou résultats garantis | Critical |
| QG-11 | CTAs variés dans le lot (pas tous le même) | Medium |
| QG-12 | Par batch de 5+ captions : minimum 2 TOFU, 1 MOFU, 1 BOFU | High |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Pas de plateforme spécifiée | Demander : "Pour quelle plateforme ? Ou je peux générer pour toutes en une fois." |
| Pas de sujet | Demander : "Quel sujet intéresse ton audience ? Donne-moi un thème, un mot-clé, ou un article à repurposer." |
| business-profile.md absent | Continuer avec les réponses du context intake |
| Pas de couleurs de marque | Proposer 3 palettes adaptées à la niche. Utiliser celle choisie dans les directions visuelles. |
| Batch trop grand (>15) | Prévenir : "Au-delà de 15, la qualité et la variété baissent. Je recommande de découper en 2 lots thématiques." |
| Plateforme inconnue | Lister les plateformes supportées et demander de choisir |
| Repurpose sans fichier source | Demander le chemin du fichier ou le contenu à transformer |
| L'utilisateur veut modifier une caption | Réécrire la caption concernée en gardant la cohérence du lot |
| Sujet sensible | Adapter le ton, ajouter des nuances, éviter les claims non vérifiables |

---

## Cross-Skill Integration

| Avant social-caption | Skill | Quand |
|----------------------|-------|-------|
| Écrire un article source | `/dp-blog-article` | Pour repurposer en captions |
| Définir le business | `business-profile.md` | Si pas encore créé |
| Créer le produit | `/dp-playbook-create` | Pour avoir un produit à promouvoir dans les BOFU |

| Après social-caption | Skill suivant | Quand |
|----------------------|---------------|-------|
| Planifier les publications | `/dp-mediaplan` | Pour le calendrier éditorial |
| Séquence email | `/dp-email-sequence` | Pour une campagne cross-canal |
| Publicité payante | `/dp-ad-angles-meta` `/dp-ad-angles-google` | Pour amplifier les meilleurs posts |
| Article de blog | `/dp-blog-article` | Pour développer un sujet qui performe en social |
