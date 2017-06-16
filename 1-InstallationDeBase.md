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



## Lister le contenu du tar

### tar tvf glpi-9.1.tar.gz | less

## Extraire le tar

### sudo tar xvzf glpi-9.1.tar.gz -C /usr/share

## Lister

### ls -ld /usr/share/glpi*

## On va déplacer le fichier

### glpi-9.1

## On va créer un lien symbolique

### ln -s glpi-9.1 glpi

## Faire la mise à jour

### Des erreurs apparaissent

#### Problème de droit

#### sudo chown -R www-data:www-data glpi-9.1/

## Correction des avertissements

### sudo apt-cache search php |grep curl sudo apt-get install php5-curl

### COmment faire pour gd ?

#### sudo apt-get install php5-gd

### Redémarrer php5

#### Comment faire avec Jessie ?

##### sudo systemctl restart apache2.service

## Serveur MySQL/MariaDB

### config

#### localhost

#### glpi

#### gmsi15

### Bdd à mettre à jour:

#### glpi

## Modifier la langue de l'administrateur glpi

# Sécurisation des comptes

## Menu Administration > Utilisateurs

### Pour chaque utilisateur, changer le mot de passe

#### Dans le cas présent: le nom de la promo
