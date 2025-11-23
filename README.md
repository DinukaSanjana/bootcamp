
# Ansible Practical - Just Enough Ansible to be Dangerous

## Environment Setup (Docker Compose)

```bash
git clone https://github.com/DinukaSanjana/bootcamp.git
cd bootcamp/setup

# Optional cleanup
docker compose down --rmi all --volumes --remove-orphans

# Start lab
docker compose up -d


## Access Web IDE (Control Node)
http://<YOUR_IP>:8000  
Example: http://159.65.77.142:8000

## Key Docker Services
```yaml
control:
  image: codespaces/ansible-control:3.0.0
  ports: ["8000:8000"]
frontend:
  image: codespaces/ansible-node-ubuntu:18.04
catalogue:
  image: codespaces/ansible-node-ubuntu:18.04
carts:
  image: codespaces/ansible-node-ubuntu:18.04
```

## SSH to Managed Nodes (from control node)
```bash
ssh devops@frontend     # Password: codespaces
ssh devops@carts
ssh devops@catalogue
```

## Inside Control Node - Project Setup
```bash
git clone https://github.com/DinukaSanjana/bootcamp.git
cd bootcamp/ansible

ls
# README.md  ansible.cfg  carts.yml  catalogue.yml  common.yml
# frontend.yml  mogambo.yml  environments  group_vars  roles
```

## Ansible Version
```bash
ansible --version
# ansible 2.9.10
# config file = /root/workspace/bootcamp/ansible/ansible.cfg
```

## Sync Fork with Upstream (if forked)
```bash
git remote add upstream https://github.com/udbc/bootcamp.git
git pull upstream master
```

## Test Connectivity
```bash
ansible all -m ping
```

## LAB 1 - Completed Tasks
- Started Docker Compose lab  
- Accessed web IDE at port 8000  
- SSH into frontend, carts, catalogue nodes  
- Cloned bootcamp repo inside control node  
- Verified Ansible ping to all nodes  

## Inventory Example
```ini
[frontend]
frontend ansible_host=172.18.0.3 ansible_user=devops

[webservers]
frontend ansible_host=172.18.0.3 ansible_user=devops

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

## Sample Playbook
```yaml
- name: Install Nginx on webserver
  hosts: webservers
  become: yes
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
```

Run:
```bash
ansible-playbook -i inventory.ini site.yml
```

## Project Structure Learned
```
ansible.cfg
environments/
group_vars/
roles/
*.yml playbooks (frontend.yml, catalogue.yml, etc.)
```

**Status: Completed**  
**Environment: Fully containerized Ansible lab with web IDE**  
**Ansible Version: 2.9.10**  
**Control Node: Browser-based Theia IDE**  
**Managed Nodes: frontend, catalogue, carts, etc.**
```
