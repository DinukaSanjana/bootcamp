<!-- ```markdown -->
# Ansible - Deploy mogombo ecommerce website ..!  

## 1. Environment Setup (Docker Compose)

```bash
git clone https://github.com/DinukaSanjana/bootcamp.git
cd bootcamp/setup

# Optional cleanup
docker compose down --rmi all --volumes --remove-orphans

# Start the lab
docker compose up -d
```


## 2. Docker Compose File Overview

**File location:** `bootcamp/setup/docker-compose.yml`

```yaml
services:
  control:
    image: codespaces/ansible-control:3.0.0
    ports:
      - "8000:8000"
    hostname: control
    container_name: control

  frontend:
    image: codespaces/ansible-node-ubuntu:18.04
    hostname: frontend

  catalogue:
    image: codespaces/ansible-node-ubuntu:18.04
    hostname: catalogue

  carts:
    image: codespaces/ansible-node-ubuntu:18.04
    hostname: carts

  # Additional nodes: user, payment, orders, etc.
```

## 3. Containers Created by Docker Compose

| Container Name | Image                            | Role                     | Access Method                     |
|----------------|----------------------------------|--------------------------|-----------------------------------|
| control        | codespaces/ansible-control:3.0.0 | Ansible Control Node + Web IDE | http://<IP>:8000                |
| frontend       | codespaces/ansible-node-ubuntu:18.04 | Managed Node          | ssh devops@frontend             |
| catalogue      | codespaces/ansible-node-ubuntu:18.04 | Managed Node          | ssh devops@catalogue            |
| carts          | codespaces/ansible-node-ubuntu:18.04 | Managed Node          | ssh devops@carts                |

## 4. Preparing & Opening Ansible Control Node

```bash
# Open in browser
http://<YOUR_SERVER_IP>:8000
# Example: http://159.65.77.142:8000
```
→ Opens Theia (VS Code-based) Web IDE → This is your Ansible Control Node

## 5. Inside Control Node - Initial Setup

```bash
# Clone project
git clone https://github.com/DinukaSanjana/bootcamp.git
cd bootcamp/ansible

# Verify Ansible
ansible --version
# ansible 2.9.10
```

## 6. Test Connectivity

```bash
ansible all -m ping
```
→ Should return green "pong" from all nodes

## 7. Inventory File & Path

**Location:** `/root/workspace/bootcamp/ansible/ansible.cfg` (config points to default inventory)

**Actual inventory used:** Dynamic inventory via Docker network + `/etc/ansible/hosts` or embedded in `ansible.cfg`

**Sample host entry format used in lab:**
```ini
frontend ansible_host=frontend ansible_user=devops ansible_ssh_pass=codespaces
```

## 8. SSH to Managed Nodes

```bash
ssh devops@frontend      # Password: codespaces
ssh devops@carts
ssh devops@catalogue
```

Check service after playbook run:
```bash
sudo systemctl status nginx
```

## 9. Ad-hoc Commands Executed

```bash
# Check uptime on all nodes
ansible all -a "uptime"

# Check disk usage
ansible all -a "df -h"

# Reboot all nodes
ansible all -a "/sbin/reboot" --forks=10
```

## 10. Run Ansible Playbooks

```bash
cd ~/bootcamp/ansible

ansible-playbook frontend.yml
ansible-playbook catalogue.yml
ansible-playbook carts.yml
ansible-playbook mogambo.yml
```

## 11. Ansible Roles - Project Structure

**Path:** `roles/`

```bash
roles/
├── frontend/
│   ├── tasks/
│   │   └── main.yml
│   ├── handlers/
│   │   └── main.yml
│   ├── templates/
│   ├── files/
│   ├── vars/
│   │   └── main.yml
│   └── defaults/
│       └── main.yml
├── catalogue/
├── carts/
└── common/
```

## 12. Normal Playbook vs Large Project (Roles-Based)

| Type                  | Structure                                 | Use Case                     |
|-----------------------|-------------------------------------------|--------------------------------|
| Simple Playbook       | All tasks, vars, templates in one .yml    | Small scripts, demos           |
| Large Project (Roles) | Separated: tasks, vars, templates, handlers, files | Reusable, modular, maintainable, team-friendly |

## 13. Generate New Role

```bash
ansible-galaxy init --offline roles/frontend
ansible-galaxy init --offline roles/catalogue
```

Creates full role directory structure automatically.

## 14. Playbook Using Roles

```yaml
# frontend.yml
- name: Deploy Frontend Service
  hosts: frontend
  become: yes
  roles:
    - common
    - nginx
    - frontend
```

## 15. Full Playbook Structure Used

```
ansible.cfg
environments/
group_vars/
hosts                  # inventory (if static)
roles/
  └── <role_name>/
      ├── tasks/
      ├── handlers/
      ├── templates/
      ├── files/
      ├── vars/
      └── defaults/
frontend.yml
catalogue.yml
carts.yml
mogambo.yml            # Master playbook
```

## 16. Deploy mogambo.org E-commerce Website

```bash
ansible-playbook mogambo.yml
```

After successful run → Open in browser:

**Website URL:**  
`http://<YOUR_SERVER_PUBLIC_IP>:80`

**Services deployed:**
- Nginx (port 80)
- Node.js apps (frontend, catalogue, carts)
- MongoDB
- Full mogambo.org e-commerce site running

Accessible via: `http://<VM_PUBLIC_IP>:80`

## Final Status: Completed

- Full multi-node Ansible lab running via Docker Compose
- Web-based control node at port 8000
- All nodes reachable via `ansible all -m ping`
- Roles-based, clean, reusable playbook structure
- Successfully deployed full-stack mogambo.org website
- Used ad-hoc commands, inventory, roles, handlers, templates

**Project Completed Successfully**  
**Live Site:** http://<YOUR_IP>:80  
**Control Panel:** http://<YOUR_IP>:8000

Ready for production-grade Ansible automation!
