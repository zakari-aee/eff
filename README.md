# GUIDE DE RÉVISION COMPLET — EFF DDOWFS (Développement Digital Web Full Stack)

> Analyse exhaustive de **15 examens** OFPPT EFF : 2023 (V1, V2, Rattrapage V1, Rattrapage V2), 2024 (V1, V2, Rattrapage V1, Rattrapage V2), 2025 (V1, V2, Report/Rattrapage), 2026 Blancs (GAHI-FIFA, DriveNow, AgriSmart, SmartParking).
>
> **Structure-type de l'examen** : Partie Théorique /40 + Partie Pratique /60 = **/100**, durée 4h00 (4h30 chez GAHI).
> - **Théorique** : D1 Cloud native (8 pts) · D2 Préparation projet web / UML (6 pts) · D3 Approche Agile + PERT + Git (7–15 pts) · D4 NoSQL/MongoDB (10–11 pts) · [D5 Web dynamique PHP (8 pts) — apparu en 2025-2026].
> - **Pratique** : D1 MySQL (10–12 pts) · D2 Front-End React/Redux (24 pts) · D3 Back-End Laravel (24–26 pts).
>
> **Échelle de fréquence utilisée** : 🔴 = quasi tous les examens (≥11/15) · 🟠 = très fréquent (7–10/15) · 🟡 = fréquent (4–6/15) · 🟢 = occasionnel (1–3/15).

---

# SECTION 1 — REACT / REDUX (Dossier Front-End, 24 pts)

Présent dans **les 15 examens**. C'est le dossier le plus rentable car répétitif. Deux variantes :
- **Variante A — store Redux fourni** (le plus courant) : on te donne `InitialState`, tu écris les composants qui **lisent** (`useSelector`) et **écrivent** (POST API ou `dispatch`). → 2023 V1/V2, 2024 V1/V2, 2024 Ratt., 2025 V1/V2, 2026 DriveNow/AgriSmart/SmartParking/GAHI.
- **Variante B — Redux à créer de zéro** : tu écris `initialState` + `reducer` + `actions` + `store`. → 2023 Ratt. V1/V2, 2025 Report, 2026 GAHI.

## 1.1 Topics classés par fréquence

| Rang | Concept | Fréq. |
|------|---------|-------|
| 1 | `App.js` : `BrowserRouter` + `Routes` + `Route` + `Provider` Redux + `<Menu/>` | 🔴 15/15 |
| 2 | `useSelector` pour lire le store | 🔴 13/15 |
| 3 | Composant **Menu** de navigation (`Link` / `NavLink`) | 🔴 13/15 |
| 4 | `.map()` pour afficher une liste + attribut **`key`** | 🔴 13/15 |
| 5 | Passage de **props** parent→enfant | 🔴 12/15 |
| 6 | `useState` (state local d'un formulaire) | 🔴 12/15 |
| 7 | Formulaire contrôlé : `value` + `onChange` (`handleChange`) + `onSubmit` (`handleSubmit`) + `<select>` | 🟠 11/15 |
| 8 | Appel API **GET** dans `useEffect` (`axios.get` / `fetch`) puis stockage dans `useState` | 🟠 10/15 |
| 9 | `useDispatch` + `dispatch(action(...))` | 🟠 9/15 |
| 10 | Appel API **POST** (`axios.post`) avec objet construit | 🟠 9/15 |
| 11 | `filter()` pour la recherche/filtrage | 🟠 8/15 |
| 12 | Composant **Détails** d'un objet unique lu du store | 🟠 9/15 |
| 13 | Calcul/simulateur (state local, calcul au clic, récapitulatif) | 🟡 6/15 |
| 14 | `reducer` + `action creators` + `configureStore`/`createStore` à écrire | 🟡 4/15 |
| 15 | `useParams` (détail par id dans l'URL) | 🟢 3/15 |
| 16 | Affichage conditionnel (« Aucun … sélectionné », barre de progression) | 🟢 3/15 |

## 1.2 Types de questions les plus répétés

1. **« Écrire le composant `DetailsXxx.js` qui lit depuis le store (useSelector) et affiche … »** — DetailsVelo, DetailsTrottinette, DetailsVoiture, DetailsSerre, DetailsParking, DetailsSatellite, DetailsBarrage, DetailsReservoir. (🔴)
2. **« Écrire `Menu.js` qui définit les liens vers les composantes … »** (tableau Lien/Composante/Titre). (🔴)
3. **« Écrire `App.js` : routage + Provider + appel de Menu. »** (🔴)
4. **« Écrire le composant `AjouterXxx.js` qui ajoute via l'API (POST) en remplissant le formulaire avec les données du store. »** (🟠)
5. **« Créer `ListeXxx.js` qui affiche la liste (depuis store ou props) avec filtre par statut + total + moyenne. »** (🟠)
6. **« Initialiser le state `xxx` de App.js en appelant l'API GET http://localhost:8000/… »** (🟠)
7. **« Créer le reducer / les actions / le store. »** (🟡 — surtout rattrapages)
8. **« Composant simulateur (Calcul_tarif / CalculTarif / Formulaire) : saisir, calculer au clic, afficher récap. »** (🟡)

## 1.3 Patterns exacts (templates prêts à recopier)

### A) `App.js` — LE squelette universel (rapporte le plus)
```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import { Provider } from "react-redux";
import { store } from "./store";          // store Redux
import Menu from "./Menu";
import DetailsVoiture from "./DetailsVoiture";
import ListeVoitures from "./ListeVoitures";
import AjouterLocation from "./AjouterLocation";
import { useState, useEffect } from "react";
import axios from "axios";

function App() {
  const [voitures, setVoitures] = useState([]);

  // Q "initialiser le state via GET"
  useEffect(() => {
    axios.get("http://localhost:8000/voitures")
         .then(res => setVoitures(res.data))
         .catch(err => console.log(err));
  }, []);

  return (
    <Provider store={store}>
      <BrowserRouter>
        <Menu />
        <Routes>
          <Route path="/DetailsVoiture" element={<DetailsVoiture />} />
          <Route path="/ListeVoitures"  element={<ListeVoitures voitures={voitures} />} />
          <Route path="/AjouterLocation" element={<AjouterLocation />} />
        </Routes>
      </BrowserRouter>
    </Provider>
  );
}
export default App;
```
> **Full marks** : il faut les 3 éléments — `<Provider store={store}>` ENGLOBE tout, `<BrowserRouter>`, et au moins une `<Route path element>`. Oublier le Provider = on perd les points Redux.

### B) `Menu.js` — navigation
```jsx
import { Link } from "react-router-dom";   // ou NavLink (DAIF 2026 demande NavLink)
function Menu() {
  return (
    <nav>
      <Link to="/DetailsVoiture">Détails Voiture</Link>
      <Link to="/ListeVoitures">Liste Voitures</Link>
      <Link to="/AjouterLocation">Nouvelle Location</Link>
    </nav>
  );
}
export default Menu;
```

### C) Composant **Détails** (lecture store via `useSelector`)
```jsx
import { useSelector } from "react-redux";
function DetailsVoiture() {
  const voiture = useSelector(state => state.voiture);   // adapter au nom du slice
  if (!voiture) return <p>Aucune voiture sélectionnée</p>;   // affichage conditionnel (GAHI)
  return (
    <div>
      <h1>Détails Voiture</h1>
      <p>Marque : {voiture.marque}</p>
      <p>Modèle : {voiture.modele}</p>
      <p>Matricule : {voiture.matricule}</p>
      <p>Prix/jour : {voiture.prix_jour}</p>
      <p>État : {voiture.etat}</p>
      <p>Agence : {voiture.Agence.nom} – {voiture.Agence.ville}</p>
    </div>
  );
}
export default DetailsVoiture;
```
> **Trap** : pour les objets imbriqués (`Agence`, `CapteurPrincipal`, `BatterieUtilisee`) il faut bien descendre dans l'objet (`voiture.Agence.ville`).

### D) Composant **Liste** (props ou store) + `.map()` + `key`
```jsx
function ListeVoitures({ voitures }) {           // reçoit en props
  return (
    <ul>
      {voitures.map(v => (
        <li key={v.id}>{v.matricule} – {v.marque} – {v.prix_jour} DH</li>
      ))}
    </ul>
  );
}
export default ListeVoitures;
```
> **Trap classique (perte de points garantie)** : oublier `key={...}` dans le `.map()`.

### E) Liste avec **filtre par statut** + total + moyenne (ListeBatteries 2025, ListeParkings 2026)
```jsx
import { useSelector } from "react-redux";
import { useState } from "react";
function ListeBatteries() {
  const batteries = useSelector(s => s.Batteries);
  const statuts   = useSelector(s => s.StatutBatterie);   // pour le <select>
  const [statut, setStatut] = useState("");
  const [resultat, setResultat] = useState([]);

  const filtrer = () => setResultat(batteries.filter(b => b.statut === statut));
  const moyenne = resultat.length
      ? resultat.reduce((a, b) => a + b.nombre_cycles, 0) / resultat.length : 0;

  return (
    <div>
      <select value={statut} onChange={e => setStatut(e.target.value)}>
        {statuts.map((s,i) => <option key={i} value={s}>{s}</option>)}
      </select>
      <button onClick={filtrer}>Filtrer</button>
      <table>
        <thead><tr><th>Id</th><th>N° Série</th><th>Statut</th><th>Cycles</th></tr></thead>
        <tbody>
          {resultat.map(b => (
            <tr key={b.id}><td>{b.id}</td><td>{b.numero_serie}</td><td>{b.statut}</td><td>{b.nombre_cycles}</td></tr>
          ))}
        </tbody>
      </table>
      <p>{resultat.length} batteries trouvées</p>
      <p>La moyenne des nombres de cycles est {moyenne}</p>
    </div>
  );
}
export default ListeBatteries;
```

### F) Composant **Ajouter** (POST API + données du store)
```jsx
import { useState } from "react";
import { useSelector } from "react-redux";
import axios from "axios";
function AjouterChangement() {
  const velo       = useSelector(s => s.velo);
  const batteries  = useSelector(s => s.Batteries);
  const techniciens= useSelector(s => s.Techniciens);
  const [form, setForm] = useState({
    id_nouvelle_batterie:"", date_changement:"", id_technicien:"", raison:""
  });
  const handleChange = e => setForm({ ...form, [e.target.name]: e.target.value });

  const handleSubmit = e => {
    e.preventDefault();
    axios.post("http://localhost:8000/api/AjouterChangement", {
      id_velo: velo.id,
      id_ancienne_batterie: velo.BatterieUtilisee.Id,
      id_nouvelle_batterie: form.id_nouvelle_batterie,
      date_changement: form.date_changement,
      id_technicien: form.id_technicien,
      raison: form.raison
    }).then(()=>alert("Ajouté"));
  };

  return (
    <form onSubmit={handleSubmit}>
      <p>Vélo : {velo.Matricule}</p>
      <p>Ancienne batterie : {velo.BatterieUtilisee.Numero_serie}</p>
      <select name="id_nouvelle_batterie" onChange={handleChange}>
        <option>-- Choisir une batterie --</option>
        {batteries.map(b => <option key={b.id} value={b.id}>{b.Numero_serie}</option>)}
      </select>
      <input type="date" name="date_changement" onChange={handleChange}/>
      <select name="id_technicien" onChange={handleChange}>
        <option>-- Choisir un technicien --</option>
        {techniciens.map(t => <option key={t.id} value={t.id}>{t.Nom}</option>)}
      </select>
      <textarea name="raison" onChange={handleChange}></textarea>
      <button type="submit">Ajouter</button>
    </form>
  );
}
export default AjouterChangement;
```
> **Full marks** : (1) `e.preventDefault()`, (2) les `<select>` peuplés par `.map()` depuis le store, (3) l'objet POST a **exactement** les clés demandées dans le tableau « Structure Objet ».

### G) **Reducer + actions + store** à écrire (Variante B)

Redux « classique » (2023 rattrapages, 2025 Report) :
```jsx
const initialState = { listeDonateurs: [] };

function donateurReducer(state = initialState, action) {
  switch (action.type) {
    case "AddDonateur":
      return { ...state, listeDonateurs: [...state.listeDonateurs, action.payload] };
    case "DeleteDonateur":
      return { ...state, listeDonateurs: state.listeDonateurs.filter(d => d.id !== action.payload) };
    default:
      return state;
  }
}
// actions
export const addDonateur    = (d)  => ({ type: "AddDonateur",    payload: d  });
export const deleteDonateur = (id) => ({ type: "DeleteDonateur", payload: id });
// store
import { createStore } from "redux";
export const store = createStore(donateurReducer);
```
Redux Toolkit (accepté, GAHI/2025 Report) :
```jsx
import { configureStore, createSlice } from "@reduxjs/toolkit";
const slice = createSlice({
  name: "donateurs",
  initialState: { listeDonateurs: [] },
  reducers: {
    addDonateur:    (s, a) => { s.listeDonateurs.push(a.payload); },
    deleteDonateur: (s, a) => { s.listeDonateurs = s.listeDonateurs.filter(d => d.id !== a.payload); }
  }
});
export const { addDonateur, deleteDonateur } = slice.actions;
export const store = configureStore({ reducer: slice.reducer });
```
Reducer « réducteur de flux » (GAHI : `ADD_FLUX`/`REMOVE_FLUX`/`SELECT_SATELLITE`) : même switch, on retrouve le satellite par `selectedSatelliteId` puis on push/filter dans son tableau `flux`.

### H) Simulateur / **CalculTarif** (state local, calcul au clic)
```jsx
import { useState } from "react";
function CalculTarif() {
  const tarif = 10;                       // tarif_horaire
  const [heures, setHeures] = useState(0);
  const [montant, setMontant] = useState(null);
  return (
    <div>
      <input type="number" value={heures} onChange={e => setHeures(e.target.value)} />
      <button onClick={() => setMontant(heures * tarif)}>Calculer</button>
      {montant !== null && <p>Montant à payer : {montant} DH</p>}
    </div>
  );
}
```
> Variante DriveNow `Formulaire.js` : nb_jours = (date_fin − date_debut), montant = nb_jours × prix_jour.

### I) `useDispatch` (ajouter via store, sans API)
```jsx
import { useDispatch, useSelector } from "react-redux";
const dispatch = useDispatch();
dispatch(addFlux({ ...form, satelliteId: selectedSatelliteId }));
```

### J) Redux Toolkit — Slice complet avec `createAsyncThunk` (CRUD API)
```jsx
import { createSlice, createAsyncThunk } from "@reduxjs/toolkit";
import axios from "axios";

const API_URL = "http://127.0.0.1:8000/api/stagiaire";

// ===== THUNKS ASYNCHRONES (CRUD) =====
export const getStagiaires = createAsyncThunk("stg/getAll", async () => {
  const response = await axios.get(API_URL);
  return response.data;
});
export const showStagiaire = createAsyncThunk("stg/getOne", async (id) => {
  const response = await axios.get(`${API_URL}/${id}`);
  return response.data;
});
export const createStagiaire = createAsyncThunk("stg/create", async (nouveauStg) => {
  const response = await axios.post(API_URL, nouveauStg);
  return response.data;
});
export const updateStagiaire = createAsyncThunk("stg/update", async (stgModifie) => {
  const response = await axios.put(`${API_URL}/${stgModifie.id}`, stgModifie);
  return response.data;
});
export const deleteStagiaire = createAsyncThunk("stg/delete", async (id) => {
  await axios.delete(`${API_URL}/${id}`);
  return id;
});

// ===== SLICE =====
const StagiaireSlice = createSlice({
  name: "stagiaireSlice",
  initialState: {
    stagiaires: [],
    details_stagiaire: null,
    loading: false,
    error: null,
    text_recherche: "",
  },
  reducers: {
    setTexteRecherche: (state, action) => {
      state.text_recherche = action.payload;
    },
  },
  extraReducers: (builder) => {
    builder
      .addCase(getStagiaires.pending, (state) => { state.loading = true; })
      .addCase(getStagiaires.fulfilled, (state, action) => {
        state.loading = false;
        state.stagiaires = action.payload;
      })
      .addCase(getStagiaires.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      })
      .addCase(showStagiaire.fulfilled, (state, action) => {
        state.details_stagiaire = action.payload;
      })
      .addCase(createStagiaire.fulfilled, (state, action) => {
        state.stagiaires.push(action.payload);
      })
      .addCase(updateStagiaire.fulfilled, (state, action) => {
        const index = state.stagiaires.findIndex(stg => stg.id === action.payload.id);
        if (index !== -1) state.stagiaires[index] = action.payload;
      })
      .addCase(deleteStagiaire.fulfilled, (state, action) => {
        state.stagiaires = state.stagiaires.filter(stg => stg.id !== action.payload);
      });
  },
});

export const { setTexteRecherche } = StagiaireSlice.actions;
export default StagiaireSlice.reducer;
```
> **Utilisation** : `dispatch(getStagiaires())` pour charger la liste, `dispatch(createStagiaire(data))` pour créer, etc. Gère automatiquement les 3 états (pending/fulfilled/rejected) du cycle de vie d'une requête API.

## 1.4 Ce qu'une réponse parfaite doit contenir
- **Tous les `import`** en haut (`react-redux`, `react-router-dom`, `axios`, `useState/useEffect`).
- `export default` du composant.
- Pour le store : `<Provider store={store}>` dans App.js.
- Pour chaque liste : `.map(... => <li key={...}>)`.
- Pour chaque formulaire : inputs contrôlés (`value`+`onChange`) **ou** au minimum `name`+`onChange`, et `onSubmit` avec `e.preventDefault()`.
- Pour POST : objet JSON avec **les clés EXACTES** du sujet, bonne URL `http://localhost:8000/api/...`.
- `useEffect(()=>{...}, [])` pour le GET initial (le tableau de dépendances **vide** est noté).

## 1.5 Pièges / erreurs fréquentes
- ❌ Oublier `key` dans `.map()`.
- ❌ Oublier `e.preventDefault()` → la page se recharge.
- ❌ Mettre l'appel `axios.get` directement dans le corps du composant (boucle infinie) au lieu de `useEffect`.
- ❌ Oublier `<Provider>` ou le mettre à l'intérieur du Router au mauvais niveau (le Provider doit englober).
- ❌ Confondre `useSelector` (lire) et `useDispatch` (écrire).
- ❌ Importer `BrowserRouter`/`useSelector` depuis le mauvais package.
- ❌ Modifier le state directement (`state.x = ...`) dans un reducer classique → toujours retourner un **nouvel** objet `{...state}` (sauf Redux Toolkit qui gère l'immutabilité via Immer).
- ❌ Dans les `<select>`, oublier de peupler les `<option>` depuis le store par `.map()`.

---

# SECTION 2 — LARAVEL (Dossier Back-End, 24–26 pts)

Présent dans **les 15 examens**. C'est très standardisé : migrations → modèles+relations → contrôleur → routes → vues blade.

## 2.1 Topics classés par fréquence

| Rang | Concept | Fréq. |
|------|---------|-------|
| 1 | Migration `up()` avec `Schema::create` (+ `down()` `dropIfExists`) | 🔴 15/15 |
| 2 | Commandes artisan (`make:migration`, `make:model`, `make:controller`, `make:seeder`, `migrate`, `db:seed`) | 🔴 14/15 |
| 3 | Modèle Eloquent : `$fillable` + relations | 🔴 14/15 |
| 4 | Relations `hasMany` / `belongsTo` (One-To-Many) | 🔴 13/15 |
| 5 | Routes `web.php` / `api.php` (+ `Route::resource`) | 🔴 14/15 |
| 6 | Contrôleur de ressource (`index`, `store`, `show`, `update`, `destroy`) | 🟠 11/15 |
| 7 | Clé étrangère dans migration (`foreignId`/`foreign`+`references`) | 🟠 10/15 |
| 8 | Vue Blade (`@foreach`, `@if`, `{{ }}`, liens modifier/supprimer) | 🟠 9/15 |
| 9 | Seeder + méthode `run()` | 🟡 6/15 |
| 10 | Validation (`$request->validate([...])` ou FormRequest) | 🟡 6/15 |
| 11 | `unique()` / `default()` / `index()` dans migration | 🟡 6/15 |
| 12 | Méthode custom du modèle/contrôleur (count, listBy, calcul) | 🟡 6/15 |
| 13 | Middleware (`make:middleware`, `handle`, abort 403, protéger une route) | 🟡 3/15 |
| 14 | Réponses **JSON** + codes HTTP (`response()->json(...,200/201/404)`) | 🟡 3/15 |
| 15 | `belongsToMany` + table pivot (`withPivot`) | 🟢 2/15 |
| 16 | `Http::get()` vers un microservice | 🟢 1/15 (GAHI) |

## 2.2 Types de questions les plus répétés
1. **« Donner le code des méthodes `up()` et `down()` de la migration de la table X »** (souvent avec `unique` sur un champ + `default` sur un autre). (🔴)
2. **« Donner les commandes artisan de création de … »** (migration / modèle / contrôleur / seeder). (🔴)
3. **« Créer les modèles X et Y avec leurs attributs et fonctions de relations. »** (🔴)
4. **« Créer le contrôleur de ressource XController (index/store/destroy/...). »** (🟠)
5. **« Donner la route `resource` / les routes web.php / api.php. »** (🔴)
6. **« Créer la vue `index.blade.php` / `show.blade.php` avec @foreach et liens. »** (🟠)
7. **« Créer un seeder + méthode `run()` pour ajouter un enregistrement. »** (🟡)
8. **« Méthode du contrôleur qui retourne une vue / du JSON / fait un calcul. »** (🟡)
9. **« Middleware autorisationSuppression + route protégée. »** (🟡 — 2025 V1/V2)

## 2.3 Patterns exacts

### A) Commandes artisan (à connaître par cœur)
```bash
php artisan make:migration create_voitures_table
php artisan make:migration add_couleur_to_voitures_table   # modification
php artisan make:model Voiture                 # +  -m pour migration, -c pour contrôleur, -mcr pour tout
php artisan make:controller VoitureController --resource
php artisan make:seeder VoitureSeeder
php artisan make:middleware AutorisationSuppression
php artisan migrate                            # appliquer les migrations
php artisan db:seed --class=VoitureSeeder      # exécuter un seeder
php artisan serve
php artisan route:list      # lister les routes
php artisan route:clear     # vider le cache des routes
```

### B) Migration `up()` / `down()` (avec contraintes)
```php
public function up() {
    Schema::create('voitures', function (Blueprint $table) {
        $table->id();
        $table->string('matricule')->unique();        // champ unique
        $table->string('marque');
        $table->string('modele');
        $table->decimal('prix_jour', 8, 2);
        $table->integer('kilometrage');
        $table->string('etat')->default('Disponible'); // valeur par défaut
        $table->foreignId('id_agence')->constrained('agences'); // clé étrangère
        $table->index('etat');                          // index
        $table->timestamps();
    });
}
public function down() {
    Schema::dropIfExists('voitures');
}
```
Migration de **modification** (ajout colonne) — nécessite `doctrine/dbal` pour `change()` :
```php
public function up() {
    Schema::table('voitures', function (Blueprint $table) {
        $table->string('couleur', 30)->nullable();
        // changer un type : $table->string('capacite', 15)->change();
    });
}
public function down() {
    Schema::table('voitures', function (Blueprint $table) {
        $table->dropColumn('couleur');
    });
}
```

### C) Modèle Eloquent + relations
```php
class Voiture extends Model {
    protected $table = 'voitures';                       // optionnel
    protected $fillable = ['matricule','marque','modele','prix_jour','etat','id_agence'];

    public function agence() {                           // N voitures -> 1 agence
        return $this->belongsTo(Agence::class, 'id_agence');
    }
    public function locations() {                         // 1 voiture -> N locations
        return $this->hasMany(Location::class, 'id_voiture');
    }
}

class Agence extends Model {
    public function voitures() {  return $this->hasMany(Voiture::class, 'id_agence'); }
}
```
Relation **belongsToMany** (table pivot — 2025 Report) :
```php
public function zones() {
    return $this->belongsToMany(ZoneAffectee::class, 'catastrophe_zone_affectee',
            'catastrophe_id', 'zone_affectee_id')->withPivot('gravite');
}
```

### D) Contrôleur de ressource
```php
class VoitureController extends Controller {
    public function index()  { $voitures = Voiture::all(); return view('voitures.index', compact('voitures')); }
    public function show($id){ $voiture = Voiture::findOrFail($id); return view('voitures.show', compact('voiture')); }
    public function store(Request $request) {
        $request->validate([
            'matricule' => 'required|unique:voitures',
            'marque'    => 'required',
            'prix_jour' => 'required|numeric',
        ]);
        Voiture::create($request->all());
        return redirect()->route('voitures.index');
    }
    public function destroy($id) {
        Voiture::destroy($id);
        return redirect()->route('voitures.index');
    }
}
```
Méthode renvoyant du **JSON** (API, GAHI/2025 Report) :
```php
public function show($id) {
    $voiture = Voiture::find($id);
    if (!$voiture) return response()->json(['message'=>'Non trouvée'], 404);
    return response()->json($voiture, 200);
}
```

### E) Routes
```php
// routes/web.php
use App\Http\Controllers\VoitureController;
Route::resource('voitures', VoitureController::class);
// routes générées par resource :
// GET    /voitures            index    voitures.index
// GET    /voitures/create     create   voitures.create
// POST   /voitures            store    voitures.store
// GET    /voitures/{id}       show     voitures.show
// GET    /voitures/{id}/edit  edit     voitures.edit
// PUT    /voitures/{id}       update   voitures.update
// DELETE /voitures/{id}       destroy  voitures.destroy

// routes/api.php
Route::get('/suivre/{id}', [DedouanementControleur::class, 'suivre_demande']);
Route::post('/AjouterChangement', [VeloController::class, 'AjouterChangement']);
// route protégée par middleware
Route::delete('/batteries/{id}', [VeloController::class,'SupprimerBatterie'])
     ->middleware('autorisationSuppression');
```

### F) Vue Blade `index.blade.php`
```blade
<table>
  <tr><th>Matricule</th><th>Marque</th><th>Action</th></tr>
  @foreach($voitures as $v)
    <tr>
      <td>{{ $v->matricule }}</td>
      <td>{{ $v->marque }}</td>
      <td>
        <a href="{{ route('voitures.edit', $v->id) }}">Modifier</a>
        <form action="{{ route('voitures.destroy', $v->id) }}" method="POST">
          @csrf @method('DELETE')
          <button type="submit">Supprimer</button>
        </form>
      </td>
    </tr>
  @endforeach
</table>
```

### G) Seeder
```php
class VoitureSeeder extends Seeder {
    public function run() {
        Voiture::create([
            'matricule'=>'12345-A-6','marque'=>'Renault','modele'=>'Clio',
            'prix_jour'=>300,'etat'=>'Disponible','id_agence'=>1
        ]);
    }
}
```

### H) Middleware (2025 V1/V2)
```php
public function handle(Request $request, Closure $next) {
    if (auth()->user()->name !== 'toto') {            // ou contrôle horaire 8h30–18h30
        abort(403, "vous n'êtes pas autorisé à effectuer cette opération de suppression");
    }
    return $next($request);
}
```
Enregistrer l'alias dans `app/Http/Kernel.php` → `'autorisationSuppression' => \App\Http\Middleware\AutorisationSuppression::class`.

### I) Communication microservice (GAHI)
```php
use Illuminate\Support\Facades\Http;
public function syncFluxFromMicroservice($idSat) {
    $reponse = Http::get("http://localhost:3000/api/satellites/$idSat/flux");
    foreach ($reponse->json() as $flux) { AssignationFlux::create($flux); }
}
// Résilience : timeout + try/catch + file d'attente (RabbitMQ) ou cache local.
```

## 2.4 Ce qu'une réponse parfaite doit contenir
- Migration : `Schema::create('table_au_pluriel', function(Blueprint $table){...})`, `$table->id()`, types corrects, `unique()/default()/foreignId()->constrained()`, **et `down()` avec `dropIfExists`**.
- Modèle : `protected $fillable = [...]` **+** méthodes de relation avec le **bon sens** (`belongsTo` côté « plusieurs », `hasMany` côté « un ») et la clé étrangère explicite.
- Contrôleur : `return view(...)` ou `return redirect()->route(...)` ou `response()->json(..., code)`.
- Routes : `use` du contrôleur en haut, syntaxe `[Controller::class, 'methode']`.
- Blade : `@foreach ... @endforeach`, `{{ $var }}`, `@csrf` + `@method('DELETE')` dans les forms de suppression.

## 2.5 Pièges / erreurs fréquentes
- ❌ Inverser `hasMany` et `belongsTo`.
- ❌ Oublier `$fillable` → erreur « MassAssignmentException » à la création.
- ❌ Oublier `down()` (souvent ½ des points de la question).
- ❌ Nom de table au singulier dans `Schema::create` (Laravel attend le **pluriel**).
- ❌ Oublier `@csrf` / `@method('DELETE')` dans les formulaires Blade.
- ❌ Oublier le `use App\Http\Controllers\...` dans les fichiers de routes.
- ❌ Confondre `web.php` (vues, session, CSRF) et `api.php` (JSON, stateless) — un endpoint qui renvoie du JSON va dans **api.php**.
- ❌ `Route::resource` mais méthodes nommées différemment dans le contrôleur (les noms `index/show/store/update/destroy` sont imposés).

---

# SECTION 3 — MySQL (Dossier Pratique D1, 10–12 pts)

Présent dans **les 15 examens**. Le bloc « fonction + trigger + gestion des rôles » est quasi systématique depuis 2024.

## 3.1 Topics classés par fréquence

| Rang | Concept | Fréq. |
|------|---------|-------|
| 1 | `CREATE TABLE` avec `PRIMARY KEY` + `FOREIGN KEY` | 🔴 14/15 |
| 2 | **TRIGGER** (`BEFORE INSERT`, blocage via `SIGNAL`) | 🔴 13/15 |
| 3 | **Fonction stockée** (`CREATE FUNCTION … RETURNS …`) | 🔴 12/15 |
| 4 | **Gestion utilisateurs/rôles** (`CREATE ROLE`, `GRANT`, `CREATE USER`, `GRANT role TO user`) | 🟠 10/15 |
| 5 | **Procédure stockée** (`CREATE PROCEDURE`, souvent COUNT par groupe) | 🟠 9/15 |
| 6 | `SELECT` + `GROUP BY` + `COUNT/SUM/AVG` | 🟠 9/15 |
| 7 | `ALTER TABLE … ADD COLUMN` | 🟠 7/15 |
| 8 | `SELECT` + `JOIN` + `WHERE` + `ORDER BY` | 🟡 6/15 |
| 9 | Trigger qui **utilise une fonction** | 🟡 3/15 |

## 3.2 Types de questions les plus répétés
1. **« Écrire le script de création de la table X. »** (🔴)
2. **« Écrire un trigger qui empêche/contrôle … lors de l'insertion. »** (🔴) — contrôle d'intervalle (0–100), blocage selon une condition métier.
3. **« Écrire une fonction qui reçoit … et retourne … (1/0, un id, un total). »** (🔴)
4. **« Gestion des rôles : créer rôle, donner droits, créer utilisateur, attribuer le rôle. »** (🟠)
5. **« Écrire une procédure qui affiche le nombre de … par … »** (🟠)
6. **« Afficher, pour chaque X, la moyenne/somme/nombre de Y. »** (`GROUP BY`) (🟠)
7. **« Modifier la table X en ajoutant la colonne … »** (🟠)

## 3.3 Patterns exacts

### A) CREATE TABLE + clés
```sql
CREATE TABLE Voiture (
  id INT AUTO_INCREMENT PRIMARY KEY,
  matricule VARCHAR(20) NOT NULL UNIQUE,
  marque VARCHAR(50),
  prix_jour DECIMAL(8,2),
  etat VARCHAR(20) DEFAULT 'Disponible',
  id_agence INT,
  FOREIGN KEY (id_agence) REFERENCES Agence(id)
);
-- ajout d'une colonne
ALTER TABLE Voiture ADD COLUMN couleur VARCHAR(30);
ALTER TABLE Vehicule ADD COLUMN nb_batteries INT UNSIGNED NOT NULL;
```

### B) TRIGGER de blocage (le plus demandé)
```sql
DELIMITER //
CREATE TRIGGER TR_charge
BEFORE INSERT ON Capteur
FOR EACH ROW
BEGIN
  IF NEW.niveau_charge < 0 OR NEW.niveau_charge > 100 THEN
     SIGNAL SQLSTATE '45000'
       SET MESSAGE_TEXT = 'Niveau de charge invalide (0-100)';
  END IF;
END //
DELIMITER ;
```
Trigger qui **modifie une autre valeur** (2025 : passer statut « En Utilisation » → « Retiré » lors d'un ajout dans Changement) :
```sql
DELIMITER //
CREATE TRIGGER TR2 AFTER INSERT ON Changement
FOR EACH ROW
BEGIN
  UPDATE Batterie SET statut = 'Retiré'
  WHERE id = NEW.id_ancienne_batterie AND statut = 'En Utilisation';
END //
DELIMITER ;
```
Trigger qui **utilise une fonction** (SmartParking, 2024 Ratt.) :
```sql
DELIMITER //
CREATE TRIGGER TR_anomalie BEFORE INSERT ON Anomalie
FOR EACH ROW
BEGIN
  IF fct_batterieFaible(NEW.id_capteur) = 1 THEN
     SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Batterie critique';
  END IF;
END //
DELIMITER ;
```

### C) Fonction stockée
```sql
DELIMITER //
CREATE FUNCTION fct_batterieFaible(p_id INT) RETURNS INT
DETERMINISTIC
BEGIN
  DECLARE v_charge INT;
  SELECT etat_batterie INTO v_charge FROM Capteur WHERE id = p_id;
  IF v_charge < 20 THEN RETURN 1; ELSE RETURN 0; END IF;
END //
DELIMITER ;
```
Fonction qui retourne un **id** selon plusieurs critères (2023 : code salarié via voiture+date ; DriveNow : client louant une voiture à une date) :
```sql
DELIMITER //
CREATE FUNCTION fct_clientLocation(p_matricule VARCHAR(20), p_date DATE) RETURNS INT
DETERMINISTIC
BEGIN
  DECLARE v_client INT;
  SELECT L.id_client INTO v_client
  FROM Location L JOIN Voiture V ON L.id_voiture = V.id
  WHERE V.matricule = p_matricule AND p_date BETWEEN L.date_debut AND L.date_fin
  LIMIT 1;
  RETURN v_client;
END //
DELIMITER ;
```

### D) Procédure stockée (COUNT par groupe)
```sql
DELIMITER //
CREATE PROCEDURE P_capteursParStatut()
BEGIN
  SELECT statut, COUNT(*) AS nombre FROM Capteur GROUP BY statut;
END //
DELIMITER ;
-- appel : CALL P_capteursParStatut();
```

### E) Gestion utilisateurs / rôles (bloc systématique)
```sql
CREATE ROLE RoleAgent;
GRANT INSERT, UPDATE, DELETE ON ma_base.Voiture   TO RoleAgent;
GRANT INSERT, UPDATE, DELETE ON ma_base.Location  TO RoleAgent;
CREATE USER 'karim'@'localhost' IDENTIFIED BY '1234';
GRANT RoleAgent TO 'karim'@'localhost';
-- (parfois) SET DEFAULT ROLE RoleAgent TO 'karim'@'localhost';
```

### F) SELECT / JOIN / GROUP BY
```sql
-- moyenne par zone (SmartParking)
SELECT Z.nom, AVG(P.taux_occupation) AS moyenne
FROM Zone Z JOIN Parking P ON P.id_zone = Z.id
GROUP BY Z.id, Z.nom;

-- nombre de caméras par satellite (GAHI)
SELECT S.nom, COUNT(C.idCam) AS nb
FROM Satellite S LEFT JOIN Flux F ON F.idSat = S.idSat
                 LEFT JOIN Camera C ON C.idCam = F.idCam
GROUP BY S.idSat, S.nom;
```

## 3.4 Ce qu'une réponse parfaite doit contenir
- `DELIMITER //` … `END //` `DELIMITER ;` autour de **chaque** trigger/fonction/procédure.
- Trigger : `BEFORE`/`AFTER` correct, `FOR EACH ROW`, `NEW.colonne`, blocage par `SIGNAL SQLSTATE '45000'`.
- Fonction : `RETURNS type`, `DECLARE` des variables, `SELECT ... INTO var`, `RETURN`.
- Rôles : les **4 étapes** (créer rôle → GRANT droits → créer user → GRANT rôle à user).
- `GROUP BY` sur la bonne colonne quand on agrège (`COUNT/SUM/AVG`).

## 3.5 Pièges / erreurs fréquentes
- ❌ Oublier `DELIMITER` → erreur de syntaxe sur le `;` interne.
- ❌ Utiliser `OLD` au lieu de `NEW` dans un trigger d'insertion (et inversement pour DELETE).
- ❌ Oublier `FOR EACH ROW`.
- ❌ Fonction sans `RETURNS`/`RETURN`, ou sans `DETERMINISTIC` (peut être refusée selon la config).
- ❌ Oublier `INTO variable` dans le `SELECT` d'une fonction.
- ❌ `GROUP BY` manquant alors qu'on a `COUNT/SUM/AVG` avec d'autres colonnes.
- ❌ Mauvais ordre des étapes de rôles, ou GRANT sans préciser la base/table.
- ❌ Oublier `IDENTIFIED BY '...'` à la création de l'utilisateur.

---

# SECTION 4 — MongoDB / NoSQL (Dossier Théorique D4, 10–11 pts)

Présent dans **les 15 examens**. Données souvent **imbriquées** (sous-documents + tableaux) depuis 2024.

## 4.1 Topics classés par fréquence

| Rang | Concept | Fréq. |
|------|---------|-------|
| 1 | Création BD (`use`) + collection + `insertOne`/`insertMany` | 🔴 14/15 |
| 2 | `find()` avec filtre + `sort()` (1 / -1) | 🔴 14/15 |
| 3 | `deleteOne` / `deleteMany` | 🔴 13/15 |
| 4 | Comptage (`countDocuments` / `find().count()`) | 🟠 11/15 |
| 5 | `updateOne` / `updateMany` avec `$set` | 🟠 11/15 |
| 6 | Projection (2e argument de `find`) | 🟠 8/15 |
| 7 | Agrégation `$group` + `$sum`/`$avg` | 🟠 8/15 |
| 8 | Filtres conditionnels (`$gt`,`$lt`,`$gte`,`$lte`,`$ne`,`$in`,`$nin`) | 🟠 9/15 |
| 9 | `$exists` (champ présent/absent) | 🟡 4/15 |
| 10 | Tri + `skip` + `limit` (Nième plus grand) | 🟡 3/15 |
| 11 | `$elemMatch` (filtrer dans un tableau) | 🟡 3/15 |
| 12 | `$push` / `$pull` (modifier un tableau imbriqué) | 🟢 2/15 |
| 13 | `$unwind` (+ top N par groupe) | 🟢 2/15 |
| 14 | Opérateur positionnel `$` (maj élément d'un tableau) | 🟢 1/15 |
| 15 | Export JSON (`mongoexport`) | 🟢 1/15 |

## 4.2 Types de questions les plus répétés
1. **« Créer la base X et la collection Y, puis insérer le(s) document(s). »** (🔴)
2. **« Afficher les … triés par … (croissant/décroissant). »** (`sort`) (🔴)
3. **« Afficher le nombre de … dont … = … »** (`countDocuments`) (🟠)
4. **« Mettre à jour le champ … du document dont _id = … »** (`updateOne $set`) (🟠)
5. **« Supprimer les … dont … > … »** (`deleteMany`) (🔴)
6. **« Afficher seulement les attributs A, B, C de … »** (projection) (🟠)
7. **« Pour chaque X, calculer la moyenne/somme/nombre de Y. »** (aggregate `$group`) (🟠)
8. **« Afficher les documents où le champ Z n'existe pas / existe. »** (`$exists`) (🟡)
9. **« Le 3ᵉ plus grand … »** (sort desc + skip 2 + limit 1) (🟡)

## 4.3 Patterns exacts

### A) Création + insertion
```js
use AgriDB
db.createCollection("capteurs")
db.capteurs.insertOne({ _id:"c1", numero_serie:"CAP-100", type:"Humidité", niveau_charge:85, statut:"En service" })
db.capteurs.insertMany([ {…}, {…} ])
```

### B) find + filtre + sort + projection
```js
// tri décroissant par niveau de charge
db.capteurs.find().sort({ niveau_charge: -1 })
// filtre + projection (1 = afficher, 0 = cacher ; _id:0 pour le masquer)
db.flux.find({ debitMbps: { $gt: 50 } }, { numero_serie:1, capacite:1, statut:1, _id:0 })
// comptage
db.capteurs.countDocuments({ type: "Humidité" })
```

### C) update / delete
```js
db.capteurs.updateOne({ _id:"c3" }, { $set:{ statut:"Défaillant" } })
db.batteries.updateMany({ statut:"en panne", capacite:{ $lt:50 } }, { $set:{ sante_batterie:"faible" } })
db.capteurs.deleteOne({ _id:"c1" })
db.voitures.deleteMany({ kilometrage:{ $gt:200000 } })
```

### D) Agrégation `$group`
```js
// moyenne des cycles par statut
db.batteries.aggregate([
  { $group: { _id:"$statut", moyenne:{ $avg:"$nombre_cycles" } } }
])
// nombre de flux par satellite
db.flux.aggregate([ { $group:{ _id:"$_id_satellite", total:{ $sum:1 } } } ])
// somme d'un champ
db.sorties.aggregate([ { $group:{ _id:null, totale:{ $sum:"$QuantiteTotale" } } } ])
```

### E) `$exists`, Nième plus grand
```js
// sorties SANS le champ TotalProductionElectricite
db.sorties.find({ TotalProductionElectricite: { $exists:false } })
// 3ᵉ plus grande QuantiteTotale
db.sorties.find().sort({ QuantiteTotale:-1 }).skip(2).limit(1)
```

### F) Tableaux imbriqués : `$elemMatch`, `$push`, `$pull`, positionnel
```js
// zones avec au moins une infra "en cours" (projection champs + tableau filtré)
db.zones_affectees.find(
  { "infrastructures.statut_reconstruction":"en cours" },
  { nom_zone:1, superficie_touchee:1, infrastructures:1 }
)
// ajouter un flux dans le tableau fluxVideo
db.satellites.updateOne({ _id:"SAT-MOROCCO-01" }, { $push:{ fluxVideo: { idFlux:"FLUX-003", debitMbps:60 } } })
// supprimer les flux latence>150 d'un satellite
db.satellites.updateOne({ _id:"SAT-MOROCCO-01" }, { $pull:{ fluxVideo:{ latenceMs:{ $gt:150 } } } })
// maj d'un élément précis du tableau (opérateur positionnel $)
db.satellites.updateOne(
  { _id:"SAT-MOROCCO-01", "fluxVideo.idFlux":"FLUX-001" },
  { $set:{ "fluxVideo.$.debitMbps":90 } }
)
```

### G) `$unwind` + top N par groupe (GAHI)
```js
db.satellites.aggregate([
  { $unwind:"$fluxVideo" },
  { $sort:{ "fluxVideo.debitMbps":-1 } },
  { $group:{ _id:"$nom", flux:{ $push:{ idFlux:"$fluxVideo.idFlux", debit:"$fluxVideo.debitMbps" } } } },
  { $project:{ flux:{ $slice:["$flux", 3] } } }
])
```

## 4.4 Ce qu'une réponse parfaite doit contenir
- `use BaseDeDonnees` avant les requêtes (souvent attendu à la Q1).
- Le **bon opérateur** : `$set` pour update, `$gt/$lt/...` pour comparer, `$exists` pour présence.
- `sort({ champ: 1 })` croissant / `-1` décroissant.
- Projection : 2ᵉ argument de `find`, `_id:0` pour le masquer.
- Agrégation : tableau `[ {étape}, {étape} ]`, `_id` = critère de groupe (`"$champ"` avec le `$`).

## 4.5 Pièges / erreurs fréquentes
- ❌ Oublier `$set` dans un update → cela **remplace** tout le document.
- ❌ Oublier le `$` devant le champ dans `$group` (`_id:"$statut"`, pas `"statut"`).
- ❌ Confondre `deleteOne` (un seul) et `deleteMany` (tous).
- ❌ Projection : mélanger 1 et 0 (interdit, sauf pour `_id`).
- ❌ Pour le « Nième plus grand » : oublier `skip(N-1)`.
- ❌ `$elemMatch` quand plusieurs conditions portent sur le **même** élément d'un tableau.
- ❌ Comptage : `find().count()` est OK mais `countDocuments({filtre})` est la forme moderne attendue.

---

# SECTION 5 — Docker / Cloud Native (Dossier Théorique D1, 8 pts)

Présent dans **les 15 examens**. Mix de définitions (cours) + commandes Docker.

## 5.1 Topics classés par fréquence

| Rang | Concept | Fréq. |
|------|---------|-------|
| 1 | Conteneur vs Machine Virtuelle | 🔴 11/15 |
| 2 | Commandes Docker (pull / images / rmi / run / ps / stop / rm) | 🔴 12/15 |
| 3 | Types de cloud (public / privé / hybride) + fournisseurs | 🟠 9/15 |
| 4 | Microservices (définition, avantage, vs API/monolithe) | 🟠 9/15 |
| 5 | Avantages du cloud native / de la conteneurisation | 🟠 7/15 |
| 6 | Services cloud : SaaS / PaaS / IaaS | 🟡 6/15 |
| 7 | `docker run -p` (mapping de ports) | 🟡 5/15 |
| 8 | DevOps (principes, CI/CD) | 🟡 5/15 |
| 9 | Outils : RabbitMQ, Postman, Express.js, Node.js, Azure | 🟡 6/15 |
| 10 | Kubernetes (orchestration), Sharding/scaling MongoDB | 🟢 2/15 (GAHI) |

## 5.2 Commandes Docker à connaître par cœur

| Besoin | Commande |
|--------|----------|
| Télécharger une image depuis Docker Hub | `docker pull mongo` |
| Lister les images locales | `docker images` |
| Supprimer une image | `docker rmi nom_image` |
| Créer + démarrer un conteneur | `docker run nom_image` |
| Avec mapping de port | `docker run -p 8080:8080 mon_app` |
| En mode détaché + nom + variables | `docker run -d --name db -e MYSQL_DATABASE=x -p 3306:3306 mysql:8.0` |
| Lister les conteneurs **actifs** | `docker ps` |
| Lister **tous** les conteneurs | `docker ps -a` |
| Arrêter un conteneur | `docker stop <id>` |
| Supprimer un conteneur arrêté | `docker rm <id>` |
| Forcer la suppression d'un conteneur en cours | `docker rm -f <id>` |

**Explication des options `docker run`** (GAHI) : `-d` = détaché (arrière-plan) · `--name` = nommer le conteneur · `-e` = variable d'environnement · `-p hôte:conteneur` = mapping de port.

## 5.3 Définitions-clés (réponses-types)
- **Conteneur vs VM** : la VM virtualise le **matériel** (un OS invité complet par VM, lourde). Le conteneur virtualise au niveau **OS** (partage le noyau de l'hôte, isole seulement l'appli + ses dépendances → léger, démarrage rapide, portable).
- **Cloud public** : ressources mutualisées chez un fournisseur (AWS, Azure, GCP). **Privé** : infra dédiée à une seule organisation. **Hybride** : combinaison public + privé.
- **Fournisseurs** : Amazon AWS, Microsoft Azure, Google Cloud Platform, OVH, IBM Cloud.
- **SaaS** (logiciel prêt : Gmail, Office365) / **PaaS** (plateforme de dev : Heroku, App Engine) / **IaaS** (infra : EC2, VM Azure).
- **Microservice** : application découpée en petits services indépendants, déployables séparément, communiquant par API/messagerie. Avantages : scalabilité ciblée, déploiement indépendant, résilience, technologies hétérogènes.
- **Avantages conteneurisation/cloud native** : portabilité, isolation, démarrage rapide, scalabilité automatique (montée en charge), densité (plus d'apps par serveur), CI/CD facilité.
- **RabbitMQ** : broker de messages (file d'attente) pour communication **asynchrone** entre microservices ; protocole natif **AMQP** ; ex. : mettre en file les flux vidéo à traiter.
- **DevOps** : culture d'automatisation + collaboration Dev/Ops. Principes : intégration/livraison continues (CI/CD), automatisation, monitoring, collaboration. **CI** = intégration continue (build + tests auto à chaque commit).
- **Kubernetes** : orchestrateur de conteneurs (scaling auto, auto-réparation, load-balancing) — plus adapté qu'une VM pour une appli scalable de flux vidéo.
- **Scaling MongoDB** : vertical = augmenter la puissance d'une machine ; horizontal = répartir sur plusieurs machines via **sharding** (partitionnement des données par clé de shard).

## 5.4 Pièges
- ❌ Confondre `docker rmi` (image) et `docker rm` (conteneur).
- ❌ Confondre `docker ps` (actifs) et `docker ps -a` (tous).
- ❌ Inverser le sens du mapping `-p` (c'est **hôte:conteneur**).
- ❌ Dire « le conteneur a son propre OS » — non, il **partage le noyau** de l'hôte.
- ❌ Confondre SaaS/PaaS/IaaS dans les exemples.

---

# SECTION 6 — Agile / PERT / Git (Dossier Théorique D3, 7–15 pts)

Présent dans **les 15 examens**. C'est le dossier qui rapporte le plus de points en théorie (jusqu'à 15 pts).

## 6.1 Topics classés par fréquence

| Rang | Concept | Fréq. |
|------|---------|-------|
| 1 | **PERT** : dates au plus tôt/tard, chemin critique, durée | 🔴 11/15 |
| 2 | **Chemin critique** | 🔴 13/15 |
| 3 | Rôles Scrum (PO / Scrum Master / Équipe de dev) | 🔴 11/15 |
| 4 | Commandes Git (init, add, commit, push, pull, clone, branch, checkout -b, merge, status) | 🔴 12/15 |
| 5 | Durée projet en **jours ouvrés** (week-ends exclus) | 🟠 7/15 |
| 6 | Agile vs méthode classique (cycle en V) | 🟠 7/15 |
| 7 | SonarQube (qualité du code : bugs, vulnérabilités, code smells) | 🟠 7/15 |
| 8 | User stories (définition + exemples + backlog) | 🟠 7/15 |
| 9 | Cérémonies/rituels Scrum (sprint planning, daily, review, rétro) | 🟡 6/15 |
| 10 | Artefacts Scrum (product backlog, sprint backlog, increment) | 🟡 5/15 |
| 11 | Outils DevOps / CI-CD (Jenkins, GitLab CI, Docker, Jira…) | 🟡 6/15 |
| 12 | **GANTT** (durée ouvrable, dates) | 🟡 4/15 |
| 13 | Ordonner les étapes d'un projet | 🟡 3/15 |
| 14 | Citer 2 méthodes de gestion de projet | 🟢 2/15 |

## 6.2 PERT — méthode pas à pas (la question qui rapporte)

**Données type** : tableau Tâche / Durée / Antériorités.

1. **Dates au plus tôt (ES/EF)** — parcours **avant** : `ES(tâche) = max(EF des prédécesseurs)`, `EF = ES + durée`. Départ à 0.
2. **Dates au plus tard (LS/LF)** — parcours **arrière** depuis la fin : `LF = min(LS des successeurs)`, `LS = LF − durée`.
3. **Marge** = `LS − ES` (ou `LF − EF`). **Marge = 0 ⇒ tâche critique.**
4. **Chemin critique** = enchaînement des tâches à marge nulle (le plus long chemin).
5. **Durée du projet** = EF de la dernière tâche = somme des durées sur le chemin critique.

**Exemple résolu (DriveNow)** : A(2,-) B(3,A) C(2,B) D(4,B) E(2,C&D) F(1,E).
- ES : A=0→2 ; B=2→5 ; C=5→7 ; D=5→9 ; E=max(7,9)=9→11 ; F=11→12.
- **Chemin critique : A → B → D → E → F**, **durée = 12 jours** (C a une marge de 2 : 9−7).

**Jours ouvrés (week-ends exclus)** — quand on donne une date de début (ex. Lundi) : on convertit la durée en jours ouvrés en **sautant samedi/dimanche**. Ex. début lundi + 12 j ouvrés ⇒ on compte 5 j/semaine. (2024 Rattrapages & GAHI insistent là-dessus : « durée en jours ouvrés, week-ends exclus ».) Attention aussi aux **périodes de formation/indisponibilité** à exclure (2025 Report : formation du 26/06 au 01/07).

## 6.3 Git — commandes à connaître

| Besoin | Commande |
|--------|----------|
| Initialiser un dépôt local | `git init` |
| Ajouter des fichiers à l'index | `git add .` (ou `git add fichier`) |
| Enregistrer (commit) | `git commit -m "message"` |
| Envoyer vers le remote | `git push origin main` |
| Récupérer/mettre à jour | `git pull` |
| Cloner | `git clone <url>` |
| État | `git status` |
| Créer une branche | `git branch B1` |
| Créer + basculer | `git checkout -b B1` |
| Fusionner une branche dans la courante | `git merge feature/x` |
| Pousser vers une branche distante | `git push origin Master` |

## 6.4 Réponses-types (cours)
- **3 rôles Scrum** : Product Owner (priorise le backlog, voix du client) · Scrum Master (facilite, lève les obstacles) · Équipe de développement (réalise l'increment).
- **3 artefacts** : Product Backlog · Sprint Backlog · Increment (produit potentiellement livrable).
- **Cérémonies** : Sprint Planning · Daily Scrum (mêlée quotidienne) · Sprint Review · Sprint Rétrospective.
- **User story** : « En tant que [rôle], je veux [fonctionnalité] afin de [bénéfice]. » (≠ tâche, qui est technique et découpée).
- **Agile vs cycle en V** : Agile = itératif/incrémental, livraisons fréquentes, adaptatif au changement, client impliqué en continu. Cycle en V/classique = séquentiel, spécifications figées au départ, livraison tardive, peu flexible.
- **SonarQube** : analyse statique de la qualité du code → détecte **bugs** (Reliability), **vulnérabilités** (Security), **code smells** (Maintainability), duplication, couverture de tests.
- **Outils DevOps/CI-CD** : Jenkins, GitLab CI, GitHub Actions, Docker, Kubernetes, Ansible, Jira (suivi), SonarQube (qualité).
- **Ordre des étapes d'un projet** : Cahier des charges → Analyse → Conception → Réalisation (développement) → Tests → Mise en production.
- **2 méthodes de gestion de projet** : Scrum, Kanban (ou : classique/cascade, cycle en V).

## 6.5 Pièges
- ❌ PERT : prendre le **min** au lieu du **max** pour les dates au plus tôt (toujours `max` des prédécesseurs en avant ; `min` des successeurs en arrière).
- ❌ Oublier d'exclure les week-ends quand une date de début est donnée.
- ❌ Donner un seul chemin alors qu'il peut y en avoir plusieurs critiques.
- ❌ Confondre **rôles** (3), **artefacts** (3) et **cérémonies** (4) du Scrum.
- ❌ `git checkout -b` (crée + bascule) vs `git checkout` (bascule seulement) vs `git branch` (crée seulement).
- ❌ Confondre `git pull` (fetch+merge) et `git clone` (1ère copie).

---

# SECTION 7 — UML / Diagrammes (Dossier Théorique D2, 6 pts)

Présent dans **les 15 examens**, sous 3 formes. Évolution nette : **diagramme de classes** (2023) → **diagramme de cas d'utilisation** (2024 → 2026, devenu dominant) + **MCD** (2025 Report).

## 7.1 Topics classés par fréquence

| Rang | Concept | Fréq. |
|------|---------|-------|
| 1 | **Diagramme de cas d'utilisation** (acteurs + cas + relations) | 🔴 10/15 |
| 2 | Relation **`<<include>>`** (souvent pour l'authentification obligatoire) | 🔴 9/15 |
| 3 | Acteurs avec **héritage/généralisation** (Admin hérite des cas de l'Agent) | 🟠 8/15 |
| 4 | Diagramme de **classes** (depuis schéma relationnel) | 🟡 5/15 |
| 5 | Relation **`<<extend>>`** (cas optionnel) | 🟡 4/15 |
| 6 | **MCD** (Modèle Conceptuel de Données) | 🟢 1/15 |
| 7 | Cardinalités / multiplicités | 🟢 3/15 |

## 7.2 Diagramme de cas d'utilisation — méthode

**Pattern récurrent du sujet** : « Le système sera utilisé par N acteurs : Administrateur, X, Y. L'acteur X peut … L'administrateur peut **tout ce que fait** X, **et en plus** … L'accès est sécurisé : chaque utilisateur doit **s'authentifier**. »

**Traduction UML** :
1. **Acteurs** = les rôles cités (Administrateur, Agent, Client/Technicien/…).
2. **Cas d'utilisation** = chaque verbe d'action (« consulter la liste », « ajouter une voiture », « enregistrer une location »…).
3. **« L'admin peut tout ce que fait l'agent + plus »** ⇒ **généralisation** : Administrateur ──▷ Agent (flèche triangle creux). L'admin hérite des cas de l'agent et a ses cas propres.
4. **« chaque utilisateur doit s'authentifier »** ⇒ tous les cas (ou un cas central) ont une relation **`<<include>>`** vers le cas « S'authentifier ».
5. **Cas optionnel/conditionnel** (« supprimer **après confirmation** ») ⇒ **`<<extend>>`** vers « Confirmer la suppression ».

**Schéma textuel type (DriveNow)** :
```
        ┌──────────────────────────── Système DriveNow ───────────────────────────┐
Client ─┤ (Consulter voitures dispo)  (Réserver en ligne) ───<<include>>──▶ (S'authentifier)
Agent ──┤ (Enregistrer location) (Consulter voitures) (Marquer rendue) ──<<include>>─▶ (S'authentifier)
Admin ──┤ ▷Agent + (Gérer voitures: ajouter/supprimer) (Gérer agences)              │
        └──────────────────────────────────────────────────────────────────────────┘
Admin ──▷ Agent   (généralisation : l'admin fait tout ce que fait l'agent)
```

## 7.3 Diagramme de classes (depuis schéma relationnel — 2023)
- Chaque **table → classe** ; colonnes → attributs (avec types).
- Clé étrangère → **association** ; cardinalités : `1..*` côté « plusieurs », `1` côté « un ».
- Table d'association (pivot) → association **plusieurs-à-plusieurs** (ou classe d'association avec attributs du pivot, ex. `gravité`).

## 7.4 Ce qu'une réponse parfaite doit contenir
- **Tous** les acteurs cités, dessinés à l'extérieur du rectangle « système ».
- **Tous** les cas (un par fonctionnalité du texte), dans le rectangle.
- La **généralisation** entre Admin et l'autre acteur (sinon on recopie tous les cas — accepté mais lourd).
- Le **`<<include>>` S'authentifier** dès que le texte parle d'authentification.
- `<<extend>>` pour les actions « après confirmation / si … ».

## 7.5 Pièges
- ❌ Confondre **`<<include>>`** (obligatoire, toujours exécuté — ex. authentification) et **`<<extend>>`** (optionnel/conditionnel — ex. confirmation).
- ❌ Mauvais sens de la flèche : `<<include>>` part du cas de base **vers** le cas inclus ; la **généralisation** d'acteurs pointe de l'acteur spécialisé (Admin) **vers** le général (Agent), flèche à triangle creux.
- ❌ Oublier un acteur (ex. l'acteur « libre » : Livreur 2025 V2 en consultation seule, Agriculteur, Citoyen).
- ❌ Mettre les acteurs dans le rectangle système.
- ❌ Diagramme de classes : oublier les cardinalités ou les clés étrangères.

---

# ANNEXE — Plan de révision express (priorités)

**Si tu as peu de temps, vise ces blocs ultra-rentables (apparaissent presque partout) :**
1. **Laravel** : migration up/down + modèle (fillable + hasMany/belongsTo) + resource controller + Route::resource. *(24 pts)*
2. **React** : App.js (Router+Provider) + Menu + Details(useSelector) + Liste(.map+key) + Ajouter(POST). *(24 pts)*
3. **MySQL** : CREATE TABLE + TRIGGER (SIGNAL) + FUNCTION (RETURNS) + bloc rôles. *(12 pts)*
4. **MongoDB** : use/insert + find/sort/count + update $set + deleteMany + aggregate $group. *(11 pts)*
5. **PERT** : dates tôt/tard + chemin critique + durée (jours ouvrés). *(jusqu'à 15 pts)*
6. **UML** : cas d'utilisation (acteurs + include authentification + généralisation admin). *(6 pts)*
7. **Docker** : les 10 commandes du tableau + conteneur vs VM. *(8 pts)*

**Tendances par année (pour anticiper 2026) :**
- 2023 : diagramme de **classes**, GANTT, Redux à créer (rattrapages).
- 2024 : bascule vers **cas d'utilisation** + **PERT** ; rattrapages = gestion des **rôles MySQL**, `docker run -p`, `$exists`, Nième plus grand.
- 2025 : ajout du **Dossier PHP (POO)** + **middleware Laravel** ; Report = **MCD**, `belongsToMany`/pivot, **API JSON + codes HTTP**, `$elemMatch`.
- 2026 (blancs) : DAIF = format classique + **PHP procédural** ; GAHI = le plus avancé (**microservices Node.js/Express**, **Kubernetes**, **$push/$pull/positionnel**, **Joi/express-validator**, `Http::get`).

> ⚠️ Si l'examen 2026 suit GAHI : réviser en plus Node.js/Express (db.js MongoClient, routes Express, middleware de validation) et l'architecture microservices (résilience si service indisponible).
