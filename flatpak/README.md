# Dusklight Flatpak

## Table of Contents
- [Dusklight Flatpak](#dusklight-flatpak)
  - [Table of Contents](#table-of-contents)
  - [Small Notes about the Flatpak](#small-notes-about-the-flatpak)
    - [The Two Versions](#the-two-versions)
    - [Flathub Version Details](#flathub-version-details)
  - ["special" Permissions used](#special-permissions-used)
  - [How build it yourself](#how-build-it-yourself)
    - [Direct Installation](#direct-installation)
    - [Exporting a .flatpak Bundle](#exporting-a-flatpak-bundle)
    - [Installing the Bundle](#installing-the-bundle)

## Small Notes about the Flatpak

### The Two Versions
There are currently 2 Versions: 
1. A version that pulls and compiles a Flatpak from source with all necessary packages. *Like compiling it directly but just as a Flatpak.*
2. A work-in-progress Flathub version of the script with no binaries due to Flathub's strict publishing guidelines [see](https://docs.flathub.org/docs/for-app-authors/requirements#building-from-source)
   
### Flathub Version Details
The Flathub version currently compiles everything from Source *except* Dawn, which is needed by Aurora. As of now: It's a really big package and I didn't manage to integrate it from source into the application.
The Flathub version also has nod- and dusklight-sources.json files where the sources are located to compile them. Both are based on the cmakelists from Dusklight and Aurora.

## "special" Permissions used
* `--device=dri` for graphics acceleration
* `--device=input` for Controller to work. => device=all not needed since EOS 1.14! But Gyro doesnt work
* `--device=all` Controller work without any problem but the flatpak has access to the devices
* `--filesystem=xdg-run/discord-ipc-0` for the Discord integration => in working order!
* `--share=network` for the Update Check to run otherwise "failed to check for update" would be displayed at the start

## How build it yourself
*Dont forget to change the version! Currently it compile v1.1.1!

Download the manifest of your choice into a folder (the Flathub folder is needed if you use the work-in-progress Flathub build).

### Direct Installation

If you just want to compile and directly install it without generating a `.flatpak` file, run:

```bash
flatpak-builder --user --install --force-clean build-dir dev.twilitrealm.dusklight.yml
```

### Exporting a .flatpak Bundle

If you want to create a `.flatpak` file for export:

```bash
flatpak-builder --repo=EXPORT --force-clean build-dir dev.twilitrealm.dusklight.yml
```
*Note: This writes into a temporary local repo named "EXPORT". You can change this name if you wish, but make sure to also change it in the next command.*

Then, build the bundle:

```bash
flatpak build-bundle EXPORT dusklight.flatpak dev.twilitrealm.dusklight
```
After this step, the `dusklight.flatpak` file will be generated in your current folder.

### Installing the Bundle

Install the exported file with:

```bash
flatpak install --user dusklight.flatpak
```
*Note: Don't forget the `--user` flag! Otherwise, it will ask for your admin password and install it system-wide instead of into your userspace.*
