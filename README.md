# universal-devbox (Go Hello World)

Ce projet fournit une architecture de développement moderne, standardisée et entièrement isolée. Il combine la puissance de **Devcontainer CLI** pour la conteneurisation et de **Taskfile** (go-task) pour l'automatisation. 

### 💡 Pourquoi Taskfile plutôt que Make ?
Traditionnellement, la gestion des tâches de projet est confiée à `Make`. Cependant, la syntaxe des `Makefile` (gestion stricte des tabulations, bizarreries des scripts Bash multilignes, lisibilité complexe) devient rapidement un frein à la maintenance et à l'onboarding de nouveaux développeurs. 

`Taskfile` résout ce problème en proposant une approche moderne basée sur le **YAML**. Il est plus simple à lire, plus facile à comprendre, possède une gestion native et robuste des dépendances entre tâches, et s'intègre parfaitement avec les outils modernes.

L'objectif de ce dépôt est de respecter les principes **DRY** (Don't Repeat Yourself) et **KISS** (Keep It Simple, Stupid) : **vous codez sur votre machine hôte, mais l'exécution, la compilation, les outils tiers et la validation de code s'exécutent de manière transparente dans le conteneur.**

---

## 🚀 Concept & Adaptabilité

Bien que configuré ici pour un exemple "Hello World" en **Go** (avec l'installation automatique de `swag` et `templ`), **ce concept est 100% universel**. 

En modifiant simplement l'image de base dans le `Dockerfile` et les commandes de build dans le `Taskfile.yml`, cette même structure peut être adaptée pour n'importe quel autre langage :
*   **Node.js** (`npm run`, `jest`, linters)
*   **Python** (`poetry run`, `pytest`, `black`)
*   **Rust** (`cargo run`, `cargo test`)

---

## 🛠️ Prérequis (Machine Hôte)

Pour utiliser ce projet, seuls trois outils doivent être installés sur votre machine. **Vous n'avez pas besoin d'installer Go localement.**

1.  **Git** : Pour la gestion de version.
2.  **Taskfile** : Le moteur d'exécution de tâches basé sur YAML.
    *   [Documentation et Installation de Taskfile](https://taskfile.dev)
3.  **Devcontainer CLI** : L'outil en ligne de commande pour piloter les environnements conteneurisés.
    *   [Documentation et Installation de Devcontainer CLI](https://code.visualstudio.com/docs/devcontainers/devcontainer-cli)
4.  **Docker** (ou un moteur compatible) : Nécessaire pour faire tourner le conteneur en arrière-plan.

---

## 💻 Compatibilité Éditeurs (VS Code, Zed, etc.)

Ce boilerplate est nativement compatible avec les éditeurs modernes supportant les Devcontainers comme **VS Code**, **Cursor**, ou **Zed**.

*   **Via l'éditeur :** Lorsque vous ouvrez le dossier, votre éditeur détectera le dossier `.devcontainer/` et vous proposera de rouvrir le projet à l'intérieur du conteneur. Vous bénéficierez ainsi de l'autocomplétion complète de Go, Swag et Templ sans rien installer sur votre système hôte.
*   **Via le terminal (Taskfile) :** Vous pouvez continuer à utiliser votre terminal local. Les tâches utilisent le CLI pour exécuter le code dans le conteneur et synchronisent les fichiers en temps réel.

---

## 📂 Structure du projet après initialisation

```text
├── .devcontainer/
│   ├── devcontainer.json
│   └── Dockerfile
├── bin/
│   └── [votre_binaire_compilé]
├── .gitignore
├── Taskfile.yml
├── go.mod
├── main.go
└── main_test.go
```

---

## 💻 Utilisation des commandes Task

Toutes les commandes sont simplifiées au maximum et s'exécutent depuis le terminal de votre **machine hôte**.

### Gestion de l'environnement

- `task up` : Démarre manuellement le conteneur de développement.
    
- `task shell` : Ouvre un terminal interactif `bash` à l'intérieur du conteneur (utile pour utiliser manuellement `swag` ou `templ`).
    
- `task down` : Arrête et détruit le conteneur associé à ce dossier pour libérer les ressources de votre machine.
    

### Cycle de vie du code (Exécution transparente dans le conteneur)

- `task run` : Lance le projet instantanément via `go run`.
    
- `task test` : Exécute les tests unitaires en mode verbeux.
    
- `task build` : Compile l'application. Le binaire Linux final est envoyé directement dans votre dossier `bin/` local grâce au partage de volumes Docker.
    

### Flux Git

Le flux Git est divisé en deux étapes pour garder un contrôle précis :

- `task sync` : Formate automatiquement le code (`go fmt`), lance la suite de tests unitaires, et indexe toutes les modifications (`git add .`).
    
- `task commit` : Ouvre le prompt Git de votre machine hôte pour vous demander votre message et valider le commit (`git commit -v`).
    
- _Note : Toutes les autres commandes occasionnelles (git push, git checkout, etc.) s'exécutent normalement et directement avec vos commandes Git habituelles._
