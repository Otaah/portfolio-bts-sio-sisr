# Procédure prévue — Sécurisation Linux

Prérequis : projets 01 et 02 validés. Le serveur possède une page Nginx et un
accès SSH fonctionnels. Garder la console VirtualBox du serveur ouverte et
prendre un instantané avant les modifications. Les commandes concernent
uniquement ces VM personnelles.

## 1. Relever l'état initial

Sur le serveur :

```bash
sudo apt update
sudo apt upgrade
sudo apt install ufw
sudo ss -lntp
sudo ufw status verbose
```

Cette procédure suppose que la VM neuve ne comporte pas de règles UFW
personnelles préexistantes. Si des règles existent, les examiner et adapter le
plan avant d'activer le filtrage ; ne pas les effacer sans les comprendre.

Depuis le client, confirmer SSH et HTTP comme au projet 02. Depuis
**PowerShell sur le PC Windows**, relever les deux résultats avant modification :

```powershell
Test-NetConnection 192.168.56.10 -Port 22
Test-NetConnection 192.168.56.10 -Port 80
```

Les deux connexions TCP doivent fonctionner pour que la comparaison avant/après
soit interprétable. En cas d'échec initial, résoudre d'abord le problème de
connexion du PC au réseau Host-only.

## 2. Créer et tester une clé SSH

Depuis le client Linux, vérifier que le nom de clé n'est pas déjà utilisé :

```bash
ls ~/.ssh/sisr_lab_ed25519*
```

L'absence de fichier est normale avant création. Si une clé existe, la conserver
et choisir un autre nom. Pour une nouvelle clé :

```bash
ssh-keygen -t ed25519 -f ~/.ssh/sisr_lab_ed25519 -C "sisr-lab"
ssh-copy-id -i ~/.ssh/sisr_lab_ed25519.pub adminlab@192.168.56.10
ssh -i ~/.ssh/sisr_lab_ed25519 -o IdentitiesOnly=yes -o PasswordAuthentication=no -o KbdInteractiveAuthentication=no adminlab@192.168.56.10
```

Choisir une phrase secrète pour protéger la clé privée. Sa demande éventuelle
est locale et différente du mot de passe du compte serveur. Après connexion,
vérifier `whoami` puis `exit`. Ne poursuivre que si cet accès fonctionne.

La clé privée reste sur le client, hors GitHub. Le serveur reçoit la clé
publique. Il n'est pas nécessaire de publier l'une ou l'autre pour prouver le
fonctionnement.

## 3. Appliquer les restrictions SSH

Sur le serveur, examiner les fichiers existants :

```bash
ls /etc/ssh/sshd_config.d
sudo sshd -T
```

Si `/etc/ssh/sshd_config.d/00-lab.conf` n'existe pas, le créer avec `sudo nano`
et y copier le contenu de `configs/00-lab.conf.example`. Conserver les fichiers
préexistants. Les directives SSH utilisent généralement la première valeur
rencontrée : le nom du fichier ne suffit pas à garantir le résultat.

Contrôler la syntaxe, puis les valeurs effectives :

```bash
sudo sshd -t
sudo sshd -T -C user=adminlab,host=client-sisr,addr=192.168.56.20
sudo sshd -T -C user=root,host=client-sisr,addr=192.168.56.20
```

Rechercher dans les sorties : `pubkeyauthentication yes`,
`passwordauthentication no`, `kbdinteractiveauthentication no`,
`permitrootlogin no` et `allowusers adminlab`. Corriger toute divergence en
examinant les configurations incluses et les éventuels blocs `Match`.

Si `sshd -t` réussit et si les valeurs conviennent :

```bash
sudo systemctl reload ssh
```

Ouvrir une **nouvelle** session par clé depuis le client et vérifier son
fonctionnement avant de fermer une session existante. Puis tenter :

```bash
ssh -o PubkeyAuthentication=no -o PreferredAuthentications=password -o NumberOfPasswordPrompts=1 adminlab@192.168.56.10
```

Cette tentative doit être refusée. Conserver le résultat exact. Ne pas
interpréter une ancienne session encore ouverte comme une validation de la
nouvelle configuration.

## 4. Filtrer les accès avec UFW

Depuis la console du serveur, définir les autorisations avant l'activation :

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow proto tcp from 192.168.56.20 to 192.168.56.10 port 22
sudo ufw allow proto tcp from 192.168.56.0/24 to 192.168.56.10 port 80
sudo ufw logging on
sudo ufw enable
sudo ufw status verbose
sudo ufw status numbered
```

Le choix est de réserver SSH au client `.20` et de rendre la page web accessible
aux machines du réseau du laboratoire. Vérifier qu'aucune règle plus large
préexistante ne contourne cette intention.

## 5. Vérifier depuis deux sources

Depuis le client `.20`, ouvrir une nouvelle connexion SSH par clé avec la
commande de l'étape 2, puis tester :

```bash
curl --max-time 5 -I http://192.168.56.10
```

Depuis **PowerShell sur le PC Windows `.1`** :

```powershell
Test-NetConnection 192.168.56.10 -Port 22
Test-NetConnection 192.168.56.10 -Port 80
```

Résultat prévu : `TcpTestSucceeded` faux sur 22 et vrai sur 80 depuis Windows ;
SSH et HTTP fonctionnent depuis le client autorisé. La tentative bloquée peut
prendre du temps à expirer. Vérifier sur le serveur que SSH écoute toujours
avec `sudo ss -lntp` : un service arrêté ne prouverait pas l'effet du pare-feu.

Noter l'adresse source affichée par PowerShell. Elle doit correspondre au réseau
privé hôte ; sinon, examiner le routage avant d'interpréter le résultat.

## 6. Lire les traces

Sur le serveur, immédiatement après les essais :

```bash
sudo journalctl -u ssh.service --since "10 minutes ago" --no-pager
sudo journalctl -k --since "10 minutes ago" --no-pager
```

Chercher une trace de connexion SSH et, si disponible, une ligne `UFW BLOCK`
correspondant à la source et au port du test. Les messages exacts dépendent de
la version et de la journalisation. En l'absence de trace, noter cette absence
et vérifier la journalisation ; ne pas inventer une ligne de log.

Publier seulement de courts extraits expliqués : heure du test, action, source,
résultat et interprétation. Le blocage réseau UFW et le refus d'authentification
SSH sont deux mécanismes distincts.

## 7. Retour en arrière

En cas de perte d'accès, utiliser la console VirtualBox du serveur :

```bash
sudo ufw disable
```

Si le problème vient du nouveau fichier SSH, déplacer uniquement le fichier
`00-lab.conf` créé pour cet exercice vers un emplacement de sauvegarde hors de
`/etc/ssh/sshd_config.d`, puis relancer `sudo sshd -t`. Recharger le service
uniquement si la syntaxe est correcte. L'instantané précédent reste un point
de récupération ; son rétablissement annule les changements réalisés depuis.

## 8. Bilan

Remplir la comparaison avant/après dans `validation.md`. Expliquer quelles
mesures sont validées et les limites : service HTTP sans TLS, absence de
supervision centralisée, pas de test exhaustif des risques ou d'IPv6. Ce projet
établit des bases de sécurisation sur ce laboratoire précis.
