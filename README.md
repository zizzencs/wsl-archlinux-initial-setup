# WSL Arch Linux Initial Setup

This repository contains Ansible playbooks that set up and configure an Arch Linux instance in Windows Subsystem for Linux (WSL).

## Historical Note

In previous versions of the playbooks, the base setup utilized the official Arch Linux Docker image, which ran as a custom distribution on WSL. However, this approach is no longer used, and the playbooks are now designed to work seamlessly with a standard Arch Linux WSL installation.

## Prerequisites

Before using these playbooks, ensure you have WSL with Arch Linux installed on your Windows system. To get started, follow the official guides:

- [Microsoft's official guide to install WSL](https://learn.microsoft.com/en-us/windows/wsl/install)
- [Arch Linux WSL installation guide](https://wiki.archlinux.org/title/Windows_Subsystem_for_Linux)

In general, you need:

1. A working Arch Linux WSL installation with systemd support
2. Basic packages installed (bash, sudo, etc.)
3. A user with sudo privileges

## Playbooks

### init.yaml

The `init.yaml` playbook is designed to validate and establish the prerequisites needed before running the main setup. It serves two primary purposes:

1. **Validation**: It checks if the necessary prerequisites are in place (such as locale settings and required packages).
2. **Kickstart**: It prepares the environment for the main `setup.yaml` playbook by installing essential tools and configuring basic system settings.

Key tasks performed by `init.yaml` include:

- Creating an initial user with sudo privileges
- Setting up the system locale to C.UTF-8
- Installing Ansible packages if not already present
- Installing and configuring the yay AUR helper
- Setting up an AUR builder user for package management

Run this playbook first to ensure your system is ready for the main setup process:

```bash
ansible-playbook init.yaml
```

### setup.yaml

The main playbook that configures your Arch Linux WSL environment. This playbook handles the bulk of the system configuration after the prerequisites have been established by `init.yaml`.

Key tasks performed by `setup.yaml` include:

- **WSL Configuration**: Sets up wsl.conf for optimal WSL behavior
- **System Optimization**:
  - Configures pacman.conf for better usability and faster package management
  - Optimizes makepkg.conf for faster builds using native architecture
  - Sets up system locale based on your preferences
  - Configures time synchronization for WSL (important as the clock may drift when suspending/hibernating the Windows host)
- **Package Installation**: Installs a curated set of useful packages including:
  - Development tools (nano, git, etc.)
  - Modern CLI replacements (eza, bat, ripgrep, etc.)
  - Cloud tools (aws-cli, kubectl, helm, etc.)
  - Container tools (docker, docker-compose)
  - Java development environment
- **Tool Configuration**:
  - Sets up nano with syntax highlighting
  - Configures procs and starship for better terminal experience
  - Sets up git with your personal information
  - Configures Java environment with Amazon Corretto
  - Sets up Terraform/OpenTofu via tenv
  - Configures Docker for non-root usage
  - Installs 1Password CLI with AWS integration
  - Customizes your bash environment

Run this playbook after successfully completing the init.yaml playbook:

```bash
ansible-playbook setup.yaml
```

You can customize the playbook behavior by modifying the variables in the `host_vars/localhost` file.
