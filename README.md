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
Par défault, le projet sera héberger sur le "http://localhost:5173/"

### Structure d'un projet
![Structure projet React](./structure.png)

### Les Hooks

#### useEffect

Le useEffect est du code qui sera éxécuté soit, au premier rendu du composant, soit à chaque rendu soit à chaque modification de la variable passé en paramètre du useEffect.

Dans ce cas-ci, l'affichage dans la console sera effectué uniquement au premier rendu du composant.

```
  useEffect(() => {
    console.log("Effectué une seule fois");
    return () => {};
  }, []);
```

Dans ce cas-ci, l'affichage dans la console sera effectué à chaque rendu du composant.

```
  useEffect(() => {
    console.log("Effectué à chaque rendu");
    return () => {};
  });
```

Dans ce cas-ci, l'affichage dans la console sera effectué à chaque modification de la variable "Value".

Attention au boucle infinie si du code dans ce useEffect met à jour la variable impliquée.
```
  useEffect(() => {
    console.log("Effectué à chaque changement de la variable");
    return () => {};
  }, [value]);
```

#### useState

Le useState est un hook utilisé pour ajouter un état local aux composants.
Il retourne une paire [valeur, fonctionDeMiseAJour]  

```
  const [value, setValue] = useState<number>(0);
```

Il est important de noté que la fonction de mise à jour (setState) ne remplace par l'état immédiatement et que c'est une opération asynchrone. L'appel à au setState, planifie une mise à jour du composant et la valeur ne sera mise à jour qu'au rendu de celui ci.

Exemple ce code affichera 1 et non 2 car la valeur n'as pas encore été mise à jour.
```
      const [value, setValue] = useState<number>(0);

      setValue((prevValue) => prevValue + 1);
      setValue((prevValue) => prevValue + 1);
```

#### useRef

Ce hook permet de créer une référence mutable qui peut persister entre les rendu sans déclencher de render lorsqu'elle est modifiée. 
Elle est utile pour accéder à des élements du DOM directement.
```
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

Ou pour stocker des valeurs persistance.
```
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

### useId
Ce hook permet de générer des identifiants unique qui peuvent être utilisé dans les composants, particulièrement utile pour créer les attributs id qui sont unique dans le DOM.
```
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

Exemple d'un formulaire basique de login.
```
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

Afin de valider les données d'un formulaire, il est possible de passer par une bibliothèque comme Yup. Celle-ci permet d'établir un schéma avec les définitions des obligations des champs du formulaire.
Exemple de code
```
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
  password: Yup.string().required("Le mot de passe."),
});

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
          <input type="text" {...register("email", { required: true })} />
          {errors.email && <p>{errors.email.message}</p>}
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
### Requête HTTP avec Axios

Exemple de code pour un get avec Axios.
```
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

  
