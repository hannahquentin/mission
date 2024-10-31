# Intégration avec Google Sheets

## 1. Création de la Feuille de Calcul

1. Aller sur [Google Sheets](https://sheets.google.com)
2. Créer une nouvelle feuille
3. La nommer "Promesses de Dons"
4. Créer les en-têtes suivants dans la première ligne :
   - Nom
   - Prénom
   - Contact
   - Montant
   - Type
   - Durée
   - Date

## 2. Configuration de Google Apps Script

1. Dans Google Sheets, aller dans "Extensions" > "Apps Script"
2. Remplacer le code par :

```javascript
function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    const data = JSON.parse(e.postData.contents);
    
    // Sanitize and validate inputs
    const sanitizedData = {
      name: sanitizeInput(data.name),
      firstname: sanitizeInput(data.firstname),
      contact: sanitizeInput(data.contact),
      amount: validateAmount(data.amount),
      type: validateType(data.type),
      duration: validateDuration(data.type, data.duration),
      date: new Date().toLocaleDateString()
    };

    // Verify all required fields are present and valid
    if (!isValidData(sanitizedData)) {
      throw new Error('Invalid data format');
    }

    // Add row with sanitized data
    sheet.appendRow([
      sanitizedData.name,
      sanitizedData.firstname,
      sanitizedData.contact,
      sanitizedData.amount,
      sanitizedData.type,
      sanitizedData.duration,
      sanitizedData.date
    ]);

    return ContentService
      .createTextOutput(JSON.stringify({ "result": "success" }))
      .setMimeType(ContentService.MimeType.JSON);

  } catch (error) {
    console.error('Error processing request:', error);
    return ContentService
      .createTextOutput(JSON.stringify({ "result": "error", "message": "Invalid data" }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

// Sanitize string inputs
function sanitizeInput(input) {
  if (typeof input !== 'string') return '';
  
  // Remove any potential formula triggers
  if (input.startsWith('=') || input.startsWith('+') || input.startsWith('-') || input.startsWith('@')) {
    input = `'${input}`;
  }
  
  // Basic XSS prevention
  return input
    .trim()
    .replace(/[<>]/g, '') // Remove < and >
    .slice(0, 500);  // Limit length
}

// Validate amount
function validateAmount(amount) {
  const num = Number(amount);
  if (isNaN(num) || num <= 0 || num > 1000000) {
    throw new Error('Invalid amount');
  }
  return num;
}

// Validate type
function validateType(type) {
  const validTypes = ['mensuel', 'ponctuel'];
  if (!validTypes.includes(type)) {
    throw new Error('Invalid type');
  }
  return type;
}

// Validate duration
function validateDuration(type, duration) {
  if (type === 'ponctuel') return '';
  
  const validDurations = ['3mois', '6mois', '1an', '2ans'];
  if (!validDurations.includes(duration)) {
    throw new Error('Invalid duration');
  }
  return duration;
}

// Verify all required fields are present and valid
function isValidData(data) {
  return data.name && 
         data.firstname && 
         data.contact && 
         data.amount && 
         data.type;
}
```

## 3. Déploiement du Script

1. Cliquer sur "Déployer" > "Nouveau déploiement"
2. Cliquer sur "Nouveau déploiement"
3. Choisir "Application web"
4. Configurer :
   - Description : "Réception des promesses de dons"
   - Exécuter en tant que : "Moi"
   - Qui a accès : "Tout le monde"
5. Cliquer sur "Déployer"
6. Autoriser les permissions demandées
7. Copier l'URL de déploiement

## 4. Intégration dans le Site Web

1. Dans `index.html`, remplacer l'URL du fetch par l'URL de déploiement :

```javascript
fetch("URL_COPIÉE_DEPUIS_APPS_SCRIPT", {
    method: "POST",
    mode: "no-cors",
    body: JSON.stringify(data),
    headers: { "Content-Type": "application/json" },
})
```

## 5. Sécurité et Limitations

### Protections Implémentées
- Validation des types de données
- Sanitization des entrées textuelles
- Protection contre l'injection de formules
- Limites sur la longueur des entrées
- Validation des montants
- Protection XSS basique

### Quotas Google Apps Script
- 20,000 requêtes par jour
- 6 minutes de temps d'exécution maximum
- 50 MB de stockage

### Bonnes Pratiques
- Valider les données côté serveur (implémenté)
- Gérer les erreurs gracieusement
- Nettoyer les données sensibles
- Sauvegarder régulièrement la feuille

## 6. Personnalisation

### Formatage Automatique
```javascript
function onEdit(e) {
  const sheet = e.source.getActiveSheet();
  
  // Formater les montants
  const amountColumn = 4; // Colonne D
  sheet.getRange(2, amountColumn, sheet.getLastRow()-1)
       .setNumberFormat("#,##0.00 CHF");
  
  // Trier par date
  const range = sheet.getRange("A2:G" + sheet.getLastRow());
  range.sort({column: 7, ascending: false}); // Trier par date décroissante
}
```

### Notifications par Email
```javascript
function sendNotification(data) {
  const email = "votre@email.com";
  const subject = "Nouvelle promesse de don";
  const message = `
    Nouveau don reçu :
    Nom : ${data.name} ${data.firstname}
    Montant : ${data.amount} CHF
    Type : ${data.type}
    Durée : ${data.duration}
  `;
  
  MailApp.sendEmail(email, subject, message);
}
```

## 7. Dépannage

### Erreurs Courantes

1. **CORS Error**
   - Vérifier les paramètres de déploiement
   - S'assurer que l'accès est configuré sur "Tout le monde"

2. **Unauthorized**
   - Revérifier les autorisations
   - Redéployer le script

3. **Quota Exceeded**
   - Surveiller l'utilisation quotidienne
   - Implémenter une mise en cache si nécessaire

### Vérification des Logs
1. Dans Apps Script, aller dans "Vue" > "Journaux d'exécution"
2. Ajouter des logs dans le code :
```javascript
console.log("Données reçues :", e.postData.contents);
```

## 8. Maintenance

### Sauvegardes
- Exporter régulièrement la feuille en CSV
- Conserver des copies de sauvegarde du script

### Monitoring
- Vérifier régulièrement les logs
- Surveiller l'utilisation des quotas
- Tester périodiquement le formulaire