## Installation and Configuration of Ansible 

### Task 0: Connect to the existing CICD machine

### Task 1: Configure the Ansible Control Node

Set the hostname as 'Control-Node'. 
```
sudo hostnamectl set-hostname control-node
```
Refresh the Shell
```
bash
```
Update the package repository with latest available versions
```
sudo apt update
```
```
sudo apt install ansible-core
```
```
ansible --version
```
### Verify AWS CLI configuration
```
aws s3 ls
```

### Task 2: Automate Infrastructure Provisioning with Ansible

Create the Script file to launch 2 more instances.
```
vi ansible_script.yaml
```
Copy paste the below code & Save the file using "ESCAPE + :wq!"
```
---
- name: This play will install apache web servers on all the target nodes
  hosts: all
  become: yes
  tasks:
    - name: Task1 will install httpd using apt
      apt:
        name: apache2
        #local cache of the package information available from the repositories configured on the system
        update_cache: yes
        state: latest
    - name: Task2 will upload custom index.html into all hosts
      copy:
        src: /home/ubuntu/index.html
        dest: /var/www/html/index.html
    - name: Task3 will setup attributes for file
      file:
        path: /var/www/html/index.html
        owner: www-data
        group: www-data
        mode:  0644
    - name: Task4 will start the httpd
      service:
        name: apache2
        state: started
```
Execute the script
```
ansible-playbook ansible_script.yaml
```
Verify the creation of the managed nodes on the AWS Management Console.

### Task 3: Set up the static inventory file

Once you get the ip addresses, do the following:

```
sudo vi /etc/ansible/hosts
```

Add the private IP addresses, by pressing "INSERT" 
```
managed-node1   ansible_ssh_host=<private-ip-managed-node1>  ansible_ssh_user=ubuntu

managed-node2   ansible_ssh_host=<private-ip-managed-node2>  ansible_ssh_user=ubuntu
 
```
Save the file using "ESCAPE + :wq!"

List the managed nodes
```
ansible all --list-hosts
```

### Task 5:  SSH into each of the managed nodes and set the hostname.
```
ssh ubuntu@< Replace Node 1 IP >
```
```
sudo hostnamectl set-hostname managed-node1
```
```
exit
```
```
ssh ubuntu@< Replace Node 2 IP >
```
```
sudo hostnamectl set-hostname managed-node2
```
```
exit
```

Use ping module to check if the managed nodes are available.
```
ansible all -m ping
```

