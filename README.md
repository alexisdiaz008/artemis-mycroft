# artemis-mycroft
prebuilt precise model

Files contained within are for use as a custom wake word for mycroft using precise as the listener,
the following are directions for a custom setup using this in tandem with a raspi with a 7 inch touchscreen running on raspbian and MATE,
 my wake screen skill, react mycroft gui, mycroft, and precise:


Raspi Setup
----------------------------------

    Download and Flash Raspbian Buster Lite Image

        https://www.raspberrypi.org/downloads/raspbian/

    Power on pi and log in with user name “pi” and password “raspberry”

    Configure localization settings

        sudo raspi-config

    Update system

        sudo apt update && sudo apt upgrade

    Decide on desktop installation

    Install MATE Desktop

        sudo apt install xorg lightdm lightdm-gtk-greeter mate-desktop-environment blueman network-manager-gnome pulseaudio pulseaudio-module-bluetooth pavucontrol bluez-firmware pi-package rc-gui mate-tweak lightdm-gtk-greeter-settings samba menulibre

    (Optional) Additional software

        sudo apt install libreoffice chromium-browser rpi-chromium-mods vlc kodi omxplayer swell-foop minecraft-pi wireshark remmina ttf-mscorefonts-installer fonts-ubuntu human-icon-theme xrdp gparted mate-desktop-environment-extras arc-theme numix-gtk-theme gimp

    Network Manager Fix

        sudo apt install network-manager network-manager-gnome openvpn openvpn-systemd-resolved network-manager-openvpn network-manager-openvpn-gnome

        sudo apt purge openresolv dhcpcd5

        Reboot

    Fix Policy Kit for applications that need elevated privileges

        sudo nano /etc/polkit-1/localauthority.conf.d/60-desktop-policy.conf

        Add the following to the empty file and save

            [Configuration]

            AdminIdentities=unix-user:pi;unix-user:0


Mycroft And Supporting Software Setup
----------------------------------

sudo apt install firefox-esr (install firefox for later use by react gui)

ssh-keygen -t rsa -b 4096 -C "alexisdiaz008@gmail.com"

cat ~/.ssh/id_rsa.pub

cd Documents

mkdir mycroft

cd mycroft

git clone git@github.com:alexisdiaz008/react-mycroft-gui.git

sudo apt-get install git npm

npm start (test react mycroft gui)

git clone git@github.com:MycroftAI/mycroft-precise.git

cd mycroft-precise

git clone git@github.com:alexisdiaz008/artemis-mycroft.git

git checkout master 



cd ..

git clone git@github.com:MycroftAI/mycroft-core.git

cd mycroft-core

bash dev_setup.sh

cd skills

git clone git@github.com:alexisdiaz008/screen-wake-on-wake-word-skill.git


Config for mycroft.conf (location: /home/alexisdiaz/.mycroft/mycroft.conf DEPRECATED)

{
  "max_allowed_core_version": 21.2,
  "listener": {
    "wake_word": "artemis"
  },
  "hotwords": {
    "artemis": {
      "module": "precise",
      "local_model_file": "/home/alexisdiaz/Documents/projects/artemis-mycroft/mycroft-precise/artemis-mycroft/artemis.pb",
      "sensitivity": 0.5
    }
  },
  "precise": {
    "dist_url": "https://github.com/MycroftAI/precise-data/raw/dist/{arch}/latest",
    "model_url": "https://raw.githubusercontent.com/MycroftAI/precise-data/models-dev/{wake_word}.tar.gz"
  }
}
