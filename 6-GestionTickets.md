# Gestion des tickets

## Création des utilisateurs


### Création d'un utilisateur standard (Self-Service)
Aller sur `Administration > Profils`

Créer un utilisateur avec les informations suivantes:

- Identifiant: cbarbier
- Nom: Barbier
- Prénom : Christian
- Mot de passe: cbarbierGLPI
- Habilitation
  - Récursif: Non
  - Entité: Entité Racine > France > Ouest > Rennes
  - Profil: Self-Service (profil par défaut)

Cliquer sur `Ajouter`



### Création d'un superviseur

Aller sur `Administration > Profils`

Créer un utilisateur avec les informations suivantes:

- Identifiant: psuperviseur
- Nom: Superviseur
- Prénom : Pierre
- Mot de passe: psuperviseurGLPI
- Habilitation
  - Récursif: Oui
  - Entité: Entité Racine > France
  - Profil: Supervisor

Cliquer sur `Ajouter`

## Création et affectation de ticket.

Dans ce scénario, Christian Barbier va créer un nouveau ticket.

Ce ticket ne sera pas visible par Jean Quelqun (un profil Technicien).

Il va d'abord être affecté par Pierre Superviseur à Jean.

Ensuite, Jean va clôturer ce ticket.


### Création du ticket

Se connecter avec Christian qui va créer un ticket:

- Sujet: Ordinateur en panne
- Description: Mon ordi ne démarre pas


### Affectation du ticket
Se connecter en tant que Pierre

Affecter ce ticket à Jean:
1. Aller dans `Assistance > Tickets`
1. Ouvrir le ticket de Christian
1. Dans la partie `Acteur` (sur la gauche), cliquer sur le `+` situé à droite de `Attribué à`.
   1. Dans le menu déroulant qui apparaît sélectionner `Utilisateur`
   1. dans le second menu déroulant qui apparaît, sélectionner `Jean`

### Résolution du ticket
Se connecter en tant que Jean

Visualiser les tickets attribués

Ouvrir le ticket de Christian

Ajouter une
