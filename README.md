Enoncé : 
Créer 3 conteneurs sur 2 réseaux différents :

•	app et db dans backend\_net

•	proxy dans frontend\_net et backend\_net (il fait le lien entre les deux)

Exercice bonus :
Empêcher db d’être accessible directement depuis l’hôte, mais rendre l’application joignable via le proxy.



-------------------------------------------------------------------------------------------------------------





TP Docker - Stack Flask + MariaDB + Reverse Proxy Nginx



But du projet :



Mise en place une infra web avec :



une base de données MariaDB



une application web Flask (Python, pymsql)



un reverse proxy nginx qui isole l'accès à l'app, organisé proprement en deux réseaux Docker pour l'isolation de l'app et la db.



1\. Organisation du projet



À la racine :



docker-compose.yml



dossier proxy qui contient le fichier nginx.conf



dossier app qui contient app.py et le Dockerfile pour Flask





2\. Points clés de la config :

MariaDB et Flask sont sur le réseau privé backend\_net.



Nginx est sur backend\_net et frontend\_net (lien avec l'extérieur).



Aucun accès a Flask ou à MariaDB a part via nginx



3\. Fichiers de conf à vérifier :

docker-compose.yml + DockerFile (voir Screenshot)



Les bons noms de services entre tous les fichiers de conf (app.py, dockerfile, compose.yml, bon chemin etc...)





4\. Vérification du reverse proxy :



Sur http://localhost:7654 (port choisi dans le compose.yml), on accède au message "Hello from app" 





Verification que rien ne répond sur http://localhost:5000 (cela signifierais qu'il y'ai un accès a la backend)





Arret de flask et verification qu'on a bien une erreur qui est renvoyé par nginx (Erreur 502 Bad Gateway, voir screenshot)



Vérification que nginx accède a la DB depuis http://localhost:7654/health.





5\. Pourquoi un reverse proxy ?



Permet d’isoler l’app, pour une sécurité optimale puis ulterieurement d'ajouter plus de services ou gérer des domaines









