Partie 1 : Gestion des utilisateurs

2.1.1

![VirtualBox_Checkpoint3-SRVLX01_06_09_2024_11_43_12](https://github.com/user-attachments/assets/90763aef-17e1-4026-8ff0-a4106bfa3500)

2.1.2
Préconisations pour sécuriser le compte :
Mot de passe fort : Utilise un mot de passe complexe.
Accès root : Utilise sudo au lieu d'un accès direct à root.
Clés SSH : Configure l'authentification par clé SSH et désactive les connexions par mot de passe.
Accès sudo limité : Ajoute l'utilisateur au groupe sudo avec des permissions restreintes.
Expiration des mots de passe : Configure l'expiration régulière des mots de passe (chage -M 90).
Surveillance : Utilise des outils comme fail2ban pour surveiller les tentatives de connexion.
Limiter les permissions : N'accorde que les droits nécessaires à l'utilisateur.

Partie 2 : Configuration de SSH

2.2.1 et 2.2.2

![VirtualBox_Checkpoint3-SRVLX01_06_09_2024_12_24_05](https://github.com/user-attachments/assets/e5a57150-780e-4ef9-ac7a-5ba7fb32fd22)

2.2.3

![VirtualBox_Checkpoint3-SRVLX01_06_09_2024_12_20_23](https://github.com/user-attachments/assets/ab13bf55-7880-48f6-b01f-261f8cb8c037)

Partie 3 : Analyse du stockage

2.3.1

![2 3 5](https://github.com/user-attachments/assets/2f809bf9-265c-4e45-82ed-ac46d97a8e8a)

2.3.2

2.3.3 

![2 3 2](https://github.com/user-attachments/assets/6fd1749b-e757-44b0-8caf-7635dbbd395f)

2.3.4

![2 3 4](https://github.com/user-attachments/assets/2b227345-b88c-4835-8832-1116f26464e6)

2.3.5

![2 3 5](https://github.com/user-attachments/assets/a7bd2496-6b47-46b7-83eb-60f1d103b64d)


Partie 4 : Sauvegardes

bareos-dir : Gère et coordonne les opérations de sauvegarde et de restauration.
bareos-sd : Gère le stockage des données de sauvegarde sur des supports physiques.
bareos-fd : Installe sur les clients, lit les fichiers pour les sauvegarder ou les restaure.

Partie 5 : Filtrage et analyse réseau

2.5.1

![2 5 1](https://github.com/user-attachments/assets/c32cf710-89f0-4257-9712-f6288d2a67d4)


2.5.2

Les communications autorisées incluent :

Les connexions déjà établies ou liées à une connexion déjà existante.
Le trafic loopback interne (lo).
Le trafic ICMP, y compris les pings.
Les connexions SSH sur le port 22.

2.5.3

Toutes les communications qui ne correspondent pas aux règles d'autorisation mentionnées ci-dessus sont interdites en raison de la politique par défaut drop. Cela inclut tous les autres ports et types de trafic non explicitement autorisés.

2.5.4

Partie 6 : Analyse de logs

Pour lister les 10 derniers échecs de connexion sur le serveur en indiquant la date, l'heure de la tentative et l'adresse IP de la machine ayant fait la tentative, nous allons utiliser la commande suivante : sudo grep "Failed password" /var/log/auth.log | tail -n 10 | awk '{print $1, $2, $3, $11}'
La commande n'affiche aucun échec de connexion.
