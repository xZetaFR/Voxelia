<div align="center">

# Voxelia

**Un jeu voxel multijoueur, dans le navigateur.**
Moteur de rendu 3D, génération de terrain, physique, multijoueur temps réel et serveur dédié — un petit Minecraft-like complet, jouable sans rien installer.

![Status](https://img.shields.io/badge/status-beta-orange)
![Platform](https://img.shields.io/badge/platform-web-blue)

</div>

---

## Le principe

Voxelia est un jeu de type voxel (blocs, monde généré, construction/destruction) qui tourne entièrement dans un navigateur web, avec un vrai mode multijoueur temps réel contre un serveur dédié, et un mode solo autonome. L'objectif : retrouver la sensation d'un jeu de blocs façon Minecraft, sans client lourd à installer — une page web suffit.

## Fonctionnalités

- **Rendu 3D temps réel** dans le navigateur, moteur voxel dédié (chunks, occlusion, éclairage).
- **Multijoueur** : protocole réseau binaire compact, synchronisation des joueurs et du monde en direct.
- **Mode solo** autonome, sans serveur.
- **Génération procédurale de terrain** (bruit de Simplex) et monde persistant.
- **Modèles d'entités personnalisés**, support d'un format compatible avec les modèles JSON d'Optifine.
- **Textures personnalisables**, chargeables depuis une URL externe.
- **Support manette et écran tactile**, en plus du clavier/souris.
- **Liste de serveurs**, chat avec couleurs et Markdown, écran de statut contrôlé par le serveur.

## Stack

`TypeScript` · `BabylonJS` · `noa-engine` · `Protobuf` · `WebSocket` · `Webpack`

---

## Architecture technique

<details>
<summary><strong>Détails pour les développeurs</strong></summary>

### Organisation

```
src/
  gui/
    menu/         écrans de menu (accueil, options, liste de serveurs)
    ingame/        interface en jeu (inventaire, tchat, barre de vie)
    parts/          composants d'UI réutilisables
  lib/
    gameplay/        logique de jeu (blocs, entités, inventaire)
    player/           contrôleur du joueur, caméra, interactions
    singleplayer/     orchestration du mode solo
    helpers/          utilitaires partagés
  protocolWrappers/   compatibilité avec les anciennes versions du protocole réseau
```

### Rendu et moteur de jeu

Le rendu s'appuie sur **BabylonJS** pour la 3D et **noa-engine** pour la partie voxel (gestion des chunks, maillage, collisions, boucle de jeu). Le monde est découpé en chunks générés et maillés à la volée ; la génération procédurale utilise du bruit de Simplex (open-simplex-noise) pour le terrain.

### Réseau

Le protocole de communication client/serveur est sérialisé en **Protobuf** (binaire, compact), transporté sur WebSocket — un choix fait après une première version utilisant Socket.io, pour réduire la bande passante et la latence. Le serveur dédié (`voxelsrv-server`) et le client partagent une définition de protocole commune (`voxelsrv-protocol`) versionnée, avec une couche de compatibilité (`protocolWrappers/`) pour les clients restés sur une ancienne version.

### Mode solo

Le mode singleplayer fait tourner le serveur de jeu directement dans le navigateur (orchestré depuis `lib/singleplayer/`), pour jouer sans connexion réseau ni serveur externe, en réutilisant la même logique de jeu que le mode multijoueur.

### Build

Bundlé avec Webpack (`npm run build` pour la production, `webpack-dev-server` en développement). Certaines dépendances historiques du launcher Node (comme `readline`) sont remplacées par des stubs (`fake_modules/`) pour permettre au bundle de tourner dans un navigateur sans ces APIs.

</details>

---

<sub>Documentation de portfolio.</sub>
