# Changelog

## [2.0.0] — 2026-04-13

### Added
- 19 skills professionnels avec context intake guide, quality gates, et error handling
- 28 fichiers de reference avec exemples concrets (produit fictif FitPro Academy)
- `dp-business-profile` : configuration centrale lue par tous les skills
- `dp-playbook-create` : creation d'ebook 60+ pages avec compteur de pages et enrichissement
- `dp-export-pdf` : conversion HTML vers PDF (navigateur + Puppeteer + wkhtmltopdf)
- `dp-lead-magnet-create` : creation de lead magnets (checklist, mini-guide, cheat sheet)
- `dp-landing-page` : pages de vente responsive avec CSS custom properties
- `dp-blog-strategy` : strategie de contenu en topic clusters avec maillage interne
- `dp-blog-article` : articles SEO avec E-E-A-T, schema JSON-LD, et publication WordPress
- `dp-blog-publish` : publication WordPress via API REST avec mots de passe d'application
- `dp-email-sequence` : sequences email (welcome, launch, abandon, nurture, post-purchase)
- `dp-ad-angles-meta` : angles publicitaires Facebook/Instagram avec copies A/B
- `dp-ad-angles-google` : campagnes Google Search/YouTube/Display
- `dp-mediaplan` : calendrier editorial 4 semaines avec briefs detailles
- `dp-social-caption` : captions multi-plateformes avec CTA adaptes
- `dp-sales-funnel` : architecture de funnel avec conversion math
- `dp-competitor-analysis` : analyse concurrentielle avec matrice et SWOT
- `dp-copy-review` : audit de copy avec scoring 6 dimensions
- `dp-playbook-audit` : audit qualite avec seuils de publication
- `dp-playbook-section` : ecriture/reecriture de sections individuelles
- `dp-playbook-sync` : synchronisation FR ↔ EN
- Documentation complete : README, INSTALL, CONTRIBUTING, CLAUDE.md

### Technical
- CSS custom properties pour l'identite visuelle (--color-primary, --color-accent)
- Design system embarque dans chaque output HTML
- Styles d'impression (@media print) pour export PDF
- Schema JSON-LD (BlogPosting, FAQPage) dans les articles
- API REST WordPress avec authentification par mot de passe d'application
- UTM parameters generes automatiquement pour les campagnes ads

## [1.0.0] — 2026-03-01

### Initial Release
- 14 skills initiaux (sous le nom CoachGuido)
- Structure de base avec SKILL.md par skill
