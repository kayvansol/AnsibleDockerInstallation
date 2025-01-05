# Install and Configure Docker via Ansible

![alt text](https://raw.githubusercontent.com/kayvansol/AnsibleDockerInstallation/refs/heads/main/img/1.webp?raw=true)

`Docker` is a set of platform as a service (PaaS) products that use OS-level virtualization to deliver software in packages called containers.[5] The service has both free and premium tiers. The software that hosts the containers is called Docker Engine.[6] It was first released in 2013 and is developed by Docker, Inc.

Docker is a tool that is used to automate the deployment of applications in lightweight containers so that applications can work efficiently in different environments in isolation.

`Ansible` is a suite of software tools that enables infrastructure as code. It is open-source and the suite includes software provisioning, configuration management, and application deployment functionality.

Ansible is an open source IT automation engine that automates provisioning, configuration management, application deployment, orchestration, and many other IT processes. It is free to use, and the project benefits from the experience and intelligence of its thousands of contributors.

# Senario :

We have 3 machine, one for ansible management and 2 for hosts to install docker on them :

![alt text](https://raw.githubusercontent.com/kayvansol/AnsibleDockerInstallation/refs/heads/main/img/1.png?raw=true)

At first we must install ansible on the DesktopTest (ansible management) :
```
sudo apt-add-repository ppa:ansible/ansible

sudo apt update

sudo apt install ansible

ansible --version
```

Note : python must be installed at any linux and consider that we have one linux for source (ansible management node) where the ansible is installed on it and one or many linux for destination that must be install docker by ansible.

Important : management node must can ssh to any destination node passwordlesslly.

for this reason, we generate a ssh key on management node and copy it to any destination nodes.

then check the ansible project and files at management node :

![alt text](https://raw.githubusercontent.com/kayvansol/AnsibleDockerInstallation/refs/heads/main/img/10.png?raw=true)

# Requirements File requirements.txt
The requirements.txt file is used in Python projects to list all the dependencies (external libraries or packages) that are required to run your application or project. This file allows users to easily install the necessary dependencies using pip, the Python package installer.
```
cd ansible

pip install -r requirements.txt
```

# Ansible Configuration File ansible.cfg
The ansible.cfg file defines key configuration settings for running Ansible playbooks and managing target servers. This file allows customization of various options, including connection details, logging, and roles directory paths, making it easier to manage Ansible behavior across different environments.

# Playbooks :
Playbooks are used to execute multiple roles and tasks across targeted servers.

‍‍docker.yml This is a Docker playbook that allows us to invoke the Docker role

# Roles:
Each role in this project is modular, focused on a specific area of system security, and designed to be reusable.

docker_installation With this role, you can install and configure Docker.

Edit the inventory file based on destination node(s) :
```
all:
  vars:
    ansible_user: root
    ansible_port: 22 #8090
  hosts:
    server01:
      ansible_host: 192.168.56.157
    server02:
      ansible_host: 192.168.56.158
```

then ping all destination host(s) to be ok before running the ansible playbooks :
```
ansible all -m ping
```

![alt text](https://raw.githubusercontent.com/kayvansol/AnsibleDockerInstallation/refs/heads/main/img/2.png?raw=true)

check 2 machines that does not have docker installed on them :

![alt text](https://raw.githubusercontent.com/kayvansol/AnsibleDockerInstallation/refs/heads/main/img/3.png?raw=true)

![alt text](https://raw.githubusercontent.com/kayvansol/AnsibleDockerInstallation/refs/heads/main/img/4.png?raw=true)

then run the playbook :
```
ansible-playbook -i inventory/RahBia.yml playbooks/docker.yml

or

ansible-playbook playbooks/docker.yml
```

![alt text](https://raw.githubusercontent.com/kayvansol/AnsibleDockerInstallation/refs/heads/main/img/5.png?raw=true)

htop on one of 2 machine :

![alt text](https://raw.githubusercontent.com/kayvansol/AnsibleDockerInstallation/refs/heads/main/img/6.png?raw=true)

if any error happens, check and correct it and run the playbook again.

Note : ansible process is idempotent, an idempotent operation is one that has no additional effect if it is called more than once with the same input parameters.

at final we have the result like below :

![alt text](https://raw.githubusercontent.com/kayvansol/AnsibleDockerInstallation/refs/heads/main/img/7.png?raw=true)

and now check 2 machines that have docker.

![alt text](https://raw.githubusercontent.com/kayvansol/AnsibleDockerInstallation/refs/heads/main/img/8.png?raw=true)

![alt text](https://raw.githubusercontent.com/kayvansol/AnsibleDockerInstallation/refs/heads/main/img/9.png?raw=true)

Congratulation. 🍹


