# Cheatsheet 

## Gestionnaire de package

| Nom du gestionnaire | Avantages | Inconvénients |
| ----------- | ----------- | ----------- |
| npm | Intégré à NodeJS <br> Compatible avec tous les paquets JS <br> Communauté <br> Simplicité d'usage | Vitesse <br> Gestion des dépendances <br> Poids des installations |
| pnpm | Vitesse <br> Espace disque réduit <br> Intégration facile avec npm <br> isolation des dépendances | Moins répandu <br> Compatibilité | 
| Yarn | Vitesse <br> Fiabilité <br> Installation parallèle <br> Plug'n'Play | Poids des installations <br> Différence avec npm <br> Mise à jour moins fréquentes| 

### Installer/Activer pnpm

````
 npm i pnpm
````

## React

### Créer un projet avec vite

````
 pnpm create vite@latest
````
Plusieurs questions vont être posées
  1. Le nom du projet ("." pour le répertoire courant)
  2. Choix de la bibliothèque / du framework
  3. Choix Javascript ou typescript (+ SWC)

Ensuite se rendre dans le dossier ou l'installation à été faite

````
pnpm install
````

Cette commande permet de télécharger les dépendances spécifiées dans le packages.json (node_modules)
Si un projet est récuépérer depuis un github, le node_module est dans le .gitignore et n'est pas récupérer, il est donc nécessaire de faire cette commande pour télécharger les dépendances.

### Lancer un projet en local

````
pnpm run dev
````
### Structure d'un projet
![Structure projet React](./structure.png)

### Les Hooks
### Le Typage

```
type User = {
  id?: number;
  name: string;
  firstname: string;
  adress: adress;
};

type Adress = {
  street: string;
  postalCode: number;
  houseNumber: number;
};
```
Le "?" dans le typage signifique c'est une valeur qui n'est pas obligatoire.

### Les Enumération

```
export enum Nat {
  BE = "BE",
  FR = "FR",
  IT = "IT",
  UK = "UK",
}

export enum Statut {
  InProgress = 1,
  Closed,
  ReOpen,
}
```

Dans le cas du statut, cette syntaxe signifique que Closed vaudra 2 et que ReOpen vaudra 3 et ainsi de suite.

### Passage de props 

#### Passage par object

```
import { ComposantEnfant } from "../ComposantEnfant/ComposantEnfant";

export const ComposantParent = () => {
  const adressUser = {
    street: "Avenue Jean Mermoz, 28",
    postalCode: "6040",
  };
  return <ComposantEnfant adress={adressUser}></ComposantEnfant>;
};
```
```
type ComposantEnfantProps = {
  adress: {
    street: string;
    postalCode: string;
  };
};
export const ComposantEnfant = ({ adress }: ComposantEnfantProps) => {
  return (
    <div>
      <div>
        <p>Rue: {adress.street}</p>
        <p>Code postal: {adress.postalCode}</p>
      </div>
    </div>
  );
};

```
#### Passage direct

```
import { ComposantEnfant } from "../ComposantEnfant/ComposantEnfant";

export const ComposantParent = () => {
  const user = {
    name: "john",
    email: "john@doe.com",
  };
  return <ComposantEnfant {...user}></ComposantEnfant>;
};
```

```
type ComposantEnfantProps = {
  name: string;
  email: string;
};
export const ComposantEnfant = (user: ComposantEnfantProps) => {
  return (
    <div>
      <div>
        <p>Nom: {user.name}</p>
        <p>Email: {user.email}</p>
      </div>
    </div>
  );
};


```

### Formulaire avec React-Hook-Form
### Validation avec Yup
### Requête HTTP
### Redux

### Extension VSCode 
### Raccourcis VSCode
### Bibliothèque 

  - i18n
  - Material UI / Shadcn
  - Yup
  - React-Hook-Form
  - SWR
  - Axios
  - json-server
  - Redux

### Extension navigateur

  -  Redux DevTools
  -  React Developper Tools
    
## Documentation / Lien utile

### Utilitaire

  - [I Hate Regex](https://ihateregex.io/)
  - [npm](https://www.npmjs.com/)
    
### Jeux/Apprentissage

  - [CssBattle](https://cssbattle.dev/)
  - [CodingFantasy](https://codingfantasy.com/)
  - [FlexboxDefense](http://www.flexboxdefense.com/)
  - [ICodeThis](https://icodethis.com/app)
  - [FlexboxFroggy](https://flexboxfroggy.com/)

### Bibliothèque

  - [i18n](https://www.i18next.com/)
  - [React-Hook-Form](https://www.react-hook-form.com/)
  - [Material UI](https://mui.com/material-ui/getting-started/)
  - [Yup](https://mui.com/material-ui/getting-started/)
  - [Axios](https://axios-http.com/docs/intro)
  - [SWR](https://swr.vercel.app/)
  - [Redux](https://redux.js.org/)

### Interface

  - [Uiball](https://uiball.com/ldrs/)

  
