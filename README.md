# My Guide to Linux as Desktop
This guide is written solely for myself, and might or might not even be published. Linux is awesome, however usually it often requires a lot of tweaking to get it perfect. Therefore I've written down some of the most common tweaks and fixes I do personally so I can more easily remember them, and not have to look up the tweaks/fixes online again.



# Table of Contents
- [Installation](#installation)
- [Display Settings](#display-settings)
- [Desktop Environment](#desktop-environment)
- [Miscellaneous Tweaks](#miscellaneous-tweaks)
    - [GNU GRUB](#gnu-grub)
    - [Terminal](#terminal)
    - [PulseAudio](#pulseaudio)
    - [IMWheel](#imwheel)
- [Software & Games](#software)
    - [Google Chrome](#google-chrome)
    - [Visual Studio Code](#visual-studio-code)
    - [MATLAB](#matlab)
    - [Discord](#discord)
    - [TeamSpeak](#teamspeak)
    - [Deluge](#deluge)
    - [Piper](#piper)
    - [ckb-next](#ckb-next)
    - [Heroes of Newerth](#heroes-of-newerth)
    - [Steam](#steam)
        - [Counter-Strike: Global Offensive](#counter-strike-global-offensive)
        - [Dota 2](#dota-2)
        - [Rocket League](#rocket-league)
    - [Minecraft](#minecraft)



# Installation
...



# Display Settings
First time booting up, the display settings need to be configured. Most of the settings will be preselected correctly already but it is a good idea to verify that is the case.

If you're already logged in, then logout. You will now be on the login screen, select your user and at the bottom right corner a cogwheel will appear. Click it, and select the display server of your choice. It is worth noting as of writing this, **Wayland** is not supported by **NVIDIA** GPU's.
- **Ubuntu** (Xorg)
- **Ubuntu on Wayland** (Wayland)

Then proceed to login. Open the display settings, and configure to your setup. Make sure to configure **Display Order**, **Primary Display**, **Resolution**, and **Refresh Rate**. Once all that is configured, if you have multiple monitors and the primary screen wasn't already correctly selected it is likely login screen is still displayed on the wrong monitor. Therefore I highly recommend running the command below.
```
sudo cp ~/.config/monitors.xml ~gdm/.config/monitors.xml
```

Finally, I recommend doing a reboot at this point and verify everything works, and stays are configured.



# Desktop Environment
I am not a fan of the GNOME layout of a dock to the left. I much more prefer the windows layout and this will mostly cover how to make GNOME look like that.

Install **GNOME Tweak Tool** and **Dash to Panel** with the commands below.
```
sudo apt install gnome-tweak-tool
sudo apt install gnome-shell-extension-dash-to-panel
```

Thereafter open **GNOME Tweak Tool** from the applications menu. Navigate to **Extensions** and enable **Dash to Panel** and **Ubuntu AppIndicators**.

Configure **Dash to Panel** by clicking the cogwheel next to enable slider. You may now want to adjust the panel size, found under **Style**. I recommend **32 px** for desktop and **48 px** for laptop. More importantly however, navigate to **Action** and enable **Use Hotkeys to Activate Apps**. Configure this further by clicking the cogwheel and change **Number Overlay** to **Never**. You are not done here and may close everything until the **GNOME Tweak Tool**.

Under **Keyboard & Mouse** I highly recommend disabling **Middle Click Paste**

Under **Top Bar** you may want to enable **Battery Percentage** if you're on a laptop. You may also want to enable **Weekday** and **Week Numbers**.

Finally, under **Workspaces** disable them by selecting **Static Workspaces** and decrease it to **1**.

https://extensions.gnome.org/extension/1317/


# Miscellaneous Tweaks

## GNU GRUB
Install using the command below.
```
sudo apt install grub-customizer
```

Open it, and customize! I like to have 3 entries under **List Configuration**. They are **Ubuntu**, **Windows Boot Manager** (Usually rename this to *Windows 10*), and **Bios Setup**.

I also like to customize the settings under **General Settings**. **Default Entry** I set to **Previously Booted Entry** and I set **Boot Default Entry After** to **4** seconds.

Save, and you're all done here!

## Terminal
There is three things I like to configure with the terminal. First and formost I don't like writing my password everytime I sudo. Therefore run the command below.
```
sudo visudo
```

Add at the bottom the following, and replace `<user>` with your username. This will take effect once you restart the terminal.
```
# Allow sudo w/o password for <user>
<user> ALL=(ALL) NOPASSWD: ALL
```

Now, let's fix copy & paste. Open the preferences and under **Shortcuts**, **Edit**, change the **Copy** and **Paste** command to **CTRL+C** and **CTRL+V** as they should be. Unfortuently however, this will prevent us from sending the interrupt signal. Therefore, run the command below which will allow us to sent interrupt command using **CTRL+K**. The terminal requires a restart before the changes take effect.
```
echo $'# Make CTRL+K send interrupt command in terminal\nstty intr \^k\n' >> ~/.bashrc
```



## PulseAudio
The sound settings can be configured in ubuntu under **Sound** in the **Settings**. However they are quite limited and therefore I like to install **PulseAudio Volume Control** with the provided command below.
```
sudo apt install pavucontrol
```

Also on some of my desktops, before and after sound plays, there's a popping/clicking sound in ths speakers. This is most likely due to the sound drivers going in and out of standby mode. To avoid this, run the command below, solution from [launchpad bug #1825754](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1825754/comments/15).
```
sudo bash -c "echo $'# Prevent High Definition Audio Drivers going into standby\noptions snd-hda-intel power_save=0' >> /etc/modprobe.d/alsa-info.conf"
```



## IMWheel
For whatever reason scrolling in Linux, or at least in Ubuntu is slow. Sadly there's not scroll speed adjustment, built in anyway. However there is quite a hack with **IMWheel**. You can install with the command below.
```
sudo apt install imwheel
```

Thereafter run [imwheel_configure.sh](imwheel_configure.sh) and choose a setting. You can adjust and play around until you find a speed that suits you. Personally I use **2**.

Finally, you have to start imwheel everytime you boot. Open **Startup Application Preferences** and add an entry simply running the `imwheel` command.

> **Nota bene!**
> 
> Sometimes there's a bug where scrolling stops working entirely.
> 
> Run `imwheel --kill` to resolve it.



# Software
In this section I will mention a ton of software I like to use and potential configurations that I recommend.

Many of these installations involve `.deb` files. In order to install those we need to get comfortable with the `dpkg` command. To install a `.deb` file, run the command below.
```
sudo dpkg -i <file>
```

If it gives you any dependencies errors, follow up by running the additional command below. This will retrieve the missing dependencies, and finalize the broken isntallation. 
```
sudo apt -f install
```

## Google Chrome
The best browser for me is by far Google Chrome **Google Chrome**. Download from https://www.google.com/chrome/ and install using `sudo dpkg -i <file>`.

First time launching it, it will ask to set it as default which I definetly want. There is also a few settings and extensions that I like to configure.

In the settings, for **On Startup** I like to use **Continue where you left off**. I also like to get all my downloads to the desktop, thus setting **Downloads Location** to `/home/<user>/Desktop`.

For extensions, I recommend using [Adblock Plus](https://chrome.google.com/webstore/detail/cfhdojbkjhnklbpkdaibdccddilifddb). Once installed, configure it by opening it's options. Under **General** disable **Allow Acceptable Ads** and under **Advanced** hit the **Update All Filter Lists** for the best experience.


## Visual Studio Code
Download from https://code.visualstudio.com/download and install.

## MATLAB
Download from https://se.mathworks.com/downloads/ and install. During installation it will ask for which toolboxes you'd like. Get the following,
- Control System Toolbox
- DSP System Toolbox
- Embedded coder
- Image Processing Toolbox
- Matlab coder
- Optimizaiton Toolbox
- Robotics System Toolbox
- Robust Control Toolbox
- Siganl Processing Toolbox
- Simscape (all versions of simscape)
- Simulink
- Simulink Coder
- Symbolic Math
- System Identification Toolbox

Finally, create a ubuntu launcher for matlab,
```
sudo gedit ~/.local/share/applications/matlab.desktop
```

```
[Desktop Entry]
Name=MATLAB R2020b
Type=Application
Exec=/usr/local/MATLAB/R2020b/bin/matlab -desktop
Terminal=false
Icon=/usr/local/MATLAB/R2020b/bin/glnxa64/cef_resources/matlab_icon.png
Comment=MATLAB
Categories=Development;Math;Science;Education;IDE;
```

## Discord
Download from https://discord.com/download and install.



## TeamSpeak
## Deluge
## Piper
## ckb-next
## Heroes of Newerth
## Steam
Install steam, login.

Settings...
In-Game > In-game FPS counter -> Top-Left High contrast pls
**Interface** untick **Notify me about additions or changes to my games, new releases, and upcoming releases**.

**Steam Play** -> **Enable Steam Play for all other titles**

### Counter-Strike: Global Offensive
sudo apt install python3 python3-pip xdotool
pip install pyxhook

```
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor && echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor && cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```



```
SDL_VIDEO_MINIMIZE_ON_FOCUS_LOSS=0 %command% -fullscreen -novid -high -threads 4 +cl_forcepreload 1 -tickrate 128 -nojoy
```
### Dota 2
### Rocket League


## Minecraft
multi mc


