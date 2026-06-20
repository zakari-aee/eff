# 📚 Révision EFF — Laravel & React (Questions + Réponses)

> Toutes les questions extraites des examens 2023→2026 + examens blancs.  
> Chaque question est présentée telle qu'elle apparaît dans le sujet, suivie directement du code attendu.

---

# 🟠 PARTIE LARAVEL (Back-End)

---

## 1. Commandes de création des migrations

### ❓ Question type :
> Donner les commandes de création des fichiers de migration : `create_techniciens_table`, `create_serres_table`, `create_capteurs_table`.

```bash
php artisan make:migration create_techniciens_table
php artisan make:migration create_serres_table
php artisan make:migration create_capteurs_table
```

---

## 2. Méthodes `up()` et `down()` d'une migration

### ❓ Question type :
> Donner le code des méthodes `up()` et `down()` du fichier de migration de la table **batteries** en définissant le champ `numero_serie` comme **unique** et le champ `statut` par défaut à `"En service"`.

```php
public function up(): void
{
    Schema::create('batteries', function (Blueprint $table) {
        $table->id();
        $table->string('numero_serie')->unique();
        $table->integer('capacite');
        $table->string('sante_batterie');
        $table->integer('nombre_cycles');
        $table->string('statut')->default('En service');
        $table->timestamps();
    });
}

public function down(): void
{
    Schema::dropIfExists('batteries');
}
```

### ❓ Variante — avec clés étrangères :
> Migration de la table **voitures** avec `matricule` unique, `etat` par défaut `"Disponible"`, et clé étrangère vers `agences`.

```php
public function up(): void
{
    Schema::create('voitures', function (Blueprint $table) {
        $table->id();
        $table->string('matricule')->unique();
        $table->string('marque');
        $table->string('modele');
        $table->decimal('prix_jour', 8, 2);
        $table->integer('kilometrage');
        $table->string('etat')->default('Disponible');
        $table->foreignId('id_agence')->constrained('agences')->onDelete('cascade');
        $table->timestamps();
    });
}

public function down(): void
{
    Schema::dropIfExists('voitures');
}
```

### ❓ Variante — migration de modification (add column / add index) :
> Donner le code des méthodes `up` et `down` pour ajouter un index sur la colonne `statut` et changer le type de la colonne `capacite` en chaîne de 15 caractères.

```php
public function up(): void
{
    Schema::table('batteries', function (Blueprint $table) {
        $table->index('statut');
        $table->string('capacite', 15)->change();
    });
}

public function down(): void
{
    Schema::table('batteries', function (Blueprint $table) {
        $table->dropIndex(['statut']);
        $table->integer('capacite')->change();
    });
}
```

---

## 3. Commande pour appliquer les migrations

### ❓ Question type :
> Donner la commande permettant d'appliquer les migrations.

```bash
php artisan migrate
```

---

## 4. Commande de création d'un fichier Seeder

### ❓ Question type :
> Donner le code pour créer un fichier seeder pour la table **batteries**.

```bash
php artisan make:seeder BatterieSeeder
```

---

## 5. Méthode `run()` du Seeder

### ❓ Question type :
> Écrire la méthode `run()` permettant d'ajouter une batterie avec des données de votre choix.

```php
public function run(): void
{
    DB::table('batteries')->insert([
        'numero_serie' => 'BAT-001',
        'capacite'     => '80%',
        'sante_batterie' => 'bonne',
        'nombre_cycles' => 150,
        'statut'       => 'En service',
        'created_at'   => now(),
        'updated_at'   => now(),
    ]);
}
```

---

## 6. Commande pour exécuter le Seeder

### ❓ Question type :
> Donner la commande pour appliquer le seeder et peupler la table batteries.

```bash
php artisan db:seed --class=BatterieSeeder
```

---

## 7. Commandes de création des Modèles

### ❓ Question type :
> Donner les commandes de création des modèles : `Batterie`, `Velo`, `Technicien`.

```bash
php artisan make:model Batterie
php artisan make:model Velo
php artisan make:model Technicien
```

---

## 8. Code du Modèle (avec `$fillable` et relations)

### ❓ Question type :
> Donner le code du modèle **Voiture** sachant qu'une voiture appartient à une agence et qu'une voiture possède plusieurs locations. Préciser la propriété `fillable` et les relations.

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Voiture extends Model
{
    protected $fillable = [
        'matricule',
        'marque',
        'modele',
        'prix_jour',
        'kilometrage',
        'etat',
        'id_agence',
    ];

    // Une voiture appartient à une agence
    public function agence()
    {
        return $this->belongsTo(Agence::class, 'id_agence');
    }

    // Une voiture possède plusieurs locations
    public function locations()
    {
        return $this->hasMany(Location::class, 'id_voiture');
    }
}
```

### ❓ Variante — avec table pivot (Many-to-Many) :
> Le modèle **Catastrophe** avec une méthode `zones()` via la table pivot `catastrophe_zone_affectee`, incluant le champ `gravite`.

```php
public function zones()
{
    return $this->belongsToMany(ZoneAffectee::class, 'catastrophe_zone_affectee', 'catastrophe_id', 'zone_affectee_id')
                ->withPivot('gravite');
}
```

### ❓ Variante — relation dans le modèle Agence :
> Donner le code de la méthode du modèle **Agence** permettant de récupérer la liste de toutes les voitures d'une agence donnée.

```php
public function voitures()
{
    return $this->hasMany(Voiture::class, 'id_agence');
}
```

---

## 9. Commande de création du Contrôleur

### ❓ Question type :
> Donner la commande de création du contrôleur de ressource **VoitureController**.

```bash
php artisan make:controller VoitureController --resource
```

---

## 10. Méthodes du Contrôleur (Resource CRUD)

### ❓ Question type :
> Créer le contrôleur de ressource `VoitureController` en écrivant les méthodes :
> - `index` — afficher toutes les voitures
> - `store` — ajouter une voiture
> - `destroy` — supprimer une voiture

```php
<?php

namespace App\Http\Controllers;

use App\Models\Voiture;
use Illuminate\Http\Request;

class VoitureController extends Controller
{
    // Afficher toutes les voitures
    public function index()
    {
        $voitures = Voiture::all();
        return view('voitures.index', compact('voitures'));
    }

    // Ajouter une voiture
    public function store(Request $request)
    {
        $request->validate([
            'matricule' => 'required|unique:voitures',
            'marque'    => 'required',
            'modele'    => 'required',
            'prix_jour' => 'required|numeric',
            'id_agence' => 'required|exists:agences,id',
        ]);

        Voiture::create($request->all());
        return redirect()->route('voitures.index');
    }

    // Supprimer une voiture
    public function destroy($id)
    {
        $voiture = Voiture::findOrFail($id);
        $voiture->delete();
        return redirect()->route('voitures.index');
    }
}
```

---

## 11. Méthodes spéciales du Contrôleur

### ❓ Méthode `show()` :
> La méthode `show($id)` affiche un événement dans la page `show.blade.php`.

```php
public function show($id)
{
    $evenement = Evenement::with(['expert', 'ateliers'])->findOrFail($id);
    return view('evenements.show', compact('evenement'));
}
```

### ❓ Méthode avec validation et contraintes :
> Méthode `Ajouter` qui insère un nouveau notaire avec les contraintes : `numn` commence par N et se termine par E, `email` valide, `age` > 22.

```php
public function Ajouter(Request $request)
{
    $request->validate([
        'numn'   => ['required', 'regex:/^N.*E$/'],
        'email'  => 'required|email',
        'age'    => 'required|integer|min:23',
        'nom'    => 'required',
        'prenom' => 'required',
    ]);

    Notaire::create($request->all());
    return redirect()->route('notaires.index');
}
```

### ❓ Méthode qui retourne du JSON (API) :
> Méthode `Verifier($id)` : si le dossier est traité, retourne les infos du terrain en JSON, sinon retourne `"dossier encours de traitement"`.

```php
public function Verifier($id)
{
    $dossier = DossierTerrain::with('terrain')->find($id);

    if (!$dossier) {
        return response()->json(['message' => 'Dossier introuvable'], 404);
    }

    if ($dossier->etat === 'traité') {
        return response()->json($dossier->terrain);
    }

    return response()->json(['message' => 'dossier encours de traitement']);
}
```

### ❓ Méthode `Recherche` (rediriger vers une vue avec données filtrées) :
> Méthode `recherche` qui redirige vers `Recherche.blade.php` affichant les demandes "encours" d'un bénéficiaire et un lien "valider" pour chaque demande.

```php
public function recherche(Request $request)
{
    $cin = $request->cin;
    $demandes = DemandeDedouanement::where('etat', 'encours')
                    ->whereHas('voiture', function($q) use ($cin) {
                        $q->where('cin', $cin);
                    })->get();

    return view('Recherche', compact('demandes'));
}
```

### ❓ Méthode `valider` :
> Méthode `valider` qui change l'état de la demande à `"validée"`.

```php
public function valider($id)
{
    $demande = DemandeDedouanement::findOrFail($id);
    $demande->update(['etat' => 'validée']);
    return redirect()->back();
}
```

---

## 12. Route `resource` dans `web.php`

### ❓ Question type :
> Donner la route de type resource permettant d'associer toutes les actions du `VoitureController`.

```php
// routes/web.php
use App\Http\Controllers\VoitureController;

Route::resource('voitures', VoitureController::class);
```

---

## 13. Routes manuelles dans `web.php`

### ❓ Question type :
> Écrire les routes vers les méthodes `Rechercher`, `Ajouter` et `Modifier` du contrôleur `DedouanementControleur`.

```php
use App\Http\Controllers\DedouanementControleur;

Route::get('/rechercher', [DedouanementControleur::class, 'rechercher'])->name('dedouanement.rechercher');
Route::post('/ajouter',   [DedouanementControleur::class, 'ajouter'])->name('dedouanement.ajouter');
Route::put('/modifier/{id}', [DedouanementControleur::class, 'modifier'])->name('dedouanement.modifier');
```

---

## 14. Routes dans `api.php`

### ❓ Question type :
> Ajouter la route dans `api.php` pour accéder à la méthode `Verifier`.

```php
// routes/api.php
use App\Http\Controllers\NotariatControleur;

Route::get('/verifier/{id}', [NotariatControleur::class, 'Verifier']);
```

### ❓ Variante — routes API RESTful avec préfixe :
> Ajouter les routes RESTful pour `SatelliteController` dans `routes/api.php` avec le préfixe `api/v1/satellites`.

```php
Route::prefix('v1')->group(function () {
    Route::apiResource('satellites', SatelliteController::class);
});
```

---

## 15. Tableau des routes générées par `Route::resource`

### ❓ Question type :
> Remplir le tableau des routes générées automatiquement par `Route::resource` pour le contrôleur `CatastropheController`.

| Méthode HTTP | URI | Action | Nom de Route |
|---|---|---|---|
| GET | /catastrophes | index | catastrophes.index |
| GET | /catastrophes/create | create | catastrophes.create |
| POST | /catastrophes | store | catastrophes.store |
| GET | /catastrophes/{id} | show | catastrophes.show |
| GET | /catastrophes/{id}/edit | edit | catastrophes.edit |
| PUT/PATCH | /catastrophes/{id} | update | catastrophes.update |
| DELETE | /catastrophes/{id} | destroy | catastrophes.destroy |

---

## 16. Vue Blade

### ❓ Question type :
> Créer la vue `index.blade.php` affichant la liste des voitures dans un tableau avec les actions (Consulter, Modifier, Supprimer).

```html
<!DOCTYPE html>
<html>
<head><title>Liste des voitures</title></head>
<body>

<h2>Liste des voitures</h2>

<a href="{{ route('voitures.create') }}">Ajouter une nouvelle voiture</a>

<table border="1">
    <thead>
        <tr>
            <th>Matricule</th>
            <th>Marque</th>
            <th>Modèle</th>
            <th>Prix/Jour</th>
            <th>État</th>
            <th>Actions</th>
        </tr>
    </thead>
    <tbody>
        @foreach ($voitures as $voiture)
        <tr>
            <td>{{ $voiture->matricule }}</td>
            <td>{{ $voiture->marque }}</td>
            <td>{{ $voiture->modele }}</td>
            <td>{{ $voiture->prix_jour }}</td>
            <td>{{ $voiture->etat }}</td>
            <td>
                <a href="{{ route('voitures.show', $voiture->id) }}">Consulter</a>
                <a href="{{ route('voitures.edit', $voiture->id) }}">Modifier</a>
                <form action="{{ route('voitures.destroy', $voiture->id) }}" method="POST" style="display:inline">
                    @csrf
                    @method('DELETE')
                    <button type="submit">Supprimer</button>
                </form>
            </td>
        </tr>
        @endforeach
    </tbody>
</table>

</body>
</html>
```

### ❓ Vue Blade avec liste et checkbox :
> Vue `recherche.blade.php` : affiche la liste des dossiers d'un cabinet avec une checkbox pour supprimer chaque dossier.

```html
<!DOCTYPE html>
<html>
<body>
<h2>Liste des dossiers</h2>
<form action="{{ route('notariat.supprimerTous') }}" method="POST">
    @csrf
    @method('DELETE')

    @foreach ($dossiers as $dossier)
    <div>
        <input type="checkbox" name="ids[]" value="{{ $dossier->iddossier }}">
        Dossier N°{{ $dossier->iddossier }} — {{ $dossier->etat }}
    </div>
    @endforeach

    <button type="submit">Supprimer les sélectionnés</button>
</form>
</body>
</html>
```

---

## 17. Middleware

### ❓ Question type :
> Donner la commande pour créer le middleware `autorisationSuppression`.

```bash
php artisan make:middleware autorisationSuppression
```

### ❓ Corps de la méthode `handle` :
> Le middleware `autorisationSuppression` accepte uniquement les requêtes de l'utilisateur `"toto"`, sinon renvoie 403.

```php
public function handle(Request $request, Closure $next): Response
{
    if (Auth::user()->name !== 'toto') {
        return response()->json([
            'message' => 'vous n\'êtes pas autorisé à effectuer cette opération de suppression'
        ], 403);
    }

    return $next($request);
}
```

### ❓ Variante — middleware selon l'heure :
> Accepte les requêtes entre 8H30 et 18H30, sinon renvoie 403.

```php
public function handle(Request $request, Closure $next): Response
{
    $heure = now()->format('H:i');

    if ($heure < '08:30' || $heure > '18:30') {
        return response()->json([
            'message' => 'Les suppressions sont permises entre 8H30 et 18H30'
        ], 403);
    }

    return $next($request);
}
```

### ❓ Route protégée par middleware :
> Donner le code de la route de suppression en la protégeant par le middleware `autorisationSuppression`.

```php
Route::delete('/batteries/{id}', [VeloController::class, 'SupprimerBatterie'])
     ->middleware('autorisationSuppression');
```

---

## 18. Commandes Artisan à connaître

### ❓ Question type (tableau à remplir) :
> Compléter le tableau en fournissant la description/rôle pour chaque commande Artisan.

| Commande Artisan | Description / Rôle |
|---|---|
| `php artisan serve` | Lance le serveur de développement local |
| `php artisan make:seeder CatastropheSeeder` | Crée un fichier seeder pour insérer des données |
| `php artisan route:list` | Affiche la liste de toutes les routes enregistrées |
| `php artisan db:seed --class=CatastropheSeeder` | Exécute le seeder pour peupler la base de données |
| `php artisan make:migration create_actions_table` | Crée un fichier de migration pour la table `actions` |
| `php artisan migrate` | Applique toutes les migrations en attente |
| `php artisan route:clear` | Supprime le cache des routes enregistrées |
| `php artisan make:request ZoneAffecteeRequest` | Crée une classe de validation pour les requêtes |

---

---

# 🔵 PARTIE REACT (Front-End)

---

## 1. Composante `Menu.js` avec `NavLink`

### ❓ Question type :
> Écrire la composante `Menu.js` qui définit les liens menant vers les composantes :
> - `/AjouterChgmt` → `AjouterChangement.js` (titre : Nouveau Changement)
> - `/ListeBatteries` → `ListeBatteries.js` (titre : Liste Batteries)
> - `/DetailsVelo` → `DetailsVelo.js` (titre : Détails Vélo)

```jsx
import React from 'react';
import { NavLink } from 'react-router-dom';

function Menu() {
    return (
        <nav>
            <NavLink to="/AjouterChgmt">Nouveau Changement</NavLink>
            <NavLink to="/ListeBatteries">Liste Batteries</NavLink>
            <NavLink to="/DetailsVelo">Détails Vélo</NavLink>
        </nav>
    );
}

export default Menu;
```

---

## 2. Composante `App.js` (Routage + Provider Redux)

### ❓ Question type :
> Écrire le code de `App.js` qui :
> - Définit le routage (`BrowserRouter`, `Routes`, `Route`)
> - Appelle la composante `Menu.js`
> - Intègre le `Provider` du store Redux

```jsx
import React, { useState, useEffect } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { Provider } from 'react-redux';
import store from './store';
import Menu from './Menu';
import DetailsVelo from './DetailsVelo';
import AjouterChangement from './AjouterChangement';
import ListeBatteries from './ListeBatteries';

function App() {
    return (
        <Provider store={store}>
            <BrowserRouter>
                <Menu />
                <Routes>
                    <Route path="/DetailsVelo"    element={<DetailsVelo />} />
                    <Route path="/AjouterChgmt"   element={<AjouterChangement />} />
                    <Route path="/ListeBatteries" element={<ListeBatteries />} />
                </Routes>
            </BrowserRouter>
        </Provider>
    );
}

export default App;
```

### ❓ Variante — App.js avec `useEffect` pour charger l'API :
> Initialiser la variable du state `voitures` en appelant l'API `http://localhost:8000/voitures`.

```jsx
import React, { useState, useEffect } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { Provider } from 'react-redux';
import store from './store';
import Menu from './Menu';
import ListeVoitures from './ListeVoitures';

function App() {
    const [voitures, setVoitures] = useState([]);

    useEffect(() => {
        fetch('http://localhost:8000/voitures')
            .then(res => res.json())
            .then(data => setVoitures(data))
            .catch(err => console.error(err));
    }, []);

    return (
        <Provider store={store}>
            <BrowserRouter>
                <Menu />
                <Routes>
                    <Route path="/ListeVoitures" element={<ListeVoitures voitures={voitures} />} />
                </Routes>
            </BrowserRouter>
        </Provider>
    );
}

export default App;
```

---

## 3. Composante qui lit depuis le Store Redux (`useSelector`)

### ❓ Question type :
> Écrire la composante `DetailsVelo.js` qui lit les informations depuis le store Redux (en utilisant `useSelector`) et affiche : Matricule, Capacité Batterie, Santé Batterie, Date Dernière Maintenance, Date Prochaine Maintenance.

```jsx
import React from 'react';
import { useSelector } from 'react-redux';

function DetailsVelo() {
    const velo = useSelector(state => state.velo);

    return (
        <div>
            <h2>Velo Details</h2>
            <p><strong>Matricule :</strong> {velo.Matricule}</p>
            <p><strong>Capacité Batterie Actuelle :</strong> {velo.BatterieUtilisee.Capacite}%</p>
            <p><strong>Santé Batterie Actuelle :</strong> {velo.BatterieUtilisee.sante_batterie}%</p>
            <p><strong>Date Dernière Maintenance :</strong> {velo.Date_derniere_maintenance}</p>
            <p><strong>Date Prochaine Maintenance :</strong> {velo.Date_prochaine_maintenance}</p>
        </div>
    );
}

export default DetailsVelo;
```

---

## 4. Composante qui filtre et affiche une liste (depuis le store)

### ❓ Question type :
> Créer la composante `ListeBatteries.js` qui lit la liste des batteries depuis le store Redux et l'affiche. La liste déroulante lit `StatutBatterie` depuis le store. Après clic sur "Filtrer", les données sont filtrées. En bas : nombre total et moyenne des cycles.

```jsx
import React, { useState } from 'react';
import { useSelector } from 'react-redux';

function ListeBatteries() {
    const batteries = useSelector(state => state.Batteries);
    const statuts   = useSelector(state => state.StatutBatterie);
    const [statutChoisi, setStatutChoisi] = useState('');
    const [resultat, setResultat] = useState([]);

    const filtrer = () => {
        const filtre = batteries.filter(b => b.statut === statutChoisi);
        setResultat(filtre);
    };

    const moyenne = resultat.length
        ? (resultat.reduce((sum, b) => sum + b.nombre_cycles, 0) / resultat.length).toFixed(2)
        : 0;

    return (
        <div>
            <h2>Liste Batteries</h2>

            <label>Choisir le Statut de la Batterie :</label>
            <select value={statutChoisi} onChange={e => setStatutChoisi(e.target.value)}>
                <option value="">-- Choisir --</option>
                {statuts.map((s, i) => (
                    <option key={i} value={s}>{s}</option>
                ))}
            </select>
            <button onClick={filtrer}>Filtrer</button>

            <table border="1">
                <thead>
                    <tr>
                        <th>Id</th>
                        <th>Numero Serie</th>
                        <th>Statut</th>
                        <th>Nombre Cycles</th>
                    </tr>
                </thead>
                <tbody>
                    {resultat.map(b => (
                        <tr key={b.id}>
                            <td>{b.id}</td>
                            <td>{b.Numero_serie}</td>
                            <td>{b.statut}</td>
                            <td>{b.nombre_cycles}</td>
                        </tr>
                    ))}
                </tbody>
            </table>

            <p>{resultat.length} batteries Trouvées</p>
            <p>La moyenne des nombres de cycles est {moyenne}</p>
        </div>
    );
}

export default ListeBatteries;
```

---

## 5. Composante avec formulaire qui affiche un récapitulatif (sans API)

### ❓ Question type :
> Écrire la composante `composant1.js` permettant à l'utilisateur de saisir les informations d'une voiture. Lors du clic sur "Confirmer", afficher le récapitulatif des informations sur la même page.

```jsx
import React, { useState } from 'react';

function Composant1() {
    const [matricule, setMatricule] = useState('');
    const [marque, setMarque]       = useState('Toyota');
    const [dateMec, setDateMec]     = useState('');
    const [couleur, setCouleur]     = useState('');
    const [recap, setRecap]         = useState(null);

    const confirmer = () => {
        setRecap({ matricule, marque, dateMec, couleur });
    };

    return (
        <div>
            <h2>Gestion Voitures</h2>

            <label>Matricule :</label>
            <input value={matricule} onChange={e => setMatricule(e.target.value)} />

            <label>Marque :</label>
            <select value={marque} onChange={e => setMarque(e.target.value)}>
                <option>Toyota</option>
                <option>Renault</option>
                <option>Peugeot</option>
            </select>

            <label>Date de mise en circulation :</label>
            <input type="date" value={dateMec} onChange={e => setDateMec(e.target.value)} />

            <label>Couleur :</label>
            <input value={couleur} onChange={e => setCouleur(e.target.value)} />

            <button onClick={confirmer}>Confirmer</button>

            {recap && (
                <div>
                    <h2>Récapitulatif des informations :</h2>
                    <ul>
                        <li>Matricule : {recap.matricule}</li>
                        <li>Marque : {recap.marque}</li>
                        <li>Date Mise en circulation : {recap.dateMec}</li>
                        <li>Couleur : {recap.couleur}</li>
                    </ul>
                </div>
            )}
        </div>
    );
}

export default Composant1;
```

---

## 6. Composante formulaire qui appelle une API (POST) avec données du store

### ❓ Question type :
> Écrire la composante `AjouterChangement.js` qui ajoute un changement de batterie via l'API POST `http://localhost:8000/api/AjouterChangement`. Les valeurs de vélo, ancienne batterie, liste batteries et techniciens sont lues depuis le store Redux.

```jsx
import React, { useState } from 'react';
import { useSelector } from 'react-redux';

function AjouterChangement() {
    const velo       = useSelector(state => state.velo);
    const batteries  = useSelector(state => state.Batteries);
    const techniciens = useSelector(state => state.Techniciens);

    const [nouvelleBatId, setNouvelleBatId] = useState('');
    const [dateChangement, setDateChangement] = useState('');
    const [technicienId, setTechnicienId] = useState('');
    const [raison, setRaison]             = useState('');

    const ajouter = () => {
        const data = {
            id_velo:            velo.id,
            id_ancienne_batterie: velo.BatterieUtilisee.Id,
            id_nouvelle_batterie: nouvelleBatId,
            date_changement:    dateChangement,
            id_technicien:      technicienId,
            raison:             raison,
        };

        fetch('http://localhost:8000/api/AjouterChangement', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(data),
        })
        .then(res => res.json())
        .then(() => alert('Changement ajouté avec succès !'))
        .catch(err => console.error(err));
    };

    return (
        <div>
            <h2>Changement de Batterie</h2>
            <p>Velo : {velo.Matricule}</p>
            <p>Ancienne Batterie : {velo.BatterieUtilisee.Numero_serie}</p>

            <label>Nouvelle Batterie :</label>
            <select value={nouvelleBatId} onChange={e => setNouvelleBatId(e.target.value)}>
                <option value="">-- Choisir une batterie --</option>
                {batteries.map(b => (
                    <option key={b.id} value={b.id}>{b.Numero_serie}</option>
                ))}
            </select>

            <label>Date de Changement :</label>
            <input type="date" value={dateChangement} onChange={e => setDateChangement(e.target.value)} />

            <label>Technicien :</label>
            <select value={technicienId} onChange={e => setTechnicienId(e.target.value)}>
                <option value="">-- Choisir un technicien --</option>
                {techniciens.map(t => (
                    <option key={t.id} value={t.id}>{t.Nom}</option>
                ))}
            </select>

            <label>Raison :</label>
            <textarea value={raison} onChange={e => setRaison(e.target.value)} />

            <button onClick={ajouter}>Ajouter</button>
        </div>
    );
}

export default AjouterChangement;
```

---

## 7. Composante qui appelle une API GET et affiche les résultats

### ❓ Question type :
> Écrire la composante `ListeHistoriqueSortie.js` qui affiche l'historique des sorties d'un barrage dont le nom est passé en paramètre, en appelant l'API GET `http://localhost:8000/api/ListeHistoriqueSortie/{NomBarrage}`.

```jsx
import React, { useState } from 'react';

function ListeHistoriqueSortie() {
    const [nomBarrage, setNomBarrage] = useState('');
    const [historique, setHistorique] = useState(null);

    const chercher = () => {
        fetch(`http://localhost:8000/api/ListeHistoriqueSortie/${nomBarrage}`)
            .then(res => res.json())
            .then(data => setHistorique(data))
            .catch(err => console.error(err));
    };

    return (
        <div>
            <h2>Historique des Sorties</h2>

            <input
                value={nomBarrage}
                onChange={e => setNomBarrage(e.target.value)}
                placeholder="Nom du barrage"
            />
            <button onClick={chercher}>Chercher</button>

            {historique && (
                <div>
                    <h3>Sorties Pompes</h3>
                    <table border="1">
                        <thead>
                            <tr><th>Id</th><th>Numero Sortie</th><th>Date Début</th><th>Date Fin</th><th>Quantité</th><th>Consommation Électricité</th></tr>
                        </thead>
                        <tbody>
                            {historique.SortiePompe?.map(s => (
                                <tr key={s.id}>
                                    <td>{s.id}</td>
                                    <td>{s.NumeroSortie}</td>
                                    <td>{s.DateDebut}</td>
                                    <td>{s.DateFin}</td>
                                    <td>{s.QuantiteTotaleSortie}</td>
                                    <td>{s.TotalConsommationElectricite}</td>
                                </tr>
                            ))}
                        </tbody>
                    </table>

                    <h3>Sorties Turbines Hydrauliques</h3>
                    <table border="1">
                        <thead>
                            <tr><th>Id</th><th>Numero Sortie</th><th>Date Début</th><th>Date Fin</th><th>Quantité</th><th>Production Électricité</th></tr>
                        </thead>
                        <tbody>
                            {historique.SortieTurbineHydrolique?.map(s => (
                                <tr key={s.id}>
                                    <td>{s.id}</td>
                                    <td>{s.NumeroSortie}</td>
                                    <td>{s.DateDebut}</td>
                                    <td>{s.DateFin}</td>
                                    <td>{s.QuantiteTotaleSortie}</td>
                                    <td>{s.TotalProductionElectricite}</td>
                                </tr>
                            ))}
                        </tbody>
                    </table>
                </div>
            )}
        </div>
    );
}

export default ListeHistoriqueSortie;
```

---

## 8. Composante qui affiche une liste reçue en `props`

### ❓ Question type :
> Créer la composante `ListeVoitures.js` qui reçoit le tableau des voitures et l'affiche sous forme de liste à puces (matricule, marque et prix par jour de chaque voiture).

```jsx
import React from 'react';

function ListeVoitures({ voitures }) {
    return (
        <div>
            <h2>Liste des Voitures</h2>
            <ul>
                {voitures.map(v => (
                    <li key={v.id}>
                        {v.matricule} — {v.marque} — {v.prix_jour} DH/jour
                    </li>
                ))}
            </ul>
        </div>
    );
}

export default ListeVoitures;
```

---

## 9. Redux — Store, Reducer, Actions (Redux Toolkit)

### ❓ Question type :
> Créer le store Redux avec son état initial et un reducer pour les deux actions :
> - `Ajouter` : ajoute une simulation dans `ListeSimulation`
> - `Supprimer` : supprime les calculs d'une marque passée en paramètre

```jsx
// store.js
import { configureStore, createSlice } from '@reduxjs/toolkit';

const simulationSlice = createSlice({
    name: 'simulation',
    initialState: {
        ListeSimulation: []
    },
    reducers: {
        Ajouter: (state, action) => {
            state.ListeSimulation.push(action.payload);
        },
        Supprimer: (state, action) => {
            state.ListeSimulation = state.ListeSimulation.filter(
                item => item.marque !== action.payload
            );
        }
    }
});

export const { Ajouter, Supprimer } = simulationSlice.actions;

const store = configureStore({
    reducer: {
        simulation: simulationSlice.reducer
    }
});

export default store;
```

### ❓ Variante — `AddDonateur` et `DeleteDonateur` :

```jsx
import { configureStore, createSlice } from '@reduxjs/toolkit';

const donateurSlice = createSlice({
    name: 'donateur',
    initialState: {
        listeDonateurs: []
    },
    reducers: {
        AddDonateur: (state, action) => {
            state.listeDonateurs.push(action.payload);
        },
        DeleteDonateur: (state, action) => {
            state.listeDonateurs = state.listeDonateurs.filter(
                d => d.id !== action.payload
            );
        }
    }
});

export const { AddDonateur, DeleteDonateur } = donateurSlice.actions;

const store = configureStore({
    reducer: { donateur: donateurSlice.reducer }
});

export default store;
```

---

## 10. Composante qui dispatche une action Redux

### ❓ Question type :
> Écrire la composante `Donateur` qui liste les donateurs du store en utilisant les actions `AddDonateur` et `DeleteDonateur`.

```jsx
import React, { useState } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { AddDonateur, DeleteDonateur } from './store';

function Donateur() {
    const donateurs = useSelector(state => state.donateur.listeDonateurs);
    const dispatch  = useDispatch();

    const [nom, setNom]       = useState('');
    const [prenom, setPrenom] = useState('');
    const [email, setEmail]   = useState('');

    const ajouter = () => {
        dispatch(AddDonateur({ id: Date.now(), nom, prenom, email }));
        setNom(''); setPrenom(''); setEmail('');
    };

    return (
        <div>
            <h2>Ajouter un Donateur</h2>
            <input placeholder="Nom"    value={nom}    onChange={e => setNom(e.target.value)} />
            <input placeholder="Prénom" value={prenom} onChange={e => setPrenom(e.target.value)} />
            <input placeholder="Email"  value={email}  onChange={e => setEmail(e.target.value)} />
            <button onClick={ajouter}>Ajouter Donateur</button>

            <h2>Liste des donateurs :</h2>
            <ul>
                {donateurs.map(d => (
                    <li key={d.id}>
                        {d.nom} {d.prenom}
                        <button onClick={() => dispatch(DeleteDonateur(d.id))}>Supprimer Donateur</button>
                    </li>
                ))}
            </ul>
        </div>
    );
}

export default Donateur;
```

---

## 11. Composante avec calcul (utilisant le store)

### ❓ Question type :
> Écrire la composante `Calcul_taxe_dedouanement.js`. Le clic sur "Valider" calcule la valeur imposable, Droit d'importation, TVA, Taxe parafiscale et Montant total. Le calcul doit être sauvegardé dans le store.

```jsx
import React, { useState } from 'react';
import { useDispatch } from 'react-redux';
import { Ajouter } from './store';

function Calcul_taxe_dedouanement() {
    const dispatch = useDispatch();

    const [marque, setMarque]       = useState('');
    const [modele, setModele]       = useState('');
    const [dateMec, setDateMec]     = useState('');
    const [valeurNeuf, setValeurNeuf] = useState('');
    const [resultat, setResultat]   = useState(null);

    const valider = () => {
        const valeur = parseFloat(valeurNeuf);
        const today  = new Date();
        const mec    = new Date(dateMec);
        const ageAns = (today - mec) / (1000 * 60 * 60 * 24 * 365);

        let tauxImposable;
        if (ageAns < 1)       tauxImposable = 1.0;
        else if (ageAns < 2)  tauxImposable = 0.9;
        else if (ageAns < 3)  tauxImposable = 0.8;
        else                  tauxImposable = 0.75;

        const valeurImposable  = valeur * tauxImposable;
        const droitImportation = valeurImposable * 0.175;
        const tva              = valeurImposable * 0.20;
        const taxeParafiscale  = valeurImposable * 0.0025;
        const montantTotal     = droitImportation + tva + taxeParafiscale;

        const calcul = { marque, date_mise_circulation: dateMec, valeur_imposable: valeurImposable, montant: montantTotal };
        setResultat(calcul);
        dispatch(Ajouter(calcul));
    };

    return (
        <div>
            <h2>Calcul des droits et taxes</h2>
            <label>Marque :</label>
            <input value={marque} onChange={e => setMarque(e.target.value)} />

            <label>Modèle :</label>
            <input value={modele} onChange={e => setModele(e.target.value)} />

            <label>Date 1ère mise en circulation :</label>
            <input type="date" value={dateMec} onChange={e => setDateMec(e.target.value)} />

            <label>Valeur à l'état neuf :</label>
            <input type="number" value={valeurNeuf} onChange={e => setValeurNeuf(e.target.value)} />

            <button onClick={valider}>Valider</button>

            {resultat && (
                <div>
                    <h3>Détails des droits et taxes exigibles</h3>
                    <p>Valeur taxable : {resultat.valeur_imposable.toFixed(2)} DH</p>
                    <p>Droit d'importation (17.5%) : {(resultat.valeur_imposable * 0.175).toFixed(2)} DH</p>
                    <p>TVA (20.0%) : {(resultat.valeur_imposable * 0.20).toFixed(2)} DH</p>
                    <p>Taxe parafiscale (0.25%) : {(resultat.valeur_imposable * 0.0025).toFixed(2)} DH</p>
                    <p><strong>Montant total : {resultat.montant.toFixed(2)} DH</strong></p>
                </div>
            )}
        </div>
    );
}

export default Calcul_taxe_dedouanement;
```

---

## 12. Composante qui affiche avec `map` des données imbriquées (props)

### ❓ Question type :
> Créer la composante `Evenements` qui reçoit la liste d'événements d'un expert en `props` et les affiche dans un tableau HTML, en incluant le coût total pour chaque événement et le total global.

```jsx
import React from 'react';

function Evenements({ evenements }) {
    const totalGlobal = evenements.reduce((sum, e) => sum + e.cout_journalier * e.durée, 0);

    return (
        <div>
            <table border="1">
                <thead>
                    <tr>
                        <th>Thème</th>
                        <th>Date début</th>
                        <th>Date fin</th>
                        <th>Description</th>
                        <th>Coût journalier</th>
                        <th>Durée (jours)</th>
                        <th>Coût Total Événement</th>
                    </tr>
                </thead>
                <tbody>
                    {evenements.map((e, i) => (
                        <tr key={i}>
                            <td>{e.thème}</td>
                            <td>{e.date_debut}</td>
                            <td>{e.date_fin}</td>
                            <td>{e.description}</td>
                            <td>{e.cout_journalier} DH</td>
                            <td>{e.durée}</td>
                            <td>{e.cout_journalier * e.durée} DH</td>
                        </tr>
                    ))}
                    <tr>
                        <td colSpan="6"><strong>Total des coûts des événements assurés est :</strong></td>
                        <td><strong>{totalGlobal} DH</strong></td>
                    </tr>
                </tbody>
            </table>
        </div>
    );
}

export default Evenements;
```

---

## 13. Composante `Formulaire.js` qui calcule (ex: tarif parking)

### ❓ Question type :
> Écrire la composante `CalculTarif.js` permettant de saisir le nombre d'heures. Au clic sur "Calculer", afficher le montant = heures × tarif (tarif = 10 DH, lu depuis le store).

```jsx
import React, { useState } from 'react';
import { useSelector } from 'react-redux';

function CalculTarif() {
    const tarif = useSelector(state => state.parking.Zone.tarif_horaire);
    const [heures, setHeures]   = useState('');
    const [montant, setMontant] = useState(null);

    const calculer = () => {
        setMontant(parseFloat(heures) * tarif);
    };

    return (
        <div>
            <h2>Calcul du Tarif de Stationnement</h2>

            <label>Nombre d'heures :</label>
            <input type="number" value={heures} onChange={e => setHeures(e.target.value)} />

            <button onClick={calculer}>Calculer</button>

            {montant !== null && (
                <p>Montant à payer : <strong>{montant} DH</strong></p>
            )}
        </div>
    );
}

export default CalculTarif;
```

---

## 14. Composante `Details` qui reçoit un objet et affiche ses donations

### ❓ Question type :
> Créer la composante `Details` qui affiche les donations du donateur cliqué. Afficher aussi le montant total.

```jsx
import React from 'react';

function Details({ donateur, retour }) {
    const total = donateur.donations.reduce((sum, d) => sum + d.montant, 0);

    return (
        <div>
            <h2>Donateur : {donateur.nom} {donateur.prenom}</h2>
            <h3>Liste des donations :</h3>
            <ul>
                {donateur.donations.map((d, i) => (
                    <li key={i}>
                        Action : {d.action}, Montant de donation : {d.montant} dhs
                    </li>
                ))}
            </ul>
            <p><strong>Montant total des donations : {total} dhs</strong></p>
            <button onClick={retour}>Revenir à la liste des donateurs</button>
        </div>
    );
}

export default Details;
```

---

## 15. Composante `listeDonateurs` avec recherche et toggle (afficher détails)

### ❓ Question type :
> Composante `listeDonateurs` avec options de recherche par nom, par action, et affichage de tous. Afficher les résultats avec un bouton "Afficher les Donations".

```jsx
import React, { useState } from 'react';
import Details from './Details';

function ListeDonateurs({ donateurs }) {
    const [recherche, setRecherche]       = useState('');
    const [typeFiltreNom, setTypeFiltreNom] = useState(true);
    const [typeAction, setTypeAction]     = useState(false);
    const [afficherTout, setAfficherTout] = useState(false);
    const [resultat, setResultat]         = useState([]);
    const [donateurChoisi, setDonateurChoisi] = useState(null);

    const chercher = () => {
        let filtre;
        if (afficherTout) {
            filtre = donateurs;
        } else if (typeFiltreNom) {
            filtre = donateurs.filter(d => d.nom.toLowerCase().includes(recherche.toLowerCase()));
        } else if (typeAction) {
            filtre = donateurs.filter(d =>
                d.donations.some(don => don.action.toLowerCase().includes(recherche.toLowerCase()))
            );
        }
        setResultat(filtre || []);
    };

    if (donateurChoisi) {
        return <Details donateur={donateurChoisi} retour={() => setDonateurChoisi(null)} />;
    }

    return (
        <div>
            <h2>Recherche des Donateurs</h2>
            <input value={recherche} onChange={e => setRecherche(e.target.value)} placeholder="Mot clé" />

            <label>
                <input type="radio" checked={typeFiltreNom} onChange={() => { setTypeFiltreNom(true); setTypeAction(false); setAfficherTout(false); }} />
                Rechercher par Nom
            </label>
            <label>
                <input type="radio" checked={typeAction} onChange={() => { setTypeAction(true); setTypeFiltreNom(false); setAfficherTout(false); }} />
                Rechercher par Action
            </label>
            <label>
                <input type="radio" checked={afficherTout} onChange={() => { setAfficherTout(true); setTypeFiltreNom(false); setTypeAction(false); }} />
                Afficher tous
            </label>

            <button onClick={chercher}>Afficher</button>

            <h3>Liste des Donateurs</h3>
            <div style={{ display: 'flex', flexWrap: 'wrap', gap: '20px' }}>
                {resultat.map(d => (
                    <div key={d.id} style={{ textAlign: 'center' }}>
                        <img src={d.image} alt={d.nom} width="80" height="80" style={{ borderRadius: '50%' }} />
                        <p>Nom : {d.nom}</p>
                        <p>Prénom : {d.prenom}</p>
                        <button onClick={() => setDonateurChoisi(d)}>Afficher les Donations</button>
                    </div>
                ))}
            </div>
        </div>
    );
}

export default ListeDonateurs;
```

---

## 📋 Résumé des patterns les plus fréquents

| Pattern | Hook utilisé |
|---|---|
| Lire depuis le store | `useSelector(state => state.xxx)` |
| Dispatcher une action | `const dispatch = useDispatch(); dispatch(action(payload))` |
| État local du composant | `const [val, setVal] = useState('')` |
| Appel API au chargement | `useEffect(() => { fetch(...) }, [])` |
| Appel API au clic | `fetch(...)` dans un handler |
| Props vers enfant | `<Composant data={data} />` puis `function Composant({ data })` |
| Routage | `<BrowserRouter><Routes><Route path="/" element={<Comp/>}/></Routes></BrowserRouter>` |
| Menu | `<NavLink to="/path">Titre</NavLink>` |
| Provider | `<Provider store={store}><App /></Provider>` |
