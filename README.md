# Personal Ansible Desktop Configs

This is a repository utilising `ansible-pull` to configure Linux desktop and server envionments. 
It is based on <https://github.com/LearnLinuxTV/personal_ansible_desktop_configs>, however it has been heavily modified, to reflect my needs.

## Disclaimer
This repository contains a copy of the Ansible configuration that I use for laptops, desktops as well as servers.
Please don't directly use this against your own machines, as it is something I developed for myself and may not translate to your use-case. It even configures OpenSSH, so if you run it you may get locked out. 

## How does it work?
As mentioned above, it uses Ansible pull, so some familiarity with that is required.

The folder structure breaks down like this:

**local.yml**: This is the Playbook that Ansible expects to find by default in pull-mode, think of it as an "index" of sorts that pulls other Playbooks in.


**ansible.cfg**: Configuration settings for Ansible goes here.


**group_vars/**: This directory is where I can place variables that will be applied on every system.


**host_vars/**: Each laptop/desktop/server gets a host_vars file in this folder, named after its hostname. Sets variables specific to that computer.


**hosts**: This is the inventory file. Even in pull-mode, an inventory file can be used. This is how Ansible knows what group to put a machine in.


**playbooks**: Additional playbooks that I may want to run, or have triggered.


**roles/**: This directory contains my base, workstation, and server roles (and future roles I am adding as-and-when). Every host gets the base role. Then either 'workstation' or 'server', depending on what it is.

**roles/base**: This role is for every host, regardles of the type of device it is. This role contains things that are intended to be on every host, such as default configs, users, etc.

**roles/workstation**: After the base role runs on a host, this role runs only on hosts that are designated to be workstations. GUI-specific things, such as GUI apps (Firefox, etc), Flatpaks, wallpaper, etc. Has a folder for the GNOME and MATE desktops.

**roles/server**: After the base role runs on a host, this role runs only on hosts designated as servers. Monitoring plugins, unattended-updates, server firewall rules, and other server-related things are configured here.

After it's run for the first time manually, this Ansible config creates its own Cronjob for itself on that machine so you never have to run it manually again going forward, and it will track all future commits and run them against all your machines as soon as you commit a change. You can find the playbook for Cron in the base role.

## How do I run it?

```sh
# -U: the https url of the repo
# -C: needed only if using certain branhc (omit, if using the branch defined in the config)
sudo ansible-pull -U https://github.com/fabricesemti80/home.ansible.linux-config-with-ansible-pull.git -C fabrice
```
