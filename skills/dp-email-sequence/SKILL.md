---
name: dp-email-sequence
description: "Create complete, ready-to-load email marketing sequences (welcome, launch, abandon, re-engagement, nurture, post-purchase) with subject lines, preview text, full body copy, CTAs, timing, and optional HTML templates with brand colors. Triggers: email, sequence, welcome email, launch sequence, abandon cart, nurture, drip campaign."
user-invokable: true
argument-hint: "[type: welcome|launch|abandon|re-engagement|nurture|post-purchase] [product]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: content
  updated: 2026-04-13
---

# Email Sequence — Email Marketing Writer

<!-- v2.0.0 | 2026-04-13 | Refonte complète : context intake, brand identity, HTML templates, quality gates, error handling -->

Expert en email marketing pour DP Créateur. Crée des séquences email complètes, prêtes à charger dans n'importe quel outil d'emailing — chaque email entièrement rédigé, aucun placeholder.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-email-sequence [type]` | Lancer la création guidée d'une séquence |
| `/dp-email-sequence express [type]` | Mode rapide — 3 questions puis rédaction complète |
| `/dp-email-sequence single [sujet]` | Écrire un email isolé (pas une séquence) |
| `/dp-email-sequence audit [fichier]` | Analyser une séquence existante et suggérer des améliorations |

## Output Format

```
LIVRABLE :
├── Séquence complète (emails/[type]-sequence.[html|md])
├── Chaque email : subject line A/B, preview text, body, CTA, P.S.
├── Timing de chaque envoi (jour + heure recommandée)
├── Métriques cibles par email (open rate, click rate)
├── Option : HTML template avec couleurs de marque
└── Résumé de la séquence + flow visuel
```

---

## Process

```
1. Context intake      → Collecter les infos essentielles (OBLIGATOIRE)
2. Read references     → Charger profil business, produit, voix
   Read references/launch-sequence-templates.md → pour le template de séquence de lancement
3. Plan sequence       → Map de la séquence validée par l'utilisateur
4. Write emails        → Rédaction complète de chaque email
5. Quality check       → Quality gates, spam check, relecture
6. Deliver             → Fichier sauvegardé + résumé + prochaines étapes
```

---

## Step 1 — Context Intake (Required: Always Do This First)

Avant toute rédaction, collecter le contexte. Sans lui, les emails seront génériques et les CTAs déconnectés du business réel.

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom, niche, produit(s), audience, ton, couleurs, email expéditeur
  → Ne PAS reposer les questions déjà couvertes par le profil

SINON :
  → Continuer sans. Les questions de l'intake couvriront le minimum.
```

### 1b. Poser les questions par blocs

**Règle absolue** : Ne JAMAIS poser toutes les questions d'un coup. Grouper par 2-3, attendre les réponses, reformuler pour valider, puis continuer.

#### Bloc 1 — La séquence et le produit (poser en premier)

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Quel type de séquence ? `welcome` / `launch` / `abandon` / `re-engagement` / `nurture` / `post-purchase` | Structure et objectif |
| Q2 | Quel produit ou service promouvoir dans cette séquence ? (nom, prix, URL si disponible) | CTAs concrets et pertinents |
| Q3 | Quel est l'événement déclencheur ? (ex: inscription newsletter, achat, abandon panier, date de lancement) | Timing et contexte des emails |

**Après les réponses** : Reformuler. "Si je comprends bien, tu veux une séquence [type] pour [produit] déclenchée par [trigger]. C'est correct ?"

#### Bloc 2 — L'audience et la voix

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | Qui reçoit ces emails ? Décris ton destinataire idéal (métier, situation, niveau). | Ton et pertinence du contenu |
| Q5 | Quel est le problème n°1 de cette personne ? Et quel résultat elle veut ? | Hook et argumentation |
| Q6 | Décris ta voix de marque en 2-3 mots (ex: "direct et chaleureux", "expert et motivant", "no-BS et data-driven") | Cohérence avec le branding |

**Après les réponses** : Valider le profil destinataire.

#### Bloc 3 — L'identité de marque et le format

| # | Question | Pourquoi |
|---|----------|----------|
| Q7 | Quel nom et email d'expéditeur ? (ex: "Marie <marie@monbiz.com>") | Personnalisation du from |
| Q8 | Tu veux des emails en HTML avec design (couleurs, boutons stylés) ou en texte brut ? Si HTML, donne ta couleur principale et ta couleur d'accent (hex ou nom). | Format de sortie adapté |
| Q9 | Combien d'emails dans la séquence ? (ou "défaut" pour la recommandation standard) | Longueur de la séquence |
| Q10 | Tu as une offre spéciale, un bonus, ou un angle promotionnel ? (ex: réduction early-bird, bonus limité, garantie) | Argument commercial |

**Après les réponses** : Confirmer le format et passer au plan.

> **Caveat honnête** : Les taux d'ouverture et de clic varient énormément selon la niche, la taille de la liste, et la qualité des leads. Les métriques cibles indiquées sont des benchmarks — pas des garanties.

---

## Sequence Types & Defaults

| Type | Emails par défaut | Trigger | Objectif |
|------|-------------------|---------|----------|
| `welcome` | 5 | Nouvel abonné | Accueillir → valeur → pitch doux → pitch direct |
| `launch` | 7 | Date de lancement | Anticipation → ouverture → urgence → fermeture |
| `abandon` | 3 | Abandon panier/page | Rappel → objection → push final |
| `re-engagement` | 4 | 30 jours inactif | Re-hook → bombe de valeur → offre → dernière chance |
| `nurture` | 6 | Continu (hebdo) | Éduquer → confiance → CTA périodique |
| `post-purchase` | 4 | Achat complété | Merci → onboarding → check-in → demande de referral |

---

## Step 2 — Plan the Sequence

Présenter la map de la séquence et attendre validation :

```
PLAN DE SÉQUENCE
================
Type : [type]
Emails : [nombre]
Trigger : [événement déclencheur]
Objectif final : [goal]
Expéditeur : [nom <email>]
Format : [HTML avec design / texte brut]

Flow :
Jour 0  : Email 1 — [objectif de cet email]
Jour 1  : Email 2 — [objectif]
Jour 3  : Email 3 — [objectif]
Jour 5  : Email 4 — [objectif]
Jour 7  : Email 5 — [objectif]
...
```

**Hard gate** : Ne PAS commencer la rédaction sans validation du plan.

---

## Step 3 — Write Each Email

Pour CHAQUE email de la séquence, produire :

```
EMAIL [#] — [Nom interne]
============================
Envoi : Jour [X] après [trigger] ([contexte de timing])
De : [Nom <email>]

Subject Line (A) : [< 50 caractères, compelling]
Subject Line (B) : [variation A/B — approche différente, pas un swap de mot]
Preview Text : [< 90 caractères, complète le subject sans le répéter]

---

[Corps complet de l'email — formaté pour email]

[Signature]
[Nom]
[Business]

P.S. [Post-script — souvent la partie la plus lue après le subject]

---

CTA : [Texte du bouton] → [URL destination]
Objectif : [Ce que cet email doit accomplir]
Métrique cible : [Open rate / Click rate / Reply rate — benchmark]
```

### Si format HTML demandé

Produire un template HTML responsive avec :
- Couleurs de marque (primaire et accent du context intake)
- Bouton CTA stylé avec la couleur d'accent
- Layout simple, mobile-first (600px max-width)
- Police web-safe (Arial, Helvetica)
- Lien de désinscription en footer
- Pas d'images externes (elles sont souvent bloquées)

---

## Email Writing Rules

### Subject Lines
- Sous 50 caractères (aperçu mobile)
- Créer curiosité ou urgence — jamais de clickbait
- Patterns qui marchent :

| Pattern | Exemple |
|---------|---------|
| Question | "Tu devines toujours comment trouver des clients ?" |
| Nombre | "La règle des 20 messages qui a tout changé" |
| Direct | "Ton [produit] est prêt" |
| Personnel | "[Prénom] ici — petite question" |
| Contrarian | "Arrête de publier du contenu" |
| Story | "Elle a signé 3 clients en 9 jours" |

### Preview Text
- Complète le subject (ne le répète pas)
- Sous 90 caractères
- Ajoute du contexte ou teaser le contenu

### Corps de l'email
- **Paragraphes courts** — 1-3 phrases max par paragraphe
- **Une idée par email** — Ne pas entasser plusieurs sujets
- **Conversationnel** — Écrire comme si on parlait à une personne
- **Mobile-first** — Lignes courtes, pas d'images larges, scannable
- **Un CTA principal** — Une action par email (bouton ou lien)
- **Ligne P.S.** — Toujours en inclure une. C'est la 2e partie la plus lue.

### Placement des CTAs
- Un CTA clair par email
- Répéter le lien CTA 2-3 fois dans le corps (en lien texte, pas seulement bouton)
- Bouton CTA à la fin
- Le P.S. contient souvent une variation plus douce du CTA

### Pacing des séquences

| Type | Rythme |
|------|--------|
| Welcome | Quotidien les 3 premiers, puis tous les 2 jours |
| Launch | Quotidien pendant la fenêtre de lancement |
| Abandon | Heures 1, 24, 72 |
| Nurture | Hebdomadaire |
| Post-purchase | Jour 0, 3, 7, 14 |

### Ton
- Chaleureux mais direct ("Hey [prénom]" pas "Cher abonné")
- Personnel (du fondateur, pas de "l'équipe")
- Partager de vrais insights, pas des teasers ("Voici le script" pas "On a un script")
- Confiant : "Ça marche." pas "On pense que ça pourrait aider."
- Honnête : "Ce n'est pas pour tout le monde." crée plus de confiance que "N'importe qui peut le faire !"

---

## Sequence Templates

### Welcome Sequence
```
Email 1 (Jour 0) : Bienvenue + livrer le lead magnet + fixer les attentes
Email 2 (Jour 1) : Erreur n°1 de l'audience + insight rapide (valeur)
Email 3 (Jour 2) : Framework ou script tiré du produit (valeur)
Email 4 (Jour 4) : Story + pitch doux vers le produit
Email 5 (Jour 6) : Pitch direct + preuve/témoignage + CTA
```

### Launch Sequence
```
Email 1 (J-3) : Annonce + ce qui arrive
Email 2 (J-1) : Le problème que ça résout (story)
Email 3 (J0)  : Ouverture + pitch complet + CTA
Email 4 (J+1) : FAQ + gestion des objections
Email 5 (J+3) : Cas concret / témoignage
Email 6 (J+5) : Dernière chance + rareté
Email 7 (J+6) : Appel final + fermeture
```

### Séquence Launch — Lancement d'Ebook (7 emails)

| # | Jour | Objectif | Objet type |
|---|------|----------|------------|
| 1 | J-7 | Teaser | "Quelque chose arrive..." |
| 2 | J-3 | Reveal | "[Titre] sort dans 3 jours — voici ce que c'est" |
| 3 | J-0 | Lancement | "C'est live → [Titre] est disponible" |
| 4 | J+1 | Early bird / Preuve | "24h après : les premiers retours + offre early bird" |
| 5 | J+3 | Social proof | "[N] personnes l'ont déjà — voici ce qu'ils en disent" |
| 6 | J+5 | Urgence | "L'offre de lancement se termine dans 48h" |
| 7 | J+7 | Last call + post-launch | "Dernière chance / Merci — et la suite" |

### Abandon Sequence
```
Email 1 (1 heure) : "Un problème ?" + lien retour
Email 2 (24 heures) : Adresser l'objection principale + rappel de la valeur
Email 3 (72 heures) : Push final + bonus ou garantie
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Aucun placeholder ([TODO], [INSERT], [votre histoire ici]) | Critical |
| QG-02 | Chaque email est ENTIÈREMENT rédigé — corps complet | Critical |
| QG-03 | Subject lines A/B sont des approches différentes, pas des swaps de mots | High |
| QG-04 | Chaque email a exactement 2-3 placements CTA (lien texte + bouton) | High |
| QG-05 | Ligne P.S. présente dans chaque email | High |
| QG-06 | Aucun spam trigger : "GRATUIT!!!", "Agis maintenant!!!", MAJUSCULES excessives | Critical |
| QG-07 | Référence à la désinscription dans le template | Critical |
| QG-08 | Chaque email apporte de la valeur standalone (même sans clic) | High |
| QG-09 | Aucune promesse de revenus ou résultats garantis | Critical |
| QG-10 | Preview text ne répète pas le subject line | Medium |
| QG-11 | Pas de 2 emails de pitch dur consécutifs | High |
| QG-12 | Si HTML : template responsive, max-width 600px, polices web-safe | High |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Type de séquence non spécifié | Demander : "Quel est ton objectif ? Je te recommande le bon type de séquence." |
| Pas de produit à promouvoir | Créer une séquence orientée valeur/engagement avec CTAs vers du contenu gratuit |
| business-profile.md absent | Continuer avec les réponses du context intake |
| L'utilisateur veut aller vite | Mode express : Q1, Q2, Q7 seulement, puis rédaction complète. Prévenir : "Résultat bon mais moins personnalisé." |
| Séquence trop longue (>10 emails) | Prévenir : "Au-delà de 10 emails, les taux d'engagement chutent significativement. Je recommande [N] emails max pour ce type." |
| Produit pas encore créé | Suggérer de créer le produit d'abord avec `/dp-playbook-create` ou `/dp-landing-page` |
| L'utilisateur veut modifier un email après rédaction | Réécrire l'email concerné en gardant la cohérence avec le reste de la séquence |
| Niche sensible (santé, finance) | Ajouter des disclaimers appropriés et éviter toute claim non vérifiable |

---

## Cross-Skill Integration

| Avant email-sequence | Skill | Quand |
|----------------------|-------|-------|
| Créer le produit à vendre | `/dp-playbook-create` | Si le produit n'existe pas encore |
| Définir le business | `business-profile.md` | Si pas encore créé |
| Créer le lead magnet | `/dp-playbook-create lead-magnet` | Pour la séquence welcome |
| Page de vente | `/dp-landing-page` | Pour avoir une URL de destination CTA |

| Après email-sequence | Skill suivant | Quand |
|----------------------|---------------|-------|
| Promouvoir sur les réseaux | `/dp-social-caption` | Pour annoncer le lancement |
| Article de blog | `/dp-blog-article` | Contenu à lier dans les emails nurture |
| Publicité | `/dp-ad-angles-meta` `/dp-ad-angles-google` | Pour l'acquisition de leads |
| Media plan | `/dp-mediaplan` | Pour planifier le calendrier de lancement |
