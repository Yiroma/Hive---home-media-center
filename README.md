# Hive - Home Media Center

![Hive Logo](./client/public/hive.jpg)

## À propos du projet

**HIVE** est une application web de home media center permettant de centraliser vos collections de médias et de partager vos critiques avec d'autres utilisateurs.

Vous pouvez gérer vos collections de :

- Films
- Jeux Vidéo
- Livres
- Musique

L'application est développée "mobile first" : l'interface est responsive et la navigation dans le catalogue permet de filtrer et trier selon vos besoins.

## Fonctionnalités

- Catalogue de films, jeux vidéo, livres et musiques
- Page d'accueil avec carrousels des derniers ajouts
- Recherche par titre dans le catalogue
- Filtres et tri des contenus
- Fiche de détail pour les films et les jeux (catégories, récompenses, acteurs/plateformes)
- Ajout de nouveaux médias (films, jeux) via un formulaire dédié
- Système de critiques et de notes (reviews) pour les films
- Inscription à la newsletter
- Page profil utilisateur

## Captures d'écran

> ![Aperçu de l'interface mobile](./client/public/screenshot-mobile.webp)
> _Aperçu de l'interface mobile_
>
> ![Aperçu de l'interface desktop](./client/public/screenshot-desktop.webp)
> _Aperçu de l'interface desktop_

## Architecture

Le projet est organisé en monorepo avec deux dossiers principaux :

```
Hive---home-media-center/
├── client/          # Application React (frontend)
├── server/          # Serveur Apollo GraphQL (backend)
└── package.json     # Scripts racine (lancement simultané)
```

## Technologies utilisées

### Frontend (`client/`)

| Technologie | Version | Rôle |
|---|---|---|
| React | 19 | Bibliothèque UI |
| TypeScript | ~5.7 | Typage statique |
| Vite | 6 | Bundler / Dev server |
| Apollo Client | 3 | Client GraphQL |
| React Router | 7 | Routage SPA |

### Backend (`server/`)

| Technologie | Version | Rôle |
|---|---|---|
| Apollo Server | 4 | Serveur GraphQL |
| Type-GraphQL | 2 | Schéma GraphQL via décorateurs |
| TypeORM | 0.3 | ORM |
| SQLite3 | 5 | Base de données |
| class-validator | 0.14 | Validation des entités |
| ts-node-dev | 2 | Serveur de développement |

### Outillage

- **Husky** — hooks Git pré-commit
- **lint-staged** — lint automatique des fichiers modifiés
- **ESLint / Prettier** — linting et formatage du code
- **concurrently** — lancement simultané client + serveur

## Installation

### Prérequis

- Node.js (v18 ou supérieur recommandé)
- npm

### Étapes

1. **Clonez le dépôt**

   ```bash
   git clone https://github.com/WildCodeSchool-CDA-FT-2025-03/Hive---home-media-center.git
   cd Hive---home-media-center
   ```

2. **Installez les dépendances à la racine**

   ```bash
   npm install
   ```

3. **Installez les dépendances du serveur**

   ```bash
   cd server && npm install && cd ..
   ```

4. **Installez les dépendances du client**

   ```bash
   cd client && npm install && cd ..
   ```

5. **Configurez les variables d'environnement**

   Côté serveur (`server/.env`) :
   ```env
   ENVIRONNEMENT=development
   PORT_SERVER=4000
   ```

   Côté client (`client/.env`) :
   ```env
   VITE_PORT_URL=http://localhost
   VITE_SERVER_URL=http://localhost:4000
   ```

6. **Lancez l'application** (depuis la racine)

   ```bash
   npm run dev
   ```

   Le client est accessible sur [http://localhost:5173](http://localhost:5173) et le serveur GraphQL sur [http://localhost:4000](http://localhost:4000).

## Peuplement de la base de données

La base de données SQLite est créée automatiquement au démarrage en mode `development` (synchronisation TypeORM).

Pour importer les données de démonstration depuis les fichiers JSON fournis :

```bash
# Migrer les jeux
cd server && npm run migrate:games

# Migrer les films
npm run migrate:movies

# Ou les deux en une commande
npm run migrate
```

## Scripts disponibles

### Racine

| Commande | Description |
|---|---|
| `npm run dev` | Lance client et serveur en parallèle |
| `npm run commit` | Script de commit guidé |

### Serveur (`server/`)

| Commande | Description |
|---|---|
| `npm run dev` | Lance le serveur en mode développement (hot reload) |
| `npm run migrate` | Importe les données jeux et films |
| `npm run migrate:games` | Importe uniquement les jeux |
| `npm run migrate:movies` | Importe uniquement les films |
| `npm run compile` | Compile TypeScript |
| `npm run lint` | Vérifie le code |

### Client (`client/`)

| Commande | Description |
|---|---|
| `npm run dev` | Lance Vite en mode développement |
| `npm run build` | Compile pour la production |
| `npm run preview` | Prévisualise le build de production |
| `npm run lint` | Vérifie le code |

## Routes de l'application

| Route | Page |
|---|---|
| `/` | Accueil — carrousels des derniers jeux et films |
| `/movie` | Catalogue des films |
| `/movie/:id` | Fiche de détail d'un film |
| `/game` | Catalogue des jeux vidéo |
| `/game/:id` | Fiche de détail d'un jeu |
| `/music` | Catalogue des musiques |
| `/book` | Catalogue des livres |
| `/add-media` | Formulaire d'ajout de média (film ou jeu) |
| `/profile` | Page de profil utilisateur |

## API GraphQL

Le serveur expose un point d'entrée GraphQL unique. Les principales opérations disponibles sont :

### Queries

| Query | Description |
|---|---|
| `getMovies` | Liste tous les films |
| `getLastMovies` | 15 derniers films ajoutés |
| `getOneMovieById(id)` | Détail d'un film (avec catégories et récompenses) |
| `getGames` | Liste tous les jeux |
| `getLastGames` | 15 derniers jeux ajoutés |
| `getOneGameById(id)` | Détail d'un jeu (avec catégories et récompenses) |
| `getAll(name, sort, asc, search?)` | Recherche/filtrage/tri pour jeux (`jeux`) ou films (`film`) |
| `getReviewMovie(id)` | Récupère une critique de film |
| `getUsers` | Liste les utilisateurs |
| `getNewsletters` | Liste les inscriptions newsletter |

### Mutations

| Mutation | Description |
|---|---|
| `createMovie(...)` | Crée un nouveau film |
| `createGame(...)` | Crée un nouveau jeu |

## Modèle de données

```
Movie          — titre, réalisateurs, acteurs, catégories, récompenses, critiques
Game           — titre, développeurs, plateformes, DLC, catégories, récompenses
Book           — titre, auteur, éditeur, résumé, ISBN
Album (Music)  — titre, artistes, tracklist, catégories, certifications
User           — nom, mot de passe, rôle admin, critiques
ReviewMovie    — note, texte, date, lien vers un film et un utilisateur
Newsletter     — email d'inscription
```

## Équipe de développement

Développé par [Alexandre Dumout](https://www.linkedin.com/in/alexandre-dumout-317505123/), [Romaric Yi](https://www.linkedin.com/in/yiromaric/) et [Ryan Decian](https://www.linkedin.com/in/ryan-decian-864696302/)

---

Ce projet est développé dans le cadre de la Formation "Développeur Concepteur d'Applications" de la [Wild Code School](https://www.wildcodeschool.com/)
