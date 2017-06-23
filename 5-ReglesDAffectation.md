# Règles d'affectation de GLPI

## Introduction

GLPI permet de récupérer des informations d'un annuaire ou d'une base de données OCS et d'automatiser certaines tâches fastidieuses.

Ceci permet par exemple de déterminer automatiquement si un utilisateur sera administrateur (Technicien) ou simple utilisateur à partir des informations de l'annuaire.

Ceci permet également d'affecter un élément du parc à une entité.


## Base de données de test

Afin de réaliser les tests effectués ici, vous pouvez utiliser la [base de données fournie ici](data/glpi-9.1.3-init-demo.sql.gz)

**Attention : ** l'utilisation de cette BDD va supprimer les plugins et leur configuration et la configuration de la base LDAP.

**Pensez donc à bien noter les informations de configuration pour :**
 - LDAP
 - OCSNG


## Affectation d'un profil et d'une entité à un utilisateur

Il est possible d'affecter un profil et une entité à un utilisateur en fonction de son département (champ LDAP `departmentnumber`). Pour automatiser cela, il est nécessaire de définir une règle.

### Définition de la règle d'affectation des habilitations

Aller dans `Administration > Règles` puis cliquer sur `Règles d'affectation d'habilitations à un utilisateur`.

#### Ajout
Cliquer sur le bouton `+` pour ajouter une nouvelle règle:

 - Nom: Admin
 - Description: Administrateur par pays

Cliquer sur `Sauvegarder`

#### Définition du critère
Cliquer sur `Critère` puis cliquer sur `Ajouter un nouveau critère`

Pour le critère (liste de choix en haut à droite) choisir: `Critères LDAP` -> `(LDAP)Department Number`

Puis sélectionner: `expression rationnelle vérifie` et mettre `#.*/adminsysteme/(.*)$#` dans le champ correspondant.

Cliquer sur `Ajouter` : la règle apparaît dans la liste.

#### Définition de l'action
Cliquer sur `Action`

Cliquer sur `Ajouter une nouvelle action`

Sélectionner `Entité depuis TAG`

Choisir `Assigner valeur depuis expression rationnelle` et entrer `#0` dans le champ correspondant

Cliquer sur `Ajouter`: l'action apparaît dans la liste.


Faire de même avec:
- Définition d'un profil
  - Action: Profils
  - Assigner
  - Technicien
- Affectation récursive (permet à l'utilisateur affecté à une entité d'avoir les droits sur ses sous-entités):
  - Action : Récursif
  - Assigner
  - Oui


#### Tests
##### Test du critère
Cliquer sur `Règle` puis sur le bouton `Tester`

Entrer `/informatique/adminsysteme/France` dans le champ `(LDAP) departmentnumber`

Cliquer sur `Tester`

Le résultat devrait être le suivant:

| Résultat de la règle ||||
|---|---|---|---|
| Validation	 | Oui |
|Résultat de l'expression rationnelle	| |Clé | Valeur |
|||0	|France|


##### Test général

Aller dans `Administration > Règles` puis cliquer sur `Règles d'affectation d'habilitations à un utilisateur`.

Cliquer sur `Tester le moteur de règles`

Entrer les valeurs suivantes:

- departmentNumber : `/informatique/adminsysteme/France`
- mail : `jean.quelqun@masocietedemo.com`

Vous devriez voir les valeurs suivantes s'afficher:

|Détail du résultat||
|---|---|
|Admin|	Oui
|Root	|Oui

|Résultat de la règle||
|---|---|
|Validation|	Oui

|Affectation d'entités||
|---|---|
|Entité |	MaSociétéDémo|
|Entité |	MaSociétéDémo |


|Affectation de droits||
|---|---|
|Profils| 	Technicien|


### Ajout des TAG sur les entités
Il reste à définir les TAG sur les entités.

**Ces TAG sont très importants :** ils permettent de faire le lien entre le `#0` renvoyé par l'expression régulière et l'entité à affecter à l'utilisateur.

#### Définition des TAG

Ici, on définit le TAG pour l'entité France, mais il faut le faire pour toutes les entités.

Aller dans `Administration > Entités`

Cliquer sur `France`

Cliquer sur `Informations avancées`Ó

Entrer les valeurs suivantes pour ces champs:
 - Information de l'outil d'inventaire (TAG) représentant l'entité	: `France`
 - Information de l'annuaire LDAP représentant l'entité : `France`

 Cliquer sur `Sauvegarder`

##### Test
Retourner dans `Administration > Règles` puis cliquer sur `Règles d'affectation d'habilitations à un utilisateur`.

Cliquer sur `Tester le moteur de règles`

Entrer les valeurs suivantes:

- departmentNumber : `/informatique/adminsysteme/France`
- mail : `jean.quelqun@masocietedemo.com`

Vous devriez voir apparaître le tableau suivant:

| Affectation d'entités ||
|---|---|
| Entité 	| Entité Racine > France |
| Profils 	|Technicien
| Récursif |	Non|

Ceci indique que l'entité France a bien été affectée.


## Utilisation

Maintenant que vous avez pu définir la règle, nous allons la tester en nous connectant avec l'utilisateur `jquelqun`.

Déconnectez-vous (ou ouvrez un autre navigateur Internet).

Connectez-vous avec l'utilisateur.

Une fois connecté, vous devriez avoir accès au menu `Parc`. Allez dans `Parc > Ordinateurs`.

Vous constatez ici que seul un ordinateur est disponible: poste_recup.

C'est le seul ordinateur qui soit affecté à l'entité France. Et comme Jean Quelqun est affecté comme `Technicien` pour l'entité France, c'est le seul ordinateur qu'il peut voir.


Nous allons maintenant effectuer quelques opérations qui vont vous permettre de manipuler des éléments du parc.

### Changement d'entité (transfert)

Sous l'utilisateur `glpi`, vous allez créer un nouvel ordinateur nommé `Ordinateur Rennes` (peu importe ses caractéristiques).

Une fois créé, cet ordinateur ne sera pas visible pour Jean Quelqun car il est dans l'entité Racine.

Nous allons donc le transférer vers l'entité France.

Pour cela:

1. sélectionner l'ordinateur correspondant dans la liste des ordinateurs (cocher la case à gauche)
1. Cliquer sur le bouton `Action`
1. Sélectionner l'action *Ajouter à la liste de transfert*
1. Choisissez le mode de transfert: `Complete`
1. Sélectionnez l'entité `Entité Racine > France > Ouest > Rennes`
1. Modifier les options selon le besoin
1. Cliquer sur `exécuter`

Désormais, Jean Quelqun devrait pouvoir visualiser l'ordinateur transféré (menu `Parc > Ordinateurs`) : l'entité affectée à l'ordinateur est incluse dans les sous-entités affectées à Jean Quelqun.
