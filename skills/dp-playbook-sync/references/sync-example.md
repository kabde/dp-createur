# Exemple de Synchronisation FR → EN

> Produit : **Le Playbook du Coach Fitness** / **The Fitness Coach Playbook** (47 EUR)
> Section synchronisée : `acquisition-clients`
> Date : 2026-04-13

---

## Section Originale FR (source de vérité)

```html
<section class="section" id="acquisition-clients">
  <h2>Acquisition de Clients</h2>

  <div class="value-block">
    <h4>Ce que tu vas apprendre</h4>
    <p>Comment attirer tes 10 premiers clients en coaching fitness sans dépenser un euro en publicité, en utilisant uniquement ta présence locale et les réseaux sociaux.</p>
  </div>

  <h3>Identifie ton terrain de chasse</h3>
  <p>Avant de poster quoi que ce soit, il faut savoir où ton client idéal passe son temps. Un coach spécialisé en remise en forme post-partum ne prospecte pas au même endroit qu'un coach CrossFit. Fais la liste de 5 lieux physiques et 3 communautés en ligne où ta cible se retrouve.</p>

  <h3>Le script de premier contact</h3>
  <p>Envoie ce message aux personnes qui correspondent à ton client idéal :</p>
  <p><em>"Salut [Prénom], j'ai vu que tu t'intéresses à [sujet fitness]. Je suis coach spécialisé en [ta spécialité] et j'aide les [profil] à [résultat concret]. Est-ce que tu aurais 15 min cette semaine pour qu'on en discute ? Aucune obligation."</em></p>

  <div class="tools-block">
    <h4>Outil recommandé</h4>
    <ul>
      <li><span class="tool-id">T6</span> - <strong>Apollo</strong> — Pour structurer ta prospection et suivre tes relances</li>
    </ul>
  </div>

  <div class="recap-block">
    <h4>À retenir</h4>
    <ul class="single-item">
      <li>Envoie 20 DMs par jour pendant 30 jours — la régularité bat le talent</li>
      <li>Personnalise chaque message — les copier-coller se repèrent à 3 km</li>
      <li>Ne vends pas dans le premier message — ouvre une conversation</li>
    </ul>
  </div>
</section>
```

---

## Section Traduite EN (résultat de la sync)

```html
<section class="section" id="acquisition-clients">
  <h2>Client Acquisition</h2>

  <div class="value-block">
    <h4>Value of this section</h4>
    <p>How to land your first 10 fitness coaching clients without spending a dime on ads, using only your local presence and social media.</p>
  </div>

  <h3>Identify your hunting ground</h3>
  <p>Before posting anything, you need to know where your ideal client spends their time. A coach specializing in postpartum fitness doesn't prospect in the same places as a CrossFit coach. List 5 physical locations and 3 online communities where your target audience hangs out.</p>

  <h3>The first contact script</h3>
  <p>Send this message to people who match your ideal client profile:</p>
  <p><em>"Hey [First Name], I noticed you're into [fitness topic]. I'm a coach specializing in [your specialty] and I help [profile] achieve [specific result]. Would you have 15 min this week for a quick chat? No strings attached."</em></p>

  <div class="tools-block">
    <h4>Primary tool</h4>
    <ul>
      <li><span class="tool-id">T6</span> - <strong>Apollo</strong> — To structure your outreach and track your follow-ups</li>
    </ul>
  </div>

  <div class="recap-block">
    <h4>Key takeaway</h4>
    <ul class="single-item">
      <li>Send 20 DMs per day for 30 days — consistency beats talent</li>
      <li>Personalize every message — copy-paste jobs are spotted from a mile away</li>
      <li>Don't sell in the first message — start a conversation</li>
    </ul>
  </div>
</section>
```

---

## Notes de Traduction

### Expressions délicates

| FR | EN | Note |
|----|----|------|
| "terrain de chasse" | "hunting ground" | Métaphore préservée -- naturelle en anglais business |
| "sans dépenser un euro" | "without spending a dime" | Adaptation culturelle -- "dime" est l'équivalent idiomatique US |
| "se repèrent à 3 km" | "spotted from a mile away" | Expression idiomatique adaptée (km → mile) |
| "Aucune obligation" | "No strings attached" | Expression plus naturelle en anglais que "No obligation" |
| "la régularité bat le talent" | "consistency beats talent" | Traduction directe -- fonctionne bien dans les deux langues |

### Adaptations culturelles

| Élément | FR | EN | Raison |
|---------|----|----|--------|
| Devise | EUR | $ | Le marché cible EN est US/international |
| Tutoiement | "tu" / "ton" | "you" / "your" | L'anglais n'a pas de distinction formel/informel |
| Exemples de niches | "remise en forme post-partum" | "postpartum fitness" | Terme standard en anglais |
| Ton du script | Chaleureux, tutoiement | Warm, direct | Même registre informel professionnel |

### Termes conservés dans la langue originale

| Terme | Raison |
|-------|--------|
| DM (Direct Message) | Utilisé tel quel dans les deux versions |
| Follow-up | Terme anglais utilisé aussi en FR dans le milieu coaching |
| CrossFit | Nom propre -- ne se traduit pas |
| Apollo | Nom d'outil -- ne se traduit pas |
| T6 | ID d'outil -- identique dans les deux versions |

### Éléments préservés exactement

- ID de section : `id="acquisition-clients"` (identique)
- Classes CSS : `value-block`, `tools-block`, `recap-block`, `single-item`, `tool-id` (identiques)
- Structure HTML : même nesting, même ordre des blocs
- Référence outil : `T6` Apollo (identique)
- Chiffres : 20 DMs, 30 jours, 15 min, 10 clients, 5 lieux, 3 communautés (identiques)

---

## Vérification Post-Sync

```
| Élément       | FR  | EN  | Match |
|---------------|-----|-----|-------|
| h2            | 1   | 1   | OK    |
| h3            | 2   | 2   | OK    |
| value-block   | 1   | 1   | OK    |
| tools-block   | 1   | 1   | OK    |
| recap-block   | 1   | 1   | OK    |
| tool-id refs  | 1   | 1   | OK    |
| Labels h4 FR  | 3   | 0   | OK    |
| Labels h4 EN  | 0   | 3   | OK    |
```
