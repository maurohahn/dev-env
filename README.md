<div align="center">
	<h1>Ubuntu 26.04 Development Environment</h1>
	<a href="https://ubuntu.com/download/flavours" target="_blank">
		<img src="./assets/logo-ubuntu-flavors.png" alt="Ubuntu Flavors">
	</a>
	<p>Installation steps and configuration files for a complete development workstation.</p>
</div>

This guide assumes a fresh Ubuntu 26.04 installation on a 64-bit system. Replace personal values, such as the Git name and email, when necessary.

## Table of Contents

- [System Preparation](#system-preparation)
- [Desktop Applications](#desktop-applications)
- [Development Tools](#development-tools)
- [Containers and Databases](#containers-and-databases)
- [Terminal](#terminal)
- [VS Code](#vs-code)
- [System Configuration](#system-configuration)
- [Useful Commands](#useful-commands)

## System Preparation

```console
sudo apt update
sudo apt upgrade -y
sudo apt install -y apt-transport-https ca-certificates curl file git software-properties-common zip wget
sudo apt install -y snapd flatpak gnome-software-plugin-flatpak
sudo systemctl enable --now snapd.socket
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
```

## Desktop Applications

### Google Chrome

```console
curl -L --fail --show-error --progress-bar "https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb" -o "./google-chrome-stable_current_amd64.deb"
sudo apt install -y ./google-chrome-stable_current_amd64.deb
rm ./google-chrome-stable_current_amd64.deb
```

### DocKit

```console
curl -L --fail --show-error --progress-bar "https://github.com/geek-fun/dockit/releases/download/v1.4.3/DocKit_1.4.3_amd64.deb" -o "./dockit.deb"
sudo apt install ./dockit.deb -y
rm ./dockit.deb
```

### IntelliJ IDEA

```console
sudo snap install intellij-idea-ultimate --classic
```

### Postman

```console
sudo snap install postman
```

### Remmina

```console
sudo snap install remmina
```

### RedisInsight

```console
sudo snap install redisinsight
```

### Flameshot

```console
flatpak install -y flathub org.flameshot.Flameshot
```

Start a Flameshot screenshot:

```console
flatpak run org.flameshot.Flameshot gui
```

### FileZilla

```console
sudo apt install -y filezilla
```

### Guake

```console
sudo apt install -y guake
```

### VirtualBox

```console
sudo apt install -y virtualbox
```

### PostgreSQL Client

```console
sudo apt install -y postgresql-client
```

### DBeaver

```console
sudo add-apt-repository ppa:serge-rider/dbeaver-ce
sudo apt update
sudo apt install dbeaver-ce
```

### AppImageLauncher

```console
sudo add-apt-repository ppa:appimagelauncher-team/stable
sudo apt update
sudo apt install appimagelauncher
```

### Android Studio

```console
sudo add-apt-repository ppa:maarten-fonville/android-studio
sudo apt update
sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1 libbz2-1.0:i386
sudo apt install android-studio
sudo chown -R $USER:$USER /opt/android-studio
```

### Optional Applications

```console
flatpak install -y flathub com.usebottles.bottles
sudo apt install -y openfortivpn fonts-firacode
```

Other applications from the setup notes are available from their official pages:

- [Scrcpy](https://github.com/Genymobile/scrcpy)
- [NoSQLBooster](https://nosqlbooster.com/downloads)
- [TeamViewer](https://www.teamviewer.com/en/download/linux/)
- [Ngrok](https://ngrok.com/download)

## Development Tools

### Git

```console
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install -y git
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global pull.rebase false
```

### Java with SDKMAN!

```console
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk list java
sdk install java 17.0.20-tem
sdk install java 25.0.4-tem
sdk default java 25.0.4-tem
sdk use java 25.0.4-tem
java --version
```

### Python with uv

Install uv and the Python versions you need:

```console
curl -LsSf https://astral.sh/uv/install.sh | sh
uv python install 3.12 3.13
uv python list
```

uv installs a *versioned* executable (e.g. `python3.13`). To also provide `python` and `python3`, use `uv python install --default`.

Create an isolated environment for a project:

```console
uv venv --python 3.12
source .venv/bin/activate
deactivate
```

Or use a managed project with automatic dependency resolution and a lockfile:

```console
uv init my-project
cd my-project
uv python pin 3.12
uv add requests
uv run python
```

Tools published as Python packages can be run with `uvx` (without installing them):

```console
uvx ruff check
```

### Node.js, Yarn, and Bun

```console
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.7/install.sh | bash
source "$HOME/.nvm/nvm.sh"
nvm install 24
nvm alias default 24
corepack enable
yarn --version
curl -fsSL https://bun.sh/install | bash
```

### Docker

Follow the [official Docker Engine installation guide for Ubuntu](https://docs.docker.com/engine/install/ubuntu/), then run:

```console
sudo systemctl enable --now docker
sudo usermod -aG docker "$USER"
```

Sign out and sign in again before using Docker as a non-root user.

## Terminal

### Zsh, Oh My Zsh, Zinit and Spaceship

```console
sudo apt install -y git curl zsh fonts-firacode
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone https://github.com/spaceship-prompt/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt" --depth=1
ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"
bash -c "$(curl --fail --show-error --silent --location https://raw.githubusercontent.com/zdharma-continuum/zinit/HEAD/scripts/install.sh)"
```

If `chsh` requires elevated permissions, use `sudo chsh -s "$(which zsh)" "$USER"` and start a new session.

The Zsh configuration file is [`terminal/.zshrc`](terminal/.zshrc).

## VS Code

### Install VS Code

```console
curl -L --fail --show-error --progress-bar "https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64" -o ./vscode.deb
sudo apt install -y ./vscode.deb
rm ./vscode.deb
```

### Install Extensions

The complete extension list is maintained in [`vscode/extensions.sh`](vscode-readme.txt). Install it with:

```console
chmod +x extensions.sh
./extensions.sh
```

### Apply Settings

The source configuration is [`vscode/settings.json`](vscode/settings.json):

```console
mkdir -p "$HOME/.config/Code/User"
if [ -f "$HOME/.config/Code/User/settings.json" ]; then
	cp "$HOME/.config/Code/User/settings.json" "$HOME/.config/Code/User/settings.json.backup"
fi
cp vscode/settings.json "$HOME/.config/Code/User/settings.json"
```

## System Configuration

### Inotify Watch Limit

```console
cat /proc/sys/fs/inotify/max_user_watches
echo "fs.inotify.max_user_watches=524288" | sudo tee /etc/sysctl.d/99-inotify.conf
sudo sysctl --system
```

### OpenVPN 3

Follow the [OpenVPN 3 Linux documentation](https://community.openvpn.net/openvpn/wiki/OpenVPN3Linux) to configure the repository:

```console
sudo apt update
sudo apt install -y apt-transport-https curl
sudo mkdir -p /etc/apt/keyrings
curl -sSfL https://packages.openvpn.net/packages-repo.gpg | sudo tee /etc/apt/keyrings/openvpn.asc > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/openvpn.asc] https://packages.openvpn.net/openvpn3/debian resolute main" \
  | sudo tee /etc/apt/sources.list.d/openvpn3.list
sudo apt update
sudo apt install -y openvpn3-client
```

## Useful Commands

### Keyboard Shortcuts

- Microphone mute: `Super+M`
- Lock screen: `Super+L`
- Flameshot screenshot: `Super+Print`, mapped to `flatpak run org.flameshot.Flameshot gui`
