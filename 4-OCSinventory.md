# Installation de l'outil d'inventaire automatique


L'installation d'OCS se fait via l'utilitaire `apt-get install`.

Les deux paquets à installer sont les suivants:

 - ocsinventory-server
 - ocsinventory-reports

Debian installe la version 2.0.5 de ces deux paquets.

Après avoir installé le paquet `build-essential`, il est possible de mettre à jour OCS NG en suivant les instructions données [sur cette page](http://wiki.ocsinventory-ng.org/index.php?title=Howtos:Install_OCS_on_debian/fr).


Vous pouvez également lire la documentation présente [sur cette page](http://wiki.ocsinventory-ng.org/index.php?title=Documentation:Server/fr) pour plus d'informations.


Pour commencer, vous allez configurer OCSinventory en tant que serveur.

Ensuite, vous allez installer l'outil de reporting d'OCS

Vous pourrez ensuite installer les agents sur les différents postes.

## Installation

### Installation des paquets

La commande suivante permet d'installer à la fois le serveur et le système de gestion (par interface Web):

```bash
sudo apt-get install ocsinventory-server ocsinventory-reports
```

Les logins et mot de passe sont dans la documentation indiquée précédemment. Cherchez cette information.



## Configuration de l'outil de reporting

### Configuration DNS et DHCP

Configurer DNS et DHCP de manière à faire pointer l'adresse `ocs.exemple.cesi` vers l'IP du serveur OCS.

N'oubliez pas de configurer le fichiers `hosts` de votre machine physique pour qu'il gère cette nouvelle adresse.

### VirtualHost Apache

La configuration de votre VirtualHost devrait ressembler à ceci (fichier `/etc/apache2/conf-available/ocsinventory-reports.conf`):

**Pensez à faire une sauvegarde du fichier original**

```conf
################################################################################
#
# OCS Inventory NG Administration Server
#
# Copyleft 2008 OCS Inventory NG Team
# Web: http://www.ocsinventory-ng.org
#
# This code is open source and may be copied and modified as long as the source
# code is always made freely available.
# Please refer to the General Public Licence http://www.gnu.org/ or Licence.txt
################################################################################

#
# ANY CHANGE ON THIS FILE REQUIRES APACHE RESTART TO TAKE EFFECT
#

################################################################################
# Administration console public pages
#
# Alias used to put Administration Server static page (typically PHP) outside
# Apache document root directory
#
<VirtualHost *:80>
    DocumentRoot /usr/share/ocsinventory-reports/
    ServerName ocs.exemple.cesi
    <Directory /usr/share/ocsinventory-reports>
        # By default, users can use console from everywhere
        Order deny,allow
        Allow from all
        Options Indexes FollowSymLinks
        DirectoryIndex index.php
        AllowOverride Options

        # Uncomment following to force use of HTTPS in Administration Server
        #SSLRequireSSL

        # PHP tuning (not working on all distribution, use php.ini instead)
        AddType application/x-httpd-php .php
        php_flag file_uploads           on
        # Some PHP tuning for deployement feature up to 8 MB
        # post_max_size must be greater than upload_max_filesize
        # because of HTTP headers
        php_value post_max_size         101m
        php_value upload_max_filesize   100m
        # You may have to uncomment following on errors
        #php_value max_execution_time -1
        #php_value max_input_time -1

        #!! Mandatory !! : set magic_quotes_gpc to off (to make ocsreports works correctly)
        php_flag magic_quotes_gpc      off

        # Uncomment following to allow HTTP body request up to 4 MB
        # instead default 512 KB
        #LimitRequestBody 4194304

        # Uncomment following if you need to specify a mysql socket
        #php_value mysql.default_socket "path/to/mysql/unix/socket"

    </Directory>

    ################################################################################
    # Deployment packages download area
    #
    # Alias to put Deployement package files outside Apache document root directory
    #
    Alias /download /var/lib/ocsinventory-reports/download
    <Directory /var/lib/ocsinventory-reports/download>
        Options Indexes MultiViews
        AllowOverride None
        Order allow,deny
        Allow from all
        Require all granted
    </Directory>
</VirtualHost>
```

Redémarrer votre serveur Apache après avoir activé cette configuration

### Initialisation d'OCS

Se connecter sur `http://ocs.exemple.cesi` pour initialiser la base de données OCSNG.

Entrer les informations de connexion:

- Login MySQL: root
- MdP root MySQL
- ocsweb
- Hôte MySQL: localhost

## Sauvegarde et restauration de la BDD

Pour pouvoir tester et vérifier si les récupérations de données se font correctement, il va être utile d'avoir une copie de sauvegarde de la BDD OCS.

On fera une première sauvegarde nommée `ocsweb-init.sql` que l'on conservera comme référence.

### Sauvegarde

Exemple de commande.

```bash
sudo mysqldump --add-drop-table --complete-insert --extended-insert --quote-names --host=localhost --user=root -p ocsweb > ocsweb-$(date +%Y%m%d%H%M%S).sql
```

### Restauration

```bash
mysql -u root -p ocsweb < ocsweb-201706170934.sql
```


## Configuration de la connexion à la base OCS NG

### Changement du mot de passe de la base ocsweb
Ceci est nécessaire pour sécuriser la base.


#### Mot de passe MySQL
Vous allez taper les commandes suivantes:

```bash
mysql -h localhost -u root -p
```

À l'invite mysql, tapez les commandes suivantes:

```sql
use mysql;
SET PASSWORD FOR 'ocs'@'localhost' = PASSWORD('ocsocsDEMO');
SET PASSWORD FOR 'ocs'@'%' = PASSWORD('ocsocsDEMO');
FLUSH PRIVILEGES;
quit
```
#### Mot de passe pour l'interface de gestion

Editer le fichier de configuration et remplacer le mot de passe:
```bash
sudo ne /etc/ocsinventory/dbconfig.inc.php
```

C'est la ligne suivante qu'il faut modifier:
```php
define("PSWD_BASE","ocsocsDEMO");
```

#### Mot de passe pour le serveur OCS
Ensuite, modifier le fichier `/etc/apache2/conf-available/ocsinventory-server.conf` et remplacer la ligne suivante:

```conf
# Password for user
PerlSetVar OCS_DB_PWD ocs
```

Par:
```conf
# Password for user
PerlSetVar OCS_DB_PWD ocsocsDEMO
```

**Redémarrer ensuite votre serveur Apache**

#### Suppression des fichiers d'installation
Puis supprimer le fichier d'installation:

```bash
sudo rm /usr/share/ocsinventory-reports/install.php
```
### Mise à jour du mot de passe admin d'OCS

Parfois le mot de passe de l'utilisateur `admin` est remis à zéro après le changement de mot de passe précédent.

Pour changer ce mot de passe:
```bash
mysql -u root -p ocsweb
```
Et tapez les commandes SQL suivantes:
```sql
use ocsweb;

UPDATE operators SET `PASSWD` = MD5('ocswebCESI') WHERE `operators`.`ID` = 'admin';

quit

```

### Installation des agents

#### Debian

Le gestionnaire de paquet permet l'installation de l'agent: `ocsinventory-agent`

Configurer l'agent pour utiliser la méthode `http` afin d'utiliser la connexion réseau.

Indiquer l'adresse du serveur telle que définie dans le DNS `ocs.exemple.cesi` (si ce n'est pas déjà fait, configurer le DNS correctement).

Ajouter un tag: `FR-Nantes-SI`

Dans cet exemple `FR-Nantes-SI` va permettre d'indiquer que l'ordinateur est affecté au service informatique de l'entité `France>Nantes`.

Vous pouvez également modifier les informations de configuration en éditant le fichier `/etc/ocsinventory/ocsinventory-agent.cfg`:

```conf
server=ocs.exemple.cesi
tag=FR-Nantes-SI
```


##### Test d'inventaire

Pour le premier test, nous allons utiliser la machine `netservice`.

Se connecter à l'interface de gestion et constater qu'aucun ordinateur n'est enregistré.

Sur la machine `netservice` lancer la commande : `sudo ocsinventory-agent`

Sur l'interface de gestion cliquer sur l'icône `Computers` ![computers](images/ocs-computers.png).

Vous verrez apparaître une ligne dans la liste des ordinateurs référencés.

#### Windows

Pour Windows, vous allez utiliser l'agent OCS compatible que vous trouverez [ici](https://github.com/OCSInventory-NG/WindowsAgent/releases) dans sa version 2.0.5

La configuration de cet agent se fait à l'installation. Cependant, il est possible de le configurer après coup.

Vous allez positionner un Tag sur chaque nouvel ordinateur. Par exemple `Fr-Rennes-RH` va permettre d'indiquer que l'ordinateur est affecté au service RH de l'entité `France>Rennes`.

La nomenclature des Tags est à définir selon les besoins et l'organisation du SI de l'entreprise.

##### Fichiers de configuration

Le fichier de configuration de cet agent est placé ici: `C:\ProgramData\OCS Inventory NG\Agent`. Une fois modifié, il est nécessaire de redémarrer Windows pour tenir compte des modifications.

Le fichier de log est situé au même emplacement et se nomme `OCSInventory.log`. Ce fichier vous permet de vérifier que la connexion au serveur s'est bien passée. Exemple:

```log
==============================================================================
Starting OCS Inventory NG Agent on Friday, June 23, 2017 09:02:50.
AGENT => Running OCS Inventory NG Agent Version 2.0.5.0
AGENT => Using OCS Inventory NG FrameWork Version 2.0.5.0
AGENT => Loading plug-in(s)
AGENT => Using network connection with Communication Server
AGENT => Using Communication Provider <OCS Inventory NG cURL Communication Provider> Version <2.0.5.0>
AGENT => Sending Prolog
AGENT => Prolog successfully sent
SUPPORT => No support registered for your installation. Check OCS Inventory NG support packages at http://www.ocsinventory-ng.com
AGENT => Inventory required
AGENT => Launching hardware and software checks
AGENT => Sending Inventory
INVENTORY => Inventory changed since last run
AGENT => Inventory successfully sent
AGENT =>  Communication Server ask for Package Download
AGENT => Unloading communication provider
AGENT => Unloading plug-in(s)
AGENT => Execution duration: 00:00:32.
```



## Connexion avec GLPI

L'intérêt d'OCS est qu'il va nous permettre de récupérer les informations dans la BDD GLPI.

### Configuration OCSNG
Sur l'interface de gestion, cliquez sur le bouton de configuration ![config](images/ocs-config.png)

Puis cliquez/survolez ![config](images/ocs-config2.png) et cliquez sur `config`. Dans l'onglet `Server` positionnez `TRACE_DELETED` sur `On`

#### En cas de problème de connexion avec `admin`

Utiliser phpMyAdmin pour générer un nouveau mot de passe dans la table `operators` (hashage en MD5).

### Installation du plugin OCSNG

 - Placez­-vous dans le menu Configuration ­ Plugins.
 - Cliquez sur le bouton Voir le catalogue des plugins.
 - Dans le sommaire, cliquez sur ocs­ng.
 - Cliquez sur le lien En savoir plus du plugin OCS INVENTORY NG. Cliquez sur le bouton Télécharger.
 - Avec l'utilitaire `wget`, vous allez récupéré une archive. Décompressez cette archive. Décompressez cette archive dans ../glpi/plugins.
   - `sudo -u www-data tar xzf glpi-ocsinventoryng-1.3.3.tar.gz  -C /usr/share/glpi/plugins/`
 - Cliquez à nouveau dans le menu Configuration ­ Plugins. Cliquez sur le bouton Installer.
 - Cliquez sur le bouton `Activer`

 Le menu `OCS Inventory NG` est désormais disponible dans le menu `Outils`.

### Importation

Une fois la configuration faite, on va pouvoir faire un premier import.

Allez dans le menu `Outils > OCS Inventory NG` puis sur l'onglet `Import de l'inventaire`.

Le bouton `+` permet d'importer de nouveaux ordinateurs. Il est également possible de synchroniser les ordinateurs existants.
