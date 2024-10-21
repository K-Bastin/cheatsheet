# Cheatsheet 

## Gestionnaire de package

| Nom du gestionnaire | Avantages | Inconvénients |
| ----------- | ----------- | ----------- |
| npm | Intégré à NodeJS <br> Compatible avec tous les paquets JS <br> Communauté <br> Simplicité d'usage | Vitesse <br> Gestion des dépendances <br> Poids des installations |
| pnpm | Vitesse <br> Espace disque réduit <br> Intégration facile avec npm <br> isolation des dépendances | Moins répandu <br> Compatibilité | 
| Yarn | Vitesse <br> Fiabilité <br> Installation parallèle <br> Plug'n'Play | Poids des installations <br> Différence avec npm <br> Mise à jour moins fréquentes| 

**Les commandes npm, pnpm et yarn doivent être éxécutée dans un terminal de commande ou directement dans le terminal de Visual code**

**Attention, de base VS Code ouvre un terminal Powershell, l'éxécution des commandes peut poser problèmes si l'éxécution des scripts est bloqué sur la machine**

### Installer/Activer pnpm

````
 npm i pnpm
````

## React

### Créer un projet avec Vite

Vite est un bundler, il permet de compiler les fichiers Javascript, css, etc ...
Vite à été conçu pour être très rapide, surtout pendant le développement.

````
 pnpm create vite@latest
````
Plusieurs questions vont être posées
  1. Le nom du projet ("." pour le répertoire courant)
  2. Choix de la bibliothèque / du framework
  3. Choix Javascript ou typescript (+ SWC)

Ensuite se rendre dans le dossier où l'installation (via cd "nom du projet") a été faite et faire la commande

````
pnpm install
````

Cette commande permet de télécharger les dépendances spécifiées dans le packages.json (node_modules)
Si un projet est récupéré depuis un repository (ex. Github), le node_module est dans le .gitignore et n'est donc pas récupéré, il est donc nécessaire de faire cette commande pour télécharger les dépendances.

### Lancer un projet en local

````
pnpm run dev
````
Cette commande est en réalité un script que vous pouvez retrouver dans votre package.json. 
Par défaut, le projet sera hébergé sur le http://localhost:5173/

### Structure d'un projet

Voici une idée globale de la structure d'un projet. 
Les dossiers dépendront de la façon dont l'équipe veut structurer les projets mais ce screenshot montre l'idée générale. 
Il pourra il avoir d'autres dossiers comme pour le store, le routing, les middlewares et autres.

![Structure projet React](./structure.png)

### Création et utilisation d'un composant simple

Un composant React est simplement un fichier .jsx (.tsx en typescript) qui contient une fonction qui retourne du jsx. Ne pas oublier de l'exporter afin qu'elle soit disponible ailleurs dans le programme.
Le composant ne peut renvoyer qu'un seul élement html, afin de ne pas générer des balises inutiles, il est possible d'utiliser les fragments, "<> </>"

```tsx
export const SimpleComponent = () => {

  return (
    <>
      <h1>Hello world</h1>
    </>
  );
};
```

### Fonction sur les Arrays

Filtre()
Renvoie tous les valeurs qui correspondent au filtre.
```tsx
  const [tableValue, setTableValue] = useState<string[]>([
    "Abricot",
    "Beignet",
    "Fraise",
    "Pomme",
  ]);
  const handleArrayFunctionFilter = () => {
    setTableValue([...tableValue.filter((e) => e !== "Beignet")]);
  };
```
Reduce()
Applique un accumulateur. Dans cet exemple, renvois le nombre d'élément différent de "Beignet"

```tsx
  const [tableValue, setTableValue] = useState<string[]>([
    "Abricot",
    "Beignet",
    "Fraise",
    "Pomme",
  ]);
  const handleArrayFunctionReduce = () => {
    const reduceTable = tableValue.reduce((accumulateur, valeur) => {
      if (valeur !== "Beignet") {
        accumulateur += 1;
      }
      return accumulateur;
    }, 0);

    //Resultat = 3
    console.log(reduceTable);
  };
```

ForEach()
Parcours chaque element du tableau.

```tsx
  const [tableValue, setTableValue] = useState<string[]>([
    "Abricot",
    "Beignet",
    "Fraise",
    "Pomme",
  ]);
  const handleArrayFunctionForEach = () => {
    tableValue.forEach((element) => console.log(element));
  };
```

Map()
la fonction map est utilisée pour créer un nouveau tableau en appliquant une fonction à chaque élément d'un tableau existant.

```tsx
      <div>
        {tableValue.map((e, i) => {
          return <p key={i}>{e}</p>;
        })}
      </div>
```

### Affichage conditionnel

L'opérateur "??" est utilisé pour fournir une valeur par défaut lorsqu'une variable est null ou undefined"
Dans ce cas-ci comme age est égal à null, c'est "Non renseigné" qui sera affiché. Si on lui assigne une valeur, alors ça sera son contenu qui sera affiché.

L'opérateur "&&" sera lui utilisé pour afficher une élement si la condition est vraie. Ici afficherPrenom est à vrai donc "Kevin" est affiché.

```tsx
export const SimpleComponent = () => {
  const afficherPrenom: boolean = true;
  const afficherAge: boolean = true;
  const age = null;
  return (
    <>
      <h1>Hello world {afficherPrenom && <p>Kevin</p>}</h1>
      {afficherAge && <p>Tu as {age ?? "Non renseigné"} an</p>}
    </>
  );
};
```

### Les Hooks

#### useEffect

Le useEffect est du code qui sera éxécuté soit, au premier rendu du composant, soit à chaque rendu, soit à chaque modification de la variable passé en paramètre du useEffect.

Dans ce cas-ci, l'affichage dans la console sera effectué uniquement au premier rendu du composant car on lui a passé un tableau vide en paramètre.

```tsx
  useEffect(() => {
    console.log("Effectué une seule fois");
    return () => {};
  }, []);
```

Dans ce cas-ci, l'affichage dans la console sera effectué à chaque rendu du composant car on ne lui a passé aucun paramètre.

```tsx
  useEffect(() => {
    console.log("Effectué à chaque rendu");
    return () => {};
  });
```

Dans ce cas-ci, l'affichage dans la console sera effectué à chaque modification de la variable "Value" qui est la variable qu'on lui a passé en paramètre.

Attention aux boucles infinies si du code dans ce useEffect met à jour la variable impliquée.

```tsx
  useEffect(() => {
    console.log("Effectué à chaque changement de la variable");
    return () => {};
  }, [value]);
```

Dans le useEffect, il est possible de spécifier une fonction de "nettoyage" via le return. Ce qui permet de nettoyer des ressources comme des abonnements, des timers ou des événements.

```tsx
useEffect(() => {
  const interval = setInterval(() => {
    console.log("Intervalle actif");
  }, 1000);

  // Le return ici nettoie l'intervalle lorsque le composant est démonté
  return () => clearInterval(interval);
}, []);
```

#### useState

Le useState est un hook utilisé pour ajouter un état local aux composants.
Il retourne une paire [valeur, fonctionDeMiseAJour]  

Cet état doit toujours être modifié via sa fonction de miseAJour sinon React ne sait pas qu'il doit render le composant.

Ici l'état "value" est de type "number" et initialisé à 0;

```tsx
  const [value, setValue] = useState<number>(0);
```

Il est important de noter que la fonction de mise à jour (setState) ne remplace pas l'état immédiatement et que c'est une opération asynchrone. 
L'appel au setState planifie une mise à jour du composant et la valeur ne sera mise à jour qu'au rendu de celui-ci.

Exemple : ce code affichera 1 et non 2 car la valeur n'a pas encore été mise à jour. 

```tsx
      const [value, setValue] = useState<number>(0);

      setValue((prevValue) => prevValue + 1); 
      setValue((prevValue) => prevValue + 1);
      console.log(value);
```

Il est important de toujours travailler sur une copie de l'état afin de ne pas écraser des parties, le plus simple reste d'utiliser l'opérateur de décomposition "..." .

```tsx
const [person, setPerson] = useState({ name: 'John', age: 30 });

function updateName(newName) {
  setPerson(prevPerson => ({ ...prevPerson, name: newName }));
}
```

#### useRef

Ce hook permet de créer une référence mutable qui peut persister entre les rendus sans déclencher de render lorsqu'elle est modifiée. 
Elle est utile pour accéder à des élements du DOM directement.

```tsx
function TextInputWithFocusButton() {
  const inputRef = useRef(null);

  const focusInput = () => {
    // Accède à l'élément DOM via inputRef.current et applique le focus
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus sur l'input</button>
    </div>
  );
}
```

Ou pour stocker des valeurs persistantes.

```tsx
function Counter() {
  const [count, setCount] = useState(0);
  const renderCount = useRef(1);

  useEffect(() => {
    renderCount.current += 1;
  });

  return (
    <div>
      <p>Le compteur est à : {count}</p>
      <p>Le composant a été rendu {renderCount.current} fois</p>
      <button onClick={() => setCount(count + 1)}>Incrémenter</button>
    </div>
  );
}
```

#### useId

Ce hook permet de générer des identifiants uniques qui peuvent être utilisés dans les composants, c'est particulièrement utile pour créer les attributs id qui sont uniques dans le DOM.

```tsx
function MyForm() {
  const id = useId();

  return (
    <div>
      <label htmlFor={`${id}-name`}>Nom :</label>
      <input id={`${id}-name`} type="text" />
    </div>
  );
}
```

### Le Typage

Les types seront généralement stockés dans un fichier *.d.ts, le "d" étant pour définition.
Le "?" dans le typage signifie que c'est une valeur qui n'est pas obligatoire.

```tsx
type User = {
  id?: number;
  name: string;
  firstname: string;
  adress: Adress;
};

type Adress = {
  street: string;
  postalCode: number;
  houseNumber: number;
};
```

### Les Enumération

Dans le cas du statut, cette syntaxe signifique que Closed vaudra 2 et que ReOpen vaudra 3 et ainsi de suite.

```tsx
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

### Passage de props 

#### Passage par object

```tsx
import { ComposantEnfant } from "../ComposantEnfant/ComposantEnfant";

export const ComposantParent = () => {
  const adressUser = {
    street: "Avenue Jean Mermoz, 28",
    postalCode: "6040",
  };
  return <ComposantEnfant adress={adressUser}></ComposantEnfant>;
};
```
```tsx
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

```tsx
import { ComposantEnfant } from "../ComposantEnfant/ComposantEnfant";

export const ComposantParent = () => {
  const user = {
    name: "john",
    email: "john@doe.com",
  };
  return <ComposantEnfant {...user}></ComposantEnfant>;
};
```

```tsx
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
### Routing avec react router

Afin de gérer le routing, il faudra utiliser une bibliothèque telle que React router. 

Dans cet exemple de code, en plus du routing basique, il y aura la protection de page privée nécessitant une connexion au préalable.

Création de 3 pages basiques : une page publique, une page privée et une page pour url non connue.

**Page publique**
```tsx
//fichier PagePublic.tsx
export const PagePublic = () => {
  return (
    <div>
      <h1>Page publique</h1>
      <p>Vous êtes sur une page publique, accessible par tous le monde.</p>
    </div>
  );
};
```

**Page privée**

```tsx
export const PagePrivate = () => {
  return (
    <div>
      <h1>Page protégée</h1>
      <p>Vous êtes sur une page nécéssitant d'être loggin.</p>
    </div>
  );
};
```

**Page de redirection - 404 not found**

```tsx
export const PageNotFound = () => {
  return (
    <div>
      <h1>Error 404 - Not Found</h1>
    </div>
  );
};
```

**Création d'un système d'authentification basique via le context**

Ce code se charge d'aller voir dans le localStore si un objet "token" existe, cela signifie qu'on est identifié.

```tsx
import { FC, ReactNode, createContext, useContext, useState } from "react";

interface AuthContextType {
  isAuthenticated: boolean;
}

const AuthContext = createContext<AuthContextType | null>(null);

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (context === undefined) {
    throw new Error("useAuth must be used within an AuthProvider");
  }
  return context;
};

interface AuthProviderProps {
  children: ReactNode;
}

export const AuthProvider: FC<AuthProviderProps> = ({ children }) => {
  const [token, setToken] = useState<string | null>(
    localStorage.getItem("token")
  );
  const [isAuthenticated, setIsAuthenticated] = useState<boolean>(!!token);

  return (
    <AuthContext.Provider value={{ isAuthenticated }}>
      {children}
    </AuthContext.Provider>
  );
};
```

Ensuite, on définit des composants pour les routes privées et les routes publiques.
Ces composants vont utiliser le contexte du système d'authentification pour vérifier si l'utilisateur est connecté ou non et nous rediriger en conséquence.

```tsx
import React from "react";
import { Navigate } from "react-router-dom";

import { useAuth } from "../../middleware/AuthProvider";

const PrivateRoute = ({ Component }: { Component: React.ComponentType }) => {
  const authContext = useAuth();

  return authContext?.isAuthenticated ? <Component /> : <Navigate to="/" />;
};

export default PrivateRoute;
```

Le composant PublicRoute, quant à lui, va empêcher l'accès aux pages publiques pour les utilisateurs déjà connectés.
Par exemple, il empêchera un utilisateur déjà connecté d'afficher la page de connexion de l'application.

```tsx
import { Navigate } from "react-router-dom";

import { useAuth } from "../../middleware/AuthProvider";

const PublicRoute = ({ Component }: { Component: React.ComponentType }) => {
  const authContext = useAuth();

  return authContext?.isAuthenticated ? (
    <Navigate to="/private" />
  ) : (
    <Component />
  );
};

export default PublicRoute;
```

Il ne reste plus qu'à englober l'application dans le contexte d'authentification et utiliser react router pour définir les chemins des composants.
Ici l'application ne reconnait que les routes "/" et "/private", les autres urls renverront vers le composants "404 - not found"
Le chemin "/" affiche la page publique et le chemin "/" private nécessite d'être "log" pour être affiché

```tsx
import { BrowserRouter, Route, Routes } from "react-router-dom";
import "./App.css";
import PrivateRoute from "./Components/Auth/PrivateRoute";
import PublicRoute from "./Components/Auth/PublicRoute";
import { AuthProvider } from "./middleware/AuthProvider";
import { PageNotFound } from "./Pages/PageNotFound";
import { PagePrivate } from "./Pages/PagePrivate";
import { PagePublic } from "./Pages/PagePublic";

function App() {
  return (
    <>
      <AuthProvider>
        <BrowserRouter>
          <Routes>
            <Route path="/" element={<PublicRoute Component={PagePublic} />} />
            <Route
              path="/private"
              element={<PrivateRoute Component={PagePrivate} />}
            />
            <Route
              path="*"
              element={<PublicRoute Component={PageNotFound} />}
            />
          </Routes>
        </BrowserRouter>
      </AuthProvider>
    </>
  );
}

export default App;
```

### Formulaire avec React-Hook-Form

Exemple d'un formulaire basique de login via la bibliothèque React-Hook-Form.
Définition des fonctions de gestion du formulaire via useForm (register, handleSubmit, reset, error ...)

"register" permet d'enregistrer les champs dans le système de gestion des formulaires de la bibliothèque, "handleSubmit" permet de gérer la soumission du formulaire, "reset" permet de remettre à zéro les champs et l'objet "error" contient les erreurs pour chaque champ.

Possibilité de passer un objet aux inputs qui contient une certaine validation mais on préférera passer par une bibliothèque comme Yup qui permettra de définir un schéma.

```tsx
import { useForm } from "react-hook-form";

type LoginProps = {
  email: string;
  password: string;
};

export const FormComponent = () => {
  const {
    register,
    handleSubmit,
    reset,
    formState: { errors },
  } = useForm<LoginProps>({
    defaultValues: {
      email: "",
      password: "",
    },
  });

  const onSubmitLogin = (data: LoginProps) => {
    // les données du formulaires se trouvent dans data
    console.log(data.email);
    reset();
  };

  return (
    <>
      <form onSubmit={handleSubmit(onSubmitLogin)}>
        <div>
          <label>Email : </label>
          <input type="text" {...register("email", { required: true })} />
          {errors.email && <p>Veuillez indiquez une adresse email.</p>}
        </div>
        <div>
          <label>Password : </label>
          <input
            type="password"
            {...register("password", { min: 1, required: true })}
          />
          {errors.email && <p>Veuillez indiquez un mot de passe.</p>}
        </div>

        <div>
          <button type="submit">Login</button>
        </div>
      </form>
    </>
  );
};
```

### Validation d'un formulaire React-Hook-Form avec Yup

Afin de valider les données d'un formulaire, il est possible de passer par une bibliothèque comme Yup. Celle-ci permet d'établir un schéma avec les définitions des obligations des champs du formulaire ainsi que les messages d'erreurs liées à celles-ci.
Exemple de code

```tsx
import { yupResolver } from "@hookform/resolvers/yup";
import { useForm } from "react-hook-form";
import * as Yup from "yup";

type LoginProps = {
  email: string;
  password: string;
};

//Définition du schéma de validation
const validationSchema = Yup.object().shape({
  email: Yup.string()
    .email("L'adresse email n'est pas valide")
    .required("L'adresse email est requise."),
  password: Yup.string().required("Le mot de passe est requis."),
});

export const SimpleComponent = () => {
  const {
    register,
    handleSubmit,
    reset,
    formState: { errors },
  } = useForm<LoginProps>({
    resolver: yupResolver(validationSchema),
  });

  const onSubmitLogin = (data: LoginProps) => {
    // les données du formulaires se trouvent dans data
    console.log(data.email);
    reset();
  };

  return (
    <>
      <form onSubmit={handleSubmit(onSubmitLogin)}>
        <div>
          <label>Email : </label>
          <input type="text" {...register("email")} />
          {errors.email && <p>{errors.email.message}</p>}
        </div>
        <div>
          <label>Password : </label>
          <input type="password" {...register("password")} />
          {errors.password && <p>{errors.password.message}</p>}
        </div>

        <div>
          <button type="submit">Login</button>
        </div>
      </form>
    </>
  );
};
```
### Requête HTTP avec Axios

Exemple de code pour un get avec Axios.

Dans cet exemple, les données viennent d'un endpoint publique contenant des données inutiles.
On passe donc par une définition d'un type reprenant les champs renvoyés par l'API, ensuite, sur base de celle-ci on crée un type reprenant les données que l'on veut (ici via Pick). 
On fait appel à la fonction d'appel à l'API dans le useEffect comme ça celle-ci est appelée au premier rendu du composant et comme on passe un tableau vide, uniquement au premier rendu.

```tsx
import axios from "axios";
import { useEffect, useState } from "react";

type catFactFromEndpoint = {
  status: Status;
  _id: string;
  user: string;
  text: string;
  __v: number;
  source: string;
  updatedAt: string;
  type: string;
  createdAt: string;
  deleted: boolean;
  used: boolean;
};

type Status = {
  verified: boolean;
  sentCount: number;
};

type catFact = Pick<catFactFromEndpoint, "text">;

export const AxiosComponent = () => {
  const endpoint: string = "https://cat-fact.herokuapp.com";

  const [dataCatFact, setDataCatFact] = useState<catFact>();
  const [errorFetch, setErrorFetch] = useState<string>("");
  const [isLoading, setIsLoading] = useState<boolean>(false);

  const fetchCatFact = async () => {
    setErrorFetch("");
    setIsLoading(true);
    setDataCatFact({
      text: "",
    });
    try {
      const response = await axios.get(endpoint + "/facts/random");
      setDataCatFact(response.data); // Mettre à jour l'état avec la donnée reçue
    } catch (err) {
      // Gérer les erreurs
      if (err) {
        setErrorFetch(err.toString());
      } else {
        setErrorFetch("Erreur lors du chargement des faits sur les chats");
      }
      
    } finally {
      // La requête est terminée, on enlève le chargement
      setIsLoading(false); 
    }
  };

  useEffect(() => {
    fetchCatFact();
  }, []);

  return (
    <div>
      {isLoading && <p>Chargement en cours</p>}
      {dataCatFact && <p> {dataCatFact?.text} </p>}
      {errorFetch && <p> {errorFetch} </p>}
      <button onClick={fetchCatFact}>Send request</button>
    </div>
  );
};
```

### Redux

Exemple de code: Application affichant des "Cat fact" qui viennent d'une API.

Création du store dans l'application. Avec redux, celui-ci doit être unique et ne sera fait qu'**une seule fois**. il sera cependant modifié pour rajouter les nouveaux reducer.

```tsx
//Fichier store.ts
import { configureStore } from "@reduxjs/toolkit";
import catFactReducer from "./catFact/catFact.reducer";

const store = configureStore({
  reducer: {
    catFact: catFactReducer,
  },
  devTools: import.meta.env.DEV,
});

// - Le type du store Redux
export type Store = typeof store;
// - Le type du "state" du store (via la méthode "getState")
export type StateStore = ReturnType<Store["getState"]>;
// - Le type des actions possible du store (via le dispatcher)
export type DispatchStore = Store["dispatch"];

export default store;
```

Création des actions et du reducer

Définition des actions pour catFact.
Ici on ne définit qu'une action, "getCatFact", celle-ci définit une intention de changer l'état et ce qu'il doit se passer. Dans ce cas-ci, un appel à une API pour récupérer un CatFact.
Ici on ne modifie pas directement l'état, c'est simplement un "message" envoyé au store.
On fait donc appel ici à notre "service" qui lui se charge de faire l'appel à l'API via Axios.

```tsx
//Fichier catFact.action.ts
import { createAsyncThunk } from "@reduxjs/toolkit";
import { requestNewCatFact } from "../../services/catFact.service";

type CatFactData = {
  text: string;
};

export const getCatFact = createAsyncThunk("catFact/getCatFact", async () => {
  // Requete ajax
  const data: CatFactData = await requestNewCatFact();

  // En cas de succes -> Envoi des données
  return data;
});
```

Le reducer est une fonction qui prend deux arguments : le state et l'action. Il décrit comment l'état de l'application doit changer en réponse à une action. 
Ici 3 scénarios possibles, l'action getCatFact étant une requête HTTP vers une API via Axios est une Promesse, elle possède donc 3 états de réponses possibles "pending", "fulfilled" et "rejected", on gère donc ici les 3 scénarios. 
Dans le cas du pending, on met à jour le state sur isLoading. Dans le cas du fulfilled, on push le catFact dans le state et on enlève le isLoading. Dans le cas du reject, on est en erreur, on enlève donc le isLoading et on set l'erreur.

```tsx
//Fichier catFact.reducer.ts
import { createReducer } from "@reduxjs/toolkit";
import { getCatFact } from "./catFact.action";

export type CatFact = {
  text: string;
};
export type catFactReducerState = {
  catFacts: CatFact[];
  isLoading: boolean;
  error: string | null;
};

const initialState: catFactReducerState = {
  catFacts: [],
  isLoading: false,
  error: null,
};

const catFactReducer = createReducer(initialState, (builder) => {
  builder
    .addCase(getCatFact.pending, (state) => {
      state.isLoading = true;
      state.error = null;
    })
    .addCase(getCatFact.fulfilled, (state, action) => {
      const catFact = action.payload;
      state.isLoading = false;
      state.catFacts.push(catFact);
    })
    .addCase(getCatFact.rejected, (state, action) => {
      state.isLoading = false;
      state.error = action.error.message ?? "Erreur inconnue";
    });
});

export default catFactReducer;

```

Création du service permettant de récupérer via Axios les données d'une API (ici un simple GET afin de récupérer un "cat fact")

```tsx
//Fichier catFact.services.ts
import axios from "axios";

export const requestNewCatFact = async () => {
  const endpoint: string = "https://cat-fact.herokuapp.com";

  const { data } = await axios.get(endpoint + "/facts/random");

  return {
    text: data.text,
  };
};
```

On englobe ensuite l'application dans le store afin que celui-ci soit disponible partout.

```tsx
//Fichier main.tsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import { Provider } from "react-redux";
import App from "./App.tsx";
import "./index.css";
import store from "./store/store.ts";

createRoot(document.getElementById("root")!).render(
  <StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </StrictMode>
);
```

Utilisation de redux dans un composant afin de récupérer et d'afficher* des catfacts.
Dans cet exemple, division en 3 composants : 
 - CatFactComponent: composant principal qui fait appel au dispatch et au store. C'est ce composant qui contient la liste des catFacts ainsi que le button qui fait les appels à l'API pour récupérer des catfacts.
 - CatFactComponentList : Composant qui affiche CatFactComponentItem. Son parent lui a passé la liste des Catfacts. Il les passera ensuite un part un à son enfant CatFactComponentListItem.
 - CatFactComponentListItem : Composant qui se charge uniquement d'afficher le catFact. Son parent CatFactComponentList lui a uniquement passé le Catfact qu'il doit afficher
   
```tsx
//Fichier CatFactComponent.tsx
import { useDispatch, useSelector } from "react-redux";
import { getCatFact } from "../../store/catFact/catFact.action";
import { DispatchStore, StateStore } from "../../store/store";
import { CatFactComponentList } from "./CatFactComponentList";

export const CatFactComponent = () => {
  const dispatch: DispatchStore = useDispatch();

  const catFacts = useSelector((state: StateStore) => state.catFact.catFacts);

  const handleGetNewCatFact = () => {
    dispatch(getCatFact());
  };

  return (
    <section>
      <div>
        <button onClick={handleGetNewCatFact}>
          Recupérer un nouveau catFact
        </button>
      </div>
      <div>
        <CatFactComponentList catFacts={catFacts}></CatFactComponentList>
      </div>
    </section>
  );
};
```

```tsx
//Fichier CatFactComponentList
import { CatFact } from "../../store/catFact/catFact.reducer";
import { CatFactComponentListItem } from "./CatFactComponentListItem";

export type CatFactComponentListProps = {
  catFacts: CatFact[];
};

export const CatFactComponentList = ({
  catFacts,
}: CatFactComponentListProps) => {
  return (
    <section>
      {catFacts.map((catFact) => (
        <CatFactComponentListItem {...catFact} />
      ))}
    </section>
  );
};
```

```tsx
//Fichier CatFactComponentListItem
import { CatFact } from "../../store/catFact/catFact.reducer";

export type CatFactComponentListItemProps = CatFact & {};

export const CatFactComponentListItem = ({
  text,
}: CatFactComponentListItemProps) => {
  return (
    <article>
      <p>CatFact : {text}</p>
    </article>
  );
};
```

### Extension utile VSCode 
- [Better comments](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments)
- [CodeSnap](https://marketplace.visualstudio.com/items?itemName=adpyke.codesnap)
- [Color Highlight](https://marketplace.visualstudio.com/items?itemName=naumovs.color-highlight)
- [Auto Rename Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)
- [Error Lens](https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens) 
- [ES7 + React/Redux/React-native snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [Gitignore](https://marketplace.visualstudio.com/items?itemName=codezombiech.gitignore)
- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [indent-rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)
- [Javascript (ES6) code snippets](https://marketplace.visualstudio.com/items?itemName=xabikos.JavaScriptSnippets)
- [Path intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense)
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Pretty TypeScript errors](https://marketplace.visualstudio.com/items?itemName=yoavbls.pretty-ts-errors)
- [SVG](https://marketplace.visualstudio.com/items?itemName=jock.svg)
- [Tailwind CSS IntelliSense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)
- [Postman](https://marketplace.visualstudio.com/items?itemName=Postman.postman-for-vscode)
- [VSCode React Refactor](https://marketplace.visualstudio.com/items?itemName=planbcoding.vscode-react-refactor)
  
### Raccourcis utile VSCode

 - **Recherche** : `Ctrl + Shift + F`
 - **Formater le code** : `Alt + Shift + F`
 - **Sélectionner la prochaine paire d'accolades** : `Ctrl + Shift + ]`
 - **Rechercher et remplacer dans le fichier** : `Ctrl + H`
 - **Ouvrir le terminal** : `Ctrl + Shift + ù`
 - **Rechercher dans le fichier** : `Ctrl + F`
 - **Mettre en commentaire la sélection** : `Ctrl + :`
 - **Aller à la définition** : `F12`
 - **Afficher des suggestions de code** : `Ctrl + espace`
 - **Indentation automatique** : `Ctrl + Shift + I`
 - **Déplacer la ligne vers la bas/haut** : `Alt +  ↓ / ↑`
 - **Copier la ligne vers le bas/haut** : `Alt + Shift + ↓ / ↑`
 - **Changer le nom de la variable partout dans le fichier** : `F2`
 - **Sélection multiple de curseurs** + : `Ctrl + D`
 - **Ouvrir le panneau de commande** : `Ctrl + Shift + P`
 - **Supprimer la ligne** : `Ctrl + Shift + K`

### Bibliothèque 

  - i18n
  - Material UI / Shadcn
  - Yup
  - React-Hook-Form
  - SWR
  - Axios
  - json-server
  - Redux
  - React-router

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
  - [React-router](https://reactrouter.com/en/main)

### Interface

  - [Uiball](https://uiball.com/ldrs/)

    <!-- repos avec projet et code pour chaque point (react router, hook, etc...    !-->

  
