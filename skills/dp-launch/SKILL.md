---
name: dp-launch
description: "Orchestrateur de lancement de produit digital. Guide l'utilisateur à travers les 22 skills DP Créateur dans le bon ordre, vérifie ce qui est déjà fait, propose la prochaine étape, et suit la progression. C'est le point d'entrée principal — lance /dp-launch et suis le guide. Triggers : lancer, launch, démarrer, commencer, start, nouveau projet, c'est quoi la suite, next step, progression."
user-invokable: true
argument-hint: "[status] [--reset] [--skip étape]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: fondation
  updated: 2026-04-19
---

# Launch — Orchestrateur de Lancement

<!-- v2.0.0 | 2026-04-19 | Création : orchestrateur 12 étapes, détection automatique, progression, recommendations -->

Le point d'entrée unique de DP Créateur. Au lieu de deviner quel skill lancer et dans quel ordre, tape `/dp-launch` et laisse-toi guider.

Ce skill **ne crée rien lui-même** — il analyse où tu en es, te dit quoi faire ensuite, et te redirige vers le bon skill.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-launch` | Afficher ta progression et la prochaine étape |
| `/dp-launch status` | Résumé complet de toutes les étapes |
| `/dp-launch start` | Démarrer un nouveau projet depuis zéro |
| `/dp-launch next` | Lancer directement la prochaine étape |
| `/dp-launch --skip [étape]` | Marquer une étape comme non applicable |
| `/dp-launch --reset` | Remettre la progression à zéro |

## Output Format

```
LIVRABLE :
├── Dashboard de progression (12 étapes)
├── Détection automatique de ce qui est fait
├── Prochaine étape recommandée avec commande exacte
├── Estimation du temps restant
└── Aucun fichier créé — redirige vers les skills
```

---

## Process

```
1. Scanner le projet     → Détecter les fichiers existants
2. Évaluer la progression → Checker chaque étape
3. Afficher le dashboard  → Progression visuelle
4. Recommander            → Prochaine étape avec commande
```

---

## Les 12 Étapes du Lancement

### Carte complète

```
PHASE 1 — FONDATION
  ├── Étape 1  : Business Profile         /dp-business-profile
  └── Étape 2  : Validation Marché        /dp-market-research

PHASE 2 — CRÉATION
  ├── Étape 3  : Créer l'Ebook            /dp-playbook-create
  ├── Étape 4  : Auditer la Qualité       /dp-playbook-audit
  ├── Étape 5  : Couverture               /dp-ebook-cover
  └── Étape 6  : Export PDF               /dp-export-pdf

PHASE 3 — MISE EN VENTE
  ├── Étape 7  : Landing Page             /dp-landing-page
  ├── Étape 8  : Séquence Email           /dp-email-sequence
  └── Étape 9  : Stratégie Upsell        /dp-upsell-strategy

PHASE 4 — PROMOTION
  ├── Étape 10 : Stratégie Blog           /dp-blog-strategy
  ├── Étape 11 : Ads (Meta + Google)      /dp-ad-angles-meta + google
  └── Étape 12 : Plan Média              /dp-mediaplan
```

---

## Step 1 — Scanner le Projet

### Détection automatique

Pour chaque étape, vérifier si les fichiers correspondants existent :

```bash
# Étape 1 — Business Profile
ls business-profile.md 2>/dev/null && echo "DONE" || echo "TODO"

# Étape 2 — Market Research
ls market-research/validation-*.md 2>/dev/null && echo "DONE" || echo "TODO"

# Étape 3 — Ebook
ls "ebook fr"/*.html "ebook en"/*.html 2>/dev/null && echo "DONE" || echo "TODO"

# Étape 4 — Audit
# Vérifier si un audit a été fait (chercher des rapports ou un score dans les fichiers)
ls market-research/audit-*.md 2>/dev/null && echo "DONE" || echo "TODO"

# Étape 5 — Cover
ls covers/*-cover.html covers/*-mockup.html 2>/dev/null && echo "DONE" || echo "TODO"

# Étape 6 — PDF
ls exports/*.pdf 2>/dev/null && echo "DONE" || echo "TODO"

# Étape 7 — Landing Page
ls landing-pages/*.html 2>/dev/null && echo "DONE" || echo "TODO"

# Étape 8 — Email Sequence
ls emails/*.html emails/*-sequence.* 2>/dev/null && echo "DONE" || echo "TODO"

# Étape 9 — Upsell Strategy
ls upsell-strategy/strategy-*.md 2>/dev/null && echo "DONE" || echo "TODO"

# Étape 10 — Blog Strategy
ls blog-strategy/strategy-*.md 2>/dev/null && echo "DONE" || echo "TODO"

# Étape 11 — Ads
ls ads/*.html ads/*.md 2>/dev/null && echo "DONE" || echo "TODO"

# Étape 12 — Mediaplan
ls mediaplan/*.html mediaplan/*.md 2>/dev/null && echo "DONE" || echo "TODO"
```

### Règles de détection

| Étape | Fichier(s) cherché(s) | Statut |
|-------|----------------------|--------|
| 1. Business Profile | `business-profile.md` | ✅ si existe |
| 2. Market Research | `market-research/validation-*.md` | ✅ si existe |
| 3. Ebook | `ebook fr/*.html` ou `ebook en/*.html` | ✅ si au moins 1 fichier |
| 4. Audit | Vérifié manuellement (demander à l'utilisateur) | ✅ si confirmé |
| 5. Cover | `covers/*-cover.html` ou `covers/*-mockup.html` | ✅ si existe |
| 6. PDF | `exports/*.pdf` | ✅ si existe |
| 7. Landing Page | `landing-pages/*.html` | ✅ si existe |
| 8. Email Sequence | `emails/*` | ✅ si existe |
| 9. Upsell | `upsell-strategy/*.md` | ✅ si existe |
| 10. Blog Strategy | `blog-strategy/*.md` | ✅ si existe |
| 11. Ads | `ads/*` | ✅ si existe |
| 12. Mediaplan | `mediaplan/*` | ✅ si existe |

---

## Step 2 — Afficher le Dashboard

```
╔══════════════════════════════════════════════════════════╗
║              DP CRÉATEUR — PROGRESSION                   ║
║              Projet : [nom depuis business-profile]      ║
╠══════════════════════════════════════════════════════════╣
║                                                          ║
║  PHASE 1 — FONDATION                                    ║
║  [✅] 1. Business Profile       /dp-business-profile     ║
║  [✅] 2. Validation Marché      /dp-market-research      ║
║                                                          ║
║  PHASE 2 — CRÉATION                                     ║
║  [✅] 3. Ebook (60+ pages)      /dp-playbook-create      ║
║  [⬜] 4. Audit Qualité          /dp-playbook-audit       ║ ← TU ES ICI
║  [⬜] 5. Couverture             /dp-ebook-cover          ║
║  [⬜] 6. Export PDF             /dp-export-pdf           ║
║                                                          ║
║  PHASE 3 — MISE EN VENTE                                ║
║  [⬜] 7. Landing Page           /dp-landing-page         ║
║  [⬜] 8. Séquence Email         /dp-email-sequence       ║
║  [⬜] 9. Stratégie Upsell      /dp-upsell-strategy      ║
║                                                          ║
║  PHASE 4 — PROMOTION                                    ║
║  [⬜] 10. Stratégie Blog        /dp-blog-strategy        ║
║  [⬜] 11. Ads Meta + Google     /dp-ad-angles-meta       ║
║  [⬜] 12. Plan Média            /dp-mediaplan            ║
║                                                          ║
╠══════════════════════════════════════════════════════════╣
║  Progression : ████████░░░░░░░░░░░░░░░░ 3/12 (25%)      ║
║  Temps estimé restant : ~10h                             ║
╚══════════════════════════════════════════════════════════╝
```

### Icônes de statut

| Icône | Signification |
|-------|--------------|
| ✅ | Terminé — fichier(s) détecté(s) |
| ⬜ | À faire |
| ⏭️ | Sauté (non applicable) |
| 🔄 | En cours (fichier partiel détecté) |
| ← TU ES ICI | Prochaine étape recommandée |

---

## Step 3 — Recommandation

### Logique de la prochaine étape

```
POUR CHAQUE étape de 1 à 12 :
  SI étape.statut == "TODO" :
    → C'est la prochaine étape
    → Afficher la recommandation
    → STOP (ne pas chercher plus loin)
```

### Format de la recommandation

```
╔══════════════════════════════════════════════════════════╗
║  PROCHAINE ÉTAPE : [N]. [Nom de l'étape]                ║
╠══════════════════════════════════════════════════════════╣
║                                                          ║
║  Commande : /dp-[skill-name]                             ║
║                                                          ║
║  Ce que ça fait :                                        ║
║  [Description en 2-3 lignes de ce que le skill produit]  ║
║                                                          ║
║  Temps estimé : [Xh]                                     ║
║                                                          ║
║  Pré-requis :                                            ║
║  [✅] [Étape précédente terminée]                        ║
║  [✅/⬜] [Autre pré-requis si applicable]                ║
║                                                          ║
║  Tape /dp-[skill-name] pour commencer.                   ║
╚══════════════════════════════════════════════════════════╝
```

### Détails par étape

| Étape | Skill | Temps estimé | Pré-requis | Ce que ça produit |
|-------|-------|-------------|------------|-------------------|
| 1 | `/dp-business-profile` | 30 min | Aucun | `business-profile.md` — couleurs, voix, audience |
| 2 | `/dp-market-research` | 45 min | Étape 1 | Rapport de validation avec score et verdict GO/TEST/STOP |
| 3 | `/dp-playbook-create` | 8-15h | Étapes 1+2 | Ebook HTML 60+ pages aux couleurs de ta marque |
| 4 | `/dp-playbook-audit` | 15 min | Étape 3 | Score 0-100 avec issues priorisées |
| 5 | `/dp-ebook-cover` | 30 min | Étape 3 | Couverture HTML + mockup 3D + prompts IA |
| 6 | `/dp-export-pdf` | 10 min | Étapes 3+4 | PDF professionnel prêt à vendre |
| 7 | `/dp-landing-page` | 45 min | Étape 6 | Landing page + thank you page + modales légales |
| 8 | `/dp-email-sequence` | 45 min | Étape 7 | 7 emails de lancement (J-7 → J+7) |
| 9 | `/dp-upsell-strategy` | 1h | Étapes 7+8 | Échelle de valeur + order bump + upsell + calcul LTV |
| 10 | `/dp-blog-strategy` | 1h | Étape 1 | 20 articles en topic clusters + calendrier |
| 11 | `/dp-ad-angles-meta` | 1h | Étape 7 | Angles publicitaires Meta + Google avec copies |
| 12 | `/dp-mediaplan` | 1h | Étape 10 | Calendrier éditorial 4 semaines |

### Temps total estimé

```
ESTIMATION TOTALE
═════════════════

Phase 1 (Fondation)   : 1h15
Phase 2 (Création)    : 9-16h  ← Le gros du travail (rédaction ebook)
Phase 3 (Mise en vente): 2h30
Phase 4 (Promotion)    : 3h
────────────────────────────
TOTAL                  : 16-23h (réparties sur 2-4 semaines)
```

---

## Step 4 — Gestion de la Progression

### Marquer une étape comme terminée

Si la détection automatique ne suffit pas (ex: l'audit a été fait oralement), l'utilisateur peut confirmer :

"L'étape 4 (Audit) est terminée ? Si oui, je la marque comme faite."

### Sauter une étape

```
/dp-launch --skip 5
→ "Étape 5 (Couverture) marquée comme non applicable. ⏭️"
```

Étapes qu'on peut sauter :
- Étape 2 (Market Research) — si l'utilisateur est sûr de son idée
- Étape 5 (Cover) — si l'utilisateur a déjà une couverture
- Étape 9 (Upsell) — si pas prêt pour ça maintenant
- Étape 10 (Blog) — si pas de stratégie SEO prévue
- Étape 12 (Mediaplan) — si pas de réseaux sociaux

Étapes qu'on ne peut PAS sauter :
- Étape 1 (Business Profile) — tout le système en dépend
- Étape 3 (Ebook) — c'est le produit
- Étape 6 (PDF) — c'est le livrable
- Étape 7 (Landing Page) — c'est la page de vente

### Remettre à zéro

```
/dp-launch --reset
→ "⚠️ Ça va remettre la progression à zéro. Les fichiers ne seront PAS supprimés.
   Tu veux continuer ? (oui/non)"
```

---

## Scénarios spéciaux

### L'utilisateur arrive avec un projet déjà avancé

Si des fichiers existent déjà (ebook créé, landing page faite), le scanner les détecte automatiquement et ne repropose pas ces étapes.

"Je vois que tu as déjà un ebook (`ebook fr/playbook-FR.html`) et une landing page (`landing-pages/mon-produit.html`). Les étapes 3 et 7 sont déjà faites. Prochaine étape recommandée : Étape 4 — Audit."

### L'utilisateur veut faire les choses dans le désordre

C'est possible mais le skill prévient :

"Tu veux lancer les ads (étape 11) avant d'avoir la landing page (étape 7). C'est techniquement possible, mais tes ads pointeront vers... rien. Je recommande de faire l'étape 7 d'abord."

### L'utilisateur a terminé toutes les étapes

```
╔══════════════════════════════════════════════════════════╗
║                                                          ║
║  🎉 TOUTES LES ÉTAPES SONT TERMINÉES !                  ║
║                                                          ║
║  Ton produit est live. Voici ce que tu as :              ║
║                                                          ║
║  📄 Ebook    : [chemin]                                  ║
║  📦 PDF      : [chemin]                                  ║
║  🎨 Cover    : [chemin]                                  ║
║  🌐 Landing  : [chemin]                                  ║
║  📧 Emails   : [chemin]                                  ║
║  📊 Ads      : [chemin]                                  ║
║  📅 Mediaplan: [chemin]                                  ║
║                                                          ║
║  PROCHAINES ACTIONS :                                    ║
║  → Publier la landing page en ligne                      ║
║  → Configurer les emails dans ConvertKit/Mailchimp       ║
║  → Lancer les premières ads avec les angles générés      ║
║  → Poster le premier contenu du mediaplan                ║
║  → Écrire le premier article du blog strategy            ║
║                                                          ║
║  POUR ALLER PLUS LOIN :                                  ║
║  → /dp-blog-article [keyword]  Écrire un article SEO    ║
║  → /dp-blog-publish            Publier sur WordPress     ║
║  → /dp-competitor-analysis     Analyser un concurrent    ║
║  → /dp-copy-review             Améliorer le copy         ║
║  → /dp-playbook-sync           Traduire FR ↔ EN          ║
║                                                          ║
╚══════════════════════════════════════════════════════════╝
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Ne JAMAIS créer de fichier — seulement scanner et recommander | Critical |
| QG-02 | La détection se base sur les FICHIERS, pas sur la mémoire | Critical |
| QG-03 | Toujours afficher la commande exacte à taper | High |
| QG-04 | Ne pas recommander une étape si ses pré-requis ne sont pas remplis | High |
| QG-05 | Être honnête sur le temps estimé (pas "5 min" pour un ebook de 60 pages) | High |
| QG-06 | Permettre de sauter des étapes non critiques sans jugement | Medium |
| QG-07 | Si toutes les étapes sont faites, féliciter ET donner les prochaines actions concrètes | Medium |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Aucun fichier dans le projet | "C'est un nouveau projet ! Commençons par l'étape 1 : `/dp-business-profile`" |
| business-profile.md manquant mais ebook existe | "Je vois un ebook mais pas de profil business. Je recommande de créer le profil d'abord — ça améliorera tous les autres outputs." |
| L'utilisateur ne sait pas par où commencer | Afficher le dashboard et pointer vers l'étape 1 |
| L'utilisateur veut tout faire en 1 jour | "Le lancement complet prend 16-23h réparties sur 2-4 semaines. Aujourd'hui, concentre-toi sur [étape suivante]." |
| Fichier détecté mais potentiellement incomplet | Demander confirmation : "J'ai trouvé [fichier]. C'est terminé ou encore en cours ?" |

---

## Cross-Skill Integration

Ce skill est l'ORCHESTRATEUR — il référence tous les autres :

| Skill | Rôle dans le lancement |
|-------|----------------------|
| `/dp-business-profile` | Étape 1 — Fondation |
| `/dp-market-research` | Étape 2 — Validation |
| `/dp-playbook-create` | Étape 3 — Création |
| `/dp-playbook-audit` | Étape 4 — Qualité |
| `/dp-ebook-cover` | Étape 5 — Visuel |
| `/dp-export-pdf` | Étape 6 — Production |
| `/dp-landing-page` | Étape 7 — Vente |
| `/dp-email-sequence` | Étape 8 — Email |
| `/dp-upsell-strategy` | Étape 9 — Monétisation |
| `/dp-blog-strategy` | Étape 10 — Contenu |
| `/dp-ad-angles-meta` + `google` | Étape 11 — Acquisition |
| `/dp-mediaplan` | Étape 12 — Social |
