# Setting Up Git

## Installing Git (Windows, macOS, Linux)

### Windows

- Download the Git installer from Git for Windows.

- Run the installer and follow the prompts, selecting the default options if unsure.

- Once installed, you can use Git Bash or Git GUI to interact with Git.

## macOS

- Open Terminal and type `git --version`. If Git is not installed, you will be prompted to install it using Xcode Command Line Tools.

- Alternatively, you can install Git via Homebrew. Run the command: `brew install git`.

# Linux:

- Open your terminal.

- For Debian/Ubuntu-based distributions, run: `sudo apt update` and `sudo apt install git`.

- For Fedora, run: `sudo dnf install git`. For other distributions, use the appropriate package manager (e.g., yum for CentOS).

- For Arch-based distributions, run: `sudo pacman -S git`

## Configuring Git (name, email, default editor)

- After installing Git, you need to configure it with your name and email address:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

- You can also set your default text editor for Git (e.g., Vim, VS Code):

```bash
git config --global core.editor "code --wait"
```

## Verifying installation

- To verify that Git is installed correctly, run:

```
git --version
```

- This command should display the installed Git version.