# Running Minecraft on ARM Linux
There are several methods which can be used to run Minecraft on arm-based devices.

I will not list all of them here but any additions are welcome.

The installation process is currently only viable for debian/ubuntu based distributions but can be adapted to other operating systems if you know how to install java manually.

# Method 1: The legacy launcher

**Background:**

The old Minecraft launcher can be used for many compatiblity-related scenarios.
For example, it can run on Windows 98,2000,XP and of course on linux.
This is the case because it does not need any native binaries to function.
It is just one simple jar file that can be run via the command line.
The game itself of course needs some of native libraries to work.
But that is not a big problem because those libraries are available for armhf and arm64.

**Installation:**

Step 1:

Install openjdk-11-jre via apt: `sudo apt update && sudo apt install -y openjdk-11-jre`

Step 2:

*If you are not using a raspberry pi, you can skip this step*

use your favourite text editor to edit /boot/config.txt, go to the bottom of the file and add the following line: `dtoverlay=vc4-fkms-v3d`

Step 3:

Download the launcher and the libraries:
```
cd ~
mkdir Minecraft
cd Minecraft
wget https://github.com/mclaunch/mclaunch/releases/download/v1.0.2/mclaunch.jar

cd ~/.local/share/applications/
echo "[Desktop Entry]
Version=1.0
Type=Application
Name=Minecraft Launcher
Comment=3D block based sandbox game
Icon=$(dirname "$0")/icon-64.png
Exec=env MESA_GL_VERSION_OVERRIDE=3.3 java -jar ~/Minecraft/mclaunch.jar
Categories=Game;" > minecraft.desktop
chmod +x minecraft.desktop

# Next step is for 32 bit arm:
wget https://github.com/chunky-milk/Minecraft/raw/main/lwjgl3arm32.tar.gz
wget https://github.com/chunky-milk/Minecraft/raw/main/lwjgl2arm32.tar.gz
tar -zxf lwjgl3arm32.tar.gz -C ~/lwjgl3arm32
tar -zxf lwjgl2arm32.tar.gz -C ~/lwjgl2arm32
mkdir -p ~/.mclaunch
echo -e '{
  "profiles": {
    "(Default)": {
      "name": "(Default)",
      "javaDir": "java",
      "javaArgs": "-Dorg.lwjgl.librarypath='$HOME'/lwjgl3arm32 -Xmx1G -XX:-UseAdaptiveSizePolicy -Xmn128M"
    }
  },
  "selectedProfile": "(Default)",
  "clientToken": "c4a6f915-4d47-47bf-a8f4-746090e7e576",
  "authenticationDatabase": {},
  "launcherVersion": {
    "name": "1.6.93",
    "format": 21,
    "profilesFormat": 1
  }
}' > $HOME/.mclaunch/launcher_profiles.json

# This step is for 64 bit arm:
wget https://github.com/chunky-milk/Minecraft/raw/main/lwjgl3arm64.tar.gz
tar -zxf lwjgl3arm64.tar.gz -C ~/lwjgl3arm64
mkdir -p ~/.mclaunch
echo -e '{
  "profiles": {
    "(Default)": {
      "name": "(Default)",
      "javaDir": "java",
      "javaArgs": "-Dorg.lwjgl.librarypath='$HOME'/lwjgl3arm64 -Xmx2G -XX:-UseAdaptiveSizePolicy -Xmn128M"
    }
  },
  "selectedProfile": "(Default)",
  "clientToken": "c4a6f915-4d47-47bf-a8f4-746090e7e576",
  "authenticationDatabase": {},
  "launcherVersion": {
    "name": "1.6.93",
    "format": 21,
    "profilesFormat": 1
  }
}' > $HOME/.mclaunch/launcher_profiles.json
```

Step 4:

Launch the game by going to Start Menu -> Games -> Minecraft Launcher.

**Note: This will only work for Minecraft 1.13-1.16.5 on default. 1.17 is not yet supported. For versions below 1.13 on 32 bit arm, you need to edit the java launch arguments to be `-Dorg.lwjgl.librarypath=~/lwjgl2arm32 -Xmx1G -XX:-UseAdaptiveSizePolicy -Xmn128M`.**

# Method 2: The shady forum post

On the raspberry pi forums is a post of someone who managed to make minecraft 1.8.9 work using a shady lwjgl port from unkown source.

The post can be found [here.](https://www.raspberrypi.org/forums/viewtopic.php?f=78&t=137279)

There have been many speculations where these two (https://www.dropbox.com/s/4oxcvz3ky7a3x6f/liblwjgl.so https://www.dropbox.com/s/m0r8e01jg2og36z/libopenal.so) files came from but none of them are confirmed by this time.
