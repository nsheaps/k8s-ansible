# k8s-ansible

Kubernetes Ansible Playbooks for WSL2 and Linux

## Project Description

This project provides a set of Ansible playbooks for setting up a Kubernetes cluster on WSL2 and Linux nodes. The playbooks cover installation and configuration of Kubernetes components, including the master and worker nodes, networking, and other dependencies.

## Requirements

The following software is required to run the playbooks:

- Ansible 2.10 or later
- Python 3.x
- WSL2 or Linux nodes with at least 2 CPU cores, 2GB RAM, and 20GB disk space

Note: WSL2 requires hardware virtualization technologies and is not suitable for every setup (including use in parallel with other hypervisors due to the use of Hyper-V). [More info](https://learn.microsoft.com/en-us/windows/wsl/faq#wsl-2)

## Setup Instructions

### WSL2 Setup

1. Install WSL2 (Windows Subsystem for Linux) on your Windows machine. Follow [these instructions](https://docs.microsoft.com/en-us/windows/wsl/install-win10) to install WSL2. These instructions assume that you install Ubuntu, and strongly suggest an LTS version (such as 22.04).

2. Open a WSL2 terminal and update the Ubuntu package list, then install ansible:
    ```
    sudo apt-get update
    sudo apt-get install ansible
    ```

3. You'll also need to enable WSL2's systemd integration in order to run services within a WSL2 instance. Add this to `/etc/wsl.conf` in the wsl2 instance:
    ```
    [boot]
    systemd=true
    ```
    For more info, see: https://devblogs.microsoft.com/commandline/systemd-support-is-now-available-in-wsl/

    Reboot the instance (or entire machine) once the configuration is in place and confirm that it is working properly by running `systemctl status`.

4. Finally you'll need your WSL2 instance to run at startup, which you can do via the Windows Terminal app 

### Linux Setup

1. Install Ansible on your Linux node:
    ```
    sudo apt-get update
    sudo apt-get install ansible
    ```

### Common setup

1. You'll also need to create the user for ansible to run as. On the nodes, run the following commands to add the `ansible` user:

    ```
    sudo useradd -m ansible
    sudo passwd ansible
    ```

    Enter a password for the `ansible` user when prompted. Save this in a secure storage location (such as LastPass or Hashicorp Vault)

2. On both nodes, run the following command to add the `ansible` user to the sudo group (allows the user to "become" root to install packages):

    ```
    sudo usermod -aG sudo ansible
    ```

## How to Run

1. Clone this repository:
    ```
    git clone https://github.com/nsheaps/k8s-ansible.git
    ```

2. Navigate to the project directory:
    ```
    cd k8s-ansible
    ```

3. Update the `inventory.ini` file with your WSL2 or Linux node IP addresses (if not already present). Be sure to push changes back to the repository.

4. Run the Ansible playbook for the Kubernetes master node:
    ```
    ansible-playbook -i inventory.ini kubernetes-master.yml
    ```

5. Run the Ansible playbook for the Kubernetes worker nodes:
    ```
    ansible-playbook -i inventory.ini kubernetes-worker.yml
    ```

6. Run the Ansible playbook for the Kubernetes networking:
    ```
    ansible-playbook -i inventory.ini kubernetes-network.yml
    ```

## Troubleshooting

If you encounter any issues during the installation or configuration process, please check the logs in `/var/log/kubernetes/` on your master node(s).
