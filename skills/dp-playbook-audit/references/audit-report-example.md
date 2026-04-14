# Exemple de Rapport d'Audit — FitPro Academy

> Produit audité : **Le Playbook du Coach Fitness** (47 EUR)
> Fichier : `ebook fr/playbook-coach-fitness-FR.html`
> Date : 2026-04-13
> Auditeur : dp-playbook-audit v2.0

---

## RÉSUMÉ

- **Score Global : 72/100**
- **Statut : REVISE** (corrections nécessaires avant publication)
- Issues Critical : **3**
- Issues High : **2**
- Issues Medium : 4
- Issues Low : 2

---

## SCORECARD

| Check | Score | Détail |
|-------|-------|--------|
| 1. Intégrité Structurelle | 13/15 | `@media print` absent, viewport meta OK |
| 2. Complétude Sections | 12/15 | Section `plan-nutrition` vide (placeholder) |
| 3. Content Blocks | 10/15 | 3 sections sans recap-block, 1 sans value-block |
| 4. Navigation / Sommaire | 7/10 | Lien cassé `#periodisation` (ID = `periodisation-entrainement`) |
| 5. Cohérence FR/EN | —/15 | Non applicable (version unique FR). Points redistribués. |
| 6. Qualité Linguistique | 9/10 | Tutoiement cohérent, 1 calque anglais ("impacter" au lieu de "influencer") |
| 7. Design & Formatting | 8/10 | 2 labels h4 incorrects ("Valeur" au lieu de "Ce que tu vas apprendre") |
| 8. Annexes | 8/10 | Annexe "Modèles de programmes" référencée dans le sommaire mais incomplète |
| **TOTAL** | **72/100** | |

---

## CHECK 1 — Intégrité Structurelle (13/15)

- **PASS** : `<!DOCTYPE html>`, `<html lang="fr">`, `<meta charset="UTF-8">` presentes
- **PASS** : Structure racine `<article class="ebook">` correcte
- **PASS** : CSS custom properties dans `:root` (`--text`, `--accent`, `--bg`)
- **WARNING** : `@media print` absent -- les blocs colorés ne s'imprimeront pas correctement (ligne 42)
- **PASS** : Aucun JS externe, Google Fonts seul import externe

## CHECK 2 — Complétude Sections (12/15)

10 sections trouvées, 10 attendues.

| Section ID | h2 | h3 | Statut |
|------------|----|----|--------|
| mindset-coach | OK | 4 | PASS |
| clients-ideaux | OK | 5 | PASS |
| offre-irresistible | OK | 3 | PASS |
| plan-nutrition | OK | 0 | **FAIL** -- contenu placeholder `[TODO: rédiger]` |
| programmation | OK | 6 | PASS |
| pricing-packages | OK | 4 | PASS |
| acquisition-clients | OK | 5 | PASS |
| retention-fidelisation | OK | 3 | PASS |
| outils-gestion | OK | 4 | PASS |
| scaling-activite | OK | 5 | PASS |

## CHECK 3 — Content Blocks (10/15)

| Type | Attendu | Trouvé | Manquants |
|------|---------|--------|-----------|
| value-block | 10 | 9 | `plan-nutrition` |
| tools-block | 10 | 8 | `mindset-coach`, `retention-fidelisation` |
| recap-block | 10 | 7 | `plan-nutrition`, `offre-irresistible`, `scaling-activite` |

## CHECK 4 — Navigation / Sommaire (7/10)

- **FAIL** : Lien `href="#periodisation"` ne correspond a aucun ID. L'ID réel est `periodisation-entrainement` (ligne 87 du sommaire, ligne 312 du contenu).
- **PASS** : Les 9 autres liens du sommaire sont valides.
- **WARNING** : La section `plan-nutrition` est dans le sommaire mais son contenu est un placeholder.

## CHECK 5 — Cohérence FR/EN (N/A)

Version unique FR. Les 15 points sont redistribués : +5 a CHECK 2, +5 a CHECK 3, +5 a CHECK 6.

## CHECK 6 — Qualité Linguistique (9/10)

- **PASS** : Tutoiement cohérent dans toutes les sections
- **PASS** : Ton DP Créateur respecté (direct, actionnable, pas de fluff)
- **WARNING** : Ligne 456 -- "impacter tes résultats" est un calque anglais. Préférer "influencer" ou "avoir un effet sur".
- **PASS** : Terminologie fitness cohérente (même vocabulaire tout au long)

## CHECK 7 — Design & Formatting (8/10)

- **WARNING** : Sections `mindset-coach` et `acquisition-clients` utilisent "Valeur" comme label h4 au lieu de "Ce que tu vas apprendre" (lignes 98, 534)
- **PASS** : Classes CSS `single-item` correctement utilisées
- **PASS** : Pas de styles inline détectés
- **PASS** : Indentation cohérente

## CHECK 8 — Annexes (8/10)

- **PASS** : Annexe "Glossaire Fitness" complète avec 25 termes définis
- **WARNING** : Annexe "Modèles de programmes" contient 2 templates sur 5 attendus (ligne 1203)
- **PASS** : Les deux annexes sont référencées dans le sommaire

---

## ISSUES PAR SÉVÉRITÉ

### Critical (3) -- Bloquent la publication

| # | Issue | Localisation | Action |
|---|-------|-------------|--------|
| C1 | Section `plan-nutrition` contient `[TODO: rédiger]` -- placeholder interdit | Ligne 267 | Rédiger la section complète via `/dp-playbook-section plan-nutrition` |
| C2 | Lien sommaire cassé `#periodisation` | Ligne 87 | Corriger en `#periodisation-entrainement` |
| C3 | `@media print` absent | Bloc `<style>` | Ajouter les styles d'impression (voir references/design-system.md) |

### High (2) -- A corriger avant publication

| # | Issue | Localisation | Action |
|---|-------|-------------|--------|
| H1 | 3 sections sans recap-block | `plan-nutrition`, `offre-irresistible`, `scaling-activite` | Ajouter un recap-block avec 3 takeaways par section |
| H2 | Labels h4 incorrects dans 2 sections | Lignes 98, 534 | Remplacer "Valeur" par "Ce que tu vas apprendre" |

### Medium (4)

- 2 tools-blocks manquants (sections `mindset-coach` et `retention-fidelisation`)
- Annexe "Modèles de programmes" incomplète (2/5 templates)
- 1 calque anglais ("impacter")
- 1 value-block manquant (section `plan-nutrition` -- sera résolu avec C1)

### Low (2)

- Police Google Fonts pourrait être remplacée par une system font pour l'offline
- Le footer ne contient pas l'année de publication

---

## PLAN D'ACTION PRIORISÉ

```
PRIORITÉ 1 — Cette semaine (bloquant)
  [ ] Rédiger la section plan-nutrition
      → /dp-playbook-section plan-nutrition --target ebook\ fr/playbook-coach-fitness-FR.html
  [ ] Corriger le lien sommaire #periodisation → #periodisation-entrainement
  [ ] Ajouter @media print au CSS

PRIORITÉ 2 — Avant publication
  [ ] Ajouter les 3 recap-blocks manquants
  [ ] Corriger les labels h4 ("Valeur" → "Ce que tu vas apprendre")

PRIORITÉ 3 — Amélioration recommandée
  [ ] Compléter l'annexe "Modèles de programmes" (3 templates manquants)
  [ ] Ajouter les tools-blocks aux sections mindset-coach et retention-fidelisation
  [ ] Remplacer "impacter" par "influencer" (ligne 456)

PRIORITÉ 4 — Nice to have
  [ ] Ajouter l'année au footer
  [ ] Considérer system fonts pour compatibilité offline
```

---

## PROCHAINES ÉTAPES

```
→ /dp-playbook-section plan-nutrition    Rédiger la section manquante
→ /dp-playbook-sync                      Synchroniser FR/EN après corrections
→ /dp-copy-review                        Revoir le copywriting global
→ /dp-export-pdf                         Exporter en PDF une fois score >= 90
```
