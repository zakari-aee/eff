# 📚 Révision EFF — MongoDB & MySQL (Questions + Réponses)

> Toutes les questions extraites des examens 2023→2026 + examens blancs.  
> Chaque question est présentée telle qu'elle apparaît dans le sujet, suivie directement du code attendu.

---

# 🟢 PARTIE MONGODB (NoSQL)

---

## 1. Créer une base de données et une collection + insérer un document

### ❓ Question type :
> Créez une base de données `"DBSalaries"` et une collection `"salaries"` contenant les informations suivantes :

```js
use DBSalaries

db.createCollection("salaries")

db.salaries.insertOne({
    "_id": "s1",
    "nomsal": "Alami",
    "prenomsal": "Sara",
    "fonction": "Technicien",
    "service": {
        "codeser": "1",
        "nomser": "informatique"
    }
})
```

### ❓ Variante — insertMany (plusieurs documents) :
> Insérer les deux documents suivants dans la collection `batteries` :

```js
use FastFoodDelivery

db.batteries.insertMany([
    {
        "_id": "1",
        "numéro_série": "s123",
        "capacité": "50%",
        "santé_batterie": "bonne",
        "nombre_cycles": 500,
        "statut": "en service"
    },
    {
        "_id": "2",
        "numéro_série": "s345",
        "capacité": "70%",
        "santé_batterie": "moyenne",
        "nombre_cycles": 600,
        "statut": "en panne"
    }
])
```

---

## 2. Afficher / Trier (`find` + `sort`)

### ❓ Question type :
> Afficher les salariés triés par ordre croissant des noms.

```js
db.salaries.find().sort({ nomsal: 1 })
```

### ❓ Variante — tri décroissant :
> Afficher les batteries triées par `capacite` en ordre décroissant.

```js
db.batteries.find().sort({ capacite: -1 })
```

### ❓ Afficher avec projection (certains champs seulement) :
> Afficher les thèmes et les descriptions des événements.

```js
db.evenements.find({}, { theme: 1, Description: 1, _id: 0 })
```

---

## 3. Compter des documents (`countDocuments`)

### ❓ Question type :
> Afficher le nombre de salariés ayant la fonction `"Technicien"`.

```js
db.salaries.countDocuments({ fonction: "Technicien" })
```

### ❓ Variante — compter par condition sur un nombre :
> Afficher le nombre de batteries dont le `nombre_cycles` est entre 600 et 1000.

```js
db.batteries.countDocuments({ nombre_cycles: { $gte: 600, $lte: 1000 } })
```

---

## 4. Supprimer des documents (`deleteOne` / `deleteMany`)

### ❓ Question type :
> Supprimer le salarié ayant l'`_id` `"s3"`.

```js
db.salaries.deleteOne({ _id: "s3" })
```

### ❓ Variante — deleteMany avec condition :
> Supprimer tous les événements qui ont une durée supérieure à 15.

```js
db.evenements.deleteMany({ Durée: { $gt: 15 } })
```

### ❓ Variante — supprimer selon plusieurs conditions :
> Supprimer les batteries dont le statut est `"en panne"` et la capacité est inférieure à 20%.

```js
db.batteries.deleteMany({
    statut: "en panne",
    capacite: { $lt: "20%" }
})
```

> ⚠️ Si `capacite` est stocké comme **nombre** :
```js
db.batteries.deleteMany({
    statut: "en panne",
    capacite: { $lt: 20 }
})
```

---

## 5. Modifier des documents (`updateOne` / `updateMany`)

### ❓ Question type :
> Modifier la valeur `coût journalier` de l'événement ayant l'`id=e2` à 600.

```js
db.evenements.updateOne(
    { _id: "e2" },
    { $set: { cout_journalier: 600 } }
)
```

### ❓ Variante — updateMany :
> Modifier le champ `santé_batterie` à `"faible"` pour toutes les batteries dont le statut est `"en panne"` et la capacité est inférieure à 50%.

```js
db.batteries.updateMany(
    { statut: "en panne", capacite: { $lt: "50%" } },
    { $set: { santé_batterie: "faible" } }
)
```

### ❓ Variante — diminuer une valeur (`$mul` ou `$inc`) :
> Diminuer de 2% la valeur actuelle de toutes les voitures.

```js
db.voitures.updateMany(
    {},
    { $mul: { Valeur_actuelle: 0.98 } }
)
```

### ❓ Variante — augmenter de 1% :
> Augmenter de 1% le prix de tous les terrains agricoles des propriétaires cin=P1 et cin=P4.

```js
db.terrains.updateMany(
    { type: "agricole", cin: { $in: ["P1", "P4"] } },
    { $mul: { prix: 1.01 } }
)
```

### ❓ Variante — ajouter un champ (`$set`) à tous les documents :
> Ajouter un champ `compression` (booléen) à tous les documents de la collection.

```js
db.FluxVideo.updateMany(
    {},
    { $set: { compression: false } }
)
```

---

## 6. Afficher par nombre de salariés par catégorie (`aggregate` + `$group`)

### ❓ Question type :
> Afficher le nombre de salariés par fonction.

```js
db.salaries.aggregate([
    { $group: { _id: "$fonction", total: { $sum: 1 } } }
])
```

### ❓ Variante — grouper et compter avec condition :
> Afficher le nombre total de terrains pour chaque type.

```js
db.terrains.aggregate([
    { $group: { _id: "$type", total: { $sum: 1 } } }
])
```

### ❓ Variante — grouper et calculer une moyenne :
> Afficher la moyenne des nombres de cycles des batteries par statut.

```js
db.batteries.aggregate([
    { $group: { _id: "$statut", moyenne_cycles: { $avg: "$nombre_cycles" } } }
])
```

### ❓ Variante — compter le nombre de propriétaires par pays d'accueil :
> Afficher le nombre total de propriétaires par pays d'accueil.

```js
db.voitures.aggregate([
    { $group: { _id: "$proprietaire.Pays_accueil", total: { $sum: 1 } } }
])
```

---

## 7. Filtres avancés

### ❓ `$in` — valeur dans une liste :
> Supprimer toutes les voitures du propriétaire `num_pass=P1`.

```js
db.voitures.deleteMany({ "proprietaire.Num_pass": "P1" })
```

### ❓ Condition sur un champ imbriqué (document nested) :
> Afficher les propriétaires qui sont nés à Paris.

```js
db.voitures.find({ "proprietaire.lieu_naissance": "Paris" })
```

### ❓ Condition sur un champ de date :
> Afficher les propriétaires qui ont une voiture mise en circulation avant le `'2023-02-11'`.

```js
db.voitures.find({
    Date_circulation: { $lt: ISODate("2023-02-11") }
})
```

### ❓ `$exists` — filtrer par présence d'un champ :
> Afficher toutes les sorties d'eau issues des points de sortie qui n'ont pas l'attribut `TotalProductionElectricite`.

```js
db.SortieBarrage.find({
    "PointSortie.TotalProductionElectricite": { $exists: false }
})
```

### ❓ `$regex` — filtrer par pattern :
> Afficher les notaires qui travaillent à `"Rabat"`, triés par nom croissant.

```js
db.terrains.find(
    { "notaire.Adresse": /Rabat/i }
).sort({ "notaire.nom": 1 })
```

---

## 8. Opérations sur des tableaux imbriqués

### ❓ `$push` — ajouter un élément dans un tableau :
> Ajouter un nouveau flux vidéo à l'intérieur du tableau `fluxVideo` du satellite `SAT-MOROCCO-01`.

```js
db.FluxVideo.updateOne(
    { _id: "SAT-MOROCCO-01" },
    {
        $push: {
            fluxVideo: {
                idFlux: "FLUX-003",
                camera: { idCam: "CAM-103", nom: "Caméra Arrière", type: "fixe", resolution: "HD" },
                debitMbps: 60,
                latenceMs: 80,
                qualite: "HD",
                statutDiffusion: "en direct"
            }
        }
    }
)
```

### ❓ `$pull` — supprimer un élément d'un tableau selon une condition :
> Supprimer du satellite `SAT-MOROCCO-01` tous les flux dont la latence est supérieure à 150 ms.

```js
db.FluxVideo.updateOne(
    { _id: "SAT-MOROCCO-01" },
    {
        $pull: {
            fluxVideo: { latenceMs: { $gt: 150 } }
        }
    }
)
```

### ❓ `$size` — afficher le nombre d'ateliers :
> Afficher le nombre des ateliers de l'événement ayant le thème `"Développement Web"`.

```js
db.evenements.aggregate([
    { $match: { theme: "Développement Web" } },
    { $project: { nombreAteliers: { $size: "$Ateliers" } } }
])
```

---

## 9. Agrégation avancée (`$unwind`, `$match`, `$project`, `$sort`, `$limit`)

### ❓ Compter le nombre de flux actifs par satellite :
> Écrire une requête d'agrégation pour compter, pour chaque satellite, le nombre total de flux vidéo actifs (`statutDiffusion = "en direct"`).

```js
db.FluxVideo.aggregate([
    { $unwind: "$fluxVideo" },
    { $match: { "fluxVideo.statutDiffusion": "en direct" } },
    { $group: { _id: "$nom", totalFluxActifs: { $sum: 1 } } }
])
```

### ❓ Latence moyenne par type de caméra :
> Calculer la latence moyenne des flux par type de caméra (fixe ou mobile).

```js
db.FluxVideo.aggregate([
    { $unwind: "$fluxVideo" },
    { $group: {
        _id: "$fluxVideo.camera.type",
        latenceMoyenne: { $avg: "$fluxVideo.latenceMs" }
    }}
])
```

### ❓ Satellites dépassant 400 Mbps de bande passante :
> Afficher les satellites dont la bande passante utilisée totale dépasse 400 Mbps.

```js
db.FluxVideo.aggregate([
    { $match: { bandePassanteUtilisee: { $gt: 400 } } },
    { $project: {
        nom: 1,
        bandePassanteUtilisee: 1,
        cameras: "$fluxVideo.camera.nom"
    }}
])
```

### ❓ Top 3 flux par satellite (débit le plus élevé) :
> Retourner pour chaque satellite les 3 flux ayant le plus haut débit, triés par ordre décroissant.

```js
db.FluxVideo.aggregate([
    { $unwind: "$fluxVideo" },
    { $sort: { "fluxVideo.debitMbps": -1 } },
    { $group: {
        _id: "$nom",
        topFlux: { $push: { idFlux: "$fluxVideo.idFlux", debitMbps: "$fluxVideo.debitMbps" } }
    }},
    { $project: {
        nom: "$_id",
        topFlux: { $slice: ["$topFlux", 3] }
    }}
])
```

### ❓ Satellites dont bande passante utilisée > 70% de la capacité max :
> Afficher tous les satellites dont la bande passante utilisée dépasse 70% de leur capacité maximale.

```js
db.FluxVideo.aggregate([
    { $match: {
        $expr: { $gt: ["$bandePassanteUtilisee", { $multiply: ["$bandePassanteMax", 0.7] }] }
    }}
])
```

---

## 10. Afficher le prix max / min

### ❓ Question type :
> Afficher le prix maximum de tous les terrains de type agricole.

```js
db.terrains.aggregate([
    { $match: { type: "agricole" } },
    { $group: { _id: null, prixMax: { $max: "$prix" } } }
])
```

---

## 11. Supprimer un champ d'un document (`$unset`)

### ❓ Question type :
> Supprimer l'adresse et le téléphone du notaire `numn=N2`.

```js
db.terrains.updateMany(
    { "notaire.Numn": "N2" },
    { $unset: { "notaire.Adresse": "", "notaire.Tel": "" } }
)
```

---

## 12. Exporter une collection vers JSON

### ❓ Question type :
> Exporter tous les documents de la collection `zones_affectees` dans un fichier JSON nommé `zones_affectees.json`.

```bash
mongoexport --db GestionCatastrophes --collection zones_affectees --out zones_affectees.json
```

---

## 13. Ajouter un nouveau document (insertOne avec données complètes)

### ❓ Question type :
> Ajouter une nouvelle sortie d'eau issue de la même pompe P19 (du même barrage) avec :
> - Date Début : 2024-06-05 03:05:00
> - Date Fin : 2024-06-05 12:35:00
> - Quantité Sortie : 13680.00

```js
db.SortieBarrage.insertOne({
    DateDebut: ISODate("2024-06-05T03:05:00Z"),
    DateFin:   ISODate("2024-06-05T12:35:00Z"),
    QuantiteTotale: 13680.0,
    PointSortie: {
        Numero: "P19",
        QuantiteTotaleEauSortie: 670300,
        TotalConsommationElectricite: 91
    },
    Barrage: {
        Nom: "sad el wahda",
        CapaciteMaximale: 14000000,
        VolumeActuel: 10000000,
        VolumeMinimal: 1000000
    }
})
```

---

---

# 🔴 PARTIE MYSQL

---

## 1. Créer une table (`CREATE TABLE`)

### ❓ Question type :
> Écrire le script de création de la table `Capteur`.

```sql
CREATE TABLE Capteur (
    id              INT PRIMARY KEY AUTO_INCREMENT,
    numero_serie    VARCHAR(50) NOT NULL,
    type            VARCHAR(50),
    precision       DECIMAL(5,2),
    niveau_charge   INT,
    nombre_mesures  INT,
    statut          VARCHAR(30)
);
```

### ❓ Variante — avec clés étrangères :
> Script de création de la table `Flux`.

```sql
CREATE TABLE Flux (
    idFlux          INT PRIMARY KEY AUTO_INCREMENT,
    idSat           INT NOT NULL,
    idCam           INT NOT NULL,
    dateDebut       DATETIME,
    dateFin         DATETIME,
    latenceMoyenne  DECIMAL(8,2),
    FOREIGN KEY (idSat) REFERENCES Satellite(idSat),
    FOREIGN KEY (idCam) REFERENCES Camera(idCam)
);
```

### ❓ Variante — table `TurbineHydrolique` :

```sql
CREATE TABLE TurbineHydrolique (
    PointSortieId               VARCHAR(10),
    ProductionElectriciteParHeure DECIMAL(10,2),
    TotalProductionElectricite  DECIMAL(10,2),
    PRIMARY KEY (PointSortieId),
    FOREIGN KEY (PointSortieId) REFERENCES PointSortie(PointSortieId)
);
```

---

## 2. Modifier une table (`ALTER TABLE`)

### ❓ Question type :
> Modifier la table `Vélo` en ajoutant la colonne `nb_batteries` de type entier positif et non nul.

```sql
ALTER TABLE Velo
ADD COLUMN nb_batteries INT UNSIGNED NOT NULL;
```

### ❓ Variante — ajouter une colonne DATE :
> Modifier la table `Capteur` en ajoutant une colonne `date_installation` de type DATE, non nulle.

```sql
ALTER TABLE Capteur
ADD COLUMN date_installation DATE NOT NULL;
```

---

## 3. Procédure stockée (`PROCEDURE`)

### ❓ Question type :
> Créer une procédure `P_capteursParStatut` qui affiche le nombre de capteurs pour chaque statut.

```sql
DELIMITER //
CREATE PROCEDURE P_capteursParStatut()
BEGIN
    SELECT statut, COUNT(*) AS nombre
    FROM Capteur
    GROUP BY statut;
END //
DELIMITER ;

-- Appel :
CALL P_capteursParStatut();
```

### ❓ Variante — procédure qui affiche des données conditionnelles :
> Créer une procédure `P_voituresParAgence` qui affiche le nombre de voitures pour chaque agence.

```sql
DELIMITER //
CREATE PROCEDURE P_voituresParAgence()
BEGIN
    SELECT a.nom AS agence, COUNT(v.id) AS nb_voitures
    FROM Agence a
    LEFT JOIN Voiture v ON v.id_agence = a.id
    GROUP BY a.nom;
END //
DELIMITER ;
```

### ❓ Variante — procédure qui met à jour un statut (avec paramètre) :
> Créer une procédure `ps_update_statut_catastrophe(catastrophe_id INT)` qui met à jour le statut à `"terminee"` si l'écart de financement est ≤ 0.

```sql
DELIMITER //
CREATE PROCEDURE ps_update_statut_catastrophe(IN catastrophe_id INT)
BEGIN
    DECLARE ecart DECIMAL(15,2);
    SET ecart = fn_get_ecart_financement(catastrophe_id);

    IF ecart <= 0 THEN
        UPDATE Catastrophes
        SET statut = 'terminee'
        WHERE id = catastrophe_id;
    END IF;
END //
DELIMITER ;
```

### ❓ Variante — procédure qui affiche les années avec le plus de rejets :
> Écrire une procédure qui affiche les années où on a rejeté le plus de demandes.

```sql
DELIMITER //
CREATE PROCEDURE AfficherAnneesMaxRejets()
BEGIN
    DECLARE max_rejets INT;

    SELECT MAX(nb) INTO max_rejets FROM (
        SELECT YEAR(date_demande) AS annee, COUNT(*) AS nb
        FROM demande_dedouanement
        WHERE etat = 'rejetée'
        GROUP BY YEAR(date_demande)
    ) AS stats;

    SELECT YEAR(date_demande) AS annee, COUNT(*) AS nb_rejets
    FROM demande_dedouanement
    WHERE etat = 'rejetée'
    GROUP BY YEAR(date_demande)
    HAVING nb_rejets = max_rejets;
END //
DELIMITER ;
```

---

## 4. Fonction (`FUNCTION`)

### ❓ Question type :
> Écrire la fonction `fct_chargeFaible` qui reçoit l'identifiant d'un capteur et retourne 1 si son niveau de charge est inférieur à 15, sinon 0.

```sql
DELIMITER //
CREATE FUNCTION fct_chargeFaible(p_id INT)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE charge INT;
    SELECT niveau_charge INTO charge FROM Capteur WHERE id = p_id;

    IF charge < 15 THEN
        RETURN 1;
    ELSE
        RETURN 0;
    END IF;
END //
DELIMITER ;
```

### ❓ Variante — fonction qui retourne le code d'un salarié :
> Créer une fonction permettant de retourner le code du salarié utilisant une voiture dont le matricule et la date sont passés en paramètre.

```sql
DELIMITER //
CREATE FUNCTION fct_codeSalarieVoiture(p_matricule VARCHAR(20), p_date DATE)
RETURNS VARCHAR(10)
DETERMINISTIC
BEGIN
    DECLARE v_code VARCHAR(10);

    SELECT u.codeSal INTO v_code
    FROM Utilisation u
    JOIN Voiture v ON v.matricule = u.matricule
    WHERE v.matricule = p_matricule
      AND p_date BETWEEN u.dateDébutUtilisation AND u.dateFinUTilisation
    LIMIT 1;

    RETURN v_code;
END //
DELIMITER ;
```

### ❓ Variante — fonction qui retourne le coût total :
> Créer une fonction `fn_get_montant_total_reconstruction(zone_id INT)` qui retourne le coût total estimé pour la reconstruction des infrastructures dans une zone donnée.

```sql
DELIMITER //
CREATE FUNCTION fn_get_montant_total_reconstruction(zone_id INT)
RETURNS DECIMAL(15,2)
DETERMINISTIC
BEGIN
    DECLARE total DECIMAL(15,2);

    SELECT COALESCE(SUM(cout_estime_reconstruction), 0) INTO total
    FROM Infrastructures
    WHERE zone_affectee_id = zone_id;

    RETURN total;
END //
DELIMITER ;
```

### ❓ Variante — fonction `fct_ProcheMinium` (condition sur différence) :
> Écrire la fonction `fct_ProcheMinimum(p_idBarrage)` qui retourne 1 si la différence entre le volume actuel et le volume minimal est inférieure à 1000, sinon 0.

```sql
DELIMITER //
CREATE FUNCTION fct_ProcheMinimum(p_idBarrage INT)
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE v_actuel DOUBLE;
    DECLARE v_minimal DOUBLE;

    SELECT VolumeActuel, VolumeMinimal
    INTO v_actuel, v_minimal
    FROM Barrage
    WHERE BarrageId = p_idBarrage;

    IF (v_actuel - v_minimal) < 1000 THEN
        RETURN 1;
    ELSE
        RETURN 0;
    END IF;
END //
DELIMITER ;
```

---

## 5. Trigger (`TRIGGER`)

### ❓ Question type (validation d'une plage de valeurs) :
> Écrire le trigger `TR1` qui contrôle la valeur de la colonne `sante_batterie` qui doit être un nombre entre 0 et 100, lors de l'ajout d'une nouvelle batterie.

```sql
DELIMITER //
CREATE TRIGGER TR1
BEFORE INSERT ON Batterie
FOR EACH ROW
BEGIN
    IF NEW.sante_batterie < 0 OR NEW.sante_batterie > 100 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'La santé batterie doit être entre 0 et 100';
    END IF;
END //
DELIMITER ;
```

### ❓ Variante — trigger qui change un statut (AFTER INSERT) :
> Écrire le trigger `TR2` qui modifie la valeur de la colonne `statut` de `"En Utilisation"` à `"Retiré"` lors d'un ajout dans la table `Changement`.

```sql
DELIMITER //
CREATE TRIGGER TR2
AFTER INSERT ON Changement
FOR EACH ROW
BEGIN
    UPDATE Batterie
    SET statut = 'Retiré'
    WHERE id = NEW.id_ancienne_batterie;
END //
DELIMITER ;
```

### ❓ Variante — trigger qui empêche une insertion conditionnelle :
> Créer un trigger qui empêche, lors de l'insertion d'une nouvelle utilisation, d'affecter une voiture à un salarié attaché au service `"informatique"`.

```sql
DELIMITER //
CREATE TRIGGER tr_empecheInformatique
BEFORE INSERT ON Utilisation
FOR EACH ROW
BEGIN
    DECLARE v_service VARCHAR(50);

    SELECT s.nomSer INTO v_service
    FROM salarié sal
    JOIN service s ON s.codeSer = sal.codeSer
    WHERE sal.codeSal = NEW.codeSal;

    IF v_service = 'informatique' THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Un salarié du service informatique ne peut pas utiliser une voiture';
    END IF;
END //
DELIMITER ;
```

### ❓ Variante — trigger qui empêche une inscription en juillet :
> Créer un trigger qui interdit l'inscription effectuée au cours du mois de juillet de l'année actuelle.

```sql
DELIMITER //
CREATE TRIGGER tr_interdireInscriptionJuillet
BEFORE INSERT ON Inscriptions
FOR EACH ROW
BEGIN
    IF MONTH(NEW.dateInscription) = 7 AND YEAR(NEW.dateInscription) = YEAR(CURDATE()) THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Les inscriptions sont interdites en juillet';
    END IF;
END //
DELIMITER ;
```

### ❓ Variante — trigger qui utilise une fonction déjà créée :
> Écrire le trigger `TR_anomalie` qui empêche l'insertion d'une anomalie si le capteur a une batterie critique (en utilisant `fct_batterieFaible`).

```sql
DELIMITER //
CREATE TRIGGER TR_anomalie
BEFORE INSERT ON Anomalie
FOR EACH ROW
BEGIN
    IF fct_batterieFaible(NEW.id_capteur) = 1 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Capteur avec batterie critique : insertion interdite';
    END IF;
END //
DELIMITER ;
```

### ❓ Variante — trigger AFTER INSERT qui insère dans une autre table :
> Trigger `tr_after_donation_insert` : après l'insertion d'une donation, si le montant est > 100 000, insérer un message dans la table `Logs`.

```sql
DELIMITER //
CREATE TRIGGER tr_after_donation_insert
AFTER INSERT ON Donations
FOR EACH ROW
BEGIN
    IF NEW.montant > 100000 THEN
        INSERT INTO Logs (action_id, donateur_id, montant, message)
        VALUES (NEW.action_id, NEW.donateur_id, NEW.montant,
                CONCAT('Donation importante : montant ', NEW.montant, ' pour l\'action ', NEW.action_id));
    END IF;
END //
DELIMITER ;
```

### ❓ Variante — trigger qui empêche le paiement si demande rejetée :
> Écrire un déclencheur qui empêche le paiement des droits de dédouanement si la demande est rejetée.

```sql
DELIMITER //
CREATE TRIGGER tr_empechePaiementRejete
BEFORE INSERT ON paiement_dedouanement
FOR EACH ROW
BEGIN
    DECLARE v_etat VARCHAR(20);

    SELECT etat INTO v_etat
    FROM demande_dedouanement
    WHERE iddemande = NEW.iddemande;

    IF v_etat = 'rejetée' THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Paiement impossible : demande rejetée';
    END IF;
END //
DELIMITER ;
```

### ❓ Variante — trigger qui empêche la suppression de dossiers récents :
> Trigger qui empêche de supprimer des dossiers de moins de 2 ans qui ne sont pas clôturés.

```sql
DELIMITER //
CREATE TRIGGER tr_empecheSuppressionDossier
BEFORE DELETE ON Dossier_terrain
FOR EACH ROW
BEGIN
    IF OLD.etat != 'traité' AND OLD.date_ouverture > DATE_SUB(CURDATE(), INTERVAL 2 YEAR) THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Impossible de supprimer un dossier de moins de 2 ans non clôturé';
    END IF;
END //
DELIMITER ;
```

### ❓ Variante — trigger qui empêche si volume critique (utilise une fonction) :
> Trigger `tr_BarrageVolumeMin` qui n'autorise pas l'ajout d'un historique de sortie si le volume actuel est proche du niveau critique.

```sql
DELIMITER //
CREATE TRIGGER tr_BarrageVolumeMin
BEFORE INSERT ON HistoriqueSortie
FOR EACH ROW
BEGIN
    DECLARE v_barrageId INT;

    SELECT BarrageId INTO v_barrageId
    FROM PointSortie
    WHERE PointSortieId = NEW.PointSortieId;

    IF fct_ProcheMinimum(v_barrageId) = 1 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Volume du barrage proche du minimum : sortie interdite';
    END IF;
END //
DELIMITER ;
```

---

## 6. Requêtes SQL (`SELECT` avancé)

### ❓ Afficher avec JOIN et filtre :
> Afficher les voitures de marque `"peugeot"` importées de la France et mises en circulation cette année.

```sql
SELECT v.*
FROM voiture v
JOIN pays p ON p.id_pays_importation = v.id_pays_importation
WHERE v.marque = 'peugeot'
  AND p.nompays = 'France'
  AND YEAR(v.date_mise_circulation) = YEAR(CURDATE());
```

### ❓ Afficher le nombre de voitures par salarié depuis son embauche :
> Afficher la liste des salariés (code, nom, prénom, service, nombre de voitures utilisées depuis l'embauche).

```sql
SELECT s.codeSal, s.nomSal, s.prenomSal, sr.nomSer,
       COUNT(u.matricule) AS nb_voitures
FROM salarié s
JOIN service sr ON sr.codeSer = s.codeSer
LEFT JOIN Utilisation u ON u.codeSal = s.codeSal
    AND u.dateDébutUtilisation >= s.dateEmbauche
GROUP BY s.codeSal, s.nomSal, s.prenomSal, sr.nomSer;
```

### ❓ Requête avec agrégation et `HAVING` :
> Afficher les notaires qui ont traité plus de 200 dossiers.

```sql
SELECT n.nom, n.prenom
FROM Notaire n
JOIN Dossier_terrain d ON d.numnotaire = n.numn
GROUP BY n.nom, n.prenom
HAVING COUNT(d.iddossier) > 200;
```

### ❓ Requête avec `ORDER BY` et date :
> Afficher la liste d'historique des sorties d'eau du Barrage `"sed El Manaa"` classées en ordre croissant selon la date.

```sql
SELECT hs.*
FROM HistoriqueSortie hs
JOIN PointSortie ps ON ps.PointSortieId = hs.PointSortieId
JOIN Barrage b ON b.BarrageId = ps.BarrageId
WHERE b.Nom = 'sed El Manaa'
ORDER BY hs.DateDebut ASC;
```

### ❓ Requête sur les cabinets ayant clôturé des dossiers entre 2 dates :
> Afficher tous les cabinets de la ville Casablanca qui ont clôturé des dossiers entre le 2 janvier et le 31 Mai 2005.

```sql
SELECT DISTINCT c.nom, c.adresse
FROM Cabinet c
JOIN Notaire n ON n.id_cab = c.id_cab
JOIN Dossier_terrain d ON d.numnotaire = n.numn
WHERE c.ville = 'Casablanca'
  AND d.date_cloture BETWEEN '2005-01-02' AND '2005-05-31';
```

---

## 7. Fonction qui retourne un nombre de demandes (rejetées + validées)

### ❓ Question type :
> Écrire une fonction qui retourne, pour un bénéficiaire passé en paramètre, le nombre de ses demandes rejetées et le nombre de ses demandes validées.

```sql
DELIMITER //
CREATE FUNCTION fct_statsDemandesBeneficiaire(p_cin VARCHAR(20))
RETURNS VARCHAR(100)
DETERMINISTIC
BEGIN
    DECLARE nb_rejetees INT;
    DECLARE nb_validees INT;

    SELECT COUNT(*) INTO nb_rejetees
    FROM demande_dedouanement d
    JOIN voiture v ON v.numvoiure = d.numvoiture
    WHERE v.cin = p_cin AND d.etat = 'rejetée';

    SELECT COUNT(*) INTO nb_validees
    FROM demande_dedouanement d
    JOIN voiture v ON v.numvoiure = d.numvoiture
    WHERE v.cin = p_cin AND d.etat = 'validée';

    RETURN CONCAT('Rejetées: ', nb_rejetees, ' | Validées: ', nb_validees);
END //
DELIMITER ;
```

---

## 8. Gestion des utilisateurs et des rôles

### ❓ Question type (4 étapes classiques) :
> a. Créer le rôle `RoleTechnicien`
> b. Donner les droits d'ajout, suppression et modification sur les tables `Velo` et `Changement`
> c. Créer l'utilisateur `ali` avec le mot de passe `0000`
> d. Attribuer le rôle `RoleTechnicien` à l'utilisateur `Ali` déjà créé

```sql
-- a. Créer le rôle
CREATE ROLE 'RoleTechnicien';

-- b. Donner les droits
GRANT INSERT, UPDATE, DELETE ON nom_base.Velo TO 'RoleTechnicien';
GRANT INSERT, UPDATE, DELETE ON nom_base.Changement TO 'RoleTechnicien';

-- c. Créer l'utilisateur
CREATE USER 'ali'@'localhost' IDENTIFIED BY '0000';

-- d. Attribuer le rôle
GRANT 'RoleTechnicien' TO 'ali'@'localhost';
```

### ❓ Variante — droits SELECT/INSERT seulement :
> Créer le rôle `TechnicienFlux`, lui donner les droits SELECT/INSERT sur `Flux`, créer l'utilisateur `tech1` avec mot de passe `pass123`.

```sql
CREATE ROLE 'TechnicienFlux';
GRANT SELECT, INSERT ON satstream_db.Flux TO 'TechnicienFlux';
CREATE USER 'tech1'@'localhost' IDENTIFIED BY 'pass123';
GRANT 'TechnicienFlux' TO 'tech1'@'localhost';
```

---

## 9. Script de création de table (avec contraintes avancées)

### ❓ Script complet avec `CHECK`, `DEFAULT`, FK :
> Écrire le script de création de la table `Parking`.

```sql
CREATE TABLE Parking (
    id                       INT PRIMARY KEY AUTO_INCREMENT,
    nom                      VARCHAR(100) NOT NULL,
    id_zone                  INT NOT NULL,
    id_capteur               INT NOT NULL,
    nb_places                INT NOT NULL,
    taux_occupation          INT DEFAULT 0,
    date_derniere_maintenance DATE,
    date_prochaine_maintenance DATE,
    FOREIGN KEY (id_zone)    REFERENCES Zone(id),
    FOREIGN KEY (id_capteur) REFERENCES Capteur(id)
);
```

---

## 10. Requête avec `SUM` et `COUNT` par groupe

### ❓ Afficher le nombre de capteurs par service :
> Créer une procédure stockée qui affiche le nombre de salariés de chaque service.

```sql
DELIMITER //
CREATE PROCEDURE AfficherNbSalariesParService()
BEGIN
    SELECT sr.nomSer, COUNT(s.codeSal) AS nb_salaries
    FROM service sr
    LEFT JOIN salarié s ON s.codeSer = sr.codeSer
    GROUP BY sr.nomSer;
END //
DELIMITER ;
```

### ❓ Afficher la somme par barrage :
> Afficher pour chaque barrage le nom ainsi que la quantité totale d'eau entrée.

```sql
SELECT b.Nom, SUM(e.QuantiteTotale) AS total_eau_entree
FROM Barrage b
LEFT JOIN EntreeEau e ON e.BarrageId = b.BarrageId
GROUP BY b.Nom;
```

---

## 11. Requête retournant la 3ème plus grande valeur

### ❓ Question type :
> Afficher la troisième plus grande quantité parmi toutes les sorties effectuées.

```sql
SELECT QuantiteTotale
FROM SortieBarrage
ORDER BY QuantiteTotale DESC
LIMIT 1 OFFSET 2;
```

---

## 12. Requête avec sous-requête

### ❓ Afficher les notaires qui n'ont traité aucun dossier d'un type donné :
> Écrire une fonction qui retourne le nombre de notaires qui n'ont traité aucun dossier concernant un type de terrain passé en paramètre.

```sql
DELIMITER //
CREATE FUNCTION fct_nbNotairesSansType(p_type VARCHAR(50))
RETURNS INT
DETERMINISTIC
BEGIN
    DECLARE nb INT;

    SELECT COUNT(*) INTO nb
    FROM Notaire n
    WHERE n.numn NOT IN (
        SELECT d.numnotaire
        FROM Dossier_terrain d
        JOIN Terrain t ON t.numter = d.numter
        WHERE t.typeTerrain = p_type
    );

    RETURN nb;
END //
DELIMITER ;
```

---

## 📋 Tableau récapitulatif — Commandes fréquentes

### MongoDB — Cheat Sheet rapide

| Opération | Syntaxe |
|---|---|
| Créer DB + collection | `use NomDB` puis `db.createCollection("nom")` |
| Insérer 1 document | `db.col.insertOne({...})` |
| Insérer plusieurs | `db.col.insertMany([{...},{...}])` |
| Afficher tout | `db.col.find()` |
| Filtrer | `db.col.find({ champ: valeur })` |
| Trier croissant | `.sort({ champ: 1 })` |
| Trier décroissant | `.sort({ champ: -1 })` |
| Compter | `db.col.countDocuments({ condition })` |
| Modifier 1 | `db.col.updateOne({ filtre }, { $set: {...} })` |
| Modifier plusieurs | `db.col.updateMany({ filtre }, { $set: {...} })` |
| Supprimer 1 | `db.col.deleteOne({ filtre })` |
| Supprimer plusieurs | `db.col.deleteMany({ filtre })` |
| Supprimer un champ | `{ $unset: { champ: "" } }` |
| Ajouter dans tableau | `{ $push: { tableau: element } }` |
| Supprimer d'un tableau | `{ $pull: { tableau: { condition } } }` |
| Multiplier une valeur | `{ $mul: { champ: facteur } }` |
| Grouper + compter | `$group: { _id: "$champ", total: { $sum: 1 } }` |
| Grouper + moyenne | `$group: { _id: "$champ", moy: { $avg: "$champ2" } }` |
| Dérouler un tableau | `$unwind: "$tableau"` |
| Limiter résultats | `$limit: N` |
| Champ existe / n'existe pas | `{ champ: { $exists: true/false } }` |
| Valeur dans liste | `{ champ: { $in: ["a","b"] } }` |
| Comparaison | `$gt, $gte, $lt, $lte, $ne` |
| Plusieurs conditions ET | `{ $and: [{...},{...}] }` (ou juste `{a:x, b:y}`) |
| Plusieurs conditions OU | `{ $or: [{...},{...}] }` |
| Export JSON | `mongoexport --db DB --collection col --out file.json` |

---

### MySQL — Cheat Sheet rapide

| Opération | Syntaxe |
|---|---|
| Créer table | `CREATE TABLE nom (...)` |
| Clé primaire | `id INT PRIMARY KEY AUTO_INCREMENT` |
| Clé étrangère | `FOREIGN KEY (col) REFERENCES table(col)` |
| Valeur par défaut | `col VARCHAR(30) DEFAULT 'valeur'` |
| Colonne unique | `col VARCHAR(50) UNIQUE` |
| Colonne NOT NULL | `col INT NOT NULL` |
| Modifier table (ajouter col) | `ALTER TABLE t ADD COLUMN col TYPE` |
| Créer procédure | `CREATE PROCEDURE nom() BEGIN ... END` |
| Créer fonction | `CREATE FUNCTION nom(param TYPE) RETURNS TYPE` |
| Retourner dans fonction | `RETURN valeur;` |
| Déclarer variable | `DECLARE nom TYPE;` |
| Affecter variable | `SELECT champ INTO var FROM t WHERE ...` |
| Créer trigger BEFORE INSERT | `CREATE TRIGGER nom BEFORE INSERT ON t FOR EACH ROW BEGIN ... END` |
| Créer trigger AFTER INSERT | `CREATE TRIGGER nom AFTER INSERT ON t FOR EACH ROW BEGIN ... END` |
| Bloquer avec SIGNAL | `SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'msg'` |
| NEW.colonne (dans trigger) | Valeur de la nouvelle ligne insérée |
| OLD.colonne (dans trigger) | Valeur avant modification/suppression |
| Créer rôle | `CREATE ROLE 'NomRole'` |
| Donner droits | `GRANT INSERT, UPDATE, DELETE ON db.table TO 'role'` |
| Créer utilisateur | `CREATE USER 'nom'@'localhost' IDENTIFIED BY 'mdp'` |
| Attribuer rôle | `GRANT 'NomRole' TO 'nom'@'localhost'` |
| Grouper + compter | `SELECT col, COUNT(*) FROM t GROUP BY col` |
| Grouper + somme | `SELECT col, SUM(col2) FROM t GROUP BY col` |
| Filtrer après group | `HAVING COUNT(*) > N` |
| Jointure | `JOIN table2 ON t1.col = t2.col` |
| Date courante | `CURDATE()` |
| Mois/Année d'une date | `MONTH(date)`, `YEAR(date)` |
| Entre deux valeurs | `BETWEEN val1 AND val2` |
| Null ou pas null | `IS NULL`, `IS NOT NULL` |
| 3ème plus grand | `ORDER BY col DESC LIMIT 1 OFFSET 2` |
| Délimiteur | `DELIMITER //` ... `DELIMITER ;` |
