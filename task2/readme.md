
# Challenge 6 - Ansible with Docker 


Date limite de remise du challenge: le 27 février 2021 

## Task 2: Ansible Lab environment using Docker
Durée estimée : 1 heure.

L'objectif ici est de construite un Environnement de développement Ansible à base de conteneurs Docker. On se réfère à l'artile de LMtx se trouvant ici: https://morioh.com/p/fa458766759f. L'auteur fournit le lien au GitHub contenant le code source de l'article.

Suivre les instructions du Quick start de l'article et prendre en compte les notes suivantes:

- **Note 1**: Après avoir clôné le dépôt GitHub de l'auteur, corrigez la version de l'image `ubuntu` pour la positionner sur la `18.04`. Cette modification sera faite sur le `Dockerfile` présent dans le dossier `ansible/base`. La ligne `FROM` doit donc ressembler à la suivante:
```shell
FROM ubuntu:18.04
```
- **Note 2**: En essayant cet exemple, j'ai rencontré la situation indiqué par l'auteur dans la section **Troubleshooting** se trouvant à la fin de son article. En effet, les hosts s'éteignaient dès leur création. Pour outrepasser ce problème, il faut changer l'encodage des fins de ligne du fichier  `ansible/hosts/run.sh` qui doit être **Linux/Unix (LF)** et non pas  **Windows (CRLF)**. Vous pouvez facilement changer ces "end of line"  avec votre editor Visual Studio Code ou bien Notepad++.
----
## Mise en place d'un environnement Ansible accessible via un  container Docker

Cloner le git repository:
`git clone[https://github.com/LMtx/ansible-lab-docker.git](https://github.com/LMtx/ansible-lab-docker.git)` 
Déplacez-vous dans le dossier:  `$ cd ansible-lab-docker`
Ouvrez-le dans vscode:  `$ code` 

1. Modification du fichier ansible/base/Dockerfile: modifier la ligne **FROM** du `FROM ubuntu:17.10` en `FROM ubuntu:18.04`

	Appliquer le build sur l'image en arrière-plan: 
	`docker-compose up -d --build`

2. Le container clignote. 
Ouvrir le fichier `ansible/hosts/run.sh` pour modifier son format CRLF de Windows** en format Linux/Unix *** (LF): 

-> Dans la barre d'état de vscode, à l’extrême droite, vous voyez le format de fin de ligne utilisé par le fichier courant (CRLF). Cliquez dessus pour le modifier: un menu dans la barre latérale du haut s'ouvre, choisissez le nouveau format (LF) et sauvegardez le fichier:  `ctrl + s`

---
**Connexion au master node**:  `docker exec -it master01 bash`
- Vérification des connections entre le master et les hôtes managés:
`ping -c 2 host01`
- Démarrer un agent ssh sur le master pour gérer les clés SSH protégées par passphrases:  `ssh-agent bash`
- Charger l'agent SSH sans devoir entrer la phrase à chaque x: 
`ssh-add master_key Enter passphrase for master_key:` 12345
---
**Run Ansibles playbooks**:  
- Vérification des connections entre le master et les hôtes : `ansible-playbook -i inventory ping_all.yml`
- Confirmez par `yes` chaque nouvel hôte pour les connections (3x)
- Installation de PHP sur le groupe d'inventaire web pour assurer une maintenance + facile:
 `ansible-playbook -i inventory install_php.yml`
---
**Copie des fichiers entre les fichiers locaux et le container**:  
- Copiez /var/ans du dossier local au container: `docker cp master01:/var/ans/ .`
- Copier ./ans du container vers le dossier local:  `docker cp ./ans master01:/var/`
- Vérifier l’exécution avec `docker cp --help`
---
**Exploration des fichiers**

Dans notre répertoire *Ansible*, nous voyons que le volume *ansible_vol* à créer un dossier (étape Copie des fichiers) /var/ans à l'intérieur duquel est stoquée la clé SSH 
- Les containers hôtes ajoutent la clé publique au fichier .ssh/authorized_keys:
 `cat /var/ans/master_key.pub >> /root/.ssh/authorized_keys` 

---
**Cleanup Docker environment** 

- Stopper le container: `docker-compose kill`
- Supprimer le container: `docker-compose rm`
- Supprimer le volume: `docker volume rm ansible_ansible_vol`
- Optionnel: Supprimer l'image:  `docker rmi ansible_host ansible_master ansible_base`

---
**Info-détail Troubleshooting:** 

Dans un fichier texte, plusieurs conventions incompatibles existent pour représenter la **fin de ligne** ou la fin de paragraphe. 
Les trois conventions principales trouvent leur origine dans des systèmes d’exploitation concurrents.

 - Dans la **convention « Unix »**, la fin de ligne est indiquée par le caractère saut de ligne `LF`, code 10 de la table ASCII
 - Dans la **convention « Mac »**, la fin de ligne est indiquée par le caractère retour chariot `CR`, code 13 de la table ASCII
 - Dans la **convention « DOS »**, la fin de ligne est indiquée par la combinaison des deux caractères `CR` puis `LF`
 src: [https://fr.wikipedia.org/wiki/Fin_de_ligne]



