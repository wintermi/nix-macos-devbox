<img src="https://daiderd.com/nix-darwin/images/nix-darwin.png" width="200px" alt="logo" />

# **Bootstrapping `nix-darwin`**


### About

A simple guide on installing [`Nix`](https://nixos.org/) and bootstrap [`nix-darwin`](https://github.com/LnL7/nix-darwin) on macOS.


### Installing Nix on macOS

A working installation of [`Nix`](https://github.com/NixOS/nix#installation) is required with the experimental features `nix-command` and `flakes` enabled, before `nix-darwin` can be installed.

Execute the following commands from a macOS Terminal and when prompted select all of the defaults.


```bash
# Install 'nix'
sh <(curl -L https://nixos.org/nix/install)

# Create a 'nix' config directory
mkdir -p "$HOME/.config/nix"

# Emable nix-command and flakes
cat <<EOF > "$HOME/.config/nix/nix.conf"
experimental-features = nix-command flakes
EOF

# Cleanup directory and file permissions
chmod 700 "$HOME/.config/nix"
chmod 600 "$HOME/.config/nix/nix.conf"
```


Once the above has finished, I would recommend restarting your computer.  This is normally not required, although I have encountered certain scenarios when it has been.  So to avoid debugging any issues, restart your computer.


### Installing nix-darwin

Execute the following commands from a macOS Terminal to initialise and install `nix-darwin`.

Once the `flake.nix` file is written out as part of the initialisation process the below will then execute two find and replace commands using the `sed` command line tool.

1. Replace `simple` with the `hostname` of the computer
2. Replace `x86_64-darwin` with `aarch64-darwin` to change the computer architecture to Apple Silicon.


```bash
# Create a 'nix-darwin' config directory
mkdir -p "$HOME/.config/nix-darwin"

# Initialise default configuration and install 'nix-darwin'
pushd -q "$HOME/.config/nix-darwin"
nix flake init -t nix-darwin
sed -i '' "s/simple/$(scutil --get LocalHostName)/" flake.nix
sed -i '' "s/x86_64-darwin/aarch64-darwin/" flake.nix
nix run nix-darwin -- switch --flake "$HOME/.config/nix-darwin"
popd -q

# Cleanup directory and file permissions
chmod 700 "$HOME/.config/nix-darwin"
chmod 600 "$HOME/.config/nix-darwin"/flake.*
```


Installation of `nix-darwin` is now complete and ready for you to configure and rebuild your environment.


### Rebuild enviroment

Since `nix-darwin` is automatically added to the PATH, you can rebuild your enviroment from anywhere in the system by executing the following commands from a macOS Terminal.


```bash
# Rebuild the macOS nix-darwin environment
darwin-rebuild switch --flake "$HOME/.config/nix-darwin#$(scutil --get LocalHostName)"
```


This will rebuild the configuration and automatically activate it.


### Add packages

Now let's modify the `nix-darwin` configuration file located at `$HOME/.config/nix-darwin/flake.nix` to add any additional packages you would like to be installed on your computer:


```nix
      environment.systemPackages =
        [ pkgs.vim
        ];
```


The default configuration above installed the latest version of the `vim` text editor.  There are thousands of potential packages which could be installed and you can use the [NixOS Search](https://search.nixos.org/packages) website to find the one you want to install.  For now, lets change the configuation to the following:


```nix
      environment.systemPackages = with pkgs; [
        wget
        git
      ];
```


Then, execute the rebuild command above to uninstall `vim` and install `wget` and `git`.

