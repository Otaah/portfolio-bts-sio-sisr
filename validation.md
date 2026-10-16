# Validation — Projet 03

**Date : Non exécuté**  
**Versions : À renseigner**

Les éléments de la colonne « Attendu » sont des critères de réussite.

| Test obligatoire | Source et action | Attendu | Avant réel | Après réel | Preuve |
| --- | --- | --- | --- | --- | --- |
| S1 — Configuration SSH | Serveur : `sshd -t` et `sshd -T -C ...` | Syntaxe valide et restrictions effectives pour adminlab et root | Non exécuté | Non exécuté | À ajouter |
| S2 — Clé SSH | Client `.20` : nouvelle connexion par clé, mot de passe désactivé côté client | Session autorisée | Non exécuté | Non exécuté | À ajouter |
| S3 — Mot de passe seul | Client `.20` : tentative sans clé | Refus après durcissement | Non exécuté | Non exécuté | À ajouter |
| S4 — Autre source SSH | PC `.1` : test TCP 22 | Accessible avant, bloqué après UFW, tandis que SSH écoute toujours | Non exécuté | Non exécuté | À ajouter |
| S5 — HTTP | Client `.20` : curl ; PC `.1` : test TCP 80 | HTTP maintenu, code 200 depuis le client | Non exécuté | Non exécuté | À ajouter |
| S6 — Règles | Serveur : `ufw status verbose` et `numbered` | SSH autorisé uniquement depuis `.20`, HTTP depuis le /24, refus entrant par défaut | Non exécuté | Non exécuté | À ajouter |
| S7 — Journaux | Serveur : traces après les essais | Au moins une trace SSH interprétée ; présence ou absence de traces UFW expliquée | Non exécuté | Non exécuté | À ajouter |

## Tableau de synthèse à compléter

| Mesure | Pourquoi ce choix ? | Ce que mon test démontre | Limite |
| --- | --- | --- | --- |
| Clé SSH | Authentifier l'administrateur avec une clé protégée | À compléter après essai | La clé privée doit rester protégée |
| Refus des mots de passe SSH | Retirer cette méthode d'authentification distante | À compléter | Ne supprime pas le mot de passe local ou celui utilisé par sudo |
| Refus de root en SSH | Utiliser un compte nominatif de laboratoire avec sudo | À compléter | Les droits sudo restent à contrôler |
| Filtrage UFW | Réduire les sources pouvant atteindre SSH | À compléter | Une adresse IP n'est pas une identité forte |

## Bilan personnel

À remplacer après les tests :

> J'ai appliqué [mesures réellement réalisées]. J'ai vérifié [accès autorisés]
> et [accès refusés] depuis [machines]. Les preuves montrent [constat précis].
> Cette configuration reste limitée à [périmètre]. Ma prochaine étape sera
> [amélioration réaliste].

## Questions d'entretien à préparer

1. Quelle différence entre clé privée, clé publique et clé hôte du serveur ?
2. Pourquoi tester une nouvelle session avant de fermer l'ancienne ?
3. Comment distinguer un filtrage réseau d'un refus d'authentification SSH ?
4. Pourquoi un port bloqué ne suffit-il pas à conclure que le serveur est sécurisé ?
