# Environnement du laboratoire

## Périmètre prévu

Deux machines virtuelles Ubuntu Server sont utilisées pour les trois projets.
Le serveur fournit les services ; le client sert à les tester. Les manipulations
se font sur ces machines personnelles avec des comptes et documents fictifs.

Le laboratoire est une proposition à réaliser. Les adresses ci-dessous sont un
plan de configuration, pas un relevé de machines déjà installées.

## Matériel et logiciels

- PC personnel capable de faire fonctionner deux VM ; prévoir environ 4 Go de RAM disponibles pour elles, en plus des besoins de Windows.
- Réglage de départ proposé : 2 Go de RAM, 1 à 2 processeurs virtuels et disque dynamique de 20 Go par VM.
- Environ 40 à 50 Go d'espace libre à prévoir, davantage si plusieurs instantanés sont conservés.
- VirtualBox et une image d'installation officielle d'Ubuntu Server 24.04 LTS, sans interface graphique.
- Les commandes des projets sont exécutées dans les VM Linux. Les rares commandes PowerShell sont explicitement signalées.

Ces réglages sont des choix pour ce petit laboratoire, pas des exigences
universelles. Réduire les applications ouvertes si le PC manque de mémoire.

## Installation initiale

1. Installer VirtualBox et télécharger l'ISO Ubuntu Server depuis le site officiel.
2. Créer deux VM distinctes : `srv-sisr` et `client-sisr`. Faire deux installations plutôt qu'un clonage pour ce premier essai.
3. Dans chaque installation, créer le compte `adminlab` et choisir un mot de passe propre au laboratoire, conservé hors du dépôt.
4. Installer le système sur le disque virtuel de la VM. Ne pas sélectionner de disque physique de l'ordinateur.
5. Utiliser une installation Server classique. Le serveur OpenSSH sera installé au projet 02 ; il n'est pas nécessaire de le sélectionner dans l'installateur.
6. Noter les versions réellement utilisées dans le tableau ci-dessous.

## Réseau prévu

Créer un réseau VirtualBox **Host-only / réseau privé hôte**. Le nom de
l'interface dépend de l'installation : noter celui qui existe réellement.
Configurer son interface côté hôte en `192.168.56.1/24` et désactiver le DHCP
de ce réseau privé hôte. Ne pas désactiver le DHCP du mode NAT.

Pour chaque VM :

- Carte 1 : **NAT**, configuration automatique par DHCP, pour les mises à jour et les requêtes Internet.
- Carte 2 : **Host-only**, le même réseau privé hôte pour les deux VM, avec adresse fixe.
- Conserver les deux câbles virtuels connectés. Aucune redirection de port n'est nécessaire.

| Machine | Adresse du réseau privé hôte | Rôle |
| --- | --- | --- |
| PC Windows | `192.168.56.1/24` | Gestion des VM et second point de test au projet 03 |
| `srv-sisr` | `192.168.56.10/24` | Serveur Linux |
| `client-sisr` | `192.168.56.20/24` | Client de test Linux |

`/24` correspond ici au masque `255.255.255.0`. Vérifier que cette plage ne
recouvre pas un réseau déjà utilisé sur le PC ou par un VPN. En cas de conflit,
choisir une autre plage privée et adapter toutes les adresses des procédures.

La route par défaut et le DNS viennent de la carte NAT. Ne pas ajouter de
passerelle sur la carte Host-only : `192.168.56.1` est ici l'adresse du PC hôte,
pas un routeur Internet. Les VM communiquent entre elles via leurs adresses
`192.168.56.x`. Leurs adresses NAT peuvent se ressembler sans appartenir au même
réseau virtuel partagé, car le NAT simple est propre à chaque VM.

Le réseau Host-only relie le PC et les VM sans raccordement direct au réseau
physique. La carte NAT permet toutefois des communications sortantes : ce
laboratoire n'est donc pas un environnement entièrement déconnecté.

## Informations à relever

| Élément | Valeur réelle |
| --- | --- |
| Version de VirtualBox | À renseigner après installation |
| Version Ubuntu de chaque VM (`cat /etc/os-release`) | À renseigner |
| Nom de l'interface NAT (`ip -br addr`) | À renseigner |
| Nom de l'interface Host-only (`ip -br addr`) | À renseigner |
| RAM et disque attribués | À renseigner |
| Date du premier test | À renseigner |

Les exemples Netplan utilisent `enp0s3` et `enp0s8`. Ce sont des exemples de
noms, à remplacer si les interfaces de la VM portent d'autres noms.

## Points de reprise

Prendre un instantané VirtualBox avant une modification importante et après
validation de chaque projet : `p1-reseau-valide`, `p2-linux-valide`, puis
`p3-securisation-validee`. Un instantané facilite le retour en arrière ; il ne
remplace pas une sauvegarde indépendante.

## Contenu à publier

Publier les explications, configurations d'exemple, tests et captures utiles.
Conserver les clés privées, mots de passe, ISO, disques virtuels et archives de
sauvegarde hors du dépôt. Relire aussi les captures et extraits de journaux.
Les IP privées prévues dans ce laboratoire peuvent rester visibles.

Le fichier `.gitignore` évite certains ajouts accidentels ; il ne retire pas
un fichier déjà suivi par Git et ne remplace pas cette relecture.
