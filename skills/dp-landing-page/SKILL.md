---
name: dp-landing-page
description: "Générateur de landing pages professionnelles avec Thank You page, Privacy Policy et CGV intégrées en modales. Responsive, HTML standalone, CSS custom properties. Génère 2 fichiers : la landing page + la thank you page (slug unique non devinable). Pages légales en popup sans quitter la page. Bilingue FR/EN. Triggers : landing page, page de vente, sales page, page produit, créer une page, squeeze page, thank you page."
user-invokable: true
argument-hint: "[produit] [url_destination] [langue: fr|en]"
allowed-tools: Read Write Bash Glob
metadata:
  author: DP Créateur
  version: "2.2.0"
  category: marketing
  updated: 2026-04-18
---

# Landing Page — Sales Page Generator

<!-- v2.2.0 | 2026-04-18 | Ajout : Thank You page (slug unique), Privacy Policy + CGV en modales popup, bilingue FR/EN -->

Expert en conversion et web design pour DP Créateur. Génère **2 fichiers HTML** : la landing page de vente + la thank you page post-achat. Les pages légales (Privacy Policy, CGV) s'ouvrent en popup sans quitter la page. Tout est bilingue (FR ou EN selon le projet).

## Quick Reference

| Commande | Description |
|----------|-------------|
| `/dp-landing-page [produit]` | Créer landing page + thank you page (guidé) |
| `/dp-landing-page express [produit]` | Mode rapide — 5 questions puis génération |
| `/dp-landing-page lead-magnet [produit]` | Version capture d'emails (pas de prix) |
| `/dp-landing-page from [fichier]` | Générer depuis un contenu ou playbook existant |
| `/dp-landing-page thank-you [landing.html]` | Créer une thank you page pour une landing existante |

## Output Format

```
LIVRABLES (2 fichiers) :
│
├── 1. Landing Page : landing-pages/[product-slug].html
│   ├── CSS embarqué avec custom properties
│   ├── Design responsive (mobile-first)
│   ├── SEO meta tags + Open Graph
│   ├── Sections : Hero → Trust → Promises → Details → FAQ → CTA → Footer
│   ├── Footer : Privacy Policy + CGV en modales popup (sans quitter la page)
│   ├── Tous les CTA pointent vers la même URL de destination
│   └── Zéro dépendance JS (sauf modales légères)
│
├── 2. Thank You Page : landing-pages/ty-[hash-unique].html
│   ├── Même design system (couleurs, typo, style)
│   ├── Message de remerciement + confirmation
│   ├── Lien de téléchargement (optionnel)
│   ├── Slug unique non devinable (hash aléatoire)
│   └── Pas indexable (meta robots noindex)
│
└── Bilingue : contenu FR ou EN selon la langue du projet
```

---

## Process

```
1. Context intake      → Collecter produit, brand identity, contenu (OBLIGATOIRE)
2. Read references     → Charger business-profile.md si dispo
   Read references/copy-templates.md → pour des exemples de copy par tier de prix
3. Build page card     → Synthèse validée par l'utilisateur
4. Generate HTML       → Page complète avec CSS custom properties
5. Quality check       → Responsive, compliance, SEO, performance
6. Deliver             → Fichier HTML + résumé + prochaines étapes
```

---

## Step 1 — Context Intake (Required: Always Do This First)

### 1a. Charger le profil business (silencieux)

```
SI business-profile.md existe à la racine du projet :
  → Lire et extraire : nom, niche, produit(s), audience, ton, couleurs, logo
  → Ne PAS reposer les questions déjà couvertes par le profil

SINON :
  → Continuer sans. Les questions de l'intake couvriront le minimum.
```

### 1b. Poser les questions par blocs

**Règle absolue** : Ne JAMAIS poser toutes les questions d'un coup. Grouper par 2-3, attendre les réponses, puis continuer.

#### Bloc 1 — Le produit (poser en premier)

| # | Question | Pourquoi |
|---|----------|----------|
| Q1 | Quel produit ou service veux-tu vendre sur cette page ? Nom + description courte. | Cadre le contenu |
| Q2 | Quel est le prix ? (ou "gratuit" si lead magnet) | Hero + CTA |
| Q3 | Quelle est l'URL de destination du bouton CTA ? (Stripe, Gumroad, Calendly, autre lien) | Tous les boutons CTA pointent là |

**Après les réponses** : Reformuler. "Page de vente pour [X] à [prix], CTA vers [URL]. Correct ?"

**Hard gate** : Ne PAS continuer sans au minimum le nom du produit et l'URL de destination.

#### Bloc 2 — L'identité visuelle (OBLIGATOIRE)

| # | Question | Pourquoi |
|---|----------|----------|
| Q4 | Quelle est ta **couleur primaire** ? (hex, ex: `#059669`, ou nom comme "vert émeraude") | CSS `--primary` |
| Q5 | Quelle est ta **couleur d'accent** ? (hex, ex: `#10b981`, ou nom comme "vert clair") | CSS `--accent` |
| Q6 | Quel **style visuel** ? `minimaliste` (épuré, beaucoup de blanc) / `bold` (contrastes forts, couleurs vives) / `premium` (sobre, tons sombres, élégant) / `warm` (tons chauds, accueillant) | Ambiance CSS globale |

> **Si l'utilisateur n'a pas de couleurs** : Proposer 3 palettes adaptées à sa niche :
> - Chaque palette = couleur primaire + couleur accent
> - Montrer un aperçu : "Palette 1 : Bleu profond (#1e3a5f) + Or (#d4a853) — style premium/confiance"
> - Demander de choisir ou combiner

**Après les réponses** : Noter les choix visuels. Ils seront appliqués aux CSS custom properties.

#### Bloc 3 — Le contenu

| # | Question | Pourquoi |
|---|----------|----------|
| Q7 | Tu as des images du produit ? (URLs ou fichiers locaux — 1 à 3 max) | Hero et sections détails |
| Q8 | Quels sont les 3-4 bénéfices principaux de ton produit ? | Section Promises |
| Q9 | Tu as des témoignages, résultats, ou chiffres de crédibilité ? | Trust bar + social proof |

**Après les réponses** : Synthèse contenu.

#### Bloc 4 — Langue et Thank You Page

| # | Question | Pourquoi |
|---|----------|----------|
| Q10 | **Quelle langue** pour la page ? `fr` (français) ou `en` (anglais) | Texte des modales légales, labels, footer |
| Q11 | Pour la **Thank You Page** : tu veux ajouter un **lien de téléchargement** ? Si oui, donne l'URL du fichier (PDF, ZIP…). Si non, la page affichera juste la confirmation. | Page post-achat |

**Après les réponses** : Noter la langue et les options thank you page.

#### Bloc 5 — La FAQ et le SEO

| # | Question | Pourquoi |
|---|----------|----------|
| Q12 | Quelles sont les 3-5 questions que tes clients posent le plus ? | Section FAQ |
| Q13 | Tu as un titre SEO en tête ? Et une meta description ? (sinon je les génère) | Balises meta |

**Après les réponses** : Passer à la génération.

---

## Step 2 — Design System (CSS Custom Properties)

### Variables CSS obligatoires

Toutes les couleurs et styles passent par des CSS custom properties. JAMAIS de couleurs hardcodées dans les composants.

```css
:root {
  /* Couleurs de marque — issues du context intake */
  --primary: [primary_color from Q4];
  --accent: [accent_color from Q5];

  /* Couleurs dérivées automatiquement */
  --primary-light: [primary + transparence 10%];
  --primary-dark: [primary assombri 20%];
  --accent-light: [accent + transparence 10%];

  /* Couleurs système */
  --text: #1a1a1a;
  --muted: #666666;
  --bg: #ffffff;
  --surface: #f9fafb;
  --border: #e5e7eb;

  /* Layout */
  --radius: 12px;
  --shadow: 0 1px 3px rgba(0,0,0,0.1);
  --max-width: 720px;
}
```

### Styles par ambiance visuelle

| Style | Modifications CSS |
|-------|-------------------|
| `minimaliste` | `--bg: #ffffff`, `--surface: #fafafa`, ombres très légères, beaucoup d'espace blanc, `--radius: 8px` |
| `bold` | Contrastes forts, `--bg: #ffffff`, boutons plus gros, titres plus grands, `--radius: 16px`, ombres marquées |
| `premium` | `--bg: #0a0a0a`, `--text: #f5f5f5`, `--surface: #1a1a1a`, `--muted: #999`, fond sombre, typographie élégante |
| `warm` | `--bg: #fffbf5`, `--surface: #fff5eb`, `--border: #f0d9b5`, tons chauds, `--radius: 16px`, ombres douces |

### Typographie
- Font : `'Inter', system-ui, -apple-system, sans-serif` (Google Fonts import pour Inter)
- Body : 16px, line-height 1.6
- H1 : 2.5rem (mobile: 1.75rem)
- H2 : 1.75rem (mobile: 1.35rem)
- H3 : 1.2rem

### Responsive Breakpoints
- Mobile : < 640px (1 colonne, touch targets larges)
- Tablet : 640-1024px (2 colonnes si applicable)
- Desktop : > 1024px (layout complet)

---

## Step 3 — Page Structure (ordre obligatoire)

### 1. Hero / Header

```html
<section class="hero">
  <!-- Image(s) produit si fournies -->
  <h1>[product_name]</h1>
  <p class="subtitle">[product_description]</p>
  <div class="price">[product_price]</div>
  <a href="[destination_url]" class="cta-button">[cta_button_text]</a>
</section>
```

Design :
- Full-width, padding généreux
- Titre large et bold
- Prix affiché avec `var(--primary)`
- CTA : `background: var(--primary)`, texte blanc, arrondi, animation hover
- Sans images : gradient dérivé de `var(--primary)` + `var(--accent)`

### 2. Trust Bar

```html
<section class="trust-bar">
  <div class="badge"><span class="icon">[icon]</span><span>[text]</span></div>
  <!-- 3-5 badges -->
</section>
```

Design : Bande horizontale, fond léger, flex + wrap mobile, icons + texte court

### 3. Promises

```html
<section class="promises">
  <h2>Ce que tu obtiens</h2>
  <div class="promise-grid">
    <div class="promise-card">
      <span class="promise-icon">[emoji]</span>
      <h3>[title]</h3>
      <p>[description]</p>
    </div>
    <!-- 2-4 cartes -->
  </div>
</section>
```

Design : Grille 2 cols desktop / 1 col mobile, cartes blanches avec ombre subtile, `var(--accent)` pour les icônes

### 4. Details & Caractéristiques

```html
<section class="details">
  <div class="detail-block">
    <img src="[image_url]" alt="[title]">
    <div class="detail-content">
      <h3>[title]</h3>
      <ul><li>[point]</li></ul>
    </div>
  </div>
  <!-- Alterner image gauche/droite -->
</section>
```

Design : Layout alterné, stacked sur mobile, parser `**bold**` en `<strong>`, sans images = blocs texte full-width

### 5. Info Cards (optionnel)

```html
<section class="info-cards">
  <div class="info-card info-card--green"><h4>[title]</h4><p>[text]</p></div>
  <!-- Types : green (positif), red (négatif), blue (info), amber (attention) -->
</section>
```

### 6. FAQ

```html
<section class="faq">
  <h2>Questions fréquentes</h2>
  <div class="faq-list">
    <details class="faq-item"><summary>[question]</summary><p>[answer]</p></details>
  </div>
</section>
```

Design : Accordion natif `<details>/<summary>`, pas de JS, animation CSS

### 7. Call to Action (mid-page)

```html
<section class="cta-section">
  <h2>[cta_title]</h2>
  <p>[cta_subtitle]</p>
  <a href="[destination_url]" class="cta-button">[cta_button_text]</a>
</section>
```

### Mode Lead Magnet (page de capture email)

Si le mode `lead-magnet` est choisi, adapter la page :
- **Hero** : Accroche + promesse du lead magnet gratuit (pas de prix)
- **CTA** : "Télécharge gratuitement" / "Reçois le guide" au lieu de "Acheter"
- **Formulaire** : Champ email + bouton (lien vers ConvertKit, Mailchimp, etc.)
- **Pas de section prix** — remplacer par les bénéfices du lead magnet
- **Social proof** : "Déjà [N] téléchargements" plutôt que "Déjà [N] clients"
- **Footer CTA** : Rappel que c'est gratuit + urgence douce ("places limitées" ou "édition limitée")

### 8. Footer (avec liens légaux en modales)

```html
<footer class="footer footer--dark">
  <h2>[footer_title]</h2>
  <p>[footer_subtitle]</p>
  <a href="[destination_url]" class="cta-button">[cta_button_text]</a>
  <div class="footer-legal">
    <p class="copyright">&copy; [year] [brand_name]. All rights reserved.</p>
    <nav class="legal-links">
      <!-- FR -->
      <a href="#" onclick="document.getElementById('modal-privacy').showModal(); return false;">Politique de confidentialité</a>
      <span class="separator">|</span>
      <a href="#" onclick="document.getElementById('modal-cgv').showModal(); return false;">Conditions générales de vente</a>
      <!-- EN -->
      <!-- <a href="#" onclick="document.getElementById('modal-privacy').showModal(); return false;">Privacy Policy</a> -->
      <!-- <span class="separator">|</span> -->
      <!-- <a href="#" onclick="document.getElementById('modal-cgv').showModal(); return false;">Terms & Conditions</a> -->
    </nav>
  </div>
</footer>
```

**Règles** :
- Utiliser les labels FR si `lang="fr"`, EN si `lang="en"`
- Les liens ouvrent des modales `<dialog>`, PAS de nouvelles pages
- Le visiteur ne quitte JAMAIS la landing page

### 8b. Modales — Pages Légales (Privacy Policy + CGV)

Insérer les modales AVANT la fermeture de `</body>` :

```html
<!-- === MODAL : PRIVACY POLICY === -->
<dialog id="modal-privacy" class="legal-modal">
  <div class="modal-content">
    <button class="modal-close" onclick="this.closest('dialog').close()">&times;</button>
    <!-- FR -->
    <h2>Politique de confidentialité</h2>
    <!-- EN: <h2>Privacy Policy</h2> -->

    <p><strong>Dernière mise à jour :</strong> [date]</p>

    <h3>1. Collecte des données</h3>
    <p>[brand_name] collecte les données personnelles suivantes lors de l'achat : nom, adresse email, données de paiement (traitées par [Stripe/Gumroad/LemonSqueezy — jamais stockées sur nos serveurs]). Ces données sont nécessaires à l'exécution de la commande et à la livraison du produit digital.</p>

    <h3>2. Utilisation des données</h3>
    <p>Vos données sont utilisées pour : la livraison du produit acheté, l'envoi d'emails relatifs à votre achat, l'amélioration de nos services. Nous ne vendons ni ne partageons vos données avec des tiers, sauf obligation légale.</p>

    <h3>3. Cookies</h3>
    <p>Ce site utilise des cookies de mesure d'audience (Google Analytics / Meta Pixel) et des cookies fonctionnels. Vous pouvez désactiver les cookies dans les paramètres de votre navigateur.</p>

    <h3>4. Vos droits</h3>
    <p>Conformément au RGPD, vous disposez d'un droit d'accès, de rectification, d'effacement et de portabilité de vos données. Pour exercer ces droits : [email de contact].</p>

    <h3>5. Contact</h3>
    <p>Pour toute question : [email] — [brand_name], [adresse si applicable].</p>
  </div>
</dialog>

<!-- === MODAL : CONDITIONS GÉNÉRALES DE VENTE === -->
<dialog id="modal-cgv" class="legal-modal">
  <div class="modal-content">
    <button class="modal-close" onclick="this.closest('dialog').close()">&times;</button>
    <!-- FR -->
    <h2>Conditions générales de vente</h2>
    <!-- EN: <h2>Terms & Conditions</h2> -->

    <p><strong>Dernière mise à jour :</strong> [date]</p>

    <h3>1. Objet</h3>
    <p>Les présentes conditions régissent la vente du produit digital « [product_name] » par [brand_name].</p>

    <h3>2. Produit</h3>
    <p>Le produit « [product_name] » est un [ebook/playbook/guide] au format [PDF/HTML]. Il est livré par téléchargement immédiat après paiement. Le contenu est fourni en l'état et ne constitue pas un conseil [professionnel/médical/financier — adapter selon la niche].</p>

    <h3>3. Prix et paiement</h3>
    <p>Le prix est de [prix] TTC. Le paiement est sécurisé et traité par [Stripe/Gumroad/LemonSqueezy]. [brand_name] ne stocke aucune donnée bancaire.</p>

    <h3>4. Livraison</h3>
    <p>Le produit est livré immédiatement par email et/ou page de téléchargement après confirmation du paiement. En cas de problème de livraison, contacter [email].</p>

    <h3>5. Droit de rétractation</h3>
    <p>Conformément à l'article L221-28 du Code de la consommation, le droit de rétractation ne s'applique pas aux contenus numériques fournis sur un support immatériel dont l'exécution a commencé avec l'accord du consommateur. Toutefois, [brand_name] offre une garantie satisfait ou remboursé de [14/30] jours. Pour toute demande : [email].</p>

    <h3>6. Propriété intellectuelle</h3>
    <p>Le contenu du produit est protégé par le droit d'auteur. L'achat confère un droit d'utilisation personnel et non transférable. Toute reproduction, revente ou distribution est interdite.</p>

    <h3>7. Responsabilité</h3>
    <p>[brand_name] ne garantit pas de résultats spécifiques. Les résultats dépendent de l'application individuelle du contenu. Les témoignages présentés ne sont pas représentatifs de tous les utilisateurs.</p>

    <h3>8. Contact</h3>
    <p>[brand_name] — [email] — [adresse si applicable].</p>
  </div>
</dialog>
```

### CSS des modales (à ajouter dans le `<style>`)

```css
/* === Modales légales (Privacy + CGV) === */
.legal-modal {
  border: none;
  border-radius: var(--radius);
  max-width: 680px;
  width: 90%;
  max-height: 80vh;
  padding: 0;
  box-shadow: 0 20px 60px rgba(0,0,0,0.3);
}
.legal-modal::backdrop {
  background: rgba(0,0,0,0.6);
}
.modal-content {
  padding: 2rem 2.5rem;
  overflow-y: auto;
  max-height: 80vh;
}
.modal-content h2 {
  font-size: 1.4rem;
  margin-bottom: 1rem;
  color: var(--primary);
}
.modal-content h3 {
  font-size: 1rem;
  margin-top: 1.5rem;
  margin-bottom: 0.5rem;
  color: var(--text);
}
.modal-content p {
  font-size: 0.9rem;
  line-height: 1.7;
  color: var(--muted);
  margin-bottom: 0.75rem;
}
.modal-close {
  position: sticky;
  top: 0;
  float: right;
  background: none;
  border: none;
  font-size: 1.8rem;
  cursor: pointer;
  color: var(--muted);
  padding: 0.5rem;
  line-height: 1;
  z-index: 10;
}
.modal-close:hover { color: var(--text); }

/* Footer legal links */
.footer-legal { margin-top: 1.5rem; }
.legal-links { font-size: 0.8rem; margin-top: 0.5rem; }
.legal-links a { color: rgba(255,255,255,0.6); text-decoration: none; }
.legal-links a:hover { color: #fff; text-decoration: underline; }
.legal-links .separator { margin: 0 0.5rem; color: rgba(255,255,255,0.3); }

/* Si style premium (dark), adapter les modales */
@media (prefers-color-scheme: dark) {
  .legal-modal { background: var(--surface, #1a1a1a); }
  .modal-content h2 { color: var(--accent); }
  .modal-content p { color: #ccc; }
}
```

### Adaptation bilingue des modales

| Élément | Français (lang="fr") | English (lang="en") |
|---------|---------------------|---------------------|
| Lien footer 1 | "Politique de confidentialité" | "Privacy Policy" |
| Lien footer 2 | "Conditions générales de vente" | "Terms & Conditions" |
| Titre modal 1 | "Politique de confidentialité" | "Privacy Policy" |
| Titre modal 2 | "Conditions générales de vente" | "Terms & Conditions" |
| Sections Privacy | Collecte, Utilisation, Cookies, Droits, Contact | Collection, Usage, Cookies, Your Rights, Contact |
| Sections CGV | Objet, Produit, Prix, Livraison, Rétractation, PI, Responsabilité, Contact | Purpose, Product, Price, Delivery, Refund Policy, IP, Liability, Contact |
| Rétractation | Référence Code de la consommation FR | Reference consumer protection law (local) |
| Fermer | &times; (universel) | &times; (universel) |

**Règle** : Utiliser la langue définie dans Q10 (ou détectée depuis `<html lang="...">`). Ne JAMAIS mixer les langues dans une même modale.

---

### 9. Thank You Page (générée automatiquement)

La thank you page est un **fichier HTML séparé** avec :
- Le même design system (couleurs, typo, style)
- Un **slug unique non devinable** (hash aléatoire)
- Un meta `noindex` (pas indexable par Google)

#### Génération du slug unique

```bash
# Générer un hash aléatoire de 12 caractères
HASH=$(openssl rand -hex 6)
# Résultat : ty-a3f7b2c91e04.html
FILENAME="ty-${HASH}.html"
```

Le fichier est sauvegardé dans `landing-pages/ty-[hash].html`.

#### Structure de la Thank You Page

```html
<!DOCTYPE html>
<html lang="[fr/en]">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="robots" content="noindex, nofollow">
  <title>Merci — [product_name]</title>
  <!-- EN: <title>Thank You — [product_name]</title> -->
  <style>
    /* MÊME CSS que la landing page (copier intégralement) */
  </style>
</head>
<body>
  <main class="thank-you">

    <!-- Section 1 : Confirmation -->
    <section class="hero thank-you-hero">
      <div class="checkmark">✓</div>
      <!-- FR -->
      <h1>Merci pour ton achat !</h1>
      <p class="subtitle">Ton accès à « [product_name] » est confirmé.</p>
      <!-- EN -->
      <!-- <h1>Thank you for your purchase!</h1> -->
      <!-- <p class="subtitle">Your access to "[product_name]" is confirmed.</p> -->
    </section>

    <!-- Section 2 : Téléchargement (si lien fourni) -->
    <!-- CONDITIONNEL : afficher seulement si Q11 a un lien de téléchargement -->
    <section class="download-section">
      <!-- FR -->
      <h2>Télécharge ton [ebook/playbook/guide]</h2>
      <p>Clique sur le bouton ci-dessous pour télécharger ton fichier. Tu recevras aussi un email de confirmation avec le lien.</p>
      <a href="[download_url]" class="cta-button download-button">Télécharger maintenant</a>
      <!-- EN -->
      <!-- <h2>Download your [ebook/playbook/guide]</h2> -->
      <!-- <p>Click the button below to download your file. You'll also receive a confirmation email with the link.</p> -->
      <!-- <a href="[download_url]" class="cta-button download-button">Download Now</a> -->
    </section>

    <!-- Section 3 : Prochaines étapes -->
    <section class="next-steps">
      <!-- FR -->
      <h2>Et maintenant ?</h2>
      <div class="steps-list">
        <div class="step">
          <span class="step-number">1</span>
          <div>
            <h3>Ouvre ton [ebook/playbook]</h3>
            <p>Lis la section "Lis ça d'abord" pour savoir par où commencer.</p>
          </div>
        </div>
        <div class="step">
          <span class="step-number">2</span>
          <div>
            <h3>Applique dès aujourd'hui</h3>
            <p>Commence par la première action de la Section 1. Pas demain — maintenant.</p>
          </div>
        </div>
        <div class="step">
          <span class="step-number">3</span>
          <div>
            <h3>Besoin d'aide ?</h3>
            <p>Réponds à l'email de confirmation. On lit tout.</p>
          </div>
        </div>
      </div>
      <!-- EN : adapter les textes -->
    </section>

    <!-- Section 4 : Social proof / partage (optionnel) -->
    <section class="share-section">
      <!-- FR -->
      <p>Tu connais quelqu'un qui en a besoin ? Partage :</p>
      <!-- EN: <p>Know someone who needs this? Share:</p> -->
      <div class="share-buttons">
        <a href="https://twitter.com/intent/tweet?text=[encoded_text]&url=[landing_page_url]" target="_blank" class="share-btn">Twitter</a>
        <a href="https://www.linkedin.com/sharing/share-offsite/?url=[landing_page_url]" target="_blank" class="share-btn">LinkedIn</a>
      </div>
    </section>

  </main>

  <!-- Footer identique à la landing page -->
  <footer class="footer footer--dark">
    <p class="copyright">&copy; [year] [brand_name]</p>
    <nav class="legal-links">
      <a href="#" onclick="document.getElementById('modal-privacy').showModal(); return false;">[Privacy Policy / Politique de confidentialité]</a>
      <span class="separator">|</span>
      <a href="#" onclick="document.getElementById('modal-cgv').showModal(); return false;">[Terms / CGV]</a>
    </nav>
  </footer>

  <!-- Mêmes modales légales que la landing page -->
  <!-- COPIER les <dialog> modal-privacy et modal-cgv de la landing page -->

</body>
</html>
```

#### CSS spécifique Thank You Page (ajouter au style existant)

```css
/* Thank You Page */
.thank-you-hero { text-align: center; padding: 4rem 2rem; }
.checkmark {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 80px; height: 80px;
  border-radius: 50%;
  background: var(--accent);
  color: #fff;
  font-size: 2.5rem;
  font-weight: 700;
  margin-bottom: 1.5rem;
}
.download-section {
  text-align: center;
  padding: 2rem;
  background: var(--surface);
  border-radius: var(--radius);
  margin: 2rem auto;
  max-width: 500px;
}
.download-button { margin-top: 1rem; }
.next-steps { max-width: 600px; margin: 3rem auto; }
.step {
  display: flex;
  gap: 1rem;
  margin-bottom: 1.5rem;
  align-items: flex-start;
}
.step-number {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 36px; height: 36px;
  border-radius: 50%;
  background: var(--primary);
  color: #fff;
  font-weight: 700;
  font-size: 0.9rem;
  flex-shrink: 0;
}
.share-section { text-align: center; padding: 2rem; }
.share-buttons { display: flex; gap: 0.75rem; justify-content: center; margin-top: 1rem; }
.share-btn {
  padding: 0.5rem 1.25rem;
  border: 1px solid var(--border);
  border-radius: 50px;
  text-decoration: none;
  color: var(--text);
  font-size: 0.85rem;
  transition: background 0.2s;
}
.share-btn:hover { background: var(--surface); }
```

### 10. SEO (dans `<head>`)

**Règle** : Seule la landing page est indexable. La thank you page a `noindex`.

```html
<!-- Landing Page <head> -->
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[seo_title]</title>
  <meta name="description" content="[seo_description]">
  <meta property="og:title" content="[seo_title]">
  <meta property="og:description" content="[seo_description]">
  <meta property="og:type" content="product">
  <meta property="og:image" content="[first product_image or empty]">
  <meta name="twitter:card" content="summary_large_image">
</head>

<!-- Thank You Page <head> -->
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="robots" content="noindex, nofollow">
  <title>Merci — [product_name]</title>
</head>
```

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[seo_title]</title>
  <meta name="description" content="[seo_description]">
  <meta property="og:title" content="[seo_title]">
  <meta property="og:description" content="[seo_description]">
  <meta property="og:type" content="product">
  <meta property="og:image" content="[first product_image or empty]">
  <meta name="twitter:card" content="summary_large_image">
</head>
```

---

## CTA Button Style (obligatoire)

```css
.cta-button {
  display: inline-block;
  background: var(--primary);
  color: #fff;
  padding: 1rem 2.5rem;
  border-radius: 50px;
  font-size: 1.1rem;
  font-weight: 700;
  text-decoration: none;
  transition: transform 0.2s, box-shadow 0.2s;
  box-shadow: 0 4px 14px rgba(0,0,0,0.15);
}
.cta-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0,0,0,0.2);
  background: var(--primary-dark);
}
```

---

## Quality Gates

| ID | Gate | Sévérité |
|----|------|----------|
| QG-01 | Aucun placeholder [TODO], [INSERT], Lorem ipsum | Critical |
| QG-02 | CSS embarqué — aucun fichier externe (sauf Google Fonts) | Critical |
| QG-03 | Tous les boutons CTA pointent vers la MÊME destination_url | Critical |
| QG-04 | Toutes les couleurs utilisent `var(--primary)`, `var(--accent)` — jamais de hex hardcodé | Critical |
| QG-05 | Page responsive et professionnelle sur mobile | Critical |
| QG-06 | Pas de texte placeholder — si contenu manquant, le générer depuis le contexte | Critical |
| QG-07 | Balises SEO complètes (title, description, OG tags) | High |
| QG-08 | HTML valide (tags fermés, structure correcte) | Critical |
| QG-09 | **Privacy Policy ET CGV** présentes en modales `<dialog>` dans le footer | Critical |
| QG-10 | **Les modales légales sont dans la MÊME langue** que la page (pas de mix FR/EN) | Critical |
| QG-11 | **Thank You Page** générée avec slug unique (`ty-[hash].html`) et `noindex` | Critical |
| QG-12 | Thank You Page utilise le **même design system** (couleurs, typo, style) | High |
| QG-13 | Attribut `lang` correct sur `<html>` (landing ET thank you) | High |
| QG-14 | Le style visuel choisi (minimaliste/bold/premium/warm) correctement appliqué aux 2 pages | High |
| QG-15 | Le slug de la thank you page est **non devinable** (hash aléatoire, pas le nom du produit) | High |
| QG-16 | Les modales se ferment avec le bouton ✕ et avec un clic en dehors (backdrop) | Medium |

---

## Error Handling

| Scénario | Action |
|----------|--------|
| Pas de nom de produit | Demander : "Quel produit veux-tu vendre ?" — ne pas continuer sans |
| Pas d'URL de destination | Demander : "Où le bouton CTA doit-il rediriger ?" — ne pas continuer sans |
| Pas de couleurs fournies | Proposer 3 palettes adaptées à la niche, demander de choisir |
| Pas d'images | Utiliser un hero gradient (`var(--primary)` → `var(--accent)`), pas de placeholder image |
| Pas de FAQ | Générer 5 FAQ pertinentes depuis le contexte produit |
| Pas de trust badges | Générer 3 badges génériques (ex: "Accès immédiat", "Satisfait ou remboursé", "Support email") |
| Produit gratuit (lead magnet) | Adapter : pas de prix affiché, CTA = "Télécharger gratuitement", ajouter un champ email si possible |
| business-profile.md absent | Continuer avec les réponses du context intake uniquement |
| Contenu trop court | Compléter avec du contenu généré depuis la description produit |
| Style "premium" demandé | Basculer tout le design en dark mode via les CSS custom properties |
| Langue non précisée | Détecter depuis business-profile.md ou demander. Par défaut : FR |
| Pas de lien de téléchargement pour la thank you page | Afficher la confirmation sans bouton de téléchargement |
| L'utilisateur veut une thank you page pour une landing existante | Lire la landing page HTML existante, extraire les couleurs et le contenu, générer la thank you page cohérente |
| Modales `<dialog>` non supportées par le navigateur | Ajouter un polyfill CSS minimal : `dialog[open] { display: block; }` pour les vieux navigateurs |

---

## Cross-Skill Integration

| Avant landing-page | Skill précédent | Quand |
|--------------------|-----------------|-------|
| Produit créé | `/dp-playbook-create` | Si on vend un ebook / playbook |
| Funnel conçu | `/dp-sales-funnel` | Pour connaître le rôle de la page dans le funnel |
| Profil business | `business-profile.md` | Pour les couleurs et infos de marque |

| Après landing-page | Skill suivant | Quand |
|--------------------|---------------|-------|
| Publicité Meta | `/dp-ad-angles-meta` | Pour générer les pubs qui pointent vers la page |
| Publicité Google | `/dp-ad-angles-google` | Pour les campagnes Search + Display |
| Séquence email | `/dp-email-sequence` | Pour envoyer du traffic email vers la page |
| Contenu social | `/dp-social-caption` `/dp-mediaplan` | Pour la promotion organique |
| Lead magnet | `/lead-magnet-create` | Si la page est un lead magnet, créer le contenu |
