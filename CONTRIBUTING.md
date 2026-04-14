# Contribuer a DP Createur

Merci de vouloir contribuer. Voici les guidelines.

## Comment contribuer

### Signaler un bug

Ouvrez une issue avec :
- Le skill concerne (ex: dp-playbook-create)
- Ce que vous avez fait
- Ce qui s'est passe
- Ce que vous attendiez

### Proposer une amelioration

Ouvrez une issue avec le tag `enhancement` et decrivez :
- Le skill concerne
- Le probleme actuel
- Votre proposition

### Contribuer du code

1. Fork le repository
2. Creez une branche : `git checkout -b feature/nom-du-skill`
3. Faites vos modifications
4. Testez le skill dans Claude Code
5. Commitez : `git commit -m "Add: description"`
6. Pushez : `git push origin feature/nom-du-skill`
7. Ouvrez une Pull Request

## Structure d'un skill

Chaque skill suit cette structure :

```
skills/dp-[nom]/
├── SKILL.md              — Definition du skill (obligatoire)
└── references/           — Fichiers de reference (optionnel)
    ├── example.md
    └── templates.md
```

### Format du SKILL.md

```yaml
---
name: dp-[nom]
description: "[Description marketing avec trigger keywords]"
user-invokable: true
argument-hint: "[syntaxe de la commande]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Createur
  version: "2.0.0"
  category: [creation|marketing|content|operations|fondation]
  updated: YYYY-MM-DD
---
```

### Sections obligatoires

1. Quick Reference (tableau des commandes)
2. Output Format (ce que l'utilisateur recoit)
3. Process (etapes numerotees)
4. Context Intake (questions par blocs)
5. Quality Gates (regles avec severite)
6. Error Handling (scenarios + actions)
7. Cross-Skill Integration (avant/apres)

## Conventions

- Prefixe `dp-` pour tous les noms de skills
- Tutoiement dans tout le contenu
- Voix directe, pas de fluff
- Quality gates mesurables (chiffres, pas de "bonne qualite")
- Exemples concrets (pas de placeholders)
- Fichiers de reference < 300 lignes

## Tests

Avant de soumettre, testez votre skill :
1. Lancez `/dp-[nom]` dans Claude Code
2. Repondez aux questions
3. Verifiez que l'output est complet et sans placeholder
4. Verifiez les quality gates
5. Verifiez les liens cross-skill
