# ROCKNITE-LOGIN: Guide d'intégration

**ROCKNITE-LOGIN** est un système d'authentification léger et universel conçu pour s'adapter à n'importe quelle plateforme. Il repose sur des requêtes HTTP POST, permettant une intégration simple et rapide.

---

## Fonctionnalités principales

- **Inscription utilisateur** : Création de comptes avec email, mot de passe et informations supplémentaires.
- **Connexion utilisateur** : Authentification avec gestion de sessions sécurisées (par exemple, via des tokens).
- **Pages protégées** : Accès restreint avec vérification des jetons d'authentification.
- **Extensibilité** : Intégrable à n'importe quelle plateforme (Android, Web, etc.).

---

## Utilisation : requêtes POST

Pour utiliser **ROCKNITE-LOGIN**, envoyez des requêtes HTTP POST aux endpoints suivants :

1. **Inscription**
   - **Méthode** : POST  
   - **URL** : `/inscription`  
   - **Corps** : 
     ```json
     {
       "email": "exemple@mail.com",
       "mot_de_passe": "votre_mot_de_passe",
       "nom": "Votre Nom"
     }
     ```
   - **Réponse** : 
     ```json
     {
       "message": "Utilisateur créé avec succès."
     }
     ```

2. **Connexion**
   - **Méthode** : POST  
   - **URL** : `/connexion`  
   - **Corps** :
     ```json
     {
       "email": "exemple@mail.com",
       "mot_de_passe": "votre_mot_de_passe"
     }
     ```
   - **Réponse** :
     ```json
     {
       "token": "votre_jeton_d_authentification"
     }
     ```

3. **Page protégée**
   - **Méthode** : GET  
   - **URL** : `/protege`  
   - **En-tête** :  
     ```
     Authorization: Bearer <votre_jeton_d_authentification>
     ```
   - **Réponse** :
     ```json
     {
       "message": "Message personnalisé."
     }
     ```

---

## Exemples d'intégration par plateforme

Voici des exemples d'utilisation de **ROCKNITE-LOGIN** sur différentes plateformes :

### 1. **HTML (site web)**  
   - Rendez-vous sur la [page d'exemple HTML](docs/html-example.md) pour voir un formulaire simple.

### 2. **Android (Java/Kotlin)**  
   - Consultez l'exemple Android [ici](docs/android-example.md) pour intégrer une activité d'authentification.

### 3. **Python (API client)**  
   - Accédez à l'exemple Python [ici](docs/python-example.md) pour tester directement les endpoints avec des requêtes HTTP.

### 4. **React ou Vue.js**  
   - Découvrez comment l'intégrer dans une application React ou Vue [ici](docs/react-example.md).

> **Note** : Les pages liées contiennent des exemples pratiques adaptés à chaque technologie.

---

## Contact & Contribuer

Pour toute question ou contribution, contactez-nous via notre page GitHub :  
👉 [GitHub Repository](https://github.com/<votre-utilisateur>/rocknite-login)
