---
name: dp-tracking-setup
description: "Configure le tracking complet pour mesurer les performances : Meta Pixel avec events, Google Analytics 4 avec conversions, Google Tag Manager, UTM generator par campagne/source. Produit le code prêt à coller et un guide d'installation pas à pas. Sans tracking, ton budget ads est dépensé à l'aveugle. Triggers : tracking, pixel, analytics, conversion, UTM, GA4, Meta Pixel, Google Tag, mesurer, suivi."
user-invokable: true
argument-hint: "[plateforme: meta|google|all] [url-landing-page]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.0.0"
  category: marketing
  updated: 2026-04-19
---

# Tracking Setup — Configurer le Suivi des Performances

<!-- v2.0.0 | 2026-04-19 | Création : Meta Pixel, GA4, GTM, UTM, events, guide installation -->

Sans tracking, tu dépenses en pub sans savoir ce qui fonctionne. Ce skill génère tout le code de suivi nécessaire et un guide d'installation clair — même si tu n'es pas technique.

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-tracking-setup` | Configuration complète guidée (Meta + GA4 + UTM) |
| `/dp-tracking-setup meta` | Meta Pixel uniquement |
| `/dp-tracking-setup google` | Google Analytics 4 uniquement |
| `/dp-tracking-setup utm [campagne]` | Générer les UTM pour une campagne |
| `/dp-tracking-setup check [url]` | Vérifier si le tracking est bien installé |

## Output Format

```
LIVRABLES :
├── tracking/setup-[projet].md
│   ├── Code Meta Pixel (base + events)
│   ├── Code Google Analytics 4 (base + conversions)
│   ├── Code Google Tag Manager (optionnel)
│   ├── Tableau UTM complet (toutes sources/campagnes)
│   └── Guide d'installation pas à pas (avec screenshots textuels)
└── Prêt à coller dans la landing page et la thank you page
```

---

## Process

```
1. Context intake      → Plateformes ads, URL landing, produit, prix
2. Read context        → business-profile.md, landing page existante
3. Générer Meta Pixel  → Code base + events (ViewContent, Purchase)
4. Générer GA4         → Code base + conversion goals
5. Générer UTM         → Tableau complet par source/campagne
6. Guide installation  → Où coller quoi, pas à pas
7. Deliver             → Fichier tracking + résumé
```

---

## Step 1 — Context Intake

### 1a. Charger le contexte (silencieux)

```
SI business-profile.md existe :
  → Lire : plateformes ads, URL site, produits, prix
  
SI landing-pages/*.html existe :
  → Identifier la landing page et la thank you page

SINON :
  → Poser les questions
```

### 1b. Questions

#### Bloc 1 — Les bases

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | **Quelles plateformes de pub** tu utilises ou prévois ? `Meta Ads` / `Google Ads` / `les deux` / `aucune pour l'instant` | Détermine quels pixels installer |
| Q2 | **Quelle est l'URL de ta landing page ?** | Le code sera adapté à cette page |
| Q3 | **Quel est ton produit et son prix ?** | Pour configurer l'event Purchase avec la valeur |

#### Bloc 2 — Les outils

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | **Tu as déjà un compte Meta Business Manager ?** Si oui, tu as un Pixel ID ? (format : 15 chiffres) | Pré-remplir le code |
| Q5 | **Tu as déjà un compte Google Analytics ?** Si oui, tu as un Measurement ID ? (format : G-XXXXXXXXXX) | Pré-remplir le code |
| Q6 | **Tu utilises Google Tag Manager (GTM) ?** Si oui, tu as un Container ID ? (format : GTM-XXXXXXX) | Alternative à l'installation directe |

> Si l'utilisateur n'a pas de Pixel ID ou GA4 ID, lui expliquer comment les créer (dans le guide d'installation).

---

## Step 2 — Meta Pixel

### 2a. Code de base (à mettre dans le `<head>` de CHAQUE page)

```html
<!-- Meta Pixel Code -->
<script>
!function(f,b,e,v,n,t,s)
{if(f.fbq)return;n=f.fbq=function(){n.callMethod?
n.callMethod.apply(n,arguments):n.queue.push(arguments)};
if(!f._fbq)f._fbq=n;n.push=n;n.loaded=!0;n.version='2.0';
n.queue=[];t=b.createElement(e);t.async=!0;
t.src=v;s=b.getElementsByTagName(e)[0];
s.parentNode.insertBefore(t,s)}(window, document,'script',
'https://connect.facebook.net/en_US/fbevents.js');
fbq('init', '[PIXEL_ID]');
fbq('track', 'PageView');
</script>
<noscript><img height="1" width="1" style="display:none"
src="https://www.facebook.net/tr?id=[PIXEL_ID]&ev=PageView&noscript=1"
/></noscript>
<!-- End Meta Pixel Code -->
```

### 2b. Events à configurer

| Page | Event | Code | Quand |
|------|-------|------|-------|
| Landing page | `ViewContent` | `fbq('track', 'ViewContent', {content_name: '[product_name]', content_type: 'product', value: [prix], currency: 'EUR'});` | Au chargement de la page |
| Clic CTA | `InitiateCheckout` | `fbq('track', 'InitiateCheckout', {value: [prix], currency: 'EUR'});` | Au clic sur le bouton CTA |
| Thank you page | `Purchase` | `fbq('track', 'Purchase', {value: [prix], currency: 'EUR', content_name: '[product_name]'});` | Au chargement de la thank you page |
| Lead magnet | `Lead` | `fbq('track', 'Lead', {content_name: '[lead_magnet_name]'});` | À la soumission du formulaire |

### 2c. Implémentation sur la landing page

```html
<!-- LANDING PAGE — dans le <head> -->
<script>
// Pixel de base (déjà installé ci-dessus)
// Event ViewContent au chargement
fbq('track', 'ViewContent', {
  content_name: '[product_name]',
  content_type: 'product',
  value: [prix],
  currency: 'EUR'
});
</script>

<!-- Sur le bouton CTA — ajouter onclick -->
<a href="[checkout_url]" class="cta-button" 
   onclick="fbq('track', 'InitiateCheckout', {value: [prix], currency: 'EUR'});">
  [CTA text]
</a>
```

```html
<!-- THANK YOU PAGE — dans le <head> -->
<script>
// Event Purchase au chargement
fbq('track', 'Purchase', {
  value: [prix],
  currency: 'EUR',
  content_name: '[product_name]'
});
</script>
```

### 2d. Comment créer un Meta Pixel (si pas encore fait)

```
GUIDE — CRÉER UN META PIXEL
════════════════════════════

1. Va sur https://business.facebook.com
2. Menu → Events Manager → Connect Data Sources
3. Choisir "Web" → "Meta Pixel"
4. Donner un nom (ex: "[Business Name] Pixel")
5. Copier le Pixel ID (15 chiffres)
6. Coller dans le code ci-dessus à la place de [PIXEL_ID]

VÉRIFIER :
  → Installer l'extension "Meta Pixel Helper" sur Chrome
  → Visiter ta landing page
  → L'extension doit montrer "PageView" en vert
```

---

## Step 3 — Google Analytics 4

### 3a. Code de base (dans le `<head>` de CHAQUE page)

```html
<!-- Google Analytics 4 -->
<script async src="https://www.googletagmanager.com/gtag/js?id=[GA4_ID]"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', '[GA4_ID]');
</script>
<!-- End GA4 -->
```

### 3b. Events de conversion

```javascript
// LANDING PAGE — Event view_item (au chargement)
gtag('event', 'view_item', {
  currency: 'EUR',
  value: [prix],
  items: [{
    item_name: '[product_name]',
    price: [prix]
  }]
});

// BOUTON CTA — Event begin_checkout (au clic)
gtag('event', 'begin_checkout', {
  currency: 'EUR',
  value: [prix],
  items: [{
    item_name: '[product_name]',
    price: [prix]
  }]
});

// THANK YOU PAGE — Event purchase (au chargement)
gtag('event', 'purchase', {
  transaction_id: 'T_' + Date.now(),
  currency: 'EUR',
  value: [prix],
  items: [{
    item_name: '[product_name]',
    price: [prix],
    quantity: 1
  }]
});
```

### 3c. Configurer les conversions dans GA4

```
GUIDE — MARQUER "PURCHASE" COMME CONVERSION
════════════════════════════════════════════

1. Va sur https://analytics.google.com
2. Admin → Events
3. Trouver l'event "purchase"
4. Activer "Mark as conversion" (toggle ON)
5. → Maintenant chaque achat est trackable dans les rapports

AUSSI MARQUER COMME CONVERSION :
  - begin_checkout (pour mesurer le taux d'abandon)
  - generate_lead (si lead magnet)
```

### 3d. Comment créer un compte GA4 (si pas encore fait)

```
GUIDE — CRÉER UN COMPTE GA4
════════════════════════════

1. Va sur https://analytics.google.com
2. "Start measuring" → Créer un compte
3. Nom du compte : "[Business Name]"
4. Créer une propriété : "[Business Name] — Website"
5. Choisir "Web" comme plateforme
6. Entrer l'URL de ton site
7. Copier le Measurement ID (format G-XXXXXXXXXX)
8. Coller dans le code ci-dessus à la place de [GA4_ID]
```

---

## Step 4 — Google Tag Manager (optionnel)

Si l'utilisateur utilise GTM, fournir les instructions pour installer le tracking VIA GTM plutôt qu'en direct.

```
AVANTAGE GTM : Tu installes UN SEUL code (GTM) sur ton site,
               et tu gères tous les pixels/tags depuis l'interface GTM.
               Plus besoin de toucher au HTML pour chaque nouveau tag.

QUAND UTILISER GTM :
  - Tu as 3+ pixels/tags à gérer
  - Tu changes souvent de configuration
  - Tu veux tester des events sans modifier le code

QUAND NE PAS UTILISER GTM :
  - Tu as juste Meta Pixel + GA4 (le code direct est plus simple)
  - Tu n'es pas à l'aise avec les interfaces techniques
```

### Code GTM (dans le `<head>`)

```html
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','[GTM_ID]');</script>
<!-- End Google Tag Manager -->
```

---

## Step 5 — Générateur UTM

### Tableau UTM complet

Générer un tableau avec tous les UTM pour chaque source de trafic :

```
TABLEAU UTM — [Projet]
═══════════════════════

FORMAT : [URL]?utm_source=[source]&utm_medium=[medium]&utm_campaign=[campaign]&utm_content=[content]

CAMPAGNES META ADS :
┌──────────────────┬────────┬────────┬───────────────┬──────────┐
│ Angle            │ Source │ Medium │ Campaign      │ Content  │
├──────────────────┼────────┼────────┼───────────────┼──────────┤
│ Pain Point v1    │ meta   │ cpc    │ launch-[mois] │ pain-v1  │
│ Pain Point v2    │ meta   │ cpc    │ launch-[mois] │ pain-v2  │
│ FOMO v1          │ meta   │ cpc    │ launch-[mois] │ fomo-v1  │
│ Authority v1     │ meta   │ cpc    │ launch-[mois] │ auth-v1  │
│ Retargeting      │ meta   │ cpc    │ retarget      │ warm     │
└──────────────────┴────────┴────────┴───────────────┴──────────┘

CAMPAGNES GOOGLE ADS :
┌──────────────────┬────────┬────────┬───────────────┬──────────┐
│ Groupe           │ Source │ Medium │ Campaign      │ Content  │
├──────────────────┼────────┼────────┼───────────────┼──────────┤
│ High Intent      │ google │ cpc    │ search-[mois] │ high     │
│ Medium Intent    │ google │ cpc    │ search-[mois] │ medium   │
│ YouTube          │ google │ video  │ yt-[mois]     │ instream │
│ Display          │ google │ display│ display-[mois]│ banner   │
└──────────────────┴────────┴────────┴───────────────┴──────────┘

ORGANIQUE :
┌──────────────────┬──────────┬─────────┬───────────────┬──────────┐
│ Source           │ Source   │ Medium  │ Campaign      │ Content  │
├──────────────────┼──────────┼─────────┼───────────────┼──────────┤
│ Blog             │ blog     │ organic │ seo           │ [slug]   │
│ Instagram bio    │ instagram│ social  │ bio-link      │ profile  │
│ Instagram post   │ instagram│ social  │ post-[date]   │ caption  │
│ LinkedIn post    │ linkedin │ social  │ post-[date]   │ caption  │
│ Email lancement  │ email    │ email   │ launch        │ email-[N]│
│ Email nurture    │ email    │ email   │ nurture       │ email-[N]│
│ Affilié          │ affiliate│affiliate│ [partenaire]  │ [place]  │
└──────────────────┴──────────┴─────────┴───────────────┴──────────┘
```

### URLs générées

Pour chaque ligne du tableau, générer l'URL complète :

```
[landing_page_url]?utm_source=meta&utm_medium=cpc&utm_campaign=launch-avr26&utm_content=pain-v1
[landing_page_url]?utm_source=meta&utm_medium=cpc&utm_campaign=launch-avr26&utm_content=fomo-v1
[landing_page_url]?utm_source=google&utm_medium=cpc&utm_campaign=search-avr26&utm_content=high
[landing_page_url]?utm_source=email&utm_medium=email&utm_campaign=launch&utm_content=email-3
...
```

### Règles UTM

| Paramètre | Règle | Exemple |
|-----------|-------|---------|
| `utm_source` | Plateforme d'origine (lowercase) | `meta`, `google`, `email`, `instagram` |
| `utm_medium` | Type de trafic | `cpc` (paid), `social` (organique), `email`, `affiliate` |
| `utm_campaign` | Nom de la campagne | `launch-avr26`, `retarget`, `nurture` |
| `utm_content` | Variante spécifique | `pain-v1`, `fomo-v2`, `email-3` |
| Jamais | Espaces (utiliser des tirets) | `pain-point` pas `pain point` |
| Jamais | Majuscules (tout en lowercase) | `meta` pas `Meta` |
| Jamais | Caractères spéciaux | Tirets et underscores uniquement |

---

## Step 6 — Guide d'Installation

### Résumé : où coller quoi

```
OÙ INSTALLER LE CODE
═════════════════════

LANDING PAGE (ex: landing-pages/mon-produit.html)
  Dans le <head> :
    ✅ Meta Pixel (code de base)
    ✅ GA4 (code de base)
    ✅ Event ViewContent (Meta)
    ✅ Event view_item (GA4)
  
  Sur le bouton CTA (attribut onclick) :
    ✅ Event InitiateCheckout (Meta)
    ✅ Event begin_checkout (GA4)

THANK YOU PAGE (ex: landing-pages/ty-xxxx.html)
  Dans le <head> :
    ✅ Meta Pixel (code de base)
    ✅ GA4 (code de base)
    ✅ Event Purchase (Meta)
    ✅ Event purchase (GA4)

BLOG / WORDPRESS :
  → Si WordPress : installer les plugins "PixelYourSite" (Meta) + "Site Kit" (GA4)
  → Si HTML statique : même code que ci-dessus dans chaque page

LEAD MAGNET PAGE :
  → Event Lead (Meta) + generate_lead (GA4) à la soumission du formulaire
```

### Checklist de vérification

```
VÉRIFICATION POST-INSTALLATION
═══════════════════════════════

[ ] Ouvrir la landing page dans Chrome
[ ] Ouvrir les DevTools (F12) → Console → vérifier : pas d'erreurs JS
[ ] Extension "Meta Pixel Helper" → doit montrer "PageView" en vert
[ ] Extension "Google Analytics Debugger" → doit montrer le hit
[ ] Cliquer le bouton CTA → vérifier l'event InitiateCheckout/begin_checkout
[ ] Ouvrir la thank you page → vérifier l'event Purchase en vert
[ ] Dans Meta Events Manager → vérifier que les events remontent (délai : 20 min)
[ ] Dans GA4 → Realtime → vérifier les events (délai : quelques secondes)
[ ] Tester avec un UTM → vérifier dans GA4 → Acquisition → Traffic Acquisition
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Le Pixel ID et le GA4 ID sont des formats valides (15 chiffres / G-XXXXXXXXXX) | Critical |
| QG-02 | L'event Purchase est UNIQUEMENT sur la thank you page (pas la landing page) | Critical |
| QG-03 | La valeur (value) dans les events correspond au prix réel du produit | Critical |
| QG-04 | La currency est correcte (EUR pour France, USD pour US) | High |
| QG-05 | Les UTM sont tous en lowercase, sans espaces, sans caractères spéciaux | High |
| QG-06 | Chaque variante d'ad a un utm_content UNIQUE (pour différencier les performances) | High |
| QG-07 | Le code de tracking ne casse pas le design de la page (testé visuellement) | High |
| QG-08 | Le guide d'installation est compréhensible par un non-développeur | Medium |
| QG-09 | Les extensions de vérification sont mentionnées (Pixel Helper, GA Debugger) | Medium |
| QG-10 | Pas de double comptage (un seul code Pixel par page, un seul GA4 par page) | Critical |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| L'utilisateur n'a pas de Pixel ID | Guider la création pas à pas (Meta Business Manager) |
| L'utilisateur n'a pas de GA4 ID | Guider la création pas à pas (analytics.google.com) |
| L'utilisateur n'a pas de landing page encore | Recommander `/dp-landing-page` d'abord, puis revenir |
| L'utilisateur utilise WordPress | Recommander les plugins au lieu du code manuel |
| Le Pixel ne remonte pas d'events | Checklist de debug : ad blocker ? code bien dans le head ? console errors ? |
| Conflit avec d'autres scripts | Vérifier la console JS, proposer l'installation via GTM |
| L'utilisateur ne fait pas de pub (organique uniquement) | GA4 seul suffit. Pas besoin de Meta Pixel. |
| business-profile.md absent | Demander les infos minimales (URL, produit, prix) |

---

## Cross-Skill Integration

| Avant | Skill | Quand |
|-------|-------|-------|
| Landing page créée | `/dp-landing-page` | Le tracking s'installe SUR la landing page |
| Ads créées | `/dp-ad-angles-meta` `/dp-ad-angles-google` | Les UTM sont générés POUR les ads |
| Profil business | `/dp-business-profile` | Pour l'URL et les plateformes |

| Après | Skill | Quand |
|-------|-------|-------|
| Lancer les ads | `/dp-ad-angles-meta` `/dp-ad-angles-google` | Utiliser les UTM générés |
| Blog articles | `/dp-blog-article` | Ajouter GA4 sur les pages blog |
| Email sequences | `/dp-email-sequence` | Ajouter les UTM aux liens email |
| Mediaplan | `/dp-mediaplan` | Ajouter les UTM aux liens sociaux |
