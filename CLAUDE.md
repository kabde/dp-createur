# DP Createur — Instructions Claude Code

## Projet

DP Createur est une suite de 19 skills Claude Code pour creer et lancer des produits digitaux (ebooks, guides, playbooks). Le workflow couvre : creation de contenu → export PDF → landing page → blog SEO → ads → email → media plan.

## Structure

```
skills/dp-[nom]/SKILL.md          — Definition du skill
skills/dp-[nom]/references/*.md   — Fichiers de reference (charges on-demand)
business-profile.md               — Profil business (genere par dp-business-profile)
```

## Conventions

- Tous les skills commencent par `dp-` (ex: dp-playbook-create, dp-landing-page)
- Chaque skill lit `business-profile.md` au demarrage (si le fichier existe)
- Les couleurs de marque utilisent des CSS custom properties (`--color-primary`, `--color-accent`)
- Les references sont chargees on-demand (pas au demarrage)
- Le produit fictif utilise dans les exemples est "FitPro Academy" / "Le Playbook du Coach Fitness" a 47 euros

## Regles

- Toujours tutoyer l'utilisateur
- Voix directe, actionnable, pas de fluff motivationnel
- Poser les questions par blocs de 2-3, jamais tout d'un coup
- Valider chaque etape avant de continuer
- Ne jamais inclure de placeholder dans les outputs finaux
- Les ebooks playbook doivent faire 60+ pages minimum
- Les exports PDF doivent preserver les couleurs (graphiques d'arriere-plan)
- Les publications WordPress sont toujours en DRAFT
