# Ansible Tutorial inside Docker

This repository provides a self-contained Ansible environment using Docker. It includes an Ansible control node and a target managed node, allowing you to practice Ansible playbooks without installing anything on your host machine other than Docker.

## Prerequisites

- Docker
- Docker Compose

## Getting Started

1.  **Start the environment:**

    ```bash
    docker-compose up -d --build
    ```

    This will start two containers:
    - `ansible-tutorial-control-1`: The control node where you will run Ansible commands.
    - `ansible-tutorial-target-1`: The target node that Ansible will manage.

2.  **Access the Control Node:**

    ```bash
    docker-compose exec control bash
    ```

    You are now inside the container with Ansible installed. The project directory is mounted at `/ansible`.

3.  **Run Example Playbooks:**

    Verify connectivity with the ping playbook:

    ```bash
    ansible-playbook playbooks/01-ping.yml
    ```

    Install Nginx on the target node:

    ```bash
    ansible-playbook playbooks/02-setup-webserver.yml
    ```

    Create multiple users using loops and variables:

    ```bash
    ansible-playbook playbooks/03-create-users.yml
    ```

    Demonstrate handlers (restart service on change):

    ```bash
    ansible-playbook playbooks/04-handlers-demo.yml
    ```

    Demonstrate conditionals:

    ```bash
    ansible-playbook playbooks/05-conditionals.yml
    ```

    Read and print Ansible facts:

    ```bash
    ansible-playbook playbooks/06-read-facts.yml
    ```

    Use an Ansible Role to configure Nginx:

    ```bash
    ansible-playbook playbooks/07-use-role.yml
    ```

## Files Structure

- `docker/`: Contains Dockerfiles for the control and target nodes.
- `playbooks/`: Contains example Ansible playbooks.
- `inventory`: Defines the hosts that Ansible manages.
- `ansible.cfg`: Ansible configuration file.
- `docker-compose.yml`: Defines the services and their relationship.

## Troubleshooting

If you encounter SSH issues, ensure that the `control` container has successfully copied its SSH key to the `target` container. You can verify this by trying to SSH from control to target:

```bash
docker-compose exec control ssh target
```

If it asks for a password, something went wrong with the setup command in `docker-compose.yml`. You can manually fix it by running:

```bash
docker-compose exec control sshpass -p 'root' ssh-copy-id -o StrictHostKeyChecking=no root@target
```