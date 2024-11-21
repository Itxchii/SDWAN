# Installation et Configuration de VirtualBox et Vagrant

## Prérequis

1. **Télécharger et installer VirtualBox** :
   - Rendez-vous sur le site officiel de [VirtualBox](https://www.virtualbox.org/) et téléchargez la version adaptée à votre système d'exploitation.
   - Suivez les étapes de l'installation (configuration par défaut recommandée).

2. **Télécharger et installer Vagrant** :
   - Accédez au site [Vagrant](https://www.vagrantup.com/) pour télécharger la dernière version.
   - Installez Vagrant en suivant les étapes.

3. **Valider l'installation** :
   - Ouvrez une invite de commande (ou terminal) et exécutez :
     ```cmd
     vagrant --version
     virtualbox --help
     ```

---

## Procédure de Configuration

### 1. Supprimer l'ancienne machine virtuelle
Si une ancienne box comme `ubuntu/trusty64` est toujours active, supprimez-la pour éviter les conflits :
    ```cmd
    vagrant destroy -f
    ```



# Initialisation d'une Configuration avec une Box Windows 10

## Étape 1 : Initialiser une nouvelle configuration
Exécutez la commande suivante pour créer une configuration avec la box Windows 10 :
    ```cmd
    vagrant init StefanScherer/windows_10 --box-version 2021.12.09
    ```

## Étape 2 : Modifier le fichier Vagrantfile
- Ouvrez le fichier Vagrantfile généré et modifiez-le pour inclure le provisionnement PowerShell :
    ```
        Vagrant.configure("2") do |config|
    config.vm.box = "StefanScherer/windows_10"
    config.vm.box_version = "2021.12.09"

    # Provisionnement PowerShell
    config.vm.provision "shell", inline: <<-SHELL
        powershell.exe -Command "Write-Output 'Windows VM provisioned successfully'"
    SHELL
    end

    ```

## Étape 3 : Démarrer la machine virtuelle
    ```
    vagrant up

    ```

> Aller sur Virtualbox ou votre hyperviseur la machine y apparaitra avec la configuration