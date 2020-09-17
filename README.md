# PANDAS

Toutes les commandes ci-dessous peuvent se faire sous Jupyter

## Commandes en vrac

### Installation

Un import est un appel à une librairie existante. Pour pouvoir l'utiliser il faut en premier lieu l'installer avec l'utilitaire pip déjà installé avec Python. Pip, c'est un peu l'apple store des librairies python.

```
!pip install MALIBRAIRIE
```

avec pandas (ça serait pareil avec matplotlib, numpy ou autre) ça donne :

```
!pip install pandas
```

### Imports

Importer une librairie, c'est signifier à Python qu'on va l'utiliser dans ses scripts. Pour importer une librairie, on utilise la commande ```import```

Pour pandas :

```
import pandas as pd
```

Le ```as pd``` n'est pas obligatoire. C'est un alias, c'est à dire un raccourci pour appeler pandas. Ainsi, si on veut appeler la fonction ```read_excel()``` on fera ```pd.read_excel(...)```et pas ```pandas.read_excel(...)```. Rien n'empêche de changer l'alias avec un autre nom par exemple ```import pandas as ps``` => On fait ce qu'on veut.

### Manipuler pandas

Pour manipuler pandas, il faut connaître au préalable ses fonctions proposées. Le tuto ci-dessous reprend les fonctions les plus basiques.

Pour plus d'info (notamment pour connaître toutes les propriétés d'une fonction pandas), ne pas hésiter à naviguer sur https://pandas.pydata.org/docs/index.html ou sur internet

qui décrit dans le détail l'ensemble des fonctions de la librairie.

#### Lire un fichier

la fonction ```read_csv``` de pandas nous permet de lire un fichier.

```
df = pd.read_csv("CHEMIN/VERS/MON/FICHIER/monfichier.csv",dtype=str)
```

le résultat de la lecture de ce fichier est stocké dans la variable df. Cette variable est communément appelé un dataframe (un dataframe c'est un tableau à deux dimensions lignes x colonnes).

Par défaut, pandas essaie de lire le fichier avec un séparateur de type ```,``` "virgule". Mais certaines fois, le fichier a un autre séparateur, souvent ```;``` "point-virgule". Si c'est le cas (ce qui implique de toujours jeter un coup d'oeil au fichier avant de la manipuler avec pandas), on peut rajouter l'option ```sep``` au moment de la lecture du fichier : 

```
df = pd.read_csv("CHEMIN/VERS/MON/FICHIER/monfichier.csv", dtype=str, sep=";")
```

#### Afficher le dataframe

Rien de plus simple :
```
df
```

Pour n'afficher que les 5 premières lignes :

```
df.head()
```

Pour afficher les 23 premières lignes : 

```
df.head(23) 
```

Pour afficher les dernières lignes : 

```
df.tail() 
```

NB : Par défaut, il y a une limite dans l'affichage sur jupyter du nombre de lignes et de colonnes max. Pour le lever, il faut faire : 
```
pd.options.display.max_rows = 1000 # Pour 1000 rows max
pd.options.display.max_columns = 1000 # Pour 1000 colonnes max
```

#### Afficher les colonnes d'un dataframe

```
df.columns
```

#### Avoir le nombre de lignes et de colonnes d'un dataframe

```
df.shape #Affiche un tuple rows x cols
df.shape[0] #Uniquement les lignes
df.shape[1] #Uniquement les colonnes
```


#### Afficher les types des colonnes détectés par pandas 

```
df.dtypes
```

#### Afficher une colonne

```
df['NOM_COLONNE']
```

#### Afficher plusieurs colonnes

```
df[['NOM_COLONNE1','NOM_COLONNE2','NOM_COLONNE3']]
```

#### Compter le nombre de valeurs unique d'une colonne

```
df['col1'].unique() # Liste toutes les valeurs unique d'une colonne
df['col1'].nunique() # Affiche le nombre de valeur unique
```

#### Enlever un row

```
df = df.drop([df.index[12]) #On enlève ici le row en 13e position
```

#### Enlever des colonnes

```
df.drop(columns=['col1','col2'])
```

#### Copier un dataframe 

```
df2 = df.copy()
```

#### Remplacer la valeur d'une colonne par une autre

```
df['col1'] = df['col1'].apply(lambda x: str(x).replace(";","-"))  #Remplace les ; trouvé dans la colonne col1 par des tirets -
```

#### Créer une nouvelle colonne

```
df['NOUVELLE_COLONNE'] = None  #On initialise toutes les valeurs de la colonne à null
df['NOUVELLE_COLONNE'] = 2 #On initialise toutes les valeurs de la colonne à 2
df['NOUVELLE_COLONNE'] = df['NOM_COLONNE1'] # On initialise une nouvelle colonne avec la valeur d'une colonne existante
df['NOUVELLE_COLONNE'] = df['NOM_COLONNE1'] * 2 # Multiplie par 2 la valeur de la colonne 1
```

#### Cast une colonne

Les types des colonnes sont automatiquement détectés par pandas. Or il arrive que pandas se trompe. On doit donc caster la colonne avec le type correct. Par exemple :

| prenom | age |
| -- | -- |
| Gérard | 57 |
| Lucien | 23 |
| Caroline | Pas de valeur |
| Gertrude | 56 |

Pandas va interpréter la colonne age de ce dataframe comme un string car il y a la valeur "Pas de valeur". On doit donc pour pouvoir manipuler ces chiffres "caster" cette colonne :

```
df['age'] = pd.to_numeric(df['total_06'], errors='coerce')
```

Le ```errors='coerce'``` indique à Pandas de mettre la valeur à nulle si il n'arrive pas à la caster

C'est pour cela qu'une bonne pratique consiste à charger le dataframe avec la propriété ```dtype=str``` vu précédemment avec la fonction ```read_csv()```. En faisant cela, on charge toutes les données comme des strings et on pourra changer leur type plus tard dans les traitements.

#### Trier un dataframe

```
df.sort_values(by=['col1']) #Permet de trier en ordre croissant une colonne
df.sort_values(by='col1', ascending=False) #Permet de trier en ordre décroissant une colonne
```

Attention, cette opération est effectuée ci-dessus uniquement à la volée et n'est pas enregistrée. Si on veut trier un dataframe et le garder comme ça pour manipulation ultérieures, il faut l'affecter à une variable (ça peut être le dataframe lui-même) : 

```
df = df.sort_values(by=['col1']) #Permet de trier en ordre croissant une colonne
df = df.sort_values(by='col1', ascending=False) #Permet de trier en ordre décroissant une colonne
```

NB : on aurait pu l'affecter à une autre variable dataframe (par exemple df2)

#### Sous-sélection d'un dataframe

On peut filtrer un dataframe de la manière suivante :

```
df_sup_20 = df[df['age'] > 20] #Affiche les valeurs dont l'age est sup à 20
```

On peut avoir plusieurs conditions : 

```
df_sup_20_inf_50 = df[(df['age] > 20) & (df['age] < 50)]
```

#### Ecarter ou garder les valeurs nulles d'une colonne

```
df = df[df['col'].notna()] #Enlève toutes les lignes ou la col1 est nulle
df = df[df['col'].isna()] #Garde toutes les lignes ou la col1 est nulle et enlève les autres
```

#### Sauvegarder un dataframe

```
df_sup_20.to_csv("age_sup_20.csv",index=False)
```

#### Renommer une colonne 

```
df = df.rename(columns={'ANCIEN_NOM_COL1':'NOUVEAU_NOM_COL1','ANCIEN_NOM_COL2':'NOUVEAU_NOM_COL2'})
```

#### Merge de deux dataframes

Mettons que df1 contienne la liste des hopitaux par département avec une colonne "code_departement" mais pas le nom du département. Mettons que df2 contient la liste des départements français avec le "code_departement" et le nom du département.

Si on veut enrichir df1 avec le nom du département, on fait un merge (ou un left join) :

```
df_enrichi = pd.merge(df1,df2,on="code_departement",how="left")
```

#### Group by

Le group by permet de regrouper les données d'un dataframe à partir d'une ou plusieurs colonnes :


| prenom | age | dep |
| -- | -- | --|
| Gérard | 57 | 75 |
| Lucien | 23 | 78 |
| Caroline | Pas de valeur | 92 |
| Gertrude | 56 | 75 |

```
df_final = df.groupby(['dep'], as_index=False).count() # On obtient le nb de personne par département
df_final = df.groupby(['dep'], as_index=False).sum() # On obtient la somme des ages par département (pas trop de sens ici)df_final = df.groupby(['dep'], as_index=False).mean() # On obtient la moyenne des ages par département
```


#### Enregistrer un dataframe dans un csv

```
df.to_csv("NOUVEAUNOM.csv",index=False)
```
