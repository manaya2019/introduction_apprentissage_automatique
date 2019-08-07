## Sélection des données pour la modélisation

Si votre jeu de données contient trop de variables, pandas offre la possibilité de les réduire afin de le rendre lisible
et plus facile à interpreter. 

Comment pouvez-vous réduire cette quantité écrasante de données à quelque chose que vous pouvez comprendre?
Nous allons commencer par choisir quelques variables en utilisant notre intuition.
Les cours ultérieurs vous montreront des techniques statistiques pour hiérarchiser automatiquement les variables.

Pour choisir des variables / colonnes, nous aurons besoin de voir une liste de toutes les colonnes de l'ensemble de données. 
Cela se fait avec la propriété columns du DataFrame (la dernière ligne du code ci-dessous).
```
import pandas as pd

melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path) 
melbourne_data.columns
```
```
index(['Suburb', 'Address', 'Rooms', 'Type', 'Price', 'Method', 'SellerG',
       'Date', 'Distance', 'Postcode', 'Bedroom2', 'Bathroom', 'Car',
       'Landsize', 'BuildingArea', 'YearBuilt', 'CouncilArea', 'Lattitude',
       'Longtitude', 'Regionname', 'Propertycount'],
      dtype='object')
```
Il existe différentes façons de sélectionner un sous-ensemble de vos données. Le micro-cours sur pandas couvre ces aspects 
plus en profondeur, mais nous allons nous concentrer sur deux approches pour le moment.

1-Notation par points, que nous utilisons pour sélectionner la "cible de prédiction"

2-Sélection avec une liste de colonnes, que nous utilisons pour sélectionner les "features"

## Sélection de la cible de prédiction
Vous pouvez extraire une variable avec la notation par points. Cette colonne unique est stockée dans une série, 
qui ressemble en gros à un DataFrame avec une seule colonne de données.

Nous utiliserons la notation par points pour sélectionner la colonne à prédire, appelée cible de prédiction. 
Par convention, la cible de prédiction est appelée y. 
Donc, le code dont nous avons besoin pour enregistrer les prix de l'immobilier dans les données de Melbourne est:
```
y = melbourne_data.Price
```

## Sélection de features
Les colonnes qui sont entrées dans notre modèle (et utilisées plus tard pour faire des prédictions) sont appelées "features"
Dans notre cas, ce sont les colonnes utilisées pour déterminer le prix de la maison. 
Dans certains cas, vous utiliserez toutes les colonnes, à l'exception de la cible, comme feature. 
Dans d'autres cas, il est préférables d'avoir moins de features.

Pour l'instant, nous allons construire un modèle avec seulement quelques features. 
Vous verrez plus tard comment itérer et comparer des modèles construits avec différentes features.
Nous sélectionnons plusieurs entités en fournissant une liste de noms de colonnes entre parenthèses. 
Chaque élément de cette liste doit être une chaîne (avec des guillemets).

Voici un exemple:
```
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'Lattitude', 'Longtitude']
```
Par convention, ces données s'appellent X.
```
X = melbourne_data[melbourne_features]
```

Passons rapidement en revue les données que nous allons utiliser pour prédire les prix de l'immobilier à l'aide de la méthode describe
et de la méthode head, qui présente les quelques lignes du haut.
```
X.describe()
```
````
       Rooms	       Bathroom	   Landsize	  Lattitude	  Longtitude
count	6196.000000	6196.000000	6196.000000	6196.000000	6196.000000
mean	2.931407	1.576340	471.006940	-37.807904	144.990201
std	0.971079	0.711362	897.449881	0.075850	0.099165
min	1.000000	1.000000	0.000000	-38.164920	144.542370
25%	2.000000	1.000000	152.000000	-37.855438	144.926198
50%	3.000000	1.000000	373.000000	-37.802250	144.995800
75%	4.000000	2.000000	628.000000	-37.758200	145.052700
max	8.000000	8.000000	37000.000000	-37.457090	145.526350
````
