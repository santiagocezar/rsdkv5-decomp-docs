# Compiling for Linux

- [Compiling Flatpak](#compiling-flatpak)
- [Compiling from git](#compiling-from-git)


## Compiling Flatpak

For Steam Deck, Fedora Silverblue, etc.

Make sure to have `flatpak-builder` installed (package has the same name on most distros).

Also install the GNOME SDK (the launcher is written in GTK):
```sh
flatpak install org.gnome.Sdk//42
```

Clone the manifest repository:
```sh
git clone --branch rsdkv5u --recurse-submodules https://github.com/santiagocezar/flathub
cd flathub
```

And compile with `flatpak-builder`:
```sh
flatpak-builder --force-clean build-dir io.github.santiagocezar.maniatic-launcher.yaml
flatpak build-export repo build-dir
```

Finally, install it with:
```sh
flatpak install --reinstall ./repo io.github.santiagocezar.maniatic-launcher
```

It'll be available as RSDKv5U in your application launcher.

## Compiling from git

### Install dependencies

On Arch Linux:
```sh
sudo pacman -S --needed glew glfw libtheora zlib sdl2
```

On Ubuntu:
```sh
sudo apt install libglew-dev libglfw3-dev libtheora-dev zlib1g-dev libsdl2-dev
```

### Compile

Clone the decompilation:
```sh
git clone --recurse-submodules https://github.com/Rubberduckycooly/Sonic-Mania-Decompilation

cd Sonic-Mania-Decompilation/dependencies/RSDKv5/
```

Compile with `make`:
```sh
make -j$(nproc) # without mod support
make -j$(nproc) RSDK_REVISION=3 # with mod support
```

Copy the `Data.rsdk` into `bin/Linux/GL3/` and then run it with:
```sh
cd bin/Linux/GL3/
./RSDKv5 # or ./RSDKv5U
```

