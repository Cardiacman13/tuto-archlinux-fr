# 🐧 ARCHITECT TUTO 📜

---

## **Table des Matières avec Ancres Markdown**

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
   - [Amélioration de la compatibilité des jeux Windows](#amélioration-compatibilité)

5. **[Optimisation](#optimisation)**
   - [Kernel TKG](#kernel-tkg)
   - [MESA-TKG](#mesa-tkg)
   - [NVIDIA-ALL](#nvidia-all)
   - [Installation Flatpak](#installation-flatpak)
   - [Tutoriel : Configuration du Multiboot avec grub](#tutoriel--configuration-du-multiboot-avec-grub)

6. **[Dépannage](#dépannage)**
   
7. **[Communauté et Sources](#communauté-et-sources)**

8. **[Remerciements](#remerciements)**

---

#### Installation <a name="installation"/>

> [!IMPORTANT]
> Suivez les étapes avec minutie

<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [ Tutoriel Arch Linux Partie 1 : Archinstall ](https://www.youtube.com/watch?v=JE6VwNHLcyk)

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

#### 3. Archinstall <a name="utilisation-darchinstall"></a>

```sh
archinstall                 # pour lancer le script d'aide à l'installation pour arch linux
```

**/!\ Le menu archinstall est sujet à changement avec les mises à jour du script /!\\**

---

### Post-installation <a name="post-installation"></a>

<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [ Tutoriel Arch Linux Partie 2 : Post Installation ](https://youtu.be/FEFhC46BkXo?si=Gi-6BOhqENLoh5Ak)

---

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

#### 3. Alias de maintenance : <a name="alias-de-maintenance"></a>

<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [ Tutoriel Arch Linux Partie 4 : Maintenance ](https://www.youtube.com/watch?v=Z7POSK2jnII)

Cette modification vous permet de simplement taper « update-arch » dans un terminal pour mettre à jour le système, « clean-arch » pour le nettoyer, ou « fix-key » en cas d'erreur avec les clés gpg.

```sh
nano ~/.bashrc
```
  
Ajoutez chacune de ces lignes à la fin du fichier :

pour yay :

```sh
alias update-arch='yay && flatpak update'
```

```sh
alias clean-arch='yay -Sc && yay -Yc && flatpak remove --unused'
```

pour Paru :

```sh
alias update-arch='paru && flatpak update'
```

```sh
alias clean-arch='paru -Sc && paru -c && flatpak remove --unused'
```

Pour tous : 

```sh
alias update-mirrors='sudo reflector --verbose --score 100 --latest 20 --fastest 5 --sort rate --save /etc/pacman.d/mirrorlist '
```

```sh
alias fix-key='sudo rm /var/lib/pacman/sync/* && sudo rm -rf /etc/pacman.d/gnupg/* && sudo pacman-key --init && sudo pacman-key --populate && sudo pacman -Sy --noconfirm archlinux-keyring && sudo pacman --noconfirm -Su'
```
   
Redémarrez le terminal.

---

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

### SUPPORT MATÉRIEL <a name="support-matériel"></a>

---

#### NVIDIA <a name="nvidia"></a>

> [!IMPORTANT]
>  Quel que soit le DE restez sur X11 au moins jusqu'au merge de ce patch : [explicit-sync](https://gitlab.freedesktop.org/xorg/xserver/-/merge_requests/967)

#### 1. Installer les composants de base :

```sh
sudo pacman -S --needed nvidia-dkms nvidia-utils lib32-nvidia-utils nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader
```

Si vous avez un  <img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [PC portable Intel / Nvidia](https://youtu.be/GhsP6btpiiw?si=ibWw_dQdty8_Q0jm) :

```sh
sudo pacman -S --needed intel-media-driver intel-gmmlib onevpl-intel-gpu nvidia-prime
```

#### 3. Activer les services Nvidia :

Services pour gerer la veille et l'hibernation.

```sh
sudo systemctl enable nvidia-suspend.service nvidia-hibernate.service nvidia-resume.service
```

#### 3. Activer nvidia-drm modeset=1 :

Ce paramètre permet de lancer le module Nvidia au démarrage.

```sh
sudo nano /etc/modprobe.d/nvidia.conf
```

Ajouter:

`options nvidia-drm modeset=1`

Sauvegarder.
   
#### 4. Charger les modules Nvidia en priorité au lancement d'Arch :

> [!WARNING]
> Cette étape est destinée aux utilisateurs avancés :star: :
> **Optionnel**, à ne faire que **si on remarque des problèmes au boot.**
> **Augmente drastiquement la taille de l'initramfs empéchant d'installer plus de 1 kernel si on a laissé les options par défaut de archinstall !**

   
```sh
sudo nano /etc/mkinitcpio.conf
```

Modifiez la ligne MODULES=() pour :

```sh
MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

Si utilisation de btrfs :

```sh
MODULES=(btrfs nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

#### 4. Débloquer Wayland Si vous etes sur Gnome:

```sh
sudo ln -s /dev/null /etc/udev/rules.d/61-gdm.rules
```

#### 5. Reconstruction de l'initramfs :

Comme nous avons déjà installé les pilotes à l'étape 1, donc avant de configurer le hook, nous devons déclencher manuellement la reconstruction de l'initramfs :

```sh
sudo mkinitcpio -P
```

---

#### AMD <a name="amd"></a>

Installer les composants de base :

```sh
sudo pacman -S --needed mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon vulkan-icd-loader lib32-vulkan-icd-loader
```

---

#### INTEL <a name="intel"></a>

Installer les composants de base :

```sh
sudo pacman -S --needed mesa lib32-mesa vulkan-intel lib32-vulkan-intel vulkan-icd-loader lib32-vulkan-icd-loader intel-media-driver
```

---

#### Imprimantes <a name="imprimantes"></a>
- Essentiels

```sh
sudo pacman -S --needed ghostscript gsfonts cups cups-filters cups-pdf system-config-printer avahi
sudo systemctl enable --now avahi-daemon cups
```

```sh
sudo systemctl disable systemd-resolved
```

- Pilotes

```sh
sudo pacman -S --needed foomatic-db-engine foomatic-db foomatic-db-ppds foomatic-db-nonfree foomatic-db-nonfree-ppds gutenprint foomatic-db-gutenprint-ppds 
```

- Imprimantes HP

```sh
yay -S --needed python-pyqt5 hplip
```

- Imprimantes Epson

```sh
yay -S --needed epson-inkjet-printer-escpr epson-inkjet-printer-escpr2 epson-inkjet-printer-201601w epson-inkjet-printer-n10-nx127
```

---

#### Bluetooth <a name="bluetooth"></a>

La seconde commande ci-dessous demande à systemd de démarrer immédiatement le service bluetooth, et aussi de l'activer à chaque démarrage.

```sh
yay -S --needed bluez bluez-utils bluez-plugins
sudo systemctl enable --now  bluetooth.service
```

---

#### [PipeWire](https://pipewire.org/)  <a name="pipewire"></a>

**/!\ Dites oui à tout pour écraser tout avec les nouveaux paquets. /!\**

```sh
sudo pacman -S --needed pipewire lib32-pipewire pipewire-pulse pipewire-alsa pipewire-jack wireplumber alsa-utils alsa-firmware alsa-tools sof-firmware
```

---

### LOGICIEL DE BASE <a name="logiciel-de-base"></a>

---

#### Composants de base

Ici, vous trouverez des codecs, utilitaires, polices, pilotes :

```sh
yay -S --needed gstreamer-vaapi gst-plugins-bad gst-plugins-base gst-plugins-ugly gst-plugin-pipewire gstreamer-vaapi gst-plugins-good gst-libav gstreamer downgrade  libva-mesa-driver lib32-libva-mesa-driver mesa-vdpau lib32-mesa-vdpau rebuild-detector xdg-desktop-portal-gtk xdg-desktop-portal neofetch power-profiles-daemon lib32-pipewire hunspell hunspell-fr p7zip unrar ttf-liberation noto-fonts noto-fonts-emoji adobe-source-code-pro-fonts otf-font-awesome ttf-meslo-nerd ttf-droid duf btop  ntfs-3g fuse2fs exfatprogs fuse2 fuse3 bash-completion man-db man-pages
```

---

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

#### Pare-feu <a name="pare-feu"></a>
La configuration par défaut peut bloquer l'accès aux imprimantes et autres appareils sur votre réseau local.
Voici un petit lien pour vous aider : https://www.dsfc.net/infra/securite/configurer-firewalld/

```sh
sudo pacman -S --needed --noconfirm firewalld python-pyqt5 python-capng
sudo systemctl enable --now firewalld.service
firewall-applet &
```

---

#### Reflector pour la mise à jour automatique des miroirs <a name="reflector-pour-la-mise-à-jour-automatique-des-miroirs"></a>

```sh
yay -S reflector-simple
```

Une commande pour générer une liste de miroirs, à faire une fois après la première installation et à répéter si vous voyagez, ou changez de pays, ou si vous trouvez le téléchargement des paquets trop lent, ou si vous rencontrez une erreur vous indiquant qu'un miroir est hors service :

```sh
sudo reflector --verbose --score 100 --latest 20 --fastest 5 --sort rate --save /etc/pacman.d/mirrorlist
```

---

#### Arch Update <a name="arch-update"></a>

<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [Arch-Update  : Notifie les updates de Arch et aide aux tâches importantes avant et après l'update.](https://youtu.be/QkOkX70SEmo?si=EwB-rSTV5dMNbv5D)

- [arch-update](https://github.com/Antiz96/arch-update)

Arch Update est un notificateur/aplicateur de mise à jour pour Arch Linux qui vous assiste dans les tâches importantes avant et après la mise à jour et qui inclut une icône cliquable (.desktop) pouvant être facilement intégrée à n'importe quel environnement de bureau/gestionnaire de fenêtres, dock, barre de statut/lançage ou menu d'application.
Support optionnel pour les mises à jour des paquets AUR/Flatpak et les notifications de bureau.

```sh
yay -S arch-update
systemctl --user enable --now arch-update.timer
```

---

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

### Grub BTRFS <a name="grub-btrfs"></a>

<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [Setup Grub BTRFS sur Arch Linux.](https://youtu.be/EyhSUBwjPUY?si=cQ0TuGI76_M1TzTp)

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
alias update-arch='yay && flatpak update'
```

```sh
alias clean-arch='yay -Sc && yay -Yc && flatpak remove --unused'
```

pour Paru :

```sh
alias update-arch='paru && flatpak update'
```

```sh
alias clean-arch='paru -Sc && paru -c && flatpak remove --unused'
```

pour tous : 

```sh
alias update-mirrors='sudo reflector --verbose --score 100 --latest 20 --fastest 5 --sort rate --save /etc/pacman.d/mirrorlist'
```

```sh
alias fix-key='sudo rm /var/lib/pacman/sync/* && sudo rm -rf /etc/pacman.d/gnupg/* && sudo pacman-key --init && sudo pacman-key --populate && sudo pacman -Sy --noconfirm archlinux-keyring && sudo pacman --noconfirm -Su'
```

- ***Redémarrez sauf si fait à l'étape 3***, les alias de tout type ne fonctionnent qu'après le redémarrage du terminal.

## <img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/game-console.png" width="30" height="30"> **Améliorez votre Expérience de Jeu** <a name="améliorez-votre-expérience-de-jeu"></a>

---

### Steam <a name="steam"></a>
Notez que les pilotes AMD ou Nvidia doivent être installés au préalable comme mentionné dans la section [SUPPORT MATÉRIEL](#HARDWARE-SUPPORT).

```sh
sudo pacman -S steam
```

---

### Lutris <a name="lutris"></a>

Lutris est un gestionnaire de jeux FOSS (Free, Open Source) pour les systèmes d'exploitation basés sur Linux.
Lutris permet de rechercher un jeu ou une plateforme (Ubisoft Connect, EA Store, GOG, Battlenet, etc.) et propose un script d'installation qui configurera ce qui est nécessaire pour que votre choix fonctionne avec Wine ou Proton.

```sh
sudo pacman -S lutris wine-staging
```

Vidéo supplémentaire :

<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [Configuration de Lutris pour ordinateur portable Intel/Nvidia](https://www.youtube.com/watch?v=Am3pgTXiUAA)

---

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

### Affichage des performances en jeu <a name="affichage-des-performances-en-jeu"></a>

[MangoHud](https://wiki.archlinux.org/title/MangoHud) est une superposition Vulkan et OpenGL qui permet de surveiller les performances du système dans les applications et d'enregistrer des métriques pour le benchmarking.
C'est l'outil dont vous avez besoin si vous voulez voir vos FPS en jeu, votre charge CPU ou GPU, etc. Ou même enregistrer ces résultats dans un fichier.
Ici, nous installons GOverlay qui est une interface graphique pour configurer MangoHud.

```sh
sudo pacman -S goverlay
```

---

### Amélioration de la compatibilité des jeux Windows <a name="amélioration-compatibilité"></a>

Nous augmentons la valeur par défaut de cette variable, permettant le stockage de plus de "zones de mappage mémoire". La valeur par défaut est très basse. L'objectif est d'améliorer la compatibilité avec les jeux Windows via Wine ou Steam (Voir [ProtonDB](https://www.protondb.com/)), sachant que certains jeux mal optimisés ont tendance à atteindre rapidement cette limite, ce qui peut entraîner un crash.

<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [Gaming LINUX supprimer les crashs / augmenter la compatibilité](https://youtu.be/sr4RgshrUYY)

- Ajouter dans :

```sh
sudo nano /etc/sysctl.d/99-sysctl.conf
``` 

la ligne suivante:

`
vm.max_map_count=2147483642
`

---

## <img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/speed.png" width="30" height="30"> **BONUS** : <a name="optimisation"></a>

---

### [Kernel TKG](https://github.com/Frogging-Family/linux-tkg) <a name="kernel-tkg"></a>

> Cette étape est destinée aux utilisateurs avancés :star:

[KERNEL TKG](https://github.com/Frogging-Family) est un noyau hautement personnalisable qui fournit une sélection de correctifs et d'ajustements pour améliorer les performances de bureau et de jeu.

Vidéo associée :
<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [Kernel TKG sur Arch + Améliorer ses perfs](https://youtu.be/43yYIWMnDJA)

```sh
git clone https://github.com/Frogging-Family/linux-tkg.git
cd linux-tkg
makepkg -si
```

---

### [MESA-TKG](https://github.com/Frogging-Family/mesa-git) <a name="mesa-tkg"></a>

> [!WARNING]
> Cette étape est destinée aux utilisateurs avancés :star:

Comme le noyau TKG, mais pour Mesa, une version patchée pour ajouter quelques correctifs et optimisations.
Très utile pour les joueurs AMD, sans intérêt pour les joueurs Nvidia.

```sh
git clone https://github.com/Frogging-Family/mesa-git.git
cd mesa-git
makepkg -si
```

Dites oui à tout pour tout écraser avec les nouveaux paquets.

---

### [NVIDIA-ALL](https://github.com/Frogging-Family/nvidia-all) <a name="nvidia-all"></a>

> [!WARNING]
> Cette étape est destinée aux utilisateurs avancés :star:

Nvidia-all est une intégration du pilote nvidia par TkG. Il comprend des patchs de support pour les nouveaux noyaux. Il vous permet de sélectionner la version du pilote que vous souhaitez installer, qu'il s'agisse de la dernière version officielle, d'une version bêta, de la version Vulkan, etc.

<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [Vous utilisez Arch et Nvidia, regardez ça !](https://youtu.be/T0laE8gPtfY)

```sh
git clone https://github.com/Frogging-Family/nvidia-all.git
cd nvidia-all
makepkg -si
```

Dites oui à tout pour tout écraser avec les nouveaux paquets.

---

### Installation [Flatpak](https://wiki.archlinux.org/title/Flatpak) <a name="installation-flatpak"></a>

Anciennement connu sous le nom de xdg-app, il s'agit d'un utilitaire de déploiement de logiciels et de gestion de paquets pour Linux. Il est promu comme offrant un environnement "sandbox" dans lequel les utilisateurs peuvent exécuter des logiciels isolément du reste du système.

<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [MangoHUD, Goverlay, Steam, Lutris FLATPAK!](https://www.youtube.com/watch?v=1dha2UDSF4M)

```sh
sudo pacman -S flatpak flatpak-kcm
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

### Tutoriel : Configuration du Multiboot avec grub <a name="tutoriel--configuration-du-multiboot-avec-grub"></a>

#### Introduction

Le multiboot est un moyen de démarrer plusieurs systèmes d'exploitation sur un même ordinateur. Dans ce tutoriel, nous allons utiliser GRUB, le gestionnaire de démarrage standard pour de nombreuses distributions Linux, pour configurer un multiboot.

1. **Modifier la Configuration de GRUB** :

   Ouvrez un terminal et exécutez la commande suivante pour ouvrir le fichier de configuration de GRUB :
   ```bash
   sudo nano /etc/default/grub
   ```

   Recherchez la ligne contenant `# GRUB_DISABLE_OS_PROBER=false` et supprimez le caractère `#` au début de la ligne pour activer la détection automatique d'autres systèmes d'exploitation.

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

4. **Générer la Configuration de GRUB** :

   Utilisez la commande suivante pour générer la configuration de GRUB basée sur les résultats de `os-prober` :
   ```bash
   sudo grub-mkconfig -o /boot/grub/grub.cfg
   ```

#### Conclusion

Une fois ces étapes terminées, redémarrez votre système pour appliquer les modifications. Vous devriez maintenant voir une option pour chaque système d'exploitation détecté lors du démarrage de votre ordinateur.


---

## Dépannage <a name="dépannage"></a>

- **[GLF-Astuces](https://github.com/Gaming-Linux-FR/glf-astuces/tree/main)** : Astuces diverses, ne concernant pas une distribution spécifique.

<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [Arch Linux Partie 3 les problèmes les plus communs.](https://youtu.be/vbOOQsYyPfc?si=wA2W8bOG1gtpfmnZ)

<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [Arch Linux Partie 4 Maintenance / mise à jour](https://youtu.be/Z7POSK2jnII?si=SNwagGGJXRVkYPdc)
 
<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [Arch Linux Partie 5 Arch-Chroot](https://youtu.be/iandJSjePiA?si=7uI8JZ-VxAVOsPTh)

- Pour de l'aide, visitez le Discord GLF (fr/en) : [Discord GLF](http://discord.gg/EP3Jm8YMvj)

---

## Sources <a name="communauté-et-sources"></a>

Sources et liens utiles :
- [ArchWiki](https://wiki.archlinux.org/)
<img src="https://github.com/Cardiacman13/tuto-archlinux-fr/blob/main/assets/images/Cardiac-icon.png" width="30" height="30"> [Fonctionnement du WIKI d'Arch.](https://www.youtube.com/watch?v=TQ3A9l2N5lI)
- [Site GLF](https://www.gaminglinux.fr/)
- [Discord GLF](http://discord.gg/EP3Jm8YMvj)
- [Ma chaîne Youtube](https://www.youtube.com/@Cardiacman)

---

## 🙏 Remerciements <a name="remerciements"></a>

- L'équipe d'[Arch Linux](https://archlinux.org/) pour leur travail remarquable.
- La communauté Arch Linux pour leur documentation exceptionnelle.
- Les mainteneurs de l'AUR pour leur travail acharné.
- Les développeurs des paquets utilisés dans ce projet. Mention spéciale à :
- [Frogging Family](https://github.com/Frogging-Family)
- [OpenRGB](https://github.com/CalcProgrammer1/OpenRGB)
- Merci au [Discord GLF](https://discord.gg/6t4REDETJd) pour les nombreux tests et retours.
