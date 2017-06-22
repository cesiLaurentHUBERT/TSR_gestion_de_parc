# Connexion avec LDAP

## Introduction

Pour permettre aux utilisateurs de se connecter via leur login/password de la société, on peut connecter GLPI à un serveur d'annuaire (type LDAP ou AD).

## Installation et configuration de la VM

### Applicance Turnkey Linux

La [VM fournie par turnkeylinux.org](https://www.turnkeylinux.org/openldap) permet de gérer un serveur LDAP préconfiguré.


### Outils LDAP


#### phpLDAPadmin

Connectez-vous à phpLdapAdmin en utilisant un navigateur et l'IP (ou le nom réseau) de la machine

#### Navigateur LDAP

Vous pouvez utiliser l'outil http://www.jxplorer.org/ pour modifier et visualiser la base LDAP.

## Configuration

Placez­-vous dans le menu `Configuration ­> Authentification`.

Cliquez sur le lien Annuaires LDAP.

Cliquez sur le bouton `Ajouter` (`+`) .

La fiche de saisie des données de connexion à l’annuaire s’affiche. Les différents champs proposés sont :

Préconfiguration : cet item se présente sous la forme de deux liens.
 -  Active Directory : ce lien permet de préremplir le champ Filtre de connexion avec un filtre par défaut et le Champ de l’identifiant avec la valeur samaccountname.
 - Valeur(s) par défaut : ce lien permet de préremplir le Champ de l’identifiant avec la valeur uid.

Nom : il est possible de définir plusieurs connexions LDAP. Ce champ permet donc d’identifier chaque connexion. Le nom devra être le plus explicite possible afin d’être identifié lorsqu’il apparaît dans d’autres écrans de l’application.

Serveur par défaut (Non/Oui) : ce champ permet de définir la connexion principale dans le cas où plusieurs connexions sont définies.

Actif (Non/Oui) : ce champ permet d’activer ou de désactiver une connexion. Cet item peut être utile en cas de maintenance du serveur pour bloquer toutes les connexions en une seule action.

Serveur : ce champ est destiné à recevoir l’adresse du serveur hébergeant l’annuaire LDAP. Port (par défaut 389) : ce champ contient le port d’échange des données avec l’annuaire LDAP.
Filtre de connexion: ce champ permet de définir une condition de recherche, afin de limiter le nombre d’enregistrements renvoyés par la requête.

BaseDN : ce champ permet de définir le point de départ des recherches dans l’arborescence de l’annuaire LDAP. DN du compte (pour les connexions non anonymes) : ce champ contient le nom du compte à utiliser par GLPI pour récupérer les données lorsque les connexions anonymes ne sont pas autorisées sur le serveur.

 - ici: `dc=masocietedemo,dc=com`


Mot de passe du compte (pour les connexions non anonymes) : ce champ contient le mot de passe associé au compte ci­dessus pour les connexions non anonymes.

Champ de l’identifiant : ce champ permet de désigner le champ à récupérer dans l’annuaire LDAP, qui servira d’identifiant dans la base GLPI. Si par exemple le login utilisé dans GLPI est l’adresse email, vous placerez la valeur mail dans ce champ.
Commentaires : ce champ vous permet d’ajouter toutes les informations que vous jugez nécessaires en vue d’une maintenance ultérieure.

### Test

Pour valider la création de la connexion, cliquez sur le bouton Ajouter. GLPI teste la connexion sur le serveur LDAP.
Retournez dans la fiche de la connexion. Un ensemble de sous­-menus a été ajouté.

Le sous-menu `Tester` lance le test de connexion à l’annuaire LDAP. Un bouton Tester permet de relancer le test de connection si nécessaire.

### Utilisateurs

Cet item contient les champs de liaison à l’annuaire LDAP. Pour chacun des champs suivants de la base GLPI vous allez pouvoir désigner le champ de l’annuaire LDAP à récupérer.
Les champs pour lesquels l’attribut LDAP fait partie des attributs courants sont :

 - Nom de famille : sn
 - Prénom : givenname
 - Courriel : mail
 - Téléphone : telephonenumber l Langue : preferredlanguage
 - Matricule : employeenumber l Titre : title


D’autres champs peuvent également être alimentés à partir des attributs LDAP de votre choix :

 - Téléphone 2
 - Téléphone mobile l Catégorie
 - Commentaires
 - Courriel 2
 - Courriel 3
 - Courriel 4
 - Image


## Vérification
### Connexion

Le nom d'utilisateur est `jquelqun`. Le mot de passe est celui saisi dans l'annuaire.

### Outils en ligne de commande

En cas de dysfonctionnement, vous pouvez installer le paquet `ldap-utils`

Pour configurer les commandes LDAP en leur indiquant le serveur, éditer le fichier `/etc/ldap/ldap.conf`

Décommenter la ligne commençant par `#URI ...`

Vous devriez avoir quelque chose ressemblant à ceci :

```conf
BASE    dc=masocietedemo,dc=com
URI     ldap://ldap.exemple.cesi
```

La commande suivante doit vous afficher immédiatement (ou presque) les informations de l'annuaire:

```bash
ldapsearch -b'dc=masocietedemo,dc=com' -x
```

### En cas d'erreur

```
$ ldapsearch -b'dc=masocietedemo,dc=com' -x
ldap_sasl_bind(SIMPLE): Can't contact LDAP server (-1)
```

Ce type d'erreur indique un blocage du firewall: vérifier que le port 389 est ouvert.
