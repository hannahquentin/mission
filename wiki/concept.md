# Projet de Site Web pour Collecte de Fonds Missionnaire

Ce projet est un site web simple conçu pour un couple missionnaire partant au Japon, permettant de collecter des promesses de dons. Le site est multilingue et comprend un formulaire de don, le tout hébergé gratuitement sur GitHub Pages.

## Objectifs du Site Web

1. **Collecte de fonds** : Permettre aux utilisateurs de faire une promesse de don (mensuel ou ponctuel) et de collecter les informations de contact.
2. **Interface multilingue** : Offrir des traductions en quatre langues (français, anglais, cantonais, et japonais).
3. **Facilité d’utilisation et de maintenance** : Hébergement gratuit sur GitHub Pages et intégration avec Google Sheets pour un suivi automatisé des promesses de dons.

## Technologies Utilisées

- **HTML, CSS et JavaScript** : Pour la structure, le style, et la gestion des fonctionnalités de la page.
- **GitHub Pages** : Pour l’hébergement gratuit du site web.
- **Google Sheets API** : Pour sauvegarder les informations des formulaires dans une feuille de calcul Google Sheets, afin de centraliser et structurer les données.

## Structure du Projet

### 1. Structure du Code HTML

Le code HTML est organisé en sections :

- **Header** : Affiche le titre de la mission et des boutons pour sélectionner la langue.
- **Section "À propos"** : Présente une courte description de la mission.
- **Formulaire de don** : Permet aux utilisateurs de remplir leurs informations de promesse de don.

### 2. Style CSS

Le style CSS est minimaliste pour offrir un design élégant et professionnel, avec des éléments réactifs adaptés aux différents écrans.

### 3. JavaScript pour Multilingue

La fonctionnalité de multilingue est gérée par JavaScript. Tous les textes des différentes langues sont stockés dans un objet JavaScript, et la fonction `setLanguage()` permet de changer la langue du contenu de la page en fonction du bouton cliqué.

### 4. Hébergement avec GitHub Pages

#### Étapes d’Hébergement

1. **Création d'un Repository GitHub** : Créer un nouveau repository sur GitHub pour héberger le code source.
2. **Ajout des Fichiers du Projet** : Uploader le fichier HTML complet, incluant le CSS et JavaScript.
3. **Activation de GitHub Pages** : Dans les paramètres du repository, activer GitHub Pages en sélectionnant la branche principale comme source d’hébergement.
4. **URL de Publication** : GitHub Pages génère une URL publique (généralement `https://votre-nom-utilisateur.github.io/votre-repository`) qui rend le site accessible à tous.

## Intégration avec Google Sheets

### Fonctionnalité

L'intégration avec Google Sheets permet de stocker les informations de chaque promesse de don dans une feuille de calcul, centralisant les données pour une gestion facile.

### Étapes d'Intégration

1. **Création d'une Feuille Google Sheets** : Créer un nouveau document Google Sheets pour stocker les informations de don.
2. **Configuration de Google Apps Script** :
   - Aller dans **Extensions > Apps Script** dans Google Sheets.
   - Ajouter le script suivant pour collecter les informations du formulaire :

   ```javascript
   function doPost(e) {
       const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Dons");
       const data = JSON.parse(e.postData.contents);
       sheet.appendRow([data.name, data.firstname, data.contact, data.amount, data.type, data.duration]);
       return ContentService.createTextOutput(JSON.stringify({ "result": "success" })).setMimeType(ContentService.MimeType.JSON);
   }
   ```

   Ce script enregistre les données envoyées dans une nouvelle ligne de la feuille de calcul, chaque fois qu'un formulaire est soumis.

3. **Déploiement en tant qu’Application Web** :
   - Dans Apps Script, aller dans **Déploiement > Déployer en tant qu’application web**.
   - Sélectionner **Qui a accès** sur **Tout le monde**, puis copier l'URL de l'application web qui servira de point de réception pour les données du formulaire.

4. **Intégration dans le Formulaire JavaScript** :
   - Dans le fichier HTML, modifier le JavaScript pour envoyer les données du formulaire à cette URL avec un `fetch()` en POST. Voici un exemple :

   ```javascript
   document.getElementById("donForm").addEventListener("submit", function(event) {
       event.preventDefault();
       const data = {
           name: document.getElementById("name").value,
           firstname: document.getElementById("firstname").value,
           contact: document.getElementById("contact").value,
           amount: document.getElementById("amount").value,
           type: document.getElementById("type").value,
           duration: document.getElementById("duration").value,
       };

       fetch("https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec", {
           method: "POST",
           body: JSON.stringify(data),
           headers: { "Content-Type": "application/json" },
       })
       .then(response => response.json())
       .then(data => alert("Merci pour votre promesse de don !"))
       .catch(error => alert("Erreur lors de l'envoi du formulaire, merci de réessayer."));
   });
   ```

   - **Remarque** : Remplacer `"YOUR_SCRIPT_ID"` par l'ID de l'application déployée dans Google Apps Script.

## Conclusion

Ce site web, hébergé sur GitHub Pages avec une intégration de Google Sheets, fournit une solution complète et gratuite pour collecter les promesses de dons en plusieurs langues. Ce projet est évolutif et peut être modifié facilement pour ajouter des fonctionnalités ou des langues supplémentaires selon les besoins futurs.