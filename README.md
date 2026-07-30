# Crapette CARL

Crapette rapide (Speed/Spit) à 2 joueurs, jouable sur un seul écran (pass-and-play en temps réel, sans tour par tour).

## Déploiement sur GitHub Pages

1. Crée un nouveau dépôt GitHub (ex: `crapette-carl`).
2. Dépose tout le contenu de ce dossier à la racine du dépôt (`index.html`, `manifest.json`, `sw.js`, dossier `icons/`).
3. Va dans **Settings → Pages**, choisis la branche `main` et le dossier `/ (root)`, puis enregistre.
4. Ton appli sera en ligne à l'adresse `https://<ton-compte>.github.io/<nom-du-repo>/`.
5. Sur mobile, ouvre cette adresse puis "Ajouter à l'écran d'accueil" pour l'installer comme une app (PWA).

## Mise à jour

Si tu modifies `index.html`, `manifest.json` ou les icônes, pense à incrémenter `CACHE_NAME` dans `sw.js` (ex: `crapette-carl-v2`) pour forcer les appareils à récupérer la nouvelle version au lieu de servir l'ancienne version en cache.
