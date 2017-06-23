# Gestion des tickets

## Création des utilisateurs


### Création d'un utilisateur standard (Self-Service)
Aller sur `Administration > Utilisateur`

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

Aller sur `Administration > Utilisateur`

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

### Création d'un technicien

Aller sur `Administration > Utilisateur`

Créer un utilisateur avec les informations suivantes:

- Identifiant: jtechnicien
- Nom: Technicien
- Prénom : Jean
- Mot de passe: jtechnicienGLPI
- Habilitation
  - Récursif: Oui
  - Entité: Entité Racine > France
  - Profil: Technicien

Cliquer sur `Ajouter`


## Création et affectation de ticket.

Dans ce scénario, Christian Barbier va créer un nouveau ticket.

Ce ticket ne sera pas visible par Jean (un profil Technicien).

Il va d'abord être affecté par Pierre Superviseur à Jean.

Ensuite, Jean va modifier ce ticket et demander sa validation.

Pierre va alors marquer ce ticket comme résolu.

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

Aller dans `Traitements du ticket`

Ajouter un suivi et proposer une solution (par exemple "J'ai rebranché le câble réseau").

Cliquer sur `Ajouter`

Puis pour valider le ticket:

1. Cliquer sur `Validations`
1. Cliquer sur `Envoyer une demande de validation`
1. Sélectionner les valeurs suivantes:
  - Valideur
     -  Utilisateur
     - Pierre Superviseur
1. cliquer sur `Ajouter`


### Validation du ticket
Se connecter en tant que Pierre

Dans le menu `Accueil`, on voit apparaître le ticket à valider.

Cliquer sur la demande de Jean (en bas)

Sélectionner `100%` comme validation minimale nécessaire

Cliquer sur `Sauvegarder`: le ticket apparaît comme accepté.

Cliquer sur le menu `Ticket` à gauche
 - Pour la valeur de `Statut`, selectionner `Résolu`
 - Cliquer sur `Sauvegarder`

### Vérification
Se reconnecter avec Christian pour visualiser le ticket (en allant dans le menu Tickets).
