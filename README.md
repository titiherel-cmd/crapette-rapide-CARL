# Crapette CARL

Crapette rapide (Speed/Spit) à 2 joueurs.
- **Mode local** : les deux joueurs sur le même téléphone.
- **Mode à distance** : chacun sur son téléphone, synchronisé en temps réel via Firebase.

Le jeu tient dans 3 fichiers seulement (`index.html`, `manifest.json`, `sw.js`) : les icônes sont
intégrées directement en base64 dans le code, il n'y a donc pas de dossier `icons/` séparé à
uploader (ça évitait un problème classique d'upload GitHub qui cassait l'installation en PWA).

## Déploiement sur GitHub Pages

1. Crée un nouveau dépôt GitHub (ex: `crapette-carl`).
2. Dépose les 3 fichiers (`index.html`, `manifest.json`, `sw.js`) à la racine du dépôt.
3. Va dans **Settings → Pages**, choisis la branche `main` et le dossier `/ (root)`, puis enregistre.
4. Ton appli sera en ligne à `https://<ton-compte>.github.io/<nom-du-repo>/`.
5. Sur Android/Chrome : ouvre le lien, un bandeau ou le menu ⋮ propose "Installer l'application".
   Sur iPhone/Safari : bouton Partager → "Sur l'écran d'accueil".

Le mode local fonctionne directement, sans rien configurer.

## Activer le mode à distance (2 téléphones) — Firebase

1. Va sur [console.firebase.google.com](https://console.firebase.google.com) et crée un projet (ou réutilise celui de Quizz CARL).
2. Dans le projet : **Build → Realtime Database → Créer une base de données** (choisis une région proche, ex. `europe-west1`), en mode **test** pour commencer.
3. Dans **Règles** de la Realtime Database, mets :
   ```json
   {
     "rules": {
       "rooms": {
         ".read": true,
         ".write": true
       }
     }
   }
   ```
   ⚠️ Ces règles sont ouvertes (aucune authentification) — suffisant pour un jeu entre amis avec un code de partie, mais à garder en tête si tu réutilises ce projet Firebase pour autre chose.
4. Dans **Paramètres du projet → Tes applications → Ajouter une application → Web**, récupère l'objet de config (`apiKey`, `authDomain`, `databaseURL`, etc.).
5. Ouvre `index.html`, cherche le bloc `firebaseConfig` (juste après les balises `<script>` Firebase) et remplace les valeurs `YOUR_...` par les tiennes.
6. Redéploie sur GitHub Pages (commit + push). Le bandeau d'avertissement "mode à distance non configuré" disparaît automatiquement une fois la config renseignée.

### Comment ça marche
- "Créer une partie" génère un code à 4 caractères et attend qu'un 2e joueur le rejoigne.
- "Rejoindre une partie" avec ce code connecte le 2e téléphone à la même partie.
- Chaque coup est envoyé à Firebase Realtime Database sous forme de transaction (le serveur valide le coup avant de le diffuser aux deux téléphones), donc le plateau reste toujours identique des deux côtés.
- Chaque joueur ne peut jouer que ses propres colonnes ; les deux piles centrales sont accessibles à tout moment, sans tour par tour.

## Mise à jour

Si tu modifies `index.html` ou `manifest.json`, incrémente `CACHE_NAME` dans `sw.js` (ex: `crapette-carl-v5`) pour forcer les appareils à récupérer la nouvelle version au lieu de servir l'ancienne version en cache.
