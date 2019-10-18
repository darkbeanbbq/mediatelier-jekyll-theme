---
layout: default
titre: Le markdown, le HTML et le CSS
---

# Grand titre

## Sous-titre ou titre de seconde importance

Paragraphe et texte normal.

Le fichier markdown `.md` est traduit en code HTML dans un fichier `.html`.

Ensuite grace a une fiche de style (code dans fichier `.css`), on donne des regles graphiques pour habiller le texte et faire la mise en page.

Voici un de code HTML :

```html
<h3>Le markdown c'est du HTML lisible</h3>

<p>Ecrire en markdown revient a faire du HTML sauf que c'est plus lisible pour un humain. Preceder un titre de trois ### le convertira en balises "h3".</p>
```

Et voici un exemple de code CSS :

```css
h3 {
  font-size: 16px;
}

p {
  color: SlateGrey;
}
```

Les deux combines ca donne ca :

<h3 style='font-size: 16px;'>Le markdown c'est du HTML lisible</h3>

<p style='color: SlateGrey;'>Ecrire en markdown revient a faire du HTML sauf que c'est plus lisible pour un humain. Preceder un titre de trois ### le convertira en balises "h3".</p>
