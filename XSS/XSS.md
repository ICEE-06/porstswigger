## C'est quoi une XSS

Cross-site scripting est une vulnérabilié web qui permet à un attaquant de commpremettre l'intéraction d'un utilisateur avec l'app vulnérable.
Cette vuln permet aussi à un attaquant de se faire passer pour un victime pour effectuer des actions que l'utilisateur à le droit de
faire, et acceder aux données de l'utilisateurs. Si la victime a un haut-privilège dans l'app, alors l'attaquant peut avoir le 
control total sur toute les fonctionnalités de l'app et les données.

## Comment ça fonctionne?

Le XSS fonctionne par la manipulation d'un site web vulnérable afin qu'il renvoye de **JavaScript malveillant** à l'utilisateur.
Quand le code malveillant s'exécute dans le navigateur de la victime, l'attaquant peut completement comprometre son intérraction avec
l'app.

## Les types de XSS

### Reflected cross-site scripting

C'est la plus simple des variétés de XSS. Ça se produit lorsque l'app reçoit des données dans des requêtes HTTP et inclut ces données
dans la réponse immédiate de manière non sécurisé.
 
Voici un simple exemple:

```
https://insecure-website.com/status?message=All+is+well.
<p>Status: All is well.</p>
```
#### Comment trouver et tester les vulns Reflected cross-site scripting
La majorité des vulns Reflected cross-site scripting peuvent être trouver en utilisant **Burp Suite's web vulnerability scanner**
Voici les étapes pour le tester manuellement:
- **Tester tout les entry point**: 
	Tester separament chaque entry point pour les données dans les requêtes HTTP de l'app. Cela inclu: les URL, les URL file path,
	les headers.
- **Envoyer des valeurs alphanumerique aléatoir**:
	Pour chaque entry point, submit une valeur aléatoire unique et determiner que la valeur soit reflecté dans les réponses(La valeur
	doit être courte(aux alentours de 8 caractères) et ne doit contenir que des caractères alphanumerique). On peut utiliser 
	**use Burp Intruder's number payloads** pour génerer une suite de valeur aléatoire, et utiliser **Burp Intruder's grep payloads settings**
	pour capturer les réponses de chaque valeur.
- **Determiner le context**:
	Dans la réponse de chaque valeur, determiner le context. Cela peut être dans les texts entre les tags HTML, dans une chaine JS, etc
- **Tester les payloads candidate**:
	Tester des payloads XSS selon le contexte.Pour être plus éfficace, laisser la valeur aléatoire avant ou après le payload
- **Tester les payloads altérnatives**:
	Si les payloads candidates n'ont pas marché, il faut tester d'autres payloads 
- **Tester une attaque dans un navigateur**:
	On utilise le navigateur lorsque une attaque sur Burp a marché.

### Stored cross-site scripting

surviennent lorsqu'une application reçoit des données d'une source non fiable et inclut ces données dans ses réponses HTTP ultérieures de manière non sécurisée. Les données en question peuvent être transmises à l'app via des requêtes HTTP.

Supposons un site qui permet aux utilisateurs de commenter des publications (les commentaires seront vu par tout les autres utilisateurs). Les utilisateurs submit leurs commentaires dans une requêtes HTTP comme suit :

```
POST /post/comment HTTP/1.1 
Host: vulnerable-website.com 
Content-Length: 100 

postId=3&comment=This+post+was+extremely+helpful.&name=Carlos+Montoya&email=carlos%40normal-user.net
```

Lorque le commentaire est envoyé, les autres users vont recevoir ceci dans l'app:

```
<p>This post was extremely helpful.</p>
```

L'application ne fait donc aucune vérification sur les données envoyées. Un attaquant peut donc envoyer ce genre de code malveillant dans les commentaires:

```
<p><script>/* Bad stuff here... */</script></p>
```

Ce script va s'exécuter dans le navigateur de chaque utilisateur.

#### Comment trouver et tester?

- tester tous les points d'entrée pertinents par lesquels des données contrôlables par un attaquant peuvent s'introduire dans le traitement de l'application.
- tester les points de sortie où ces données pourraient apparaître dans les réponses de l'application

Les **entry points** incluent:
-  Paramètres ou autres données figurant dans la chaîne de requête URL et le corps du message.
- Les headers des requêtes HTTP
- Les file path

Les points de sortie des attaques XSS stockées sont toutes les réponses HTTP possibles renvoyées à tout type d'utilisateur d'application, dans n'importe quelle situation.

Pour tester cette vuln, il faut:
- localiser les liens entre les points d'entrée et de sortie.
- Pour identifier de manière exhaustive les liens entre les points d'entrée et de sortie, il faudrait tester chaque permutation séparément : soumettre une valeur spécifique au point d'entrée, accéder directement au point de sortie et vérifier si la valeur y figure. Cependant, cette approche n'est pas pratique pour une application comportant plus de quelques pages.
- de données, en soumettant une valeur spécifique à chacun et en surveillant les réponses de l'application afin de détecter les cas où cette valeur apparaît.
- Une fois les liens identifiés, on a plus qu'à tester les payloads appropriés.