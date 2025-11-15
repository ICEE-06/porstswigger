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





