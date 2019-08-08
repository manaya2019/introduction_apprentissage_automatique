Vous avez construit un modèle. Mais est-il bon ou pas?

Dans cette leçon, vous apprendrez à utiliser la validation de modèle pour mesurer la qualité de votre modèle. 
Mesurer la qualité d'un modèle est la clé pour améliorer vos modèles de manière itérative.

## C'est quoi un modèle de validation?
Vous aurez envie d'évaluer presque tous les modèles que vous avez jamais construits. 
Dans la plupart des applications (mais pas toutes), la mesure pertinente de la qualité du modèle est la précision prédictive. 
En d'autres termes, les prédictions du modèle seront-elles proches de ce qui se passe réellement?

Beaucoup de gens font une énorme erreur en mesurant la précision prédictive. Ils font des prévisions avec leurs données d'apprentissage et les comparent aux valeurs cibles dans les données d'apprentissage. 
Vous verrez le problème avec cette approche et la façon de le résoudre dans un instant, mais réfléchissons 
à la façon dont nous le ferions en premier.
Vous devez d’abord résumer la qualité du modèle de manière compréhensible. Si vous comparez les valeurs prévues et réelles des maisons pour 10 000 maisons, vous obtiendrez probablement un mélange de bonnes et de mauvaises prévisions. Il serait inutile de parcourir une liste de 10 000 valeurs prédites et réelles.
Nous devons résumer cela en une seule métrique.
Il existe de nombreuses mesures permettant de résumer la qualité du modèle, mais nous commencerons par une erreur appelée Mean Absolute Error
(MAE). Décomposons cette métrique en commençant par le dernier mot, erreur.

L'erreur de prédiction pour chaque maison est:
```erreur = valeur réelle - prédiction```
Donc, si une maison coûte 150 000 € et que vous prévoyez qu'il en coûtera 100 000 €, l'erreur est de 50 000 €.

Avec la métrique MAE, nous prenons la valeur absolue de chaque erreur. Cela convertit chaque erreur en un nombre positif.
Nous prenons ensuite la moyenne de ces erreurs absolues. C'est notre mesure de la qualité du modèle. En clair, on peut dire qu'en moyenne, nos prévisions sont faussées d'environ X.

Pour calculer le MAE, nous avons d’abord besoin d’un modèle. Cela est construit dans une cellule cachée ci-dessous, que vous pouvez consulter en cliquant sur le bouton de code.
```
import pandas as pd

# Télécharger les données
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path) 

# Filtrer les lignes contenant des valeurs manquantes
filtered_melbourne_data = melbourne_data.dropna(axis=0)

# Choisir les labels et features
y = filtered_melbourne_data.Price
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'BuildingArea', 
                        'YearBuilt', 'Lattitude', 'Longtitude']
                        
X = filtered_melbourne_data[melbourne_features]

from sklearn.tree import DecisionTreeRegressor

# Definir le modele
melbourne_model = DecisionTreeRegressor()

# Fit model
melbourne_model.fit(X, y)
```
output
```
          DecisionTreeRegressor(criterion='mse', max_depth=None, max_features=None,
           max_leaf_nodes=None, min_impurity_decrease=0.0,
           min_impurity_split=None, min_samples_leaf=1,
           min_samples_split=2, min_weight_fraction_leaf=0.0,
           presort=False, random_state=None, splitter='best')
 ```
           
Une fois que nous avons un modèle, voici comment nous calculons l'erreur absolue moyenne:

```from sklearn.metrics import mean_absolute_error

predicted_home_prices = melbourne_model.predict(X)
mean_absolute_error(y, predicted_home_prices)```
``264736.065203357``

Notez qu'il existe de nombreuses façons d'améliorer ce modèle, 
par exemple en essayant de trouver de meilleures features ou différents types de modèles.
