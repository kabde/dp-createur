---
name: dp-blog-publish
description: "Publie des articles sur WordPress via l'API REST. Gère l'authentification par mot de passe d'application, la mise en brouillon, les catégories, les tags, les images à la une, et les meta SEO (Yoast/RankMath). Peut publier un article existant ou un batch d'articles. Triggers: publier, wordpress, publish, wp, mise en ligne blog, poster article."
user-invokable: true
argument-hint: "[fichier article] [url-wordpress]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: content
  updated: 2026-04-13
---

# Blog Publish — Publication WordPress

<!-- v2.0.0 | 2026-04-13 | Création : publication WP REST API, auth app password, catégories, tags, meta SEO -->

Publie des articles de blog sur WordPress via l'API REST. Gère l'authentification sécurisée, le formatage, les catégories/tags, et les meta SEO pour Yoast ou RankMath.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-blog-publish [fichier]` | Publier un article existant sur WordPress |
| `/dp-blog-publish batch [dossier]` | Publier plusieurs articles d'un dossier |
| `/dp-blog-publish test` | Tester la connexion WordPress |
| `/dp-blog-publish list` | Lister les derniers articles publiés |
| `/dp-blog-publish update [post-id] [fichier]` | Mettre à jour un article existant |

## Output Format

```
LIVRABLE :
├── Article publié en BROUILLON sur WordPress
├── URL admin pour édition
├── Catégories et tags assignés
├── Meta SEO configurés (si Yoast/RankMath détecté)
└── Checklist de vérification avant publication finale
```

---

## Process

```
1. Context intake     → URL WordPress, credentials, article source
2. Test connexion     → Vérifier l'API REST et l'authentification
3. Préparer le post   → Extraire titre, contenu, meta, catégories
4. Confirm            → Demander confirmation avant envoi
5. Publish (draft)    → Envoyer via API REST en brouillon
6. Post-publish       → Lien admin + checklist
```

---

## Step 1 — Context Intake

### 1a. Charger la config (silencieux)

```
SI un fichier wp-config.md existe à la racine :
  → Lire URL WordPress et username
  → NE PAS stocker de mot de passe dans ce fichier
  
SI business-profile.md existe :
  → Lire pour le nom de l'auteur et la marque
```

### 1b. Questions

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Quel **article** veux-tu publier ? (chemin du fichier ou "je vais coller le contenu") | Source du contenu |
| Q2 | Quelle est l'**URL de ton site WordPress** ? (ex: `https://monsite.com`) | Endpoint API |
| Q3 | Quel est ton **nom d'utilisateur** WordPress ? | Authentification |
| Q4 | Quel est ton **mot de passe d'application** ? | Authentification API |

> **Comment créer un mot de passe d'application WordPress :**
> 1. Va dans ton WordPress → **Utilisateurs** → **Profil**
> 2. Scroll jusqu'à **Mots de passe d'application**
> 3. Entre un nom (ex: "DP Créateur") et clique **Ajouter**
> 4. Copie le mot de passe généré (format: `xxxx xxxx xxxx xxxx xxxx xxxx`)
> 5. Ce mot de passe ne sera plus visible après — garde-le en sécurité
>
> **Prérequis** : WordPress 5.6+ (les mots de passe d'application sont natifs depuis cette version)

---

## Step 2 — Test Connexion

Avant toute publication, tester que la connexion fonctionne :

```bash
# Test 1 : L'API REST est-elle activée ?
curl -s "[WP_URL]/wp-json/" | head -c 200

# Test 2 : L'authentification fonctionne-t-elle ?
curl -s -u "[username]:[app_password]" "[WP_URL]/wp-json/wp/v2/users/me"

# Test 3 : Les permissions sont-elles suffisantes ?
# L'utilisateur doit avoir le rôle "Auteur" ou supérieur
```

### Diagnostic en cas d'erreur

| Erreur | Cause probable | Solution |
|--------|---------------|----------|
| `rest_no_route` | API REST désactivée | Activer dans Réglages > Permaliens (re-sauvegarder) |
| `403 Forbidden` | Plugin de sécurité bloque l'API | Whitelister l'API REST dans le plugin (Wordfence, iThemes, etc.) |
| `401 Unauthorized` | Credentials incorrects | Vérifier username + créer un nouveau mot de passe d'application |
| `invalid_username` | Utilisateur inexistant | Vérifier l'orthographe du username |
| `application_passwords_disabled` | Feature désactivée | WordPress < 5.6 ou désactivé par un plugin. Mettre à jour WP. |
| Timeout | Serveur lent ou URL incorrecte | Vérifier l'URL (https vs http, sous-dossier, etc.) |

**Si la connexion échoue** : Proposer de sauvegarder en local et d'aider l'utilisateur à résoudre le problème.

---

## Step 3 — Préparer le Post

### 3a. Extraire les données de l'article

```
SI l'article est en HTML :
  → Extraire le <title> → titre du post
  → Extraire le <meta name="description"> → excerpt
  → Extraire le contenu du <body>/<article> → contenu
  → Extraire les keywords → tags

SI l'article est en Markdown avec frontmatter :
  → Extraire title, description, slug, keywords du frontmatter
  → Convertir le markdown en HTML pour le contenu
```

### 3b. Récupérer les catégories existantes

```bash
# Lister les catégories WordPress
curl -s -u "[user]:[pass]" "[WP_URL]/wp-json/wp/v2/categories?per_page=100"
```

Demander à l'utilisateur : "Dans quelle catégorie ? Voici les catégories existantes : [liste]. Tu peux aussi en créer une nouvelle."

### 3c. Préparer le payload

```json
{
  "title": "[Titre extrait]",
  "content": "[Contenu HTML nettoyé]",
  "status": "draft",
  "slug": "[slug]",
  "excerpt": "[Meta description]",
  "categories": [ID],
  "tags": [ID1, ID2],
  "meta": {}
}
```

### 3d. Upload d'images (optionnel)

Si l'article contient des images ou si l'utilisateur veut une image à la une :

```bash
# Upload une image sur WordPress
curl -X POST "[WP_URL]/wp-json/wp/v2/media" \
  -u "[username]:[app_password]" \
  -H "Content-Disposition: attachment; filename=[image.jpg]" \
  -H "Content-Type: image/jpeg" \
  --data-binary @[chemin/image.jpg]
```

Après l'upload, récupérer l'ID de l'image et l'ajouter au payload du post :
```json
{
  "featured_media": [IMAGE_ID]
}
```

Demander à l'utilisateur : "Tu as une image à la une ? (chemin du fichier ou URL)"

### 3e. Meta SEO (si Yoast ou RankMath détecté)

```bash
# Détecter si Yoast est installé
curl -s "[WP_URL]/wp-json/yoast/v1/get_head?url=[WP_URL]" 2>/dev/null

# Détecter si RankMath est installé
curl -s "[WP_URL]/wp-json/rankmath/v1/getHead?url=[WP_URL]" 2>/dev/null
```

**Si Yoast détecté**, ajouter au payload :
```json
{
  "meta": {
    "_yoast_wpseo_title": "[SEO Title]",
    "_yoast_wpseo_metadesc": "[Meta description]",
    "_yoast_wpseo_focuskw": "[Mot-clé principal]"
  }
}
```

**Si RankMath détecté**, ajouter :
```json
{
  "meta": {
    "rank_math_title": "[SEO Title]",
    "rank_math_description": "[Meta description]",
    "rank_math_focus_keyword": "[Mot-clé principal]"
  }
}
```

---

## Step 4 — Confirmation

**Hard gate** : TOUJOURS demander confirmation avant d'envoyer.

```
📝 PRÊT À PUBLIER EN BROUILLON
═══════════════════════════════

Site       : [URL WordPress]
Titre      : [titre]
Slug       : /[slug]
Catégorie  : [catégorie]
Tags       : [tag1, tag2]
Mots       : ~[N]
SEO Plugin : [Yoast/RankMath/Aucun]
Status     : BROUILLON (draft)

Tu confirmes l'envoi ? (oui/non)
```

---

## Step 5 — Publish

```bash
# Créer le post en brouillon
curl -X POST "[WP_URL]/wp-json/wp/v2/posts" \
  -u "[username]:[app_password]" \
  -H "Content-Type: application/json" \
  -d '[PAYLOAD JSON]'
```

### Gérer la réponse

```
SI status 201 (Created) :
  → Extraire l'ID du post et l'URL d'édition
  → Afficher le résumé de publication

SI status 400 (Bad Request) :
  → Afficher l'erreur, proposer une correction

SI status 401 (Unauthorized) :
  → Mot de passe expiré ou révoqué. Guider pour en créer un nouveau.

SI status 403 (Forbidden) :
  → Permissions insuffisantes. L'utilisateur doit être Auteur ou Admin.

SI status 500 (Server Error) :
  → Problème côté serveur. Sauvegarder en local et réessayer plus tard.
```

---

## Step 6 — Post-Publication

```
✅ ARTICLE PUBLIÉ EN BROUILLON

📄 URL admin    : [WP_URL]/wp-admin/post.php?post=[ID]&action=edit
📌 Titre        : [titre]
🔗 Permalink    : [WP_URL]/[slug]/ (visible après publication)
📊 Status       : Brouillon
🏷️ Catégorie    : [catégorie]
🔖 Tags         : [tags]
🔍 SEO          : [Yoast/RankMath meta configurés / Pas de plugin SEO]

CHECKLIST AVANT PUBLICATION FINALE :
  [ ] Relire l'article dans WordPress
  [ ] Ajouter l'image à la une
  [ ] Vérifier le rendu mobile (preview)
  [ ] Vérifier les liens internes (cliquables)
  [ ] Vérifier le SEO (vert dans Yoast/RankMath)
  [ ] Planifier ou publier

APRÈS PUBLICATION :
  → Mettre à jour le maillage (ajouter des liens depuis d'autres articles)
  → /dp-social-caption — Créer les posts de promotion
  → /dp-email-sequence — Envoyer à la liste
```

---

## Publication en Batch

Pour publier plusieurs articles :

```
1. Lister les fichiers dans le dossier blog/
2. Pour chaque fichier :
   a. Extraire les données
   b. Afficher un résumé compact
3. Demander confirmation globale : "Je vais publier [N] articles en brouillon. OK ?"
4. Publier un par un avec 2s de délai entre chaque
5. Résumé final avec tous les liens admin
```

---

## Sécurité

| Règle | Détail |
|-------|--------|
| **Ne JAMAIS stocker le mot de passe** | Pas de fichier, pas de variable persistante. Mémoire uniquement. |
| **Ne JAMAIS publier en "publish"** | Toujours "draft". L'utilisateur publie manuellement. |
| **Ne JAMAIS modifier un post existant sans confirmation** | Demander l'ID et confirmer avant d'update. |
| **Ne JAMAIS supprimer un post** | Hors périmètre. Rediriger vers l'admin WordPress. |
| **Ne JAMAIS utiliser le mot de passe WP principal** | Seulement les mots de passe d'application. |
| **Tester avant de publier** | Toujours tester la connexion d'abord. |

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Connexion testée avant toute publication | Critical |
| QG-02 | Confirmation explicite de l'utilisateur avant envoi | Critical |
| QG-03 | Status = "draft" obligatoire (jamais "publish") | Critical |
| QG-04 | Mot de passe non écrit dans un fichier | Critical |
| QG-05 | Titre et contenu non vides | Critical |
| QG-06 | Slug défini et valide | High |
| QG-07 | Meta SEO configurés si plugin détecté | Medium |
| QG-08 | Catégorie assignée | Medium |
| QG-09 | Délai entre publications batch (2s minimum) | Medium |
| QG-10 | Contenu HTML nettoyé (pas de scripts malicieux) | Critical |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| API REST désactivée | Guider : "Réglages > Permaliens, re-sauvegarder" |
| Plugin sécurité bloque l'API | Identifier le plugin, donner les instructions de whitelist |
| Mot de passe incorrect | Guider vers Profil > Mots de passe d'application |
| WordPress < 5.6 | Les app passwords n'existent pas. Recommander la mise à jour. |
| Article sans titre | Extraire le H1 du contenu ou demander |
| Article Markdown (pas HTML) | Convertir en HTML avant envoi |
| Catégorie inexistante | Proposer de la créer via l'API ou d'en choisir une existante |
| Timeout réseau | Sauvegarder en local, réessayer plus tard |
| Quota ou rate limit | Attendre et réessayer avec un délai plus long |
| L'utilisateur veut publier directement (pas en draft) | Refuser poliment. "Pour ta sécurité, je publie toujours en brouillon. Tu peux publier en 1 clic depuis l'admin." |

---

## Cross-Skill Integration

| Avant | Skill | Quand |
|-------|-------|-------|
| Écrire l'article | `/dp-blog-article` | L'article doit exister avant publication |
| Stratégie blog | `/dp-blog-strategy` | Pour savoir quel article publier ensuite |

| Après | Skill | Quand |
|-------|-------|-------|
| Promouvoir | `/dp-social-caption` | Après publication |
| Email | `/dp-email-sequence` | Pour notifier la liste |
| Maillage | `/dp-blog-article update` | Pour ajouter les liens entrants depuis d'autres articles |
