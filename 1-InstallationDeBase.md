# Installation de GLPI

## Installation

### Installation par gestionnaire de paquets

```bash
sudo apt-get install glpi
```

### Installation manuelle

Voir la page : http://glpi-project.org/spip.php?article31

## Mise à jour à la dernière version

Voir la page http://glpi-project.org/spip.php?article171

### Pour connaître le répertoire d'installation
Aller dans le répertoire /etc/apache2/conf-enabled/

Visualiser `glpi.conf` et trouver le document root.

Sous Debian, le dossier cible pour `glpi` est `/usr/share/glpi`

On va déplacer le dossier par défaut dans un répertoire dont le nom contient le numéro de version.

Utiliser la commande `apt-cache showpkg | grep -A1 Versions` pour connaître ce numéro.

Par exemple: `glpi-0.84.8`

### Récupération de la mise à jour
Voir le site de [GLPI](http://glpi-project.org/) et télécharger le fichier `tar.gz` correspondant à la mise à jour.

#### Quelques opérations courantes
Lister le contenu du tar :

`tar tvf glpi-9.1.tar.gz | less`

Lister les versions installées sur le système :

`ls -ld /usr/share/glpi*`

#### Installation de la mise à jour

Suivre les instructions de la page sur la [Mise à jour de GLPI](http://glpi-project.org/spip.php?article171) ainsi que les instructions suivantes.

Extraire le tar :

```bash
sudo tar xvzf glpi-9.1.tar.gz -C /usr/share
```

Renommage:

```bash
mv /usr/share/glpi /usr/share/glpi-9.1
```

## Création du lien symbolique

```bash
cd /usr/share
ln -s glpi-9.1 glpi
```

Ensuite se connecter avec le navigateur sur la page de votre serveur `glpi.serveur.domaine.com`
ou `<adresse_ip>/glpi`


## Si des erreurs apparaissent

### Problème de droit

```bash
sudo chown -R www-data:www-data /usr/share/glpi-9.1/
```

### Correction des avertissements

Vérifier que les paquets correspondants aux avertissement sont installés:

```bash
sudo apt-cache search php |grep curl
sudo apt-get install php5-curl
```

Après chaque installation, il faut redémarrer le serveur Apache

### Installation de gd

```bash
sudo apt-get install php5-gd
```

### Installation du module IMAP

```bash
sudo apt-get install php5-imap
```



### Redémarrer php5

#### Sous Debian Jessie

```bash
sudo systemctl restart apache2.service
```

## Configuration du serveur MySQL/MariaDB

Entrer les paramètres suivants:

Host: `localhost`
User: `glpi`
Password: `MOT DE PASSE DE LA BDD GLPI que vous avez entré précédemment`


## Gestion du nom de domaine

### Configuration Apache2

Dans le fichier /etc/apache2/conf-available/glpi.conf (faites une sauvegarde de l'original) vous devez avoir le contenu suivant:

```conf
<VirtualHost *:80>
  DocumentRoot /usr/share/glpi
  ServerName glpi.exemple.cesi
</VirtualHost>
```

Activez cette configuration (si ce n'est déjà fait) avec la commande:

```bash
sudo a2enconf glpi.conf
```

Puis redémarrez votre serveur Apache:

```bash
sudo systemctl restart apache2
```
