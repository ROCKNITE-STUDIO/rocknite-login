## Documentation pour page WEB

### Objectif
Cet docs vous explique comment gèrer :
1. L'inscription des utilisateurs.
2. La connexion avec stockage d'un token JWT.
3. L'accès à une page protégée grâce au token JWT.
4. La déconnexion de l'utilisateur.

---

### Structure HTML requise

- **Formulaire d'inscription :**
  ```html
  <form id="register-form">
      <input type="email" id="email" placeholder="Email" required />
      <input type="password" id="mot_de_passe" placeholder="Mot de passe" required />
      <input type="text" id="nom" placeholder="Nom" required />
      <button type="submit">S'inscrire</button>
  </form>
  ```

- **Formulaire de connexion :**
  ```html
  <form id="login-form">
      <input type="email" id="email" placeholder="Email" required />
      <input type="password" id="mot_de_passe" placeholder="Mot de passe" required />
      <button type="submit">Se connecter</button>
  </form>
  ```

- **Page protégée :**
  ```html
  <p id="message"></p>
  <button id="logout-button">Déconnexion</button>
  ```

---

### Fonctionnalités

#### 1. **Inscription des utilisateurs**
- **Action :** Lors de la soumission du formulaire, les données saisies (email, mot de passe et nom) sont envoyées via une requête `POST` à l'API `/inscription`.
- **Réponse attendue :**
  - Succès : Affiche un message et redirige vers `/login/login.html`.
  - Échec : Affiche une alerte avec un message d'erreur.

- **Code associé :**
  ```javascript
  registerForm.addEventListener('submit', function(event) {
      event.preventDefault();

      fetch('https://institutis.serveo.net/inscription', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ email, mot_de_passe, nom })
      })
      .then(response => response.json())
      .then(data => {
          alert(data.message);
          if (data.message === 'Utilisateur créé avec succès.') {
              window.location.href = '/login/login.html';
          }
      })
      .catch(error => alert('Erreur lors de l\'inscription.'));
  });
  ```

---

#### 2. **Connexion des utilisateurs**
- **Action :** Envoie une requête `POST` avec email et mot de passe à l'API `/connexion`.
- **Réponse attendue :**
  - Succès : Stocke le token JWT dans le `localStorage` et redirige vers `/login/protected.html`.
  - Échec : Affiche une alerte d'erreur.

- **Code associé :**
  ```javascript
  loginForm.addEventListener('submit', function(event) {
      event.preventDefault();

      fetch('https://institutis.serveo.net/connexion', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ email, mot_de_passe })
      })
      .then(response => response.json())
      .then(data => {
          if (data.token) {
              localStorage.setItem('token', data.token);
              window.location.href = '/login/protected.html';
          } else {
              alert('Identifiants incorrects.');
          }
      })
      .catch(error => alert('Erreur lors de la connexion.'));
  });
  ```

---

#### 3. **Accès à une page protégée**
- **Action :** Envoie une requête `GET` à l'API `/protege` avec le token JWT dans les headers.
- **Réponse attendue :**
  - Succès : Affiche le message reçu sur la page.
  - Échec : Supprime le token JWT et affiche un message d'erreur.

- **Code associé :**
  ```javascript
  const token = localStorage.getItem('token');

  fetch('https://institutis.serveo.net/protege', {
      method: 'GET',
      headers: { 'Authorization': `Bearer ${token}` }
  })
  .then(response => response.json())
  .then(data => {
      messageElement.textContent = data.message || 'Erreur d\'authentification.';
  })
  .catch(error => {
      localStorage.removeItem('token');
      messageElement.textContent = 'Erreur lors de l\'accès.';
  });
  ```

---

#### 4. **Déconnexion**
- **Action :** Supprime le token JWT du `localStorage` et redirige vers la page de connexion.

- **Code associé :**
  ```javascript
  logoutButton.addEventListener('click', function() {
      localStorage.removeItem('token');
      window.location.href = '/login/login.html';
  });
  ```

---

### API attendues

#### 1. **POST `/inscription`**
- **Entrée :**
  ```json
  {
      "email": "utilisateur@example.com",
      "mot_de_passe": "123456",
      "nom": "NomUtilisateur"
  }
  ```
- **Sortie (succès) :**
  ```json
  { "message": "Utilisateur créé avec succès." }
  ```
- **Sortie (échec) :**
  ```json
  { "message": "Erreur lors de la création de l'utilisateur." }
  ```

#### 2. **POST `/connexion`**
- **Entrée :**
  ```json
  {
      "email": "utilisateur@example.com",
      "mot_de_passe": "123456"
  }
  ```
- **Sortie (succès) :**
  ```json
  { "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." }
  ```
- **Sortie (échec) :**
  ```json
  { "message": "Identifiants incorrects." }
  ```

#### 3. **GET `/protege`**
- **Headers :**
  ```
  Authorization: Bearer <token>
  ```
- **Sortie (succès) :**
  ```json
  { "message": "Bienvenue, utilisateur." }
  ```
- **Sortie (échec) :**
  ```json
  { "message": "Erreur d'authentification." }
  ```

exemple complet d'un script js : 
