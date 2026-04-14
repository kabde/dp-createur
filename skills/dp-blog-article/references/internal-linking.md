# Maillage Interne — Stratégie de Liens

## Principes

Le maillage interne est le système nerveux du blog. Il permet :
1. **SEO** : distribuer l'autorité (link juice) entre les pages
2. **UX** : guider le lecteur vers du contenu complémentaire
3. **Crawl** : aider Google à découvrir et comprendre la structure
4. **Conversion** : créer un parcours lecteur → lead → client

## Stratégie Hub & Spoke

```
                    ┌──────────┐
              ┌────→│ Sat. 1   │←───┐
              │     └──────────┘    │
              │                     │
┌──────────┐  │  ┌──────────────┐   │  ┌──────────┐
│ Sat. 4   │←─┼─→│   PILIER     │←──┼─→│ Sat. 2   │
└──────────┘  │  └──────────────┘   │  └──────────┘
              │                     │
              │     ┌──────────┐    │
              └────→│ Sat. 3   │←───┘
                    └──────────┘
```

- Le **pilier** linke vers chaque satellite
- Chaque **satellite** linke vers le pilier
- Les satellites se linkent entre eux si pertinent
- Les piliers de clusters différents se linkent si thèmes liés

## Implémentation dans l'article

### Où placer les liens

```
ARTICLE
├── Introduction (mots 1-200)
│   └── 1 lien vers le pilier ou un article fondamental
│       Naturel : "Si tu débutes, commence par [notre guide sur X]."
│
├── Section 1 (H2)
│   └── 1 lien vers un satellite complémentaire
│       Naturel : "On détaille cette méthode dans [article spécifique]."
│
├── Section 2-3 (H2)
│   └── 1-2 liens vers des articles du même cluster
│       Naturel : "Comme on l'a vu dans [article], [concept] est clé."
│
├── Section FAQ
│   └── 1 lien dans une réponse FAQ
│       Naturel : "Pour aller plus loin sur ce point, lis [article]."
│
└── Conclusion
    └── 1-2 liens vers le pilier + un article "prochaine étape"
        Naturel : "Prochaine étape : [article sur le sujet suivant]."
```

### Patterns d'ancrage (exemples)

| Pattern | Exemple | Quand l'utiliser |
|---------|---------|-----------------|
| Référence directe | "notre guide sur [sujet]" | Lien vers un pilier |
| Approfondissement | "on détaille [sujet] ici" | Lien vers un satellite |
| Contexte | "comme on l'a vu dans [article]" | Rappel d'un article précédent |
| Prochaine étape | "la suite logique : [sujet]" | En conclusion |
| Ressource | "utilise notre [template/checklist/outil]" | Lien vers un lead magnet |

### Ce qu'il faut éviter

| Interdit | Pourquoi |
|----------|----------|
| "Cliquez ici" | Ancre vide, pas de valeur SEO |
| 10+ liens dans 500 mots | Spam, dilue la valeur |
| Même ancre pour 3 liens | Suspicion de sur-optimisation |
| Liens dans un bloc "À lire aussi" en bas | Faible CTR, valeur SEO limitée |
| Liens vers des pages 404 | Expérience négative + signal négatif |
| Liens non pertinents | Confuse Google sur la relation entre pages |

## Vérification du maillage

### Avant publication, vérifier :

```
[ ] L'article a minimum [N] liens internes (selon longueur)
[ ] Au moins 1 lien dans les 200 premiers mots
[ ] Toutes les ancres sont descriptives et différentes
[ ] Les liens pointent vers des articles existants et actifs
[ ] L'article est lui-même lié DEPUIS au moins 1 autre article
[ ] Le lien vers le pilier du cluster est présent
[ ] Pas de liens orphelins (pages 404)
```

### Après publication, mettre à jour :

```
[ ] Ajouter un lien VERS ce nouvel article depuis 2-3 articles existants
[ ] Mettre à jour le pilier du cluster pour inclure ce satellite
[ ] Vérifier que la carte de maillage dans la stratégie est à jour
```

## Schéma JSON-LD pour le maillage

Si le site utilise des données structurées, l'article peut inclure :

```json
{
  "@context": "https://schema.org",
  "@type": "BlogPosting",
  "headline": "[Titre]",
  "author": { "@type": "Person", "name": "[Auteur]" },
  "datePublished": "[Date]",
  "dateModified": "[Date]",
  "description": "[Meta description]",
  "mainEntityOfPage": "[URL]",
  "isPartOf": {
    "@type": "Blog",
    "name": "[Nom du blog]",
    "url": "[URL du blog]"
  }
}
```
