<img src="https://daiderd.com/nix-darwin/images/nix-darwin.png" width="200px" alt="logo" />

# **Create a DevBox via Nix on macOS**

## Overview

A simple guide to installing [Nix](https://nixos.org/) on macOS and then creating an isolated shell for development using a [JetPack.io DevBox](https://www.jetpack.io/devbox/).

## Installing Nix

> **Note**
> Note: The last time I tested these instructions was November 2023.

To install [`Nix`](https://nixos.org/) on your macOS computer follow these steps.

### Install Dependencies

Install the Xcode Command Line Tools, which is a collection of tools for software developers that run on the command line, in the Terminal application.

```bash
xcode-select --install
```

### Install Nix

Install Nix using the [Determinate Nix Installer](https://github.com/DeterminateSystems/nix-installer)! Which is a fast, friendly, and reliable tool to help you use Nix with Flakes everywhere.

Execute the following command from a macOS Terminal application and when prompted select all of the defaults.

```bash
curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | sh -s -- install
```

Once the above is complete, you will need to add the following line to your `.zshrc` file to ensure the Nix binaries are available within your system path:

```bash
source '/nix/var/nix/profiles/default/etc/profile.d/nix-daemon.sh'
```

On completion, I would recommend restarting your computer. This is normally not required, although I have encountered certain scenarios when it has been. So to avoid debugging any issues, restart your computer.

## Installing DevBox

There are a couple of options which could be followed when installing the [JetPack.io DevBox](https://www.jetpack.io/devbox/) on macOS. However, since we went through the process of first installing Nix, the best option in my opinion is to install using Nix.

Since we started with a clean install of Nix we must first add the available Nix Package channels which will be used when installing software. This can be achieved by executing the following commands from a macOS Terminal application.

```bash
nix-channel --add https://nixos.org/channels/nixpkgs-23.05-darwin nixpkgs;
nix-channel --add https://nixos.org/channels/nixpkgs-unstable unstable;
nix-channel --update;
```

Once complete, execute the following command to install the DevBox package.

```bash
nix-env -iA unstable.devbox
```

JetPack.io provides a great set of documentation on how to use DevBox, which can be found [here](https://www.jetpack.io/devbox/docs/).

### Using DevBox as your Primary Package Manager

In addition to managing isolated development environments, you can use DevBox as a general package manager. DevBox Global allows you to add packages to a global `devbox.json`. This is useful for installing a standard set of tools you want to use across multiple DevBox Projects.

### Adding and Managing Global Packages

You can install a package using `devbox global add [<package>]`, where the package names should be a list of [Nix Packages](https://search.nixos.org/packages) you want to install.

For example, if we wanted to install ripgrep, vim, and git to our global profile, we could run:

```bash
devbox global add ripgrep wget git
```

Once installed, the packages will be available whenever you start a Devbox Shell, even if it's not included in the project's `devbox.json`.

To view a full list of global packages, you can run `devbox global list`:

```bash
devbox global list
```

## Execute Garbage Collection for Nix

```bash
nix-collect-garbage
```
