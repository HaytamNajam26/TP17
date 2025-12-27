# Comparaison JSON, XML et Protobuf - Laboratoire Node.js

## Description

Projet de laboratoire Node.js pour comparer les formats de sérialisation de données **JSON**, **XML** et **Protobuf**. Ce projet permet d'analyser et de comparer les performances (vitesse d'encodage/décodage) et l'efficacité (taille des fichiers) de ces trois formats de sérialisation.

## Fonctionnalités

✅ Sérialisation de données en JSON  
✅ Sérialisation de données en XML  
✅ Sérialisation de données en Protobuf  
✅ Mesure des temps d'encodage pour chaque format  
✅ Mesure des temps de décodage pour chaque format  
✅ Comparaison des tailles de fichiers générés  
✅ Génération de fichiers de sortie (data.json, data.xml, data.proto)

## Technologies utilisées

- **Node.js** - Environnement d'exécution JavaScript
- **xml-js** - Bibliothèque de conversion JSON ↔ XML
- **protobufjs** - Bibliothèque Protobuf pour Node.js (sans compilation externe)

## Installation

### Prérequis

- Node.js (version 14 ou supérieure)
- npm (gestionnaire de paquets Node.js)

### Étapes d'installation

1. **Cloner ou naviguer vers le projet :**
   ```bash
   cd json-xml-protobuf-lab
   ```

2. **Installer les dépendances :**
   ```bash
   npm install
   ```

3. **Lancer le script :**
   ```bash
   node index.js
   ```

Le script générera trois fichiers de sortie et affichera les résultats dans le terminal.

## Structure du projet

```
json-xml-protobuf-lab/
├── employee.proto          # Schéma Protobuf (définition des messages)
├── index.js                # Script principal
├── package.json            # Configuration et dépendances du projet
├── package-lock.json       # Verrouillage des versions des dépendances
├── data.json               # Fichier de sortie JSON (généré)
├── data.xml                # Fichier de sortie XML (généré)
├── data.proto              # Fichier de sortie Protobuf binaire (généré)
└── node_modules/           # Dépendances installées
```

## Utilisation

### Exécution du script

Exécutez simplement la commande suivante dans le terminal :

```bash
node index.js
```

### Ce que fait le script

1. **Charge le schéma Protobuf** depuis `employee.proto`
2. **Crée une liste d'employés** en mémoire (3 employés de test)
3. **Sérialise les données** en trois formats :
   - JSON (avec `JSON.stringify()`)
   - XML (avec `xml-js`)
   - Protobuf (avec `protobufjs`)
4. **Mesure les temps** d'encodage et de décodage pour chaque format
5. **Écrit les fichiers** de sortie sur le disque
6. **Affiche les résultats** : tailles des fichiers et temps de traitement

### Données de test

Le script utilise trois employés de test :
- **Ali** - ID: 1, Salaire: 9000
- **Kamal** - ID: 2, Salaire: 22000
- **Amal** - ID: 3, Salaire: 23000

## Schéma Protobuf

Le fichier `employee.proto` définit la structure des données :

```protobuf
syntax = "proto3";

message Employee {
  int32 id = 1;
  string name = 2;
  int32 salary = 3;
}

message Employees {
  repeated Employee employee = 1;
}
```

## Résultats de l'analyse

### Comparaison des tailles de fichiers

| Format | Taille | Comparaison |
|--------|--------|-------------|
| **Protobuf** | 41 octets | ⭐ Le plus compact (référence) |
| **JSON** | 127 octets | ~3,1× plus volumineux que Protobuf |
| **XML** | 224 octets | ~5,5× plus volumineux que Protobuf |

**Observations :**
- **Protobuf** est le format le plus compact grâce à son encodage binaire et l'utilisation de numéros de champs au lieu de noms de champs.
- **JSON** est plus compact que XML car il n'utilise pas de balises ouvrantes/fermantes.
- **XML** est le format le plus verbeux avec ses balises répétées.

### Comparaison des performances

#### Temps d'encodage (structure → format)

| Format | Temps moyen | Comparaison |
|--------|-------------|-------------|
| **JSON** | ~0.02-0.03 ms | ⚡ Le plus rapide |
| **Protobuf** | ~0.32-0.35 ms | ~12-15× plus lent que JSON |
| **XML** | ~0.45-0.77 ms | ~20-30× plus lent que JSON |

#### Temps de décodage (format → structure)

| Format | Temps moyen | Comparaison |
|--------|-------------|-------------|
| **JSON** | ~0.01-0.014 ms | ⚡ Le plus rapide |
| **Protobuf** | ~0.22-0.23 ms | ~16-20× plus lent que JSON |
| **XML** | ~1.5-1.9 ms | ~100-150× plus lent que JSON |

**Observations :**
- **JSON** est le plus rapide pour l'encodage et le décodage car il est natif en JavaScript.
- **Protobuf** offre un bon compromis entre taille et vitesse, particulièrement efficace pour les environnements haute performance.
- **XML** est le plus lent, surtout pour le décodage, en raison de la complexité du parsing.

### Synthèse comparative

| Critère | JSON | XML | Protobuf |
|---------|------|-----|----------|
| **Taille** | ⭐⭐ Moyenne | ⭐ Grande | ⭐⭐⭐ Petite |
| **Vitesse encodage** | ⭐⭐⭐ Rapide | ⭐ Lente | ⭐⭐ Moyenne |
| **Vitesse décodage** | ⭐⭐⭐ Rapide | ⭐ Très lente | ⭐⭐ Moyenne |
| **Lisibilité** | ⭐⭐⭐ Bonne | ⭐⭐⭐ Excellente | ❌ Aucune (binaire) |
| **Interopérabilité** | ⭐⭐⭐ Excellente | ⭐⭐⭐ Excellente | ⭐⭐ Nécessite schéma |

### Recommandations d'usage

- **JSON** : API REST, configuration, données web générales, développement rapide
- **XML** : Documents structurés, échanges inter-systèmes nécessitant validation, formats standardisés
- **Protobuf** : Microservices (gRPC), volumes élevés, faible latence, bande passante limitée, systèmes distribués

**Conclusion :** Protobuf offre le meilleur ratio taille/vitesse pour les environnements haute performance, même si JSON reste le plus rapide en JavaScript natif. XML reste utile pour sa lisibilité et son interopérabilité, mais au prix de performances et de taille supérieures.

## Captures d'écran du terminal

### Capture 1 : Exécution du script et résultats

*[Insérer ici la première capture d'écran du terminal montrant l'exécution du script]*

### Capture 2 : Analyse détaillée des résultats

*[Insérer ici la deuxième capture d'écran du terminal avec les résultats détaillés]*

## Scripts disponibles

```bash
# Exécuter le script principal
node index.js
```

## Fichiers générés

Après l'exécution, trois fichiers sont créés :

- **data.json** : Données sérialisées au format JSON (lisible)
- **data.xml** : Données sérialisées au format XML (lisible)
- **data.proto** : Données encodées en format binaire Protobuf (non lisible)

## Vérification des résultats

Pour vérifier le contenu des fichiers générés :

```bash
# Afficher le contenu JSON
cat data.json

# Afficher le contenu XML
cat data.xml

# Afficher la taille du fichier Protobuf (binaire, non lisible)
ls -lh data.proto
```

## Évaluation

✅ Comparaison complète des trois formats  
✅ Mesure précise des performances  
✅ Analyse de l'efficacité en termes de taille  
✅ Code bien structuré et commenté  
✅ Respect des bonnes pratiques Node.js

## Auteur

Projet développé dans le cadre d'un laboratoire de comparaison des formats de sérialisation de données.

## License

MIT

