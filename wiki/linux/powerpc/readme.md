# Running Minecraft on PowerPC(64) Linux
There are currently two working launchers for this.
Both are optimised for use under [Lubuntu Remix PPC](https://forums.macrumors.com/threads/lubuntu-16-04-remix-updated.2204742/) or [Debian Bullseye](https://macintoshgarden.org/apps/debian-bullseye-ppc-installer).

The minumum system requirements are:
```
CPU: G3, G4, G5 or compatible big endian IBM Power CPU
RAM: 512 MB RAM
GPU: OpenGL 1.2 or higher
```

# Method 1: The legacy launcher

**Background:**

The old Minecraft launcher can be used for many compatiblity-related scenarios.
For example, it can run on Windows 98,2000,XP and of course on linux.
This is the case because it does not need any native binaries to function.
It is just one simple jar file that can be run via the command line.
The game itself of course needs some of native libraries to work.
But that is not a big problem because those libraries are available for powerpc and ppc64.
There is no working audio support in the ppc version at the moment as I have not implemented it yet.

**Installation:**

**Step 1:** Installing Java (32 bit):

### Use this if your device has a G3, G4 or a 32 bit IBM Power CPU:
All Java versions included in the 32 bit debian repository currently do not support JIT meaning the game would not be playable.
To get around this, we will use an old build of IBM Java instead.

Download IBM java 6: `cd /tmp/ && wget https://archive.org/download/ibm-j2sdk1.6_1.6.0_powerpc_202109/ibm-j2sdk1.6_1.6.0_powerpc.deb && sudo apt update`

Install Java: `sudo apt install ./ibm-j2sdk1.6_1.6.0_powerpc.deb`

If you get errors of missing .so files during the previous command, run `sudo touch [path of missing .so]` and try the command again.

If the touch command gives a *No such file or directory* error, try to `sudo mkdir` all folders in the missing path and then try touch again.

This process may need to be repeated up to 3 times.

These errors are not critical to operation and known as a wont-fix.

### Use this if your device has a G5 or a 64 bit IBM Power CPU:
On ppc64 debian the official Java releases have JIT support meaning that the install is as simple as running `sudo apt update && sudo apt install openjdk-8-jre-headless`

**Step 2:** Installing the game:

### Use this if your device has a G3, G4 or a 32 bit IBM Power CPU:
```
cd ~
mkdir Minecraft
cd Minecraft
wget https://vlauncher.net/index.php?do=download&id=118

cd ~/.local/share/applications/
echo "[Desktop Entry]
Version=1.0
Type=Application
Name=Minecraft4PPC
Comment=3D block based sandbox game
Icon=$(dirname "$0")/icon-64.png
Exec=java -jar ~/Minecraft/minecraft.jar
Categories=Game;" > minecraft.desktop
chmod +x minecraft.desktop
cd ~

mkdir -p ~/lwjgl
cd ~/lwjgl
wget https://github.com/iAmInActions/multiplatformcraft-meta/blob/main/natives/ppc/2/liblwjgl.so
cd ..
mkdir -p ~/.minecraft
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
}' > $HOME/.minecraft/launcher_profiles.json
```

### Use this if your device has a G5 or a 64 bit IBM Power CPU:
```
cd ~
mkdir Minecraft
cd Minecraft
wget https://vlauncher.net/index.php?do=download&id=118

cd ~/.local/share/applications/
echo "[Desktop Entry]
Version=1.0
Type=Application
Name=Minecraft4PPC
Comment=3D block based sandbox game
Icon=$(dirname "$0")/icon-64.png
Exec=java -jar ~/Minecraft/minecraft.jar
Categories=Game;" > minecraft.desktop
chmod +x minecraft.desktop
cd ~

mkdir -p ~/lwjgl
cd ~/lwjgl
wget
cd ..
mkdir -p ~/.minecraft
echo -e '{
  "profiles": {
    "(Default)": {
      "name": "(Default)",
      "javaDir": "java",
      "javaArgs": "-Dorg.lwjgl.librarypath='$HOME'/lwjgl -Xmx2G -XX:-UseAdaptiveSizePolicy -Xmn128M"
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
}' > $HOME/.minecraft/launcher_profiles.json
```

**Step 3:** Profit:

Launch the game by going to Start Menu -> Games -> Minecraft Launcher.

**Note: This will only work for Minecraft versions up to 1.12.2 on 32 bit clients and versions newer than 1.14 on 64 bit.**
