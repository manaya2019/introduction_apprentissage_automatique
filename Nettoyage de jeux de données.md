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
        Rooms	       Bathroom	   Landsize	       Lattitude	  Longtitude
count	6196.000000	6196.000000	6196.000000	6196.000000	6196.000000
mean	2.931407	1.576340	471.006940	-37.807904	144.990201
std	0.971079	0.711362	897.449881	0.075850	0.099165
min	1.000000	1.000000	0.000000	-38.164920	144.542370
25%	2.000000	1.000000	152.000000	-37.855438	144.926198
50%	3.000000	1.000000	373.000000	-37.802250	144.995800
75%	4.000000	2.000000	628.000000	-37.758200	145.052700
max	8.000000	8.000000	37000.000000	-37.457090	145.526350
````
``X.head()
`` permet d'afficher les premières lignes du tableau.

Vérifier visuellement vos données avec ces commandes ci-dessus, est une partie importante du travail d'un data scientist. 
Vous trouverez souvent dans un dataset des surprises qui méritent d’être examinées plus en profondeur.

## Construire votre modèle
Vous utiliserez la bibliothèque scikit-learn pour créer vos modèles. Lors du codage, cette bibliothèque est écrite en tant que sklearn, comme vous le verrez dans l'exemple de code. Scikit-learn est la bibliothèque la plus répandue pour modéliser les types de données généralement stockées dans les DataFrames.

Les étapes pour construire et utiliser un modèle sont les suivantes:

**Définir** : de quel type de modèle s'agira-t-il? Un arbre de décision? Un autre type de modèle? Certains autres paramètres du type de modèle sont également spécifiés.
**Fit** : Capture des modèles à partir des données fournies. C'est le cœur de la modélisation.
**Prédire** : à quoi ça ressemble
**Evaluer** : Déterminez la précision des prévisions du modèle.

Voici un exemple de définition d’un modèle d’arbre de décision avec scikit-learn et d’adapter celui-ci aux caractéristiques et à la variable cible.
```
from sklearn.tree import DecisionTreeRegressor

# Définir le model. Choisir un  nombre  pour random_state pour avoir les memes resultats à chaque execution
melbourne_model = DecisionTreeRegressor(random_state=1)

# Fit model
melbourne_model.fit(X, y)
```
De nombreux modèles d’apprentissage automatique permettent une certaine aléatoire dans la formation des modèles. La spécification d'un nombre pour ``random_state`` permet de garantir les mêmes résultats à chaque exécution. Ceci est considéré comme une bonne pratique. Vous utilisez n’importe quel nombre et la qualité du modèle ne dépend pas de la valeur que vous choisissez.

Nous avons maintenant un modèle ajusté que nous pouvons utiliser pour faire des prévisions.

En pratique, vous serez amenés à faire des prédictions pour les nouvelles maisons à venir sur le marché plutôt que pour les maisons pour lesquelles nous avons déjà des prix. Mais nous ferons des prévisions pour les premières lignes des données d'apprentissage afin de voir le fonctionnement de la fonction de prévision.

```
print("Making predictions for the following 5 houses:")
print(X.head())
print("The predictions are")
print(melbourne_model.predict(X.head()))
```
```
Making predictions for the following 5 houses:
   Rooms  Bathroom  Landsize  Lattitude  Longtitude
1      2       1.0     156.0   -37.8079    144.9934
2      3       2.0     134.0   -37.8093    144.9944
4      4       1.0     120.0   -37.8072    144.9941
6      3       2.0     245.0   -37.8024    144.9993
7      2       1.0     256.0   -37.8060    144.9954
The predictions are
[1035000. 1465000. 1600000. 1876000. 1636000.]
```
