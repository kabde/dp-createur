# DP Créateur — Analyse & Plan d'Amélioration v2

**Date** : 13 avril 2026  
**Version** : 2.0 — Post-audit complet des 18 skills

---

## OBJECTIF

Permettre à un créateur de produit digital de :

1. **Analyser une idée** de produit digital (ebook, guide, playbook)
2. **Générer la structure** et **créer un ebook de 60+ pages** aux couleurs de l'utilisateur
3. **Créer une landing page** pour vendre le produit
4. **Générer du contenu blog** autour du produit (stratégie, articles SEO, maillage, publication WordPress)
5. **Générer des ads** avec des angles marketing (Meta + Google)
6. **Créer un plan média** complet pour la promotion

**Workflow cible :**
```
Idée → Validation → Création ebook (60+ pages) → Export PDF → Landing page
  → Blog (stratégie + articles + publication WP) → Ads (Meta + Google) → Media plan
```

---

## INVENTAIRE — 18 Skills

| # | Skill | Catégorie | Lignes | Rôle dans le workflow |
|---|-------|-----------|--------|----------------------|
| 1 | `dp-business-profile` | Fondation | 384 | Config centrale (couleurs, marque, audience) |
| 2 | `dp-playbook-create` | Création | 508 | Créer l'ebook (guidé, 60+ pages) |
| 3 | `dp-playbook-section` | Création | 344 | Écrire/réécrire une section |
| 4 | `dp-playbook-audit` | Création | 351 | Auditer la qualité avant publication |
| 5 | `dp-playbook-sync` | Création | 349 | Synchroniser EN ↔ FR |
| 6 | `dp-export-pdf` | Production | 500 | Convertir HTML → PDF vendable |
| 7 | `dp-landing-page` | Vente | 379 | Page de vente responsive |
| 8 | `dp-sales-funnel` | Vente | 358 | Architecture de funnel complète |
| 9 | `dp-blog-strategy` | Contenu | 386 | Stratégie de contenu, topic clusters |
| 10 | `dp-blog-article` | Contenu | 530 | Rédaction article SEO + maillage |
| 11 | `dp-blog-publish` | Contenu | 351 | Publication WordPress |
| 12 | `dp-email-sequence` | Contenu | 327 | Séquences email marketing |
| 13 | `dp-social-caption` | Contenu | 328 | Captions réseaux sociaux |
| 14 | `dp-mediaplan` | Promotion | 486 | Calendrier éditorial 4 semaines |
| 15 | `dp-ad-angles-meta` | Promotion | 326 | Campagnes Facebook/Instagram |
| 16 | `dp-ad-angles-google` | Promotion | 408 | Campagnes Google/YouTube |
| 17 | `dp-competitor-analysis` | Analyse | 363 | Intelligence concurrentielle |
| 18 | `dp-copy-review` | Qualité | 450 | Revue et optimisation de copy |

**Total : 18 skills — 7128 lignes**

---

## AUDIT DÉTAILLÉ PAR SKILL

### 1. `dp-business-profile` — Score : 9/10

**Ce qui fonctionne :**
- 6 blocs de questions complets (identité, produits, audience, voix, visuel, stack)
- 3 palettes de couleurs proposées si l'utilisateur n'en a pas
- Modes update/check/show bien séparés

**Améliorations nécessaires :**
- [ ] Ajouter Q sur le budget marketing mensuel (utilisé par ads et sales-funnel)
- [ ] Ajouter un mode `express` (5 questions essentielles)
- [ ] Générer les CSS variables automatiquement (accent-light, accent-dark, accent-mid)

---

### 2. `dp-playbook-create` — Score : 7/10

**Ce qui fonctionne :**
- Context intake en 5 blocs avec validation progressive
- 6 types de produit supportés
- Framework QUOI/POURQUOI/COMMENT/MESURE par section

**Améliorations nécessaires :**
- [ ] **CRITIQUE** : Ajouter une exigence de **60 pages minimum** pour le type `playbook` — actuellement aucune contrainte de pages, seulement "5000+ mots" ce qui fait ~15 pages
- [ ] **CRITIQUE** : Le mode express saute les questions de couleurs (Q12-Q14) → l'ebook n'aura pas d'identité visuelle
- [ ] Définir les minimums de mots PAR SECTION (pas globalement) : playbook 500 mots/section, guide 300, lead-magnet 150
- [ ] Ajouter un compteur de pages estimé dans le plan (1 page ≈ 350 mots)
- [ ] Ajouter des guidelines pour atteindre 60 pages : plus de sous-sections, exercices, templates, études de cas, checklists
- [ ] Le CSS design system doit utiliser les couleurs du business-profile.md ET celles choisies dans le context intake
- [ ] Ajouter un Step intermédiaire "Enrichissement" : après le plan, proposer d'ajouter des exercices, templates, études de cas pour étoffer

---

### 3. `dp-playbook-section` — Score : 8/10

**Ce qui fonctionne :**
- Focalisé sur UNE section (bonne séparation)
- Modes add/rewrite clairs
- Pattern value-block/tools-block/recap-block enforced

**Améliorations nécessaires :**
- [ ] Ajouter un guidage de longueur : "Pour un playbook de 60 pages, chaque section principale devrait faire 1500-2500 mots (~4-7 pages)"
- [ ] Ajouter la possibilité d'insérer des exercices pratiques et templates dans une section

---

### 4. `dp-playbook-audit` — Score : 8.5/10

**Ce qui fonctionne :**
- 8 checks structurés avec commandes bash
- QG-05 "Ne rien corriger" (audit-only, bonne séparation)
- Score 0-100 transparent

**Améliorations nécessaires :**
- [ ] Ajouter un check "Nombre de pages estimé" avec seuil minimum (60 pages pour playbook)
- [ ] Définir les seuils de publication : < 70 = bloquer, 70-89 = publier avec réserves, 90+ = prêt

---

### 5. `dp-playbook-sync` — Score : 8/10

**Ce qui fonctionne :**
- FR = source de vérité (règle claire)
- Table de traduction avec termes à préserver
- Vérification post-sync

**Améliorations nécessaires :**
- [ ] Ajouter une étape de relecture humaine recommandée (flag, pas bloquer)

---

### 6. `dp-export-pdf` — Score : 7/10

**Ce qui fonctionne :**
- 5 outils supportés avec fallback
- CSS print optimisé
- Page de couverture avec couleurs de marque

**Améliorations nécessaires :**
- [ ] **CRITIQUE** : Ne charge PAS business-profile.md → les couleurs de couverture sont demandées mais pas pré-remplies
- [ ] Ajouter un check de nombre de pages post-conversion (vérifier ≥ 60 pour les playbooks)
- [ ] Ajouter la génération automatique d'un script Node.js de conversion (pas juste montrer le code)
- [ ] Ajouter le support des images de couverture depuis un fichier local

---

### 7. `dp-landing-page` — Score : 9/10

**Ce qui fonctionne :**
- CSS custom properties (--primary, --accent) 
- 4 styles visuels (minimaliste, bold, premium, warm)
- 9 sections structurées (Hero → Footer)
- Responsive + SEO meta tags

**Améliorations nécessaires :**
- [ ] Ajouter un mode "lead magnet" (page de capture email, pas de prix)
- [ ] Ajouter un mockup 3D de l'ebook dans la section Hero (brief pour Canva/IA)
- [ ] Ajouter un countdown timer pour les lancements (CSS-only)

---

### 8. `dp-sales-funnel` — Score : 8/10

**Ce qui fonctionne :**
- 6 stages bien définis
- Conversion math avec ROI
- 8 automations documentées

**Améliorations nécessaires :**
- [ ] Définir "budget adapté" avec des seuils concrets (< 500€/mois = organique, 500-2000€ = mixte, > 2000€ = paid-first)
- [ ] Ajouter un exemple de funnel concret pour "ebook à 27€" (le use case principal)

---

### 9. `dp-blog-strategy` — Score : 8.5/10

**Ce qui fonctionne :**
- Topic clusters hub & spoke
- Matrice de priorisation impact × effort
- Règles de maillage (pas d'orphelins)

**Améliorations nécessaires :**
- [ ] Définir les ratios d'intent cibles : TOFU 40% / MOFU 35% / BOFU 25%
- [ ] Ajouter un mode "audit articles existants" plus détaillé

---

### 10. `dp-blog-article` — Score : 9/10

**Ce qui fonctionne :**
- E-E-A-T complet
- Maillage interne avec ancres variées
- Schema JSON-LD BlogPosting + FAQPage
- Publication WordPress intégrée
- Score SEO 0-100

**Améliorations nécessaires :**
- [ ] Harmoniser la version à 2.0.0 (actuellement 3.0.0, incohérent)
- [ ] Définir le minimum de liens internes dans le skill (pas seulement dans la référence) : "3-5 pour 1500 mots, 5-8 pour 2000+"

---

### 11. `dp-blog-publish` — Score : 8.5/10

**Ce qui fonctionne :**
- Sécurité exemplaire (draft only, pas de stockage mdp)
- Diagnostic d'erreurs API clair
- Détection Yoast/RankMath

**Améliorations nécessaires :**
- [ ] Ajouter la gestion des images (upload media + featured image)
- [ ] Ajouter le scheduling (planifier la publication à une date/heure)

---

### 12. `dp-email-sequence` — Score : 8/10

**Ce qui fonctionne :**
- 6 types de séquences
- Subject lines patterns
- CTA placement rules

**Améliorations nécessaires :**
- [ ] Ajouter le dark mode pour les emails HTML
- [ ] Ajouter un type "launch" spécifique pour un lancement d'ebook (7 emails : teaser → reveal → early bird → proof → urgency → last call → post-launch)

---

### 13. `dp-social-caption` — Score : 8/10

**Ce qui fonctionne :**
- Specs par plateforme (chars, hashtags, hooks)
- Batch avec variété de funnel stages
- Direction visuelle avec couleurs de marque

**Améliorations nécessaires :**
- [ ] Ajouter des templates de visuels (pas juste direction — brief détaillé pour Canva)
- [ ] Ajouter le type de CTA optimal par plateforme (LinkedIn = commentaire, Instagram = DM, etc.)

---

### 14. `dp-mediaplan` — Score : 7.5/10

**Ce qui fonctionne :**
- Mix funnel TOFU/MOFU/BOFU
- Content mix par plateforme
- 6 hooks patterns

**Améliorations nécessaires :**
- [ ] **CRITIQUE** : Les questions Q7-Q8 sur les couleurs sont dans le Bloc 3 (trop tard) et pas assez explicites → déplacer en Bloc 1 avec primary_color + accent_color
- [ ] Ajouter un calendrier de lancement spécifique (J-14 à J+7 pour un lancement d'ebook)

---

### 15. `dp-ad-angles-meta` — Score : 7.5/10

**Ce qui fonctionne :**
- 12 angles psychologiques
- A/B testing framework
- Audience targeting par angle

**Améliorations nécessaires :**
- [ ] **BUG** : QG-08 parle de "broad match without smart bidding" → c'est du Google, pas du Meta. Remplacer par un gate spécifique Meta (ex: "budget minimum ≥ 5× CPA par ad set")
- [ ] Ajouter les UTM parameters pour chaque URL
- [ ] Ajouter des briefs créatifs visuels détaillés (specs image/vidéo par placement)

---

### 16. `dp-ad-angles-google` — Score : 8/10

**Ce qui fonctionne :**
- RSA avec 15 headlines + 4 descriptions
- YouTube scripts (3 formats)
- Bidding strategy recommendations

**Améliorations nécessaires :**
- [ ] Ajouter Performance Max campaigns
- [ ] Ajouter les extensions d'annonces (sitelinks, callouts, structured snippets)

---

### 17. `dp-competitor-analysis` — Score : 8.5/10

**Ce qui fonctionne :**
- Objectivité enforced (le concurrent peut gagner)
- Counter-angles sans nommer le concurrent
- SWOT + Porter

**Améliorations nécessaires :**
- [ ] Ajouter le scénario "concurrent n'a pas de page publique" 
- [ ] Ajouter l'analyse des prix concurrents avec positionnement recommandé

---

### 18. `dp-copy-review` — Score : 8.5/10

**Ce qui fonctionne :**
- 6 dimensions de scoring
- Red flags checklist détaillée
- Compliance rules strictes

**Améliorations nécessaires :**
- [ ] Clarifier le scoring quand le copy est déjà excellent (98/100 = quoi exactement ?)
- [ ] Ajouter la gestion du copy en plusieurs langues

---

## PROBLÈMES TRANSVERSAUX

### P1 — Ebook de 60 pages : pas garanti

Le skill `dp-playbook-create` demande "5000+ mots" pour un playbook, ce qui fait ~15 pages. Pour 60 pages, il faut **~21000 mots**. Il manque :
- Une exigence de pages, pas seulement de mots
- Des guidelines pour étoffer (exercices, templates, études de cas, checklists, annexes)
- Un compteur de pages dans le plan
- Un enrichissement post-plan

### P2 — Couleurs inconsistantes

6 skills ne demandent PAS les couleurs de marque alors qu'ils génèrent du contenu visuel :
- `dp-ad-angles-google` (briefs créatifs sans couleurs)
- `dp-ad-angles-meta` (idem)
- `dp-blog-article` (HTML sans brand colors)
- `dp-competitor-analysis` (rapport sans branding)
- `dp-export-pdf` (ne charge pas business-profile.md)
- `dp-playbook-create` en mode express

### P3 — Quality gates copy-paste

`dp-ad-angles-meta` QG-08 est copié de `dp-ad-angles-google` (parle de "broad match" qui n'existe pas sur Meta).

### P4 — Références manquantes

Plusieurs skills mentionnent des fichiers dans `references/` qui n'existent pas. Seuls 3 skills ont un dossier `references/` :
- `dp-playbook-create` (6 fichiers)
- `dp-blog-article` (2 fichiers)
- `dp-copy-review` (2 fichiers)
- `dp-playbook-audit` (2 fichiers)

---

## TODO — CORRECTIONS PAR PRIORITÉ

### Priorité CRITIQUE (bloquer la publication)

| # | Tâche | Skill | Description |
|---|-------|-------|-------------|
| T01 | Ebook 60 pages | `dp-playbook-create` | Refondre pour garantir 60+ pages : exigence en pages, pas en mots. Ajouter guidelines d'enrichissement, compteur de pages, step "Enrichissement" post-plan. Min 21000 mots pour playbook. |
| T02 | Couleurs express | `dp-playbook-create` | Le mode express DOIT demander les couleurs (ajouter aux 5 questions essentielles) |
| T03 | Load business-profile | `dp-export-pdf` | Ajouter Step 0 qui charge business-profile.md pour les couleurs de couverture |
| T04 | Fix QG Meta | `dp-ad-angles-meta` | Remplacer QG-08 (broad match) par un gate Meta spécifique |

### Priorité HAUTE (qualité pro)

| # | Tâche | Skill | Description |
|---|-------|-------|-------------|
| T05 | Couleurs dans ads | `dp-ad-angles-meta` + `dp-ad-angles-google` | Charger business-profile.md, intégrer les couleurs dans les briefs créatifs |
| T06 | Couleurs blog HTML | `dp-blog-article` | Si format HTML, utiliser les couleurs du business-profile |
| T07 | Mediaplan couleurs | `dp-mediaplan` | Déplacer Q couleurs en Bloc 1, rendre obligatoire |
| T08 | Version harmonisée | `dp-blog-article` | Passer de 3.0.0 à 2.1.0 pour cohérence |
| T09 | Seuils publication | `dp-playbook-audit` | Définir : < 70 = bloquer, 70-89 = réserves, 90+ = prêt |
| T10 | Liens internes min | `dp-blog-article` | Définir inline : 3-5 pour 1500 mots, 5-8 pour 2000+ |
| T11 | Séquence lancement | `dp-email-sequence` | Ajouter type "launch" (7 emails pour lancement ebook) |
| T12 | Budget seuils | `dp-sales-funnel` | Définir : < 500€ = organique, 500-2000€ = mixte, > 2000€ = paid |

### Priorité MOYENNE (polish)

| # | Tâche | Skill | Description |
|---|-------|-------|-------------|
| T13 | Express business | `dp-business-profile` | Ajouter mode express 5 questions |
| T14 | LP lead magnet | `dp-landing-page` | Ajouter mode "capture email" (sans prix) |
| T15 | WP images | `dp-blog-publish` | Upload media + featured image |
| T16 | Mediaplan launch | `dp-mediaplan` | Calendrier de lancement J-14 → J+7 |
| T17 | CTA par plateforme | `dp-social-caption` | LinkedIn=comment, IG=DM, TikTok=follow |
| T18 | Ratios intent | `dp-blog-strategy` | Définir TOFU 40% / MOFU 35% / BOFU 25% inline |

---

## ORDRE D'EXÉCUTION

```
T01 → T02 → T03 → T04    (critiques — d'abord)
T05 → T06 → T07           (couleurs cohérentes partout)
T08 → T09 → T10           (qualité et seuils)
T11 → T12                 (fonctionnalités manquantes)
T13 → T18                 (polish)
```
