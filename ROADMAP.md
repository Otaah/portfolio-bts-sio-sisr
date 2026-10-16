# Progression et publication

## Point de départ

La structure et les procédures sont préparées. Aucun laboratoire n'est encore
déclaré réalisé et aucun résultat technique n'est fourni.

## Ordre de travail

Les durées ci-dessous sont des estimations d'organisation, à adapter au temps
nécessaire pour apprendre et résoudre les difficultés.

| Étape | Travail | Budget indicatif | Publication |
| --- | --- | --- | --- |
| 0 | Préparer le dépôt et lire l'environnement. | 1 h | Publier la structure avec les trois projets « À réaliser ». |
| 1 | Installer les VM, configurer et dépanner le réseau. | 6 à 8 h | Ajouter les preuves du projet 01 et changer uniquement son statut. |
| 2 | Gérer comptes, droits, service et restauration. | 6 à 8 h | Ajouter les preuves du projet 02. |
| 3 | Sécuriser SSH et vérifier le filtrage. | 6 à 8 h | Ajouter les preuves du projet 03 et un bilan avant/après. |
| 4 | Relire le dépôt et préparer une démonstration. | 1 h | Épingler le dépôt sur le profil et ajouter son lien au CV. |

Un rythme de 3 à 4 semaines est possible avec plusieurs petites séances.
Le projet 01 peut déjà accompagner une candidature une fois ses tests validés ;
il n'est pas nécessaire d'attendre les trois projets.

## Statuts

- **À réaliser** : procédure préparée, manipulations non commencées.
- **En cours** : manipulations commencées, validation incomplète.
- **Validé en laboratoire** : tests obligatoires effectués, preuves ajoutées et limites expliquées.

Mettre à jour le statut dans le README du projet et dans le tableau du README
principal. « Validé en laboratoire » ne signifie pas « prêt pour la production ».

## Critères de fin d'un projet

- [ ] L'environnement et les versions réellement utilisés sont renseignés.
- [ ] La procédure correspond aux manipulations effectuées.
- [ ] Tous les tests obligatoires possèdent un résultat réel et une preuve.
- [ ] Une difficulté réelle ou l'exercice de panne prévu est expliqué.
- [ ] Le bilan distingue ce qui a fonctionné de ce qui reste à apprendre.
- [ ] Les fichiers et captures ont été relus avant publication.

## Messages de commit proposés

Utiliser ces messages uniquement après le travail correspondant :

1. `docs: initialiser le portfolio et les objectifs`
2. `docs(reseau): ajouter la configuration du laboratoire`
3. `docs(reseau): documenter les tests et la panne simulee`
4. `docs(linux): documenter les comptes et les permissions`
5. `docs(linux): valider le service et la restauration`
6. `docs(securite): documenter SSH et les regles UFW`
7. `docs(securite): ajouter les tests avant et apres filtrage`
8. `docs: actualiser le bilan du portfolio`

Un commit correspond à un travail réel. Il n'est pas utile de multiplier les
commits pour donner artificiellement une impression d'activité.

## Publication sur GitHub

1. Créer un dépôt nommé `portfolio-bts-sio-sisr`.
2. Description : `Laboratoire personnel BTS SIO SISR : réseau, administration Linux et premières bases de cybersécurité.`
3. Utiliser les fichiers extraits de l'archive : le README doit se trouver à la racine du dépôt, sans dossier supplémentaire autour.
4. Publier d'abord un brouillon avec les statuts exacts. Avant de partager le lien dans une candidature, terminer au moins le projet 01.
5. Épingler le dépôt sur le profil lorsque le premier projet est présentable.
6. Sujets proposés : `bts-sio`, `sisr`, `homelab`, `linux`, `networking`, `cybersecurity`.

Le nom du dépôt peut être différent du pseudonyme GitHub. Un dépôt portant
exactement le pseudonyme sert à afficher un README de profil ; il est facultatif
et n'est pas nécessaire pour ce portfolio.

## Phrase pour le CV

Après validation du projet 01 seulement :

> Portfolio technique personnel : laboratoire de deux machines virtuelles,
> adressage IPv4 et diagnostic d'une panne réseau documentée.

Après validation des trois projets seulement :

> Portfolio technique personnel : réseau de deux VM, administration Linux,
> droits utilisateurs, sauvegarde et restauration, sécurisation SSH et pare-feu UFW.

Ajouter le véritable lien du dépôt à cette phrase.
