# Ansible playbooks for Linux

This repository contains a collection of Ansible playbooks designed to automate the setup and configuration of various Linux distributions. These playbooks cover a range of tasks including system updates, package installations, user management, and service configurations.

## Prerequisites & Installation

Before running playbooks ensure your system is up to date and Ansible is installed. On Debian/Ubuntu systems you can run:

```bash
# Update package index
sudo apt update

# Install Ansible non-interactively
sudo apt install ansible -y
```

## Playbooks usage

Run playbooks from this repository with `ansible-playbook`. For example, to run the `master.yml` playbook locally (uses a local connection):

```bash
# Check syntax of a playbook before running
ansible-playbook led-iot-ap.yml --syntax-check

# Run the master playbook locally
ansible-playbook -i localhost, master.yml -e "ansible_connection=local"
```

Use the `-i` flag to point to an inventory file or a comma-separated host list. Many playbooks live under the `playbooks/` directory; check individual playbook headers for specific variables or required inventories.

## Cleanup

When you're finished and want to remove Ansible from the host, run:

```bash
sudo apt purge --auto-remove ansible
```