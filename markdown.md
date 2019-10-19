---
layout: responsive
titre: Le markdown, le HTML et le CSS
---

# Markdown, HTML et CSS (h1)

## Tests de rendu : markdown et CSS (h2)

Paragraphe et texte normal.

Le fichier markdown `.md` est traduit en code HTML dans un fichier `.html`.

Ensuite grace a une fiche de style (code dans fichier `.css`), on donne des regles graphiques pour habiller le texte et faire la mise en page.

Voici un de code HTML :

```html
<h3>Le markdown c'est du HTML lisible</h3>

<p>Écrire en markdown revient à faire du HTML sauf que c'est plus lisible pour un humain.</p>
```

Pour arriver au même résultât, en markdown il suffit d'écrire :

```markdown
### Le markdown c'est du HTML lisible
Écrire en markdown revient à faire du HTML sauf que c'est plus lisible pour un humain.
```

Et voici un exemple de code CSS :

```css
h3 {
  font-size: 24px;
}

p {
  color: red;
}
```

Les deux combines ça donne ça :

<h3 style='font-size: 24px;'>Le markdown c'est du HTML lisible</h3>

<p style='color: red;'>Écrire en markdown revient à faire du HTML sauf que c'est plus lisible pour un humain.</p>

> "Le markdown c'est super pratique pour écrire du contenu pour le web rapidement et sans logiciel de traitement de texte particulier !"

Voici du markdown et son résultât côte à côte :

![markdown preview atom](images/markdown-preview.png)

---

# Très grand titre (h1)
Référence pour taille de texte normal.

## Sous-titre (h2)
Référence pour taille de texte normal.

### Sous-titre (h3)
Référence pour taille de texte normal.

#### Sous-partie (h4)
Référence pour taille de texte normal.

##### Petit partie (h5)
Référence pour taille de texte normal.

###### Toute petite partie (h6)
Référence pour taille de texte normal.


| Tableau           | type d'alignement | chiffre  |
|:----------------- |:-----------------:| --------:|
| colonne numéro 3  | à droite          |    $1600 |
| colonne 2         | centré            |      $12 |
| première colonne  | à gauche          |       $1 |
