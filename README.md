# ARCHLINUX GUIDE

> [!WARNING]
> ArchLinux est une distribution DIY (Do It Yourself). Il est crucial d'avoir de solides compétences techniques ou d'être prêt à consulter abondamment la documentation. Il est impensable de rester sur Arch Linux si l'on dépend constamment de l'aide des autres. En cas de problème, il faut absolument être capable de trouver et de réparer soi-même rapidement. Sinon, on risque de devenir dépendant des autres ou de passer des heures à réparer ou réinstaller en boucle.
> 
> Extrait de [du WIKI officiel de Arch Linux](https://wiki.archlinux.org/title/Arch_Linux_(Fran%C3%A7ais)) : "Tandis que de nombreuses distributions GNU/Linux tentent d'être plus conviviales, Arch Linux a toujours été, et restera toujours centrée sur l'utilisateur. Elle est destinée à répondre aux besoins de ceux qui y contribuent, plutôt que d'essayer d'attirer le plus grand nombre. Elle est destinée à l'utilisateur compétent de GNU/Linux ou à toute personne ayant une attitude de bricoleur et disposée à lire la documentation et à résoudre ses propres problèmes."
>
> Être sur Arch Linux sans lire la documentation et en étant dépendant des autres va à l'encontre de ce qu'est cette distribution.

## **Table des Matières**

1. **[Installation](#installation)**
   - [Configurer le clavier en français](#configurer-le-clavier-en-français)
   - [Configurer votre Wi-Fi](#configurer-votre-wi-fi)
   - [Utilisation d'archinstall](#utilisation-darchinstall)
   - [Post-installation](#post-installation)

2. **[Post-installation](#post-installation)**
   - [Optimiser pacman](#optimiser-pacman)
   - [Installation d'un AUR helper](#installation-dun-aur-helper)
   - [Alias de maintenance](#alias-de-maintenance)
   - [Compilation multithread des paquets AUR](#compilation-multithread-des-paquets-aur)

3. **[Support matériel](#support-matériel)**
   - [Nvidia](#nvidia)
   - [AMD](#amd)
   - [Intel](#intel)
   - [Imprimantes](#imprimantes)
   - [Bluetooth](#bluetooth)
   - [PipeWire](#pipewire)
   - [Logiciel de base](#logiciel-de-base)
   - [Pare-feu](#pare-feu)
   - [Reflector pour la mise à jour automatique des miroirs](#reflector-pour-la-mise-à-jour-automatique-des-miroirs)
   - [Arch Update](#arch-update)
   - [Timeshift](#timeshift)
   - [Grub BTRFS](#grub-btrfs)
   - [Fish](#fish)

4. **[Améliorez votre Expérience de Jeu](#améliorez-votre-expérience-de-jeu)**
   - [Steam](#steam)
   - [Lutris](#lutris)
   - [Support avancé de manettes](#support-avancé-de-manettes)
   - [Affichage des performances en jeu](#affichage-des-performances-en-jeu)

5. **[Optimisation](#optimisation)**
   - [Tutoriel : Configuration du Multiboot avec grub](#tutoriel--configuration-du-multiboot-avec-grub)
   - [Zram](#zram)

6. **[Dépannage](#dépannage)**
   
7. **[Communauté et Sources](#communauté-et-sources)**

8. **[Remerciements](#remerciements)**

--- 

<br>

#### Installation <a name="installation"/>

Téléchargez et lancez la dernière iso de Arch Linux : https://archlinux.org/download/

#### 1. Configurer le clavier en français <a name="configurer-le-clavier-en-français"></a>

```sh
loadkeys fr
```

#### 2. Configurer votre Wi-Fi <a name="configurer-votre-wi-fi"></a>

```sh
iwctl
```
    
Puis (remplacez VOTRE-NOM-WIFI par le nom de votre wifi)

```sh
station wlan0 connect VOTRE-NOM-WIFI (SSID)
```

Entrez votre mot de passe wifi puis tapez `quit` pour quitter iwctl.

--- 

<br>

#### 3. Utilisation d'archinstall <a name="utilisation-darchinstall"></a>

Vous pouvez simplement taper : ```archinstall``` pour le lancer.

**Cependant :**  
Au moment où ces lignes sont écrites, la version de l'ISO d'Arch Linux 01/03/2024 tente d'installer le paquet "plasma-wayland" qui n'existe plus depuis la sortie de Plasma 6 et mène donc à une erreur qui plante archinstall. Ce problème est corrigé dans les versions plus récentes d'`archinstall`, donc mettre à jour avant de lancer l'installation est essentiel.

**Mise à jour de archinstall :**

```sh
pacman -Sy archinstall
```

D'autres erreurs de ce type peuvent arriver, il peut donc être parfois intéressant de prendre la dernière version de archinstall.

--- 

<br>

### Post-installation <a name="post-installation"></a>

#### 1. Optimiser pacman <a name="optimiser-pacman"></a>

Cette [modification](https://wiki.archlinux.org/title/Pacman#Enabling_parallel_downloads) permet la parallélisation des téléchargements de paquets.

```sh
sudo nano /etc/pacman.conf
```

Décommentez (retirez le **#** des lignes suivantes) :
   
```
#Options diverses
#UseSyslog
Color <-
#NoProgressBar
#CheckSpace
VerbosePkgLists <- 
ParallelDownloads = 5 <-
```
---

#### 2. **Installation d'un AUR helper** <a name="installation-dun-aur-helper"></a>

Les AUR helpers sont des outils pratiques pour gérer l'installation et la mise à jour des logiciels sur les systèmes basés sur Arch Linux.
Yay et paru facilitent particulièrement l'utilisation du dépôt AUR, un dépôt géré par la communauté qui étend considérablement la bibliothèque de logiciels disponible. Cela inclut la compilation de ces programmes à partir de leur source, à moins que "-bin" ne soit spécifié à la fin de leur nom.
**/!\ Soyez prudent /!\ Comme les paquets dans l'AUR sont fournis par la communauté, n'installez pas tout et n'importe quoi !**

Vous pouvez choisir entre **YAY** ou **Paru**

[Yay](https://github.com/Jguer/yay)
   
```sh
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
```

Ajout du support pour les mises à jour des paquets git. (Normalement, cela ne doit être fait qu'une seule fois)

```sh
yay -Y --gendb
yay -Y --devel --save
```

[Paru](https://github.com/Morganamilo/paru)

```sh
sudo pacman -S --needed base-devel
git clone https://aur.archlinux.org/paru-bin.git
cd paru-bin
makepkg -si
```

Ajout du support pour les mises à jour des paquets git. (Normalement, cela ne doit être fait qu'une seule fois)

```sh
paru --gendb
```

--- 

<br>

#### 3. Alias de maintenance : <a name="alias-de-maintenance"></a>

Cette modification vous permet de simplement taper « update-arch » dans un terminal pour mettre à jour le système, « clean-arch » pour le nettoyer, ou « fix-key » en cas d'erreur avec les clés gpg.

```sh
nano ~/.bashrc
```
  
Ajoutez chacune de ces lignes à la fin du fichier :

pour yay :

```sh
alias update-arch='yay'
```

```sh
alias clean-arch='yay -Sc && yay -Yc'
```

pour Paru :

```sh
alias update-arch='paru'
```

```sh
alias clean-arch='paru -Sc && paru -c'
```

Pour tous : 

```sh
alias update-mirrors='sudo reflector --verbose --score 100 --latest 20 --fastest 5 --sort rate --save /etc/pacman.d/mirrorlist '
```

```sh
alias fix-key='sudo rm /var/lib/pacman/sync/* && sudo rm -rf /etc/pacman.d/gnupg/* && sudo pacman-key --init && sudo pacman-key --populate && sudo pacman -Sy --noconfirm archlinux-keyring && sudo pacman --noconfirm -Su'
```

```sh
alias reinstall-all-pkg='sudo pacman -S  $(pacman -Qnq)'
```

Redémarrez le terminal.

--- 

<br>

#### 4. Compilation multithread des paquets AUR : <a name="compilation-multithread-des-paquets-aur"></a>

```sh
nano /etc/makepkg.conf
```

Pour utiliser tous les threads, ajoutez :

```sh
MAKEFLAGS="-j$(nproc)"
```

Ou si, par exemple, vous souhaitez utiliser 6 threads :

```sh
MAKEFLAGS="-j6"
```

Remplacez le 6 par le nombre de threads que vous souhaitez utiliser. Il est conseillé d'avoir 2 Go de RAM par cœur utilisé.

--- 

<br>

### SUPPORT MATÉRIEL <a name="support-matériel"></a>

--- 

<br>

#### Installation des pilotes NVIDIA <a name="nvidia"></a>

> [!IMPORTANT]
> Vous avez besoin des headers de votre kernel pour que `nvidia-dkms` fonctionne. Par exemple, si vous avez choisi le kernel zen, il faut installer `linux-zen-headers`.

#### 1. Installation des composants de base

Pour installer les pilotes et utilitaires Nvidia de base, utilisez la commande suivante :

```sh
sudo pacman -S --needed nvidia-open-dkms nvidia-utils lib32-nvidia-utils nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader opencl-nvidia lib32-opencl-nvidia
```
Remplacez `nvidia-open-dkms` par `nvidia-dkms` si votre catre Nvidia est de génération Pascal (gtx1000) ou inférieure.

##### Installation supplémentaire pour PC portable Intel/Nvidia

Si vous avez un [PC portable avec Intel/Nvidia](https://youtu.be/GhsP6btpiiw?si=ibWw_dQdty8_Q0jm), utilisez également les commandes suivantes :

```sh
sudo pacman -S --needed intel-media-driver intel-gmmlib onevpl-intel-gpu nvidia-prime
```

#### 2. Charger les modules Nvidia en priorité au lancement d'Arch :

Cette étape est non obligatoire et ne devrait être éffectuée que si on note des problèmes au démarrage.
   
```sh
sudo nano /etc/mkinitcpio.conf
```

Modifiez la ligne MODULES=() pour :

```sh
MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

Exemple si utilisation de btrfs :

```sh
MODULES=(btrfs nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

#### 3. Débloquer Wayland Si vous etes sur Gnome:

```sh
sudo ln -s /dev/null /etc/udev/rules.d/61-gdm.rules
```

#### 4. Reconstruction de l'initramfs :

Comme nous avons déjà installé les pilotes à l'étape 1, donc avant de configurer le hook, nous devons déclencher manuellement la reconstruction de l'initramfs :

```sh
sudo mkinitcpio -P
```

#### 5. Dynamic Boost :

Si votre PC portable est compatible Dynamic Bosst activez :

```bash
sudo systemctl enable --now nvidia-powerd
```

Vérifiez bien qu'il est compatible.

#### 6. Veille 

1. **Activer les services système nécessaires** :
   
   Exécutez les commandes suivantes pour activer les services nécessaires à la gestion de la suspension avec Nvidia :

   ```bash
   sudo systemctl enable nvidia-suspend.service
   sudo systemctl enable nvidia-hibernate.service
   sudo systemctl enable nvidia-resume.service
   ```

2. **Ajouter les paramètres du module du noyau** :
   
   Créez ou modifiez le fichier `/etc/modprobe.d/nvidia-sleep.conf` avec le contenu suivant :

   ```plaintext
   options nvidia NVreg_PreserveVideoMemoryAllocations=1 NVreg_TemporaryFilePath=/var/tmp
   ```

   - **NVreg_PreserveVideoMemoryAllocations=1** : Cette option permet de préserver les allocations de mémoire vidéo pendant la suspension, réduisant ainsi les problèmes graphiques lors de la reprise.
   - **NVreg_TemporaryFilePath=/var/tmp** : Définit le chemin du fichier temporaire utilisé par le pilote Nvidia pour gérer les allocations temporaires. Cette option peut aider à résoudre les problèmes de mémoire.
   
3. Rebuild de l’initramfs :

   ```bash
   sudo mkinitcpio -P
   ```
   
4. **Test de la configuration** :
   
   Après avoir effectué ces modifications, redémarrez votre système et testez une session de suspension et de reprise pour voir si les problèmes graphiques persistent. Si le problème persiste, vous pourriez envisager de revenir à X11, comme certaines configurations peuvent ne pas être totalement compatibles avec Wayland sur certaines versions de pilotes Nvidia.

--- 

<br>

#### AMD <a name="amd"></a>

Installer les composants de base :

```sh
sudo pacman -S --needed mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon vulkan-icd-loader lib32-vulkan-icd-loader vulkan-mesa-layers
```

AMD ROCM : 

```sh
sudo pacman -S rocm-opencl-runtime rocm-hip-runtime
```
---

#### INTEL <a name="intel"></a>

Installer les composants de base :

```sh
sudo pacman -S --needed mesa lib32-mesa vulkan-intel lib32-vulkan-intel vulkan-icd-loader lib32-vulkan-icd-loader intel-media-driver
```

--- 

<br>

#### Imprimantes <a name="imprimantes"></a>

1. **Essentiels** :
   - Installez les paquets nécessaires pour la gestion des imprimantes :
     ```sh
     sudo pacman -S --needed ghostscript gsfonts cups cups-filters cups-pdf system-config-printer avahi
     ```
   - Activez et démarrez les services Avahi et CUPS :
     ```sh
     sudo systemctl enable --now avahi-daemon cups
     ```
   - Désactivez le service systemd-resolved (si vous ne l'utilisez pas) :
     ```sh
     sudo systemctl disable systemd-resolved
     ```

2. **Pilotes** :
   - Installez les paquets pour les pilotes d'imprimantes génériques (Ces pilotes gèrent la plus part des imprimantes) :
     ```sh
     sudo pacman -S --needed foomatic-db-engine foomatic-db foomatic-db-ppds foomatic-db-nonfree foomatic-db-nonfree-ppds gutenprint foomatic-db-gutenprint-ppds
     ```

3. **Imprimantes HP** :
   - Pour les imprimantes HP, installez également les paquets suivants :
     ```sh
     yay -S --needed python-pyqt5 hplip
     ```

4. **Imprimantes Epson** :
   - Pour les imprimantes Epson, utilisez les commandes suivantes :
     ```sh
     yay -S --needed epson-inkjet-printer-escpr epson-inkjet-printer-escpr2 epson-inkjet-printer-201601w epson-inkjet-printer-n10-nx127
     ```

Tous les cas ne sont pas gérés par les points précédents, des fois les drivers sont sur AUR il faut fouiller comme par exemple pour la [brother-mfc-9340cdw](https://aur.archlinux.org/packages/brother-mfc-9340cdw).

--- 

<br>

#### Bluetooth <a name="bluetooth"></a>

La seconde commande ci-dessous demande à systemd de démarrer immédiatement le service bluetooth, et aussi de l'activer à chaque démarrage.

```sh
yay -S --needed bluez bluez-utils bluez-plugins
sudo systemctl enable --now  bluetooth.service
```

--- 

<br>

#### [PipeWire](https://pipewire.org/)  <a name="pipewire"></a>

**/!\ Dites oui à tout pour écraser tout avec les nouveaux paquets. /!\**

```sh
sudo pacman -S --needed pipewire lib32-pipewire pipewire-pulse pipewire-alsa pipewire-jack wireplumber alsa-utils alsa-firmware alsa-tools sof-firmware
```

---- 

<br>

### LOGICIEL DE BASE <a name="logiciel-de-base"></a>

--- 

<br>

#### Composants de base

Ici, vous trouverez des codecs, utilitaires, polices, pilotes :

```sh
yay -S --needed gstreamer-vaapi gst-plugins-bad gst-plugins-base gst-plugins-ugly gst-plugin-pipewire gstreamer-vaapi gst-plugins-good gst-libav gstreamer downgrade  libva-mesa-driver lib32-libva-mesa-driver mesa-vdpau lib32-mesa-vdpau rebuild-detector xdg-desktop-portal-gtk xdg-desktop-portal neofetch power-profiles-daemon lib32-pipewire hunspell hunspell-fr p7zip unrar ttf-liberation noto-fonts noto-fonts-emoji adobe-source-code-pro-fonts otf-font-awesome ttf-meslo-nerd ttf-droid duf btop  ntfs-3g fuse2fs exfatprogs fuse2 fuse3 bash-completion man-db man-pages
```

--- 

<br>

#### Logiciels divers

```sh
yay -S libreoffice-fresh libreoffice-fresh-fr vlc discord gimp obs-studio gnome-disk-utility visual-studio-code-bin openrgb-bin spotify
```

---

#### Logiciels KDE

Voici divers logiciels pour graphisme, vidéo (édition, support de codec), utilitaires d'interface graphique, etc.

```sh
sudo pacman -S --needed xdg-desktop-portal-kde okular print-manager kdenlive gwenview spectacle partitionmanager ffmpegthumbs qt6-wayland kdeplasma-addons powerdevil kcalc plasma-systemmonitor qt6-multimedia qt6-multimedia-gstreamer qt6-multimedia-ffmpeg kwalletmanager
```

--- 

<br>

#### Pare-feu <a name="pare-feu"></a>
La configuration par défaut peut bloquer l'accès aux imprimantes et autres appareils sur votre réseau local.
Voici un petit lien pour vous aider : https://www.dsfc.net/infra/securite/configurer-firewalld/

```sh
sudo pacman -S --needed --noconfirm firewalld python-pyqt5 python-capng
sudo systemctl enable --now firewalld.service
firewall-applet &
```

--- 

<br>

#### Reflector pour la mise à jour automatique des miroirs <a name="reflector-pour-la-mise-à-jour-automatique-des-miroirs"></a>

```sh
yay -S reflector-simple
```

Une commande pour générer une liste de miroirs, à faire une fois après la première installation et à répéter si vous voyagez, ou changez de pays, ou si vous trouvez le téléchargement des paquets trop lent, ou si vous rencontrez une erreur vous indiquant qu'un miroir est hors service :

```sh
sudo reflector --verbose --score 100 --latest 20 --fastest 5 --sort rate --save /etc/pacman.d/mirrorlist
```

--- 

<br>

#### Arch Update <a name="arch-update"></a>

[Arch-Update  : Notifie les updates de Arch et aide aux tâches importantes avant et après l'update.](https://youtu.be/QkOkX70SEmo?si=EwB-rSTV5dMNbv5D)

- [arch-update](https://github.com/Antiz96/arch-update)

Arch Update est un notificateur/aplicateur de mise à jour pour Arch Linux qui vous assiste dans les tâches importantes avant et après la mise à jour et qui inclut une icône cliquable (.desktop) pouvant être facilement intégrée à n'importe quel environnement de bureau/gestionnaire de fenêtres, dock, barre de statut/lançage ou menu d'application.
Support optionnel pour les mises à jour des paquets AUR/Flatpak et les notifications de bureau.

```sh
yay -S arch-update
systemctl --user enable --now arch-update.timer
systemctl --user enable --now arch-update-tray.service
```

--- 

<br>

#### Timeshift <a name="timeshift"></a>

- [Timeshift](https://github.com/linuxmint/timeshift) est un utilitaire Linux open source pour créer des sauvegardes de tout votre système.

**/!\ ATTENTION : par défaut, c'est uniquement le système qui est sauvegardé, pas votre dossier utilisateur (le /home/) ! /!\\**

```sh
sudo pacman -S timeshift
```

- Évitez timeshift et btrfs sur Arch, J’ai déjà eu de la [casse](https://github.com/linuxmint/timeshift).

*“BTRFS snapshots are supported only on BTRFS systems having an Ubuntu-type subvolume layout ”*

- Pour bénéficier des sauvegardes automatiques, vous aurez besoin de démarrer cronie. (facultatif) 

```sh
sudo systemctl enable --now cronie
```

--- 

<br>

### Grub BTRFS <a name="grub-btrfs"></a>

[Setup Grub BTRFS sur Arch Linux.](https://youtu.be/EyhSUBwjPUY?si=cQ0TuGI76_M1TzTp)

Permet de prendre des snapshots à chaque update et de booter dessus à partir de grub.

Déjà comme son nom l'indique il faut avoir choisi grub comme bootloader et btrfs en file system.

On installe Timeshift comme vu à l'étape précedante. puis,

```sh
yay -S timeshift-autosnap grub-btrfs
```

On active le service brtfsd

```sh
sudo systemctl enable --now grub-btrfsd
```

On édite le service : 

```sh
sudo systemctl edit --full grub-btrfsd
```

On remplace `ExecStart=/usr/bin/grub-btrfsd --syslog /.snapshots` par `ExecStart=/usr/bin/grub-btrfsd --syslog --timeshift-auto`

crtl + x pour sauvegarder.

Enfin on lance une fois Timeshift, je conseille de laisser tout par défaut.

--- 

<br>

#### Fish <a name="fish"></a>

[Fish](https://fishshell.com/) est un shell en ligne de commande conçu pour être interactif et convivial. Voir aussi [ArchWiki](https://wiki.archlinux.org/title/fish) sur le sujet. Il remplace le shell par défaut, bash.

- Installez fish.
    
```sh
sudo pacman -S fish                             # 1. installez fish
chsh -s /usr/bin/fish                           # 2. Définissez-le par défaut.
fish                                            # 3. Lancez fish ou redémarrez et il sera par défaut.
fish_update_completions                         # 4. Mettez à jour les complétions.
set -U fish_greeting                            # 5. Supprimez le message de bienvenue.
sudo nano ~/.config/fish/config.fish            # 6. Créez un alias comme pour bash au début de ce tutoriel.
```
- Ajoutez ensuite les alias suivants entre if et end :

pour yay :

```sh
alias update-arch='yay && flatpak update' --save
```

```sh
alias clean-arch='yay -Sc && yay -Yc && flatpak remove --unused' --save
```

pour Paru :

```sh
alias update-arch='paru && flatpak update' --save
```

```sh
alias clean-arch='paru -Sc && paru -c && flatpak remove --unused' --save
```

pour tous : 

```sh
alias update-mirrors='sudo reflector --verbose --score 100 --latest 20 --fastest 5 --sort rate --save /etc/pacman.d/mirrorlist' --save
```

```sh
alias fix-key='sudo rm /var/lib/pacman/sync/* && sudo rm -rf /etc/pacman.d/gnupg/* && sudo pacman-key --init && sudo pacman-key --populate && sudo pacman -Sy --noconfirm archlinux-keyring && sudo pacman --noconfirm -Su' --save
```

- ***Redémarrez sauf si fait à l'étape 3***, les alias de tout type ne fonctionnent qu'après le redémarrage du terminal.

## **Améliorez votre Expérience de Jeu** <a name="améliorez-votre-expérience-de-jeu"></a>

<br>

### Steam <a name="steam"></a>
Notez que les pilotes AMD ou Nvidia doivent être installés au préalable comme mentionné dans la section [SUPPORT MATÉRIEL](#HARDWARE-SUPPORT).

```sh
sudo pacman -S steam
```
**[Steam](/Gaming-Linux-FR/steam-post-install)** : Guide de post-installation pour Steam

--- 

<br>

### Lutris <a name="lutris"></a>

Lutris est un gestionnaire de jeux FOSS (Free, Open Source) pour les systèmes d'exploitation basés sur Linux.
Lutris permet de rechercher un jeu ou une plateforme (Ubisoft Connect, EA Store, GOG, Battlenet, etc.) et propose un script d'installation qui configurera ce qui est nécessaire pour que votre choix fonctionne avec Wine ou Proton.

```sh
sudo pacman -S lutris wine-staging
```

Vidéo supplémentaire :

### Support avancé de manettes <a name="support-avancé-de-manettes"></a>

Pilote Linux avancé pour manettes sans fil Xbox 360|One|S|X ([xpadneo](https://github.com/atar-axis/xpadneo)) ([xone](https://github.com/medusalix/xone))

```sh
yay -S xpadneo-dkms-git xone-dkms-git
```

Pilote Linux avancé pour manettes PS5

```sh
yay -S dualsensectl-git
```

--- 

<br>

### Affichage des performances en jeu <a name="affichage-des-performances-en-jeu"></a>

[MangoHud](https://wiki.archlinux.org/title/MangoHud) est une superposition Vulkan et OpenGL qui permet de surveiller les performances du système dans les applications et d'enregistrer des métriques pour le benchmarking.
C'est l'outil dont vous avez besoin si vous voulez voir vos FPS en jeu, votre charge CPU ou GPU, etc. Ou même enregistrer ces résultats dans un fichier.
Ici, nous installons GOverlay qui est une interface graphique pour configurer MangoHud.

```sh
sudo pacman -S goverlay
```

Ajoutez `lib32-mangihud` si vous jouez à de vieux jeux 32bit.

--- 

<br>

## **BONUS** : <a name="optimisation"></a>

<br>

### Tutoriel : Configuration du Multiboot avec grub <a name="tutoriel--configuration-du-multiboot-avec-grub"></a>

#### Introduction

Le multiboot est un moyen de démarrer plusieurs systèmes d'exploitation sur un même ordinateur. Dans ce tutoriel, nous allons utiliser GRUB, le gestionnaire de démarrage standard pour de nombreuses distributions Linux, pour configurer un multiboot.

1. **Modifier la Configuration de GRUB** :

   Ouvrez un terminal et exécutez la commande suivante pour ouvrir le fichier de configuration de GRUB :
   ```bash
   sudo nano /etc/default/grub
   ```

   Recherchez la ligne contenant `# GRUB_DISABLE_OS_PROBER=false` et supprimez le caractère `#` au début de la ligne pour activer la détection automatique d'autres systèmes d'exploitation.

   Si vous voulez également que votre grub mémorise le dernier OS lancé, remplacez la ligne `GRUB_DEFAULT=0` par `GRUB_DEFAULT=saved` et ajoutez `GRUB_SAVEDEFAULT="true"`

   Enregistrez les modifications et quittez l'éditeur de texte.

2. **Installer `os-prober`** :

   Utilisez votre gestionnaire de paquets pour installer `os-prober`, un utilitaire qui permet à GRUB de détecter d'autres systèmes d'exploitation :
   ```bash
   sudo pacman -S os-prober
   ```

3. **Exécuter `os-prober`** :

   Exécutez `os-prober` pour rechercher d'autres systèmes d'exploitation installés sur votre ordinateur :
   ```bash
   sudo os-prober
   ```

5. **Générer la Configuration de GRUB** :

   Utilisez la commande suivante pour générer la configuration de GRUB basée sur les résultats de `os-prober` :
   ```bash
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

#### Conclusion

Une fois ces étapes terminées, redémarrez votre système pour appliquer les modifications. Vous devriez maintenant voir une option pour chaque système d'exploitation détecté lors du démarrage de votre ordinateur.

--- 

<br>

### Zram

Permet de compresser une partie de la ram.

```sh
sudo pacman -S zram-generator
echo -e '[zram0]\nzram-size = ram / 4\ncompression-algorithm = zstd\nswap-priority = 100\nfs-type = swap' | sudo tee -a /etc/systemd/zram-generator.conf
```

Ce qui donne dans le fichier zram-generator.conf :

```
[zram0]
zram-size = ram / 4
compression-algorithm = zstd
swap-priority = 100
fs-type = swap
```

Cette configuration indique que :

- Un dispositif Zram (`zram0`) est créé.
- La taille de la zone de swap Zram est définie à un quart de la RAM totale (`ram / 4`). Garder la partition Zram petite est intentionnel, car la compression introduit un surcoût. Pour les systèmes orientés jeux, il est crucial de minimiser ce surcoût pour maintenir une haute performance. En limitant la quantité de RAM utilisée pour Zram, plus de RAM physique est disponible pour les applications de jeux, ce qui peut être particulièrement bénéfique pour les systèmes à mémoire limitée.
- L'`algorithme de compression` est défini sur `zstd` (Zstandard), connu pour son équilibre entre le taux de compression et la vitesse. Zstandard offre une compression efficace, aidant à économiser de l'espace RAM sans impacter significativement la performance.
- La `priorité de swap` est définie à `100`, indiquant la priorité de cette zone de swap par rapport aux autres. Une priorité plus élevée aide à garantir que cet espace de swap est utilisé de manière préférentielle.

--- 

<br>

## Dépannage <a name="dépannage"></a>

- **[GLF-Astuces](/Gaming-Linux-FR/glf-astuces/)** : Astuces diverses, ne concernant pas une distribution spécifique.

- Pour de l'aide, visitez le Discord GLF : [Discord GLF](http://discord.gg/EP3Jm8YMvj)

--- 

<br>


## Sources <a name="communauté-et-sources"></a>

Sources et liens utiles :
- [ArchWiki](https://wiki.archlinux.org/)
- [Fonctionnement du WIKI d'Arch.](https://www.youtube.com/watch?v=TQ3A9l2N5lI)
- [Site GLF](https://www.gaminglinux.fr/)
- [Discord GLF](http://discord.gg/EP3Jm8YMvj)
- [Ma chaîne Youtube](https://www.youtube.com/@Cardiacman)

--- 

<br>

## Remerciements <a name="remerciements"></a>

Un grand merci à l'équipe d'Arch Linux, à la communauté Arch Linux, aux mainteneurs AUR, aux contributeurs et développeurs des paquets utilisés dans ce projet. Merci à toute la communauté du Discord GLF pour leurs tests et retours.
