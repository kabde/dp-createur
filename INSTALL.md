# Guide d'installation — DP Createur

## Prerequis

| Outil | Version | Obligatoire | Lien |
|-------|---------|-------------|------|
| Claude Code | Derniere version | Oui | [claude.ai/code](https://claude.ai/code) |
| Git | 2.0+ | Oui | Pre-installe sur macOS/Linux |
| Node.js | 18+ | Non (pour export PDF) | [nodejs.org](https://nodejs.org) |

---

## Etape 1 — Installer Claude Code

### Option A : CLI (Terminal)

```bash
# macOS / Linux
npm install -g @anthropic-ai/claude-code

# Verifier l'installation
claude --version
```

### Option B : Application Desktop

Telecharger depuis [claude.ai/code](https://claude.ai/code) :
- **macOS** : fichier `.dmg`
- **Windows** : fichier `.exe`

### Option C : Extension VS Code / JetBrains

Chercher "Claude Code" dans le marketplace de votre IDE.

---

## Etape 2 — Cloner le repository

```bash
# Cloner DP Createur
git clone https://github.com/votre-username/dp-skills.git

# Aller dans le dossier
cd dp-skills
```

---

## Etape 3 — Lancer Claude Code dans le projet

### CLI

```bash
# Depuis le dossier dp-skills
claude
```

### Desktop / IDE

Ouvrir le dossier `dp-skills/` dans l'application ou l'IDE.

---

## Etape 4 — Verifier que les skills sont detectes

Dans Claude Code, taper :

```
/dp-business-profile
```

Si le skill se lance et pose des questions, l'installation est reussie.

### Si le skill n'est pas detecte

1. Verifier que vous etes dans le bon dossier :
   ```bash
   pwd
   # Doit afficher : .../dp-skills
   ```

2. Verifier que les skills existent :
   ```bash
   ls skills/dp-*/SKILL.md
   # Doit lister 19 fichiers
   ```

3. Relancer Claude Code depuis le dossier `dp-skills/`

---

## Etape 5 — Configurer votre profil (premiere utilisation)

```
/dp-business-profile
```

Repondez aux questions :
1. **Nom de votre business** et votre niche
2. **Votre produit principal** (nom, type, prix)
3. **Votre audience cible** (qui, probleme, resultat)
4. **Votre voix de marque** (3 mots)
5. **Vos couleurs** (primaire + accent en hex)
6. **Votre stack technique** (plateforme de vente, email, WordPress)

Un fichier `business-profile.md` est cree a la racine. Tous les autres skills le liront automatiquement.

---

## Etape 6 (optionnel) — Installer les outils d'export PDF

Si vous voulez convertir vos ebooks en PDF avec des options avancees (numerotation, couverture, headers/footers) :

### Option rapide : navigateur (aucune installation)

Ouvrir le fichier HTML dans Chrome → `Ctrl+P` → "Enregistrer au format PDF".
Cocher "Graphiques d'arriere-plan".

### Option avancee : Puppeteer

```bash
npm install -g puppeteer
```

### Option avancee : wkhtmltopdf

```bash
# macOS
brew install wkhtmltopdf

# Ubuntu / Debian
sudo apt-get install wkhtmltopdf

# Windows
# Telecharger depuis https://wkhtmltopdf.org/downloads.html
```

---

## Etape 7 (optionnel) — Configurer WordPress

Si vous voulez publier vos articles de blog directement sur WordPress :

### 1. Activer les mots de passe d'application

1. Aller dans votre WordPress → **Utilisateurs** → **Profil**
2. Scroller jusqu'a **Mots de passe d'application**
3. Entrer un nom (ex: "DP Createur")
4. Cliquer **Ajouter un nouveau mot de passe d'application**
5. **Copier le mot de passe genere** (il ne sera plus visible apres)

### 2. Verifier que l'API REST est activee

```bash
curl -s https://votre-site.com/wp-json/ | head -c 100
```

Si vous voyez du JSON, l'API est active. Si erreur 403/404 :
- Allez dans **Reglages** → **Permaliens** → cliquer **Enregistrer** (sans rien changer)
- Si un plugin de securite bloque l'API (Wordfence, iThemes), ajoutez l'API REST en whitelist

### 3. Tester la connexion

```
/dp-blog-publish test
```

---

## Commandes principales

| Commande | Description |
|----------|-------------|
| `/dp-business-profile` | Configurer votre profil business |
| `/dp-playbook-create [sujet]` | Creer un ebook/playbook |
| `/dp-export-pdf [fichier]` | Convertir en PDF |
| `/dp-landing-page` | Creer une page de vente |
| `/dp-lead-magnet-create [sujet]` | Creer un lead magnet gratuit |
| `/dp-blog-strategy [niche]` | Planifier votre strategie blog |
| `/dp-blog-article [keyword]` | Ecrire un article SEO |
| `/dp-blog-publish [fichier]` | Publier sur WordPress |
| `/dp-email-sequence` | Creer une sequence email |
| `/dp-ad-angles-meta` | Generer des ads Facebook/Instagram |
| `/dp-ad-angles-google` | Generer des ads Google/YouTube |
| `/dp-mediaplan` | Creer un calendrier editorial |
| `/dp-social-caption` | Generer des captions reseaux sociaux |
| `/dp-sales-funnel` | Architecturer un funnel de vente |
| `/dp-competitor-analysis` | Analyser un concurrent |
| `/dp-copy-review` | Auditer et ameliorer du copy |
| `/dp-playbook-audit` | Auditer la qualite d'un ebook |
| `/dp-playbook-section [section]` | Ecrire/reecrire une section |
| `/dp-playbook-sync` | Synchroniser FR ↔ EN |

---

## Workflow recommande pour un premier lancement

```
1. /dp-business-profile              → 30 min
2. /dp-playbook-create [sujet]       → 2-4 sessions de 2h
3. /dp-playbook-audit                → 15 min
4. /dp-export-pdf [fichier.html]     → 10 min
5. /dp-lead-magnet-create [sujet]    → 1h
6. /dp-landing-page                  → 45 min
7. /dp-email-sequence                → 45 min
8. /dp-blog-strategy [niche]         → 1h
9. /dp-blog-article [keyword]        → 1h par article
10. /dp-ad-angles-meta               → 1h
11. /dp-mediaplan                    → 1h
```

**Temps total estime : 12-20 heures** (reparties sur plusieurs jours)

---

## Support

- **Issues** : [github.com/votre-username/dp-skills/issues](https://github.com/votre-username/dp-skills/issues)
- **Documentation** : Ce fichier + [README.md](README.md)
