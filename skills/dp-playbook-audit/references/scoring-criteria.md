# Scoring Criteria — Playbook Audit

## Score Breakdown (100 points total)

| Check | Category | Max Points | Weight |
|-------|----------|-----------|--------|
| CHECK 1 | Intégrité Structurelle | 15 | Structure HTML, CSS, meta tags |
| CHECK 2 | Complétude des Sections | 15 | Sections présentes et non vides |
| CHECK 3 | Content Blocks | 15 | value-block, tools-block, recap-block |
| CHECK 4 | Navigation / Sommaire | 10 | Liens fonctionnels, couverture |
| CHECK 5 | Cohérence FR/EN | 15 | Comptages identiques, traduction complète |
| CHECK 6 | Qualité Linguistique | 10 | Ton, grammaire, cohérence terminologique |
| CHECK 7 | Design & Formatting | 10 | Labels, HTML propre, styles |
| CHECK 8 | Annexes | 10 | Contenu substantiel, IDs, sommaire |

## Pénalités par type d'issue

### Critical (-3 points chacun)
- Section manquante (ID absent)
- Lien de sommaire cassé
- Texte non traduit (page entière de FR dans EN)
- Placeholder ([TODO], [INSERT], Lorem ipsum)
- Promesse de revenus garantis
- HTML cassé (tags non fermés)
- Pas de DOCTYPE ou lang attribute

### High (-2 points chacun)
- Bloc value ou recap manquant dans une section
- Section vide (ID présent mais pas de contenu)
- Écart de comptage h2 ou h3 entre FR et EN
- Problème de ton (trop formel, trop hype)
- Styles inline
- Label h4 incorrect

### Medium (-1 point chacun)
- Tools-block manquant (optionnel mais recommandé)
- Recap-block avec < 3 takeaways
- Erreur de grammaire/orthographe
- Incohérence terminologique mineure
- Section non listée dans le sommaire
- Indentation incohérente

### Low (-0.5 point chacun)
- Suggestion d'amélioration stylistique
- Optimisation de structure possible
- Outil non référencé qui pourrait l'être

## Redistribution sans CHECK 5

Si pas de comparaison FR/EN (un seul fichier audité), les 15 points du CHECK 5 sont redistribués :
- +5 à CHECK 2 (Complétude → 20 points)
- +5 à CHECK 3 (Content Blocks → 20 points)
- +5 à CHECK 6 (Qualité Linguistique → 15 points)

## Seuils de décision

| Score | Statut | Action |
|-------|--------|--------|
| 90-100 | PUBLISH | Prêt à publier. Corrections mineures optionnelles. |
| 80-89 | REVISE | Bon mais corrections nécessaires. 1-2h de travail. |
| 70-79 | REVISE | Correct mais plusieurs problèmes. Demi-journée de travail. |
| 60-69 | REWRITE | Problèmes significatifs. Réécriture partielle recommandée. |
| <60 | REWRITE | Problèmes majeurs. Réécriture complète recommandée. |
