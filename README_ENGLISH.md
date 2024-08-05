
# AWX Installation Playbook

## Introduction

This project provides an Ansible playbook to install and configure AWX.

## Requirements

- Ansible 2.9 or higher
- Access to a CentOS or RHEL machine with root privileges
- Internet access to download necessary packages and software

## Installation

1. **Clone the repository**:
   ```sh
   git clone https://github.com/oVogEnt/awx_installation.git
   cd awx_installation
   ```

2. **Install the required Ansible collections**:
   ```sh
   ansible-galaxy collection install -r collections/requirements.yml
   ```

3. **Run the playbook**:
   ```sh
   ansible-playbook setup_awx.yml -i inventory/hosts -u root -kK
   ```

   - `-i inventory/hosts`: Specifies the inventory file.
   - `-u root`:  Uses the root user for SSH connections. You can also use any other user with sudo rights.
   - `-k`: Prompts for the SSH password.
   - `-K`: Prompts for the sudo password.

## Playbook Overview

The playbook performs the following steps:

1. **Pre-tasks**:
   - Install required packages (e.g., `epel-release`, `git`, `make`, `firewalld`, `podman`, `python3-pip`).

2. **Tasks**:
   - Ensure `firewalld` is installed, started, and configured to use `iptables`.
   - Download and install `k3s` (a lightweight Kubernetes distribution).
   - Wait for `k3s` to deploy default services.
   - Install the Kubernetes Python library.
   - Copy the `kubeconfig` to the home directory.
   - Fetch the latest version of the AWX Operator from GitHub if not already defined.
   - Clone the AWX Operator repository.
   - Create a Kubernetes namespace for AWX.
   - Create and apply the `kustomization.yaml` file for deploying the AWX Operator.
   - Create and apply the `awx.yml` file to deploy the AWX instance.
   - Wait for the AWX instance to be ready.
   - Retrieve the AWX admin password and service NodePort.
   - Save the AWX access details to a file (`/tmp/awx_access_details.txt`).

## Access Details

After running the playbook, the AWX admin password and URL will be saved to `/tmp/awx_access_details.txt`.

Login with the user `admin`