# Exemples de Couvertures — 5 Styles

> Produit : Le Playbook du Coach Fitness — FitPro Academy
> Couleurs : #059669 (primaire) / #10b981 (accent)

---

## Style 1 — Minimaliste

### HTML (extraits CSS clés)

```css
.cover {
  background: #ffffff;
  color: #111111;
}
.cover-title {
  font-weight: 800;
  font-size: 3.5rem;
  color: #111;
}
.cover-subtitle {
  font-weight: 300;
  color: #666;
}
.cover-decorator {
  background: #059669;
}
.cover-brand {
  color: #059669;
}
```

### Rendu ASCII

```
┌──────────────────────────┐
│                          │
│      FITPRO ACADEMY      │ ← Vert #059669, 14px, uppercase
│                          │
│                          │
│                          │
│        ━━━━━━━━          │ ← Ligne verte #059669
│                          │
│     LE PLAYBOOK          │ ← Noir #111, 72px, Extra-Bold
│     DU COACH             │
│     FITNESS              │
│                          │
│  10 clients en 30 jours  │ ← Gris #666, 24px, Light
│                          │
│        ━━━━━━            │
│                          │
│     Par Sarah Dupont     │ ← Noir #333, 20px
│                          │
│          FOND BLANC      │
└──────────────────────────┘
```

### Prompt Midjourney

```
Minimalist ebook cover design, "LE PLAYBOOK DU COACH FITNESS" in black 
extra-bold sans-serif typography on pure white background, thin emerald 
green #059669 horizontal accent line, clean spacious layout, subtitle 
in light gray, author name at bottom, no images, no photos, professional 
digital product, editorial aesthetic --ar 5:8 --v 6 --style raw --s 200
```

---

## Style 2 — Bold

### CSS clés

```css
.cover {
  background: #059669;
  color: #ffffff;
}
.cover-title {
  font-weight: 800;
  font-size: 4rem;
  color: #fff;
  text-shadow: 0 4px 20px rgba(0,0,0,0.3);
}
.cover-subtitle {
  font-weight: 600;
  color: rgba(255,255,255,0.9);
  background: rgba(0,0,0,0.15);
  padding: 8px 20px;
  border-radius: 50px;
}
```

### Rendu ASCII

```
┌──────────────────────────┐
│██████████████████████████│ ← Fond vert #059669 SATURÉ
│██                      ██│
│██    FITPRO ACADEMY    ██│ ← Blanc, bold
│██                      ██│
│██                      ██│
│██   LE PLAYBOOK        ██│ ← Blanc, 96px, EXTRA-BOLD
│██   DU COACH           ██│   Ombre portée
│██   FITNESS            ██│
│██                      ██│
│██  ╭─────────────────╮ ██│
│██  │ 10 clients en   │ ██│ ← Badge arrondi, fond semi-transparent
│██  │ 30 jours        │ ██│
│██  ╰─────────────────╯ ██│
│██                      ██│
│██    Par Sarah Dupont  ██│
│██                      ██│
│██████████████████████████│
└──────────────────────────┘
```

---

## Style 3 — Premium

### CSS clés

```css
.cover {
  background: #0a0a0a;
  color: #f5f5f5;
}
.cover-title {
  font-family: 'Playfair Display', serif;
  font-weight: 700;
  font-size: 3rem;
  color: #f5f5f5;
}
.cover-decorator {
  background: linear-gradient(90deg, #d4a853, #f0c878);
}
.cover-brand {
  color: #d4a853;
}
.cover-author {
  color: #d4a853;
}
```

### Rendu ASCII

```
┌──────────────────────────┐
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│ ← Fond NOIR #0a0a0a
│▓▓                      ▓▓│
│▓▓    FITPRO ACADEMY    ▓▓│ ← Or #d4a853, small caps
│▓▓                      ▓▓│
│▓▓    ════════════════   ▓▓│ ← Ligne dorée gradient
│▓▓                      ▓▓│
│▓▓    Le Playbook       ▓▓│ ← Blanc cassé, SERIF, élégant
│▓▓    du Coach          ▓▓│
│▓▓    Fitness           ▓▓│
│▓▓                      ▓▓│
│▓▓  10 clients en 30j   ▓▓│ ← Or 60% opacité, italic
│▓▓                      ▓▓│
│▓▓    ════════════       ▓▓│ ← Ligne dorée fine
│▓▓                      ▓▓│
│▓▓    Sarah Dupont      ▓▓│ ← Or #d4a853
│▓▓                      ▓▓│
│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│
└──────────────────────────┘
```

---

## Style 4 — Warm

### CSS clés

```css
.cover {
  background: #fef6ee;
  color: #3d2c1e;
}
.cover-title {
  font-weight: 800;
  color: #3d2c1e;
}
.cover-decorator {
  background: #e07a3a;
  border-radius: 4px;
}
.cover-subtitle {
  color: #8b6b4a;
}
```

### Rendu ASCII

```
┌──────────────────────────┐
│                          │ ← Fond CRÈME #fef6ee
│      FITPRO ACADEMY      │ ← Orange terre #e07a3a
│                          │
│        ━━━━━━━━          │ ← Orange arrondi
│                          │
│     LE PLAYBOOK          │ ← Brun foncé #3d2c1e, bold
│     DU COACH             │
│     FITNESS              │
│                          │
│  10 clients en 30 jours  │ ← Brun moyen #8b6b4a
│                          │
│        ━━━━━━            │
│     Par Sarah Dupont     │
│                          │  Ambiance CHALEUREUSE
└──────────────────────────┘
```

---

## Style 5 — Editorial

### CSS clés

```css
.cover {
  background: #fafaf8;
  color: #111;
}
.cover-title {
  font-family: 'Playfair Display', serif;
  font-weight: 700;
  font-size: 3.5rem;
  text-align: left;
}
.cover-subtitle {
  font-family: 'Inter', sans-serif;
  font-style: italic;
  font-weight: 300;
  text-align: left;
}
.cover-line-left {
  position: absolute;
  left: 60px;
  top: 80px;
  bottom: 80px;
  width: 3px;
  background: #059669;
}
```

### Rendu ASCII

```
┌──────────────────────────┐
│ │                        │ ← Ligne verticale verte à GAUCHE
│ │   FITPRO ACADEMY       │   Style MAGAZINE
│ │                        │
│ │                        │
│ │   Le Playbook          │ ← SERIF (Playfair), aligné gauche
│ │   du Coach             │
│ │   Fitness              │
│ │                        │
│ │   10 clients en        │ ← Sans-serif, italic, gris
│ │   30 jours — le guide  │
│ │   actionnable          │
│ │                        │
│ │   ─────────            │
│ │                        │
│ │   Sarah Dupont         │
│ │                        │   Fond blanc cassé #fafaf8
└──────────────────────────┘
```

---

## Mockup 3D — Exemple de Rendu

```
         ╱─────────────────╲
        ╱                   ╲
       ╱  FITPRO ACADEMY     ╲
      │                       │
      │    ━━━━━━━━━          │
      │                       │
      │   LE PLAYBOOK         │
      │   DU COACH            │
      │   FITNESS             │
      │                       │
      │   10 clients en       │
      │   30 jours            │
      │                       │
      │    ━━━━━━             │
      │   Par Sarah Dupont    │
      │                       │
       ╲                     ╱
        ╲                   ╱
    ████ ╲─────────────────╱
    ████   ← Tranche du livre (ombre)
```

Le mockup 3D est généré en pur CSS (perspective + rotateY + box-shadow). Aucune image nécessaire.
