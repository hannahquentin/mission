# Site Web Mission Japon

Ce site web permet de présenter votre projet missionnaire au Japon et de récolter des promesses de dons. Voici un guide pour vous aider à faire des modifications si nécessaire.

## Structure du Site

Le site est composé de 4 pages, une pour chaque langue :
- `fr.html` - Version française
- `en.html` - Version anglaise
- `jp.html` - Version japonaise
- `cn.html` - Version chinoise

Tous les styles sont dans le fichier `styles.css`.
Les images sont directement dans le dossier principal.

## Comment Faire des Modifications

### 1. Modifier les Couleurs

Les couleurs du site sont définies au début du fichier `styles.css` sous forme de variables :

```css
:root {
    --color-primary: #34495e;      /* Couleur principale */
    --color-primary-dark: #2c3e50; /* Version foncée de la couleur principale */
    --color-accent: #e74c3c;       /* Couleur d'accent (boutons, etc.) */
    --color-accent-dark: #c0392b;  /* Version foncée de la couleur d'accent */
    --color-text: #555;            /* Couleur du texte */
    --color-background: #f7f7f7;   /* Couleur de fond */
    --color-white: #fff;           /* Blanc */
    --color-border: #bdc3c7;       /* Couleur des bordures */
}
```

Pour changer une couleur, modifiez simplement le code hexadécimal correspondant.

### 2. Modifier le Contenu

1. Allez sur [GitHub](https://github.com) et connectez-vous
2. Naviguez jusqu'au fichier que vous souhaitez modifier
3. Cliquez sur l'icône crayon (Edit) en haut à droite
4. Faites vos modifications
5. En bas de la page, ajoutez une description de vos changements
6. Cliquez sur "Commit changes"

Les modifications seront visibles sur le site après quelques minutes.

### 3. Ajouter/Remplacer des Images

1. Sur GitHub, allez dans le dossier principal du site
2. Cliquez sur "Add file" > "Upload files"
3. Glissez-déposez vos nouvelles images
4. Ajoutez un message de commit et validez

⚠️ Important : 
- Gardez les mêmes noms de fichiers si vous remplacez des images existantes
- Optimisez vos images pour le web (taille raisonnable, format .jpg ou .png)
- Vérifiez que les noms de fichiers correspondent exactement à ceux utilisés dans le HTML

### 4. Utiliser ChatGPT pour les Modifications

Vous pouvez utiliser ChatGPT pour vous aider à faire des modifications. Voici comment :

1. Copiez le code que vous voulez modifier
2. Collez-le dans ChatGPT
3. Expliquez clairement ce que vous voulez changer
4. Vérifiez que le code retourné est complet et cohérent
5. Testez les modifications avant de les mettre en ligne

## Système de Promesses de Dons

Les promesses de dons sont gérées via Google Sheets. La configuration est expliquée en détail dans `/wiki/googlesheets.md`.

## Hébergement

Le site est hébergé sur GitHub Pages. Les détails de l'hébergement sont disponibles dans `/wiki/hebergement.md`.

## Besoin d'Aide ?

Si vous avez besoin d'aide pour des modifications plus complexes, n'hésitez pas à contacter le développeur :
- Email : contact@neuchatech.ch
- Site web : www.neuchatech.ch

---

Développé avec ❤️ par Neuchatech