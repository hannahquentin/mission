# Hébergement sur GitHub Pages

## 1. Création du Repository

1. Aller sur [GitHub](https://github.com) et se connecter
2. Cliquer sur le bouton "+" en haut à droite puis "New repository"
3. Nommer le repository : `mission-japon` (ou autre nom souhaité)
4. Cocher "Add a README file"
5. Cliquer sur "Create repository"

## 2. Upload des Fichiers

### Via l'Interface Web
1. Dans votre repository, cliquer sur "Add file" > "Upload files"
2. Glisser-déposer ou sélectionner les fichiers :
   - `index.html`
   - `image01.jpg` à `image05.jpg`
3. Ajouter un message de commit : "Initial upload"
4. Cliquer sur "Commit changes"

### Via Git (Alternative)
```bash
git clone https://github.com/votre-username/mission-japon.git
cd mission-japon
# Copier les fichiers dans le dossier
git add .
git commit -m "Initial upload"
git push origin main
```

## 3. Activation de GitHub Pages

1. Aller dans "Settings" du repository
2. Dans le menu latéral, cliquer sur "Pages"
3. Dans "Source", sélectionner :
   - Branch: `main`
   - Folder: `/ (root)`
4. Cliquer sur "Save"

## 4. Accès au Site

- GitHub va générer une URL du type : `https://votre-username.github.io/mission-japon`
- La première publication peut prendre quelques minutes
- Le site sera automatiquement mis à jour à chaque modification du repository

## 5. Modifications et Mises à Jour

### Via l'Interface Web
1. Naviguer jusqu'au fichier à modifier
2. Cliquer sur l'icône crayon (Edit)
3. Faire les modifications
4. Commiter les changements

### Via Git
```bash
git pull  # Récupérer les dernières modifications
# Modifier les fichiers
git add .
git commit -m "Description des modifications"
git push origin main
```

## 6. Nom de Domaine Personnalisé (Optionnel)

1. Dans les paramètres GitHub Pages
2. Section "Custom domain"
3. Entrer votre nom de domaine
4. Configurer les DNS chez votre registrar :
   ```
   Type: CNAME
   Name: www
   Value: votre-username.github.io
   ```

## 7. Bonnes Pratiques

- Toujours tester les modifications localement avant de les publier
- Utiliser des messages de commit descriptifs
- Maintenir une structure de fichiers claire
- Optimiser les images pour le web
- Vérifier régulièrement que le site fonctionne correctement

## 8. Résolution des Problèmes Courants

### Le Site ne se Met pas à Jour
- Vérifier que les fichiers sont dans la bonne branche
- Attendre quelques minutes pour la propagation
- Vider le cache du navigateur

### Images non Affichées
- Vérifier les chemins relatifs
- S'assurer que les noms de fichiers correspondent exactement
- Vérifier la casse des extensions (.jpg vs .JPG)

### Problèmes de Style
- Valider le HTML et CSS
- Vérifier les chemins des ressources
- Tester sur différents navigateurs