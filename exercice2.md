## Partie 1 : Gestion des utilisateurs

### 2.1.1 Création d'un utilisateur :

![VirtualBox_Checkpoint3-SRVLX01_06_09_2024_11_43_12](https://github.com/user-attachments/assets/90763aef-17e1-4026-8ff0-a4106bfa3500)

### 2.1.2
Préconisations pour le compte :

Mot de passe fort : Utilise un mot de passe complexe.

Accès root : Utilise sudo au lieu d'un accès direct à root.

Clés SSH : Configure l'authentification par clé SSH et désactive les connexions par mot de passe.

Accès sudo limité : Ajoute l'utilisateur au groupe sudo avec des permissions restreintes.

Expiration des mots de passe : Configure l'expiration régulière des mots de passe.

Surveillance : Utilise des outils comme fail2ban pour surveiller les tentatives de connexion.

Limiter les permissions : N'accorde que les droits nécessaires à l'utilisateur.


## Partie 2 : Configuration de SSH

### 2.2.1 et 2.2.2 Désactiver complètement l'accès à distance de l'utilisateur root et autoriser l'accès à distance à ton compte personnel uniquement.

![VirtualBox_Checkpoint3-SRVLX01_06_09_2024_12_24_05](https://github.com/user-attachments/assets/e5a57150-780e-4ef9-ac7a-5ba7fb32fd22)

### 2.2.3 Mise en place d'une authentification par clé valide et désactiver l'authentification par mot de passe (screen ci-dessus)

![VirtualBox_Checkpoint3-SRVLX01_06_09_2024_12_20_23](https://github.com/user-attachments/assets/ab13bf55-7880-48f6-b01f-261f8cb8c037)

## Partie 3 : Analyse du stockage

### 2.3.1

![Capture d’écran 2024-09-06 à 15 41 48](https://github.com/user-attachments/assets/f2566d7b-cc29-4358-b92f-cac1127312fb)

#### /dev/mapper/cp3--vg-root :

Système de fichiers : ext4
Monté sur : /
Taille : 2,7 Go
Utilisé : 1,5 Go
Disponible : 1,1 Go
Utilisation : 59%


#### /dev/md0p1 :

Système de fichiers : ext2
Monté sur : /boot
Taille : 471 Mo
Utilisé : 49 Mo
Disponible : 398 Mo
Utilisation : 11%


#### Autres systèmes de fichiers temporaires (tmpfs) :

Utilisés pour les besoins temporaires en mémoire, comme /run, /dev/shm, /run/lock, et /run/user/0.

### 2.3.2

![Capture d’écran 2024-09-06 à 15 41 57](https://github.com/user-attachments/assets/a65c32e2-da86-4248-b071-8bf381a0a42d)


 le type de système de stockage utilisé sur votre serveur est :

#### RAID1 :
Le périphérique /dev/md0 est configuré en RAID1, un niveau de RAID qui offre une redondance des données en les dupliquant sur plusieurs disques. Ce type de stockage est utilisé pour la partition /boot (/dev/md0p1), qui utilise le système de fichiers ext2.

#### LVM (Logical Volume Manager) :
Le système utilise LVM pour la gestion flexible du stockage sur le volume logique /dev/cp3--vg-root monté sur / et /dev/cp3--vg-swap_1 pour la partition swap. Le volume physique /dev/md0p5 est configuré en tant que membre LVM (LVM2_member), permettant une gestion dynamique et aisée des volumes logiques.
Le volume logique /dev/cp3--vg-root utilise le système de fichiers ext4, ce qui est courant pour sa fiabilité et ses bonnes performances.

### 2.3.3 et 2.3.4 Ajout d'un nouveau disque de 8,00 Gio au serveur et réparer le volume RAID et ajout d'un nouveau volume logique LVM de 2 Gio qui servira à héberger des sauvegardes.

![2 3 4](https://github.com/user-attachments/assets/2b227345-b88c-4835-8832-1116f26464e6)

2.3.5 Il reste 1.79 Go d'espace libre dans le groupe de volumes cp3-vg. Cet espace peut être utilisé pour créer de nouveaux volumes logiques ou pour étendre les volumes logiques existants.

![2 3 5](https://github.com/user-attachments/assets/a7bd2496-6b47-46b7-83eb-60f1d103b64d)


## Partie 4 : Sauvegardes

bareos-dir : Gère et coordonne les opérations de sauvegarde et de restauration.

bareos-sd : Gère le stockage des données de sauvegarde sur des supports physiques.

bareos-fd : Installe sur les clients, lit les fichiers pour les sauvegarder ou les restaure.


## Partie 5 : Filtrage et analyse réseau

### 2.5.1  les règles appliquées sur Netfilter sont les suivantes : 

![2 5 1](https://github.com/user-attachments/assets/c32cf710-89f0-4257-9712-f6288d2a67d4)


### 2.5.2  Les types de communications sont autorisées sont les suivantes :

Les connexions déjà établies ou liées à une connexion déjà existante.

Le trafic loopback interne (lo).

Le trafic ICMP, y compris les pings.

Les connexions SSH sur le port 22.

### 2.5.3 Les types de communications interdit sont les suivantes : 

Toutes les communications qui ne correspondent pas aux règles d'autorisation mentionnées ci-dessus sont interdites en raison de la politique par défaut drop. Cela inclut tous les autres ports et types de trafic non explicitement autorisés.

### 2.5.4 Ajout des règles nécessaires pour autoriser bareos à communiquer avec les clients

![2 5 4](https://github.com/user-attachments/assets/b8df2f8b-4e45-4959-8546-a522b66b6199)


## Partie 6 : Analyse de logs

Pour lister les 10 derniers échecs de connexion sur le serveur en indiquant la date, l'heure de la tentative et l'adresse IP de la machine ayant fait la tentative, nous allons utiliser la commande suivante : sudo grep "Failed password" /var/log/auth.log | tail -n 10 
La commande n'affiche aucun échec de connexion.
