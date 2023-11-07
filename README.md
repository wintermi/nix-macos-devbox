<img src="https://daiderd.com/nix-darwin/images/nix-darwin.png" width="200px" alt="logo" />

# **Bootstrapping `nix-darwin`**

## About

A simple guide explaining how to install [Nix](https://nixos.org/) and bootstrap [`nix-darwin`](https://github.com/LnL7/nix-darwin) on macOS.

## Prerequisites

### Installing Nix on macOS

To install `nix-darwin`, a working installation of [Nix](https://github.com/NixOS/nix#installation) is required, and one in which the experimental features `nix-command` and `flakes` have been enabled.

To do so, execute the following commands from within a macOS Terminal window and applying all of the defaults.

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

Once the above has finished, I would recommend restarting your computer.  This is normally not required, though I have encountered certain scenarios when it has been.  So to avoid debugging any issues, restart your computer.

