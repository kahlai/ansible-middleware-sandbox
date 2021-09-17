## Ansible Middleware Demo

This repository simulate the ansible playbook to install wildfly and configure wildfly using jcliff

## Requirements

1. Docker / Docker compose https://docs.docker.com/compose/install/

## Topology

| Hostname | Private IP address | Expose Port | Description                 |
| -------- | ------------------ | ----------- | --------------------------- |
| ansible   | 172.16.238.10      |        | Ansible 2.9 |
| proxy1   | 172.16.238.11      | 10001,2122  | Load Balancer (Wildfly)     |
| proxy2   | 172.16.238.12      | 10002,2222  | Load Balancer (Wildfly      |
| slave1   | 172.16.238.22      | 18080,2422 | Host Controller 1 (Wildfly) |
| slave2   | 172.16.238.23      | 28080,2522  | Host Controller 2 (Wildfly) |
| slave3   | 172.16.238.24      | 38080,2622  | Empty (Standby for extra Wildfly) |

## Launch the environment

1. Start all docker in daemon mode. (Inside the folder with docker-compose.yml) 

```bash
docker-compose up -d
```

2. Connect into the ansible container 

```bash
docker exec -it ansible-middleware-sandbox_ansible_1 bash
```

3. Install  middleware_automation.jcliff (replacement for wildfly.jcliff) collection (Inside the ansible container) 

```bash
ansible-galaxy collection install middleware_automation.jcliff
```

4. Switch to workspace and run playbook (Inside the ansible container) 

```bash
cd /workspace/
ansible-playbook playbook.yml
```

Note: You can edit the playbook using vscode while trying it in the container.

## Destroy the environment

1. Stop and remove all docker (Inside the folder with docker-compose.yml) 

```
docker-compose down
```


## Useful command

Show all running containers
```
docker-compose ps
```

Stop all containers without remove the content
```
docker-compose stop
```

Stop only slave3
```
docker-compose stop slave3
```

Start only slave3
```
docker-compose start slave3
```

## Reference
Using ansible collection jcliff to scan for common criteria
Switch to workspace and run playbook (Inside the ansible container) 

```bash
cd /workspace/
ansible-playbook plays/scan_ccc.yml
```
https://github.com/ansible-middleware/ansible_collections_jcliff
