**Note :** Ce guide a été généré par une intelligence artificielle pour vous aider avec les commandes MongoDB.

-----

# Commandes de base MongoDB (mongosh)

| Commande                 | Description                         | Exemple                                  |
| :----------------------- | :---------------------------------- | :--------------------------------------- |
| `mongosh`                | Lancer le shell MongoDB             | `mongosh`                                |
| `use nomDeLaDB`          | Créer/sélectionner une base de données | `use testDB`                             |
| `show dbs`               | Lister les bases de données existantes | `show dbs`                               |
| `db`                     | Affiche la base de données active   | `db`                                     |
| `show collections`       | Lister les collections dans la DB active | `show collections`                       |
| `db.createCollection("nom")` | Créer une nouvelle collection       | `db.createCollection("clients")`         |

-----

# Opérations CRUD (Create, Read, Update, Delete)

-----

## ➕ CREATE

| Commande                               | Description               | Exemple                                                |
| :------------------------------------- | :------------------------ | :----------------------------------------------------- |
| `db.nomCollection.insertOne({...})`    | Insérer un document       | `db.clients.insertOne({nom: "Jean", age: 25})`         |
| `db.nomCollection.insertMany([{...}, {...}])` | Insérer plusieurs documents | `db.clients.insertMany([{nom: "Alice"}, {nom: "Bob"}])` |

-----

## 🔍 READ

| Commande                           | Description                       | Exemple                                  |
| :--------------------------------- | :-------------------------------- | :--------------------------------------- |
| `db.nomCollection.find()`          | Lire tous les documents           | `db.clients.find()`                      |
| `db.nomCollection.findOne({clé: valeur})` | Lire un seul document correspondant | `db.clients.findOne({nom: "Jean"})`      |
| `db.nomCollection.find().pretty()` | Affichage lisible                 | `db.clients.find().pretty()`             |

-----

## ✏️ UPDATE

| Commande                             | Description                 | Exemple                                                     |
| :----------------------------------- | :-------------------------- | :---------------------------------------------------------- |
| `db.nomCollection.updateOne(filter, update)` | Met à jour un document      | `db.clients.updateOne({nom: "Jean"}, {$set: {age: 30}})`    |
| `db.nomCollection.updateMany(filter, update)` | Met à jour plusieurs documents | `db.clients.updateMany({}, {$set: {status: "actif"}})` |

-----

## ❌ DELETE

| Commande                             | Description                 | Exemple                                                |
| :----------------------------------- | :-------------------------- | :----------------------------------------------------- |
| `db.nomCollection.deleteOne(filter)` | Supprime un seul document   | `db.clients.deleteOne({nom: "Jean"})`                  |
| `db.nomCollection.deleteMany(filter)` | Supprime plusieurs documents | `db.clients.deleteMany({age: {$lt: 18}})`              |

-----

# Opérateurs de requête utiles

| Opérateur        | Description                     | Exemple                                     |
| :--------------- | :------------------------------ | :------------------------------------------ |
| `$gt`, `$lt`     | Supérieur/inférieur             | `{age: {$gt: 25}}`                          |
| `$in`, `$nin`    | Inclus / Non inclus dans liste  | `{ville: {$in: ["Paris", "Lyon"]}}`         |
| `$and`, `$or`    | Conditions combinées            | `{$or: [{age: {$lt: 20}}, {ville: "Paris"}]}` |

-----

# Commandes d’administration

| Commande              | Description                     | Exemple               |
| :-------------------- | :------------------------------ | :-------------------- |
| `db.stats()`          | Infos sur la DB actuelle        | `db.stats()`          |
| `db.collection.stats()` | Infos sur une collection        | `db.clients.stats()`  |
| `db.dropDatabase()`   | Supprime la DB active           | `db.dropDatabase()`   |
| `db.collection.drop()` | Supprime une collection         | `db.clients.drop()`   |

-----

# Gestion des utilisateurs (si authentification activée)

| Commande                      | Description                     | Exemple                                             |
| :---------------------------- | :------------------------------ | :-------------------------------------------------- |
| `use admin`                   | Aller sur la DB admin           | `use admin`                                         |
| `db.createUser({...})`        | Créer un utilisateur            | `db.createUser({user: "admin", pwd: "pass", roles: ["root"]})` |
| `db.auth("user", "pass")`     | S’authentifier                  | `db.auth("admin", "pass")`                          |

-----

# Exemple complet de workflow MongoDB

```bash
mongosh
use boutique
db.createCollection("produits")
db.produits.insertMany([
  { nom: "Téléphone", prix: 300 },
  { nom: "Ordinateur", prix: 1000 }
])
db.produits.find()
db.produits.updateOne({ nom: "Téléphone" }, { $set: { prix: 350 } })
db.produits.deleteOne({ nom: "Ordinateur" })
```

-----

# Rôles courants lors de la création d'un utilisateur MongoDB

-----

## 📌 Exemple général de création d’un utilisateur

```javascript
db.createUser({
  user: "nom_utilisateur",
  pwd: "mot_de_passe",
  roles: [
    { role: "readWrite", db: "nom_de_la_db" }
  ]
})
```

-----

## 🧾 Liste des rôles prédéfinis importants

| Rôle                     | Description                                            | Base de données           |
| :----------------------- | :----------------------------------------------------- | :------------------------ |
| `read`                   | Lecture seule (find) sur une base                      | Spécifique                |
| `readWrite`              | Lecture/écriture (find, insert, update, delete)        | Spécifique                |
| `dbAdmin`                | Accès aux outils d'administration de la DB (indexes, stats, profiler) | Spécifique                |
| `userAdmin`              | Gère les utilisateurs et rôles dans une base           | Spécifique                |
| `dbOwner`                | Toutes les permissions sur une base (readWrite, dbAdmin, userAdmin) | Spécifique                |
| `clusterAdmin`           | Administration du cluster MongoDB (sharding, etc.)     | `admin` uniquement      |
| `readAnyDatabase`        | Lecture sur toutes les bases                           | `admin` uniquement      |
| `readWriteAnyDatabase`   | Lecture/écriture sur toutes les bases                  | `admin` uniquement      |
| `userAdminAnyDatabase`   | Gérer les utilisateurs sur toutes les bases            | `admin` uniquement      |
| `dbAdminAnyDatabase`     | Admin de toutes les bases (statistiques, indexes…)     | `admin` uniquement      |
| `root`                   | Accès total à toutes les bases et fonctions admin      | `admin` uniquement      |

-----

# Exemples pratiques

-----

## ✅ Utilisateur applicatif avec lecture/écriture

```javascript
db.createUser({
  user: "app_user",
  pwd: "securePass123",
  roles: [ { role: "readWrite", db: "app_db" } ]
})
```

-----

## ✅ Utilisateur admin complet

```javascript
use admin
db.createUser({
  user: "admin",
  pwd: "adminpass",
  roles: [ { role: "root", db: "admin" } ]
})
```

-----

## ✅ Utilisateur lecture seule sur plusieurs bases

```javascript
db.getSiblingDB("db1").createUser({
  user: "readonly",
  pwd: "readonlypass",
  roles: [
    { role: "read", db: "db1" },
    { role: "read", db: "db2" }
  ]
})
```

-----

## ✨ Astuce : Créer des rôles personnalisés

MongoDB permet aussi de créer des rôles personnalisés si les rôles prédéfinis ne couvrent pas vos besoins spécifiques.

Exemple :

```javascript
db.createRole({
  role: "lectureAvecProfilage",
  privileges: [
    { resource: { db: "app_db", collection: "" }, actions: ["find", "profile"] }
  ],
  roles: []
})
```

Puis assigner ce rôle à un utilisateur.

-----
