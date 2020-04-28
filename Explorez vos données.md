## Utilisez pandas pour vous familiariser avec vos données

La première étape de tout projet d’apprentissage automatique consiste à vous familiariser avec les données. 
Pandas est la principale libriairie utilisée par les scientifiques pour explorer et manipuler des données. 
Pour l'utiliser il faut l'importer grâce à la commande:

```
import pandas as pd
```

Le composant le plus important de la bibliothèque Pandas est le DataFrame. 
Un DataFrame contient le type de données que vous pourriez considérer comme une table. 
Ceci est similaire à une feuille dans Excel ou à une table dans une base de données SQL.

Pandas offre des méthodes puissantes de manipulation des données.
À titre d'exemple, examinons les données sur les prix des maisons à Melbourne, en Australie.
Dans les exercices pratiques, vous appliquerez les mêmes processus à un nouvel ensemble de données, qui présente les prix des maisons
dans l'Iowa.

Les exemples de données (Melbourne) se trouvent sur le chemin du fichier ../input/melbourne-housing-snapshot/melb_data.csv.
Nous chargeons et explorons les données avec les commandes suivantes:
```
pd.read_csv("../input/melbourne-housing-snapshot/melb_data.csv") 
```

 
