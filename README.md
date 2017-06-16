# bash-ansible-dynamic-inventory-dns

Use TXT DNS records to create a dynamic inventory list usable by ansible

Utilisez les enregistrements DNS type TXT pour créer un inventaire d'hôte utilisable par ansible

----------------------------------

# Simple use in two minutes

1. Clone this repository
2. Edit your DNS Zone in ./inventory/bash-ansible-dynamic-inventory-dns.sh
3. Create a TXT record in your DNS
4. Launch ping playbook :
    ```
    ansible-playbook ping.yml
    ```

# Complete usage - Step by step :
 
1. Create a directory in your ansible working dir, for example « mkdir inventory »
2. Create a file named « ansible.cfg » in your ansible working dir, content below:
    ```
   [defaults]
   hostfile = inventory
    ```
3. Put this script in directory create in step 1, for this case « inventory »
4. Make you sure script file is executable « chmod +x inventory/name-of-this-script.sh »
5. Launch your Ansible playbook 
    ```
    ansible-playbook my-playbook-file.yml
    ```

## DNS entry TXT example:

* Host 1 (Debian, power-on, webserser)
  - host-1.example.org 86400 IN A 192.168.1.10
  - host-1.example.org 86400 IN TXT "os-debian"
  - host-1.example.org 86400 IN TXT "service-web"
* Host 2 (Debian, power-off, webserver
  - host-2.example.org 86400 IN A 192.168.1.12
  - host-2.example.org 86400 IN TXT "os-debian"
  - host-2.example.org 86400 IN TXT "service-web"
  - host-2.example.org 86400 IN TXT "power-off"
* Host 3 (OpenSuse, power-on, database)
  - host-3.example.org 86400 IN A 192.168.1.13
  - host-3.example.org 86400 IN TXT "os-suse"
  - host-3.example.org 86400 IN TXT "service-db"
* Host 4 (OpenSuse, power-on, webserver) 
  - host-4.example.org 86400 IN A 192.168.1.14
  - host-4.example.org 86400 IN TXT "os-suse"
  - host-4.example.org 86400 IN TXT "service-web"
* Host 5 (OpenSuse, power-on, webserver) 
  - host-5.example.org 86400 IN A 192.168.1.15
  - host-5.example.org 86400 IN TXT "os-suse"
  - host-5.example.org 86400 IN TXT "service-web"
  - host-5.example.org 86400 IN TXT "power-off"

## You can specify host(s) in command line

Whith use **-l SUBSET** or **--limit=SUBSET** option

Ansible help :
*-l SUBSET, --limit=SUBSET : further limit selected hosts to an additional pattern*

    ansible-playbook ping_all.yml -l os-debian

## You can specify host(s) in your playbook

* all Debian:
    ```
    - hosts: os-debian
    ```
* all Webserver:
    ```
    - hosts: service-web
    ```
* Webserver without Debian
    ```
   - hosts: service-web:!os-debian
    ```
* all, without offline
    ```
   - all:!power-off
    ```
* all:
    ```
  - all
    ```

## You can list hosts without run your playbook :

    ansible-playbook ping.yml --list-hosts

    ansible-playbook ping.yml -l os-debian --list-hosts
