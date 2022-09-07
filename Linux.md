# Compiling for Linux

- [Installing/Compiling Flatpak](#compiling-flatpak)
- [Compiling from git](#compiling-from-git)
- [Raspberry Pi](#raspberry-pi)


## Compiling Flatpak

For Steam Deck, Fedora Silverblue, etc.

> Note: A build is already available on [Flathub](https://flathub.org) under the name of Maniatic Launcher. Install it using  
> ```sh
> flatpak install flathub io.github.santiagocezar.maniatic-launcher
> ```
> Know that this doesn't have the Plus DLC enabled.

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

If you want to enable the Plus DLC, remove the AUTOBUILD=1 line in the io.github.santiagocezar.maniatic-launcher.yaml file.

Then compile with `flatpak-builder`:
```sh
flatpak-builder --force-clean build-dir io.github.santiagocezar.maniatic-launcher.yaml
flatpak build-export repo build-dir
```

Finally, install it with:
```sh
flatpak install --reinstall ./repo io.github.santiagocezar.maniatic-launcher
```

It'll be available as Maniatic Launcher in your application uh, launcher.

## Compiling from git

### Install dependencies

On Arch Linux:
```sh
sudo pacman -S --needed glew glfw libtheora zlib sdl2
```

On Ubuntu or Debian:
```sh
# Note: The decompilation needs GLFW 3.3+, only available from Ubuntu 20.04 and Debian 11.
sudo apt install libglew-dev libglfw3-dev libtheora-dev zlib1g-dev libsdl2-dev
```

On Fedora:
```sh
sudo dnf install glew-devel glfw-devel libtheora-devel zlib-devel SDL2-devel
```

### Compile

Clone the decompilation:
```sh
git clone --recurse-submodules https://github.com/Rubberduckycooly/Sonic-Mania-Decompilation

cd Sonic-Mania-Decompilation/dependencies/RSDKv5/
```

Compile with `make`:
```sh
make -j$(nproc)
```

See also: [Make options](make-opts.md#rsdk)

Copy the `Data.rsdk` into `bin/Linux/GL3/` and then run it with:

```sh
cd bin/Linux/GL3/
./RSDKv5 # or ./RSDKv5U
```

## Raspberry Pi

_thanks to awefour44_

> Note: If you're using Raspbian to compile from git you'll need version 11 or later, same as Debian. To compile the flatpak the version does not matter.

The RPi doesn't support the OpenGL version used by the decomp. To work around this, run the decomp with these env variables:
```sh
MESA_GL_VERSION_OVERRIDE=3.3 MESA_GLSL_VERSION_OVERRIDE=330 ./RSDKv5
```

On Flatpak you can permanently set these variables using `flatpak override`:
```sh
flatpak override --env=MESA_GL_VERSION_OVERRIDE=3.3 --env=MESA_GLSL_VERSION_OVERRIDE=330  io.github.santiagocezar.maniatic-launcher
```