# Dev Shell for GitHub Work on Windows

A minimal, Linux-based development environment that enhances productivity. Features include a pretty terminal, autocomplete, autosuggestions, and efficient directory switching.

## Table of Contents

- [Installation of Ubuntu](#installation-of-ubuntu)
- [Setting Ubuntu as Default Shell](#setting-ubuntu-as-default-shell)
- [Installing Zsh](#installing-zsh)
- [Enable Autocompletion & Autosuggestions](#enable-autocompletion--autosuggestions)
- [Directory Switching with fzf](#directory-switching-with-fzf)
- [GitHub Productivity](#github-productivity)
- [Star Ship](#Adding-Starship-for-a-Pretty-Terminal)

## Installation of Ubuntu

1. Open your command prompt and run the following command to download and install Ubuntu:
   ```cmd
   wsl --install -d ubuntu
   ```

## Setting Ubuntu as Default Shell

1. Open Windows Terminal.
2. Press `[Ctrl + ,]` to open the settings.
3. Open the `settings.json` file by clicking on the "Open JSON file" icon at the bottom.
4. Find the `"defaultProfile"` field.
5. Set its value to Ubuntu's `guid` from the list of profiles in the `settings.json` file.

## Installing Zsh

1. Open your Ubuntu shell.
2. Run the following command to install Zsh

``` shell
sudo apt install zsh -y
```


## Adding Starship for a Pretty Terminal

1. Install Starship via a shell command:
    ```zsh
    sh -c "$(curl -fsSL https://starship.rs/install.sh)"
    ```
    
2. Add the Starship initialization script to the end of your `~/.zshrc` file:
    ```zsh
    echo 'eval "$(starship init zsh)"' >> ~/.zshrc
    ```

3. Update your Zsh settings to apply the changes:
    ```zsh
    source ~/.zshrc
    ```

With Starship, your terminal will now show useful information about your development environment, such as the current Git branch, language versions, and much more, all while being visually pleasing.

## Enable Autocompletion & Autosuggestions

1. Open your `~/.zshrc` file with a text editor. For example, using Neovim:
    ```zsh
    nvim ~/.zshrc
    ```
2. Add the following lines to enable autocompletion and autosuggestions:
    ```zsh
    autoload -Uz compinit
    compinit
    ```

## Directory Switching with fzf

1. Install `fzf` with the following command:
    ```zsh
    sudo apt install fzf
    ```
2. Add fzf configuration to `~/.zshrc` for easier directory navigation:
    ```zsh
    source /usr/share/doc/fzf/examples/key-bindings.zsh
    ```

## GitHub Productivity

1. Install the GitHub CLI tool:
    ```zsh
    sudo apt install gh
    ```
2. Configure the GitHub CLI for optimal usage:
    ```zsh
    gh auth login
    ```
3. Optionally, add GitHub CLI autocompletion to `~/.zshrc`:
    ```zsh
    eval "$(gh completion -s zsh)"
    ```

## Final Steps

1. After making all the changes, update your Zsh settings:
    ```zsh
    source ~/.zshrc
    ```

Now you have a minimal, feature-rich development shell optimized for GitHub workflows.

### Context Menu in Windows
Regedit: HKEY_CLASSES_ROOT\Directory\Background\shell
Create new Key: WSL Here
New String Key: Icon: %userprofile%\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04LTS_79rhkp1fndgsc\LocalState\rootfs\usr\share\wslu\wsl.ico
New Key: command
Set (Default): wsl.exe --cd "%V"

