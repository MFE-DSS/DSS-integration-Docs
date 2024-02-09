## Prérequis

    Accès root ou sudo sur le système Linux cible.
    Instance Dataiku DSS installée (Design Node ou Automation Node).
    Accès à la configuration de l'instance Dataiku DSS.
    
## Mise à jour de R sur Linux
Pour CentOS 7

### Activer EPEL Repository :
```bash
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```
### Activer le dépôt optionnel (pour On Premise ou Public Cloud) :

```bash

sudo subscription-manager repos --enable "rhel-*-optional-rpms"
```
### Spécifier la version de R :
Déterminez avec votre groupe d'utilisateurs R la version souhaitée et compatible avec votre système.
```bash

export R_VERSION=4.x.x
```
### Télécharger et installer R :

```bash

curl -O https://cdn.rstudio.com/r/centos-7/pkgs/R-${R_VERSION}-1-1.x86_64.rpm
sudo yum install R-${R_VERSION}-1-1.x86_64.rpm
```
Vérifier l'installation de R :

```bash

    /opt/R/${R_VERSION}/bin/R --version
```
### Adaptations pour Autres Systèmes

- Ubuntu / Debian : Utilisez apt au lieu de yum et ajustez les URLs de téléchargement en fonction de votre distribution.
- SUSE Linux : Utilisez zypper pour l'installation et suivez les instructions spécifiques à la distribution pour les dépôts.

## Configuration au niveau de DSS pour jouer l'intégration de R sur la version choisi.

### Ajouter R au PATH de Dataiku DSS :
    Arrêtez Dataiku DSS, eventuellement vérifier la configuration du proxy puis ajoutez la ligne suivante à DATADIR/bin/env-site.sh :
```bash
DATA_DIR/bin/dss stop.
export PATH=/opt/R/4.x.x/bin/:$PATH

DATA_DIR/bin/dssadmin install-R-integration

```
exemple de dir;
- Design Node: DATA_DIR=/data/dataiku/design
- Automation Node: DATA_DIR=/data/dataiku/automatin


Redémarrez DSS et déconnectez-vous, puis reconnectez-vous avec l'utilisateur dataiku dans le terminal.

```bash
./bin DSS start
```

### Vérifiez la Version de R.
Utilisez la commande suivante pour confirmer que Dataiku utilise maintenant R 4 :
```bash
    which R
```
### Puis connectez vous sur votre instance et mettez à jour les codes environnements en R :

Dans l'interface utilisateur de Dataiku DSS;
- Naviguez vers Administration > Code Envs.
- Sélectionnez l'environnement de code R concerné.
- Cliquez sur "Rebuild Environment" et cochez "Rebuild from scratch".
- Sauvegardez les modifications et lancez la reconstruction.
