# Configuration générale

## Menu Configuration>Générale

## Langages par défaut

## Format des dates

# Création des entités

Création de l'arborescence suivante:

![Entités](images/entites.png)

## Entité racine

### La renommer pour correspondre au schéma

## Entités de premier niveau

### France

#### Commentaire:

##### Société mère

### Espagne

#### Commentaire:

##### Filiale Espagnole du groupe

### Allemagne

#### Commentaire:

##### Filiale Allemande du groupe

## Une fois les entités créées

### SE DECONNECTER puis se reconnecter

## Ajout de sous-entités à l'entité Ouest

### Menu Administration>Entités

#### Sélectionner l'entité Ouest

#### Cliquer sur le sous-menu Entités

#### Dans la section Nouvel intitulé enfant

##### Entrer un nom

###### Laval

##### Cliquer sur Ajouter

## Cliquer sur l'entité Laval depuis Ouest + sous-entités

## Ajout des adresses dans les Entités situées dans les villes

### Rennes

#### 18 boulevard de la Tour d'Auvergne

### Nantes

#### 44 avenue de la Duchesse Anne

### Laval

#### 27 rue du Mont

### Le Mans

#### 24 rue des Heures

## Définition des options de notification pour une entité

### Voir la personnalisation faite sur l'entité Ouest

# Création d'une sauvegarde de la BDD

## En mode graphique

## En mode serveur

# Profils

## Définition simpliste

### un profil est une liste de droits, définissant pour chaque fonction de l’application l’autorisation d’utiliser ou non cette fonction.

## Les profils par défaut

### Technicien

#### ce profil correspond à la fonction de technicien de service informatique. Il permet d’accéder aux fonctions de gestion de parc en lecture et d’intervenir dans le traitement des tickets du helpdesk.

### Supervisor

#### ce profil possède des droits similaires à ceux du technicien et possède les droits pour gérer l’organisation d’une équipe (attribution des tickets).

### Super-Admin

#### ce profil possède tous les droits et donc celui de configurer et de paramétrer l’application. Le nombre d’utilisateurs ayant ce profil doit rester restreint.

#### Comptes

##### glpi

##### tech

#### Ne jamais lui associé l'interface simplifiée

##### Sinon la configuration de GLPI peut être impossible de manière définitive

### Self-Service

#### ce profil est dédié au dépôt de tickets d’assistance. Il est défini à l’installation de GLPI comme étant le profil par défaut. Ainsi, GLPI attribue ce profil à tout nouvel utilisateur qui se connecte. Vous découvrirez dans ce chapitre comment les droits peuvent être attribués à un utilisateur (application d’un profil sur une ou plusieurs entités).

### Observateur

#### ce profil n’accède qu’en lecture aux informations liées à l’inventaire et à sa gestion. S’agissant des tickets, ce profil peut les déclarer ou s’en voir attribuer mais il ne peut pas en attribuer.

### Hotliner

#### ce profil permet de faire la saisie des tickets et d’avoir accès à leur suivi. En revanche, ce profil ne permet pas de les prendre en charge.

### Admin

#### ce profil administre les droits sur l’intégralité du logiciel GLPI. Cependant, il n’a pas accès à toutes les fonctionnalités sur la configuration des règles, des entités et autres points sensibles pouvant dégrader les actions de GLPI.

## Interface simplifiée

### Pour les demandeurs d'assistance

#### Connectez-vous avec post-only pour essayer

## Interface standard

## Exploration des profils par défaut

### À vous d'explorer la section Administration>Profils
