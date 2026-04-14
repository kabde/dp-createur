# Error Handling — Playbook Create

## Scénarios et actions

| Scénario | Détection | Action |
|----------|-----------|--------|
| Utilisateur n'a pas d'idée de sujet | Réponse vague ou "je ne sais pas" | Lancer les questions de découverte d'idée (voir SKILL.md Phase 1 — cas particuliers) |
| Sujet trop large | Ex: "le marketing" | Proposer 3-5 sous-niches spécifiques. Demander de choisir. |
| Sujet trop niche | Ex: "email marketing pour dentistes pédiatriques en zone rurale" | Valider que l'audience est suffisante. Suggérer un élargissement si nécessaire. |
| Expertise insuffisante | Utilisateur dit "je débute sur ce sujet" | Proposer un format `guide` ou `checklist` plutôt que `playbook`. Suggérer de curer du contenu existant. |
| Pas de différenciation | "Je ne sais pas ce qui me rend différent" | Poser des questions sur : parcours, résultats obtenus, méthode utilisée, erreurs évitées. Extraire l'angle unique. |
| Changement de direction en cours de rédaction | Utilisateur veut modifier le sujet/angle après Phase 2 | Mettre à jour la fiche produit. Identifier les sections impactées. Réécrire seulement ce qui change. |
| Fichier HTML existant à écraser | Un fichier avec le même slug existe déjà | Demander confirmation avant d'écraser. Proposer un suffixe (-v2, -updated). |
| Contenu existant fourni par l'utilisateur | Utilisateur colle du texte ou pointe vers un fichier | Analyser le contenu. Identifier forces/faiblesses. Intégrer dans la structure DP Créateur plutôt que repartir de zéro. |
| Langue mixte | Utilisateur parle français mais veut un ebook en anglais | Rédiger dans la langue cible. Poser les questions dans la langue de l'utilisateur. |
| Demande de contenu illégal/trompeur | Promesses de revenus garantis, claims médicaux, etc. | Refuser poliment. Expliquer les risques légaux. Proposer une formulation conforme (quality gate QG-02). |
| Timeout / conversation longue | Ebook en cours depuis >10 échanges | Proposer de sauvegarder l'état actuel et de continuer plus tard. Écrire les sections terminées dans le fichier. |
| business-profile.md introuvable | Le fichier n'existe pas à la racine | Continuer sans. Poser les questions de contexte business directement à l'utilisateur. |

## Dégradation gracieuse

| Ressource manquante | Fallback |
|---------------------|----------|
| `business-profile.md` | Poser les questions de contexte directement |
| Playbook de référence (ebook en/ ou ebook fr/) | Utiliser le design system embarqué dans `references/design-system.md` |
| Dossier de sortie inexistant (ebook en/, ebook fr/) | Créer le dossier automatiquement avant d'écrire |
| Titre non défini | Proposer 5 options avant de continuer |
| Langue non précisée | Détecter la langue de conversation. Confirmer avec l'utilisateur. |
