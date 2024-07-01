This repository contains an Ansible playbook that sets up some aspects of an Archlinux instance in WSL.

To run this, the following configuration should be present in the Windows user's home directory, in `.wslconfig`:

```[wsl2]
processors=8
memory=8GB
swap=0
guiApplications=false
firewall=false
kernelCommandLine=systemd.firstboot=false

[experimental]
autoMemoryReclaim=gradual
sparseVhd=true```

Note that `systemd.firstboot` might still interact with the first run after changing to systemd.
