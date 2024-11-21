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


# Installation de FortiGate-VM sur VirtualBox avec le fichier VBH

## Étapes d'installation

### 1. Télécharger et préparer les fichiers

Téléchargez le fichier **FortiGate-VM** à partir du site **Fortinet** et décompressez-le dans un dossier de votre choix. Vous trouverez plusieurs fichiers, dont :

- **Virtual Hard Disks** (`*.vhd`), qui contiennent les systèmes de fichiers de la machine virtuelle.
- **Virtual Machines** (`fortios.xml`), qui est le fichier de configuration de la machine virtuelle pour **VirtualBox**.

### 2. Créer une nouvelle machine virtuelle dans VirtualBox

1. Ouvrez **VirtualBox** et cliquez sur **Nouvelle** pour créer une nouvelle machine virtuelle.
2. Donnez un nom à la machine virtuelle (par exemple, `FortiGate-VM`).
3. Sélectionnez le type de système d'exploitation comme **Linux** et la version **Other Linux (64-bit)** (FortiGate est basé sur un système Linux personnalisé).
4. Cliquez sur **Suivant** pour passer à l'étape de la mémoire (généralement, 2 Go ou plus suffisent).

### 3. Attacher les disques virtuels

1. Lors de la configuration du disque dur, sélectionnez **Utiliser un fichier de disque virtuel existant**.
2. Choisissez l'un des fichiers **VHD** fournis (par exemple, `fortios.vhd`) et attachez-le à la machine virtuelle.
3. Vous pouvez ajouter un autre disque si nécessaire (comme `DATADRIVE.vhd`).

### 4. Modifier les paramètres de la machine virtuelle

1. Allez dans **Paramètres** > **Système** et désactivez l'option **Floppy** si elle est activée, car elle n'est pas nécessaire pour cette installation.
2. Dans **Réseau**, assurez-vous que l'interface réseau est en mode **Accès par pont** ou **Réseau interne**, en fonction de la configuration réseau que vous souhaitez utiliser pour la machine virtuelle FortiGate.

### 5. Importer la configuration (fortios.xml)

1. **Importer le fichier `fortios.xml`** dans VirtualBox :
   - Allez dans le dossier **Virtual Machines**.
   - Ouvrez le fichier `fortios.xml` dans un éditeur de texte et vérifiez que le chemin des fichiers **VHD** correspond à celui de votre installation.
   - Si nécessaire, modifiez le fichier pour pointer vers le bon emplacement des fichiers **VHD**.
   
2. **Importer le fichier `.xml` dans VirtualBox** :
   - Si vous avez créé le fichier `.xml` manuellement, vous pouvez l'utiliser pour configurer les paramètres de la machine virtuelle.

### 6. Lancer la machine virtuelle

Une fois la machine virtuelle configurée, cliquez sur **Démarrer** pour lancer la machine virtuelle.

Vous verrez la sortie de démarrage de **FortiGate**, et une fois que l'OS a démarré, vous aurez accès à l'interface de gestion via la ligne de commande.

### 7. Accéder à l'interface de gestion

1. **Accéder à l'interface graphique de FortiGate** :
   - Ouvrez un navigateur Web et connectez-vous à l'IP de l'interface de gestion FortiGate (par défaut, `192.168.1.99`).
   - Vous pouvez vous connecter via HTTPS avec le mot de passe administrateur par défaut (généralement `admin`).

2. **Configurer les interfaces réseau** :
   - Si vous avez des interfaces réseau comme `wan` et `lan`, vous pouvez les configurer via la CLI de **FortiGate**.
   - Exemple de commande pour configurer une interface **WAN** en mode statique :
   
   ```bash
   config system interface
   edit wan
   set mode static
   set ip 192.168.1.2 255.255.255.0
   set allowaccess ping https ssh
   end
   ```
