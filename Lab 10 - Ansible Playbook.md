## Implementing Ansible Playbook

`An Ansible Playbook is a YAML-based automation script that defines a set of tasks to configure, deploy, and manage systems in a repeatable and declarative manner.`

```
mkdir ansible-labs && cd ansible-labs
```

### Task 1: Write a Playbook to Install & Setup Apache (Web Server)

Create the playbook
```
vi install-httpd.yml
```
Add the given content, by pressing "INSERT"
```
---
- name: This play will install apache web servers on all the hosts
  hosts: all
  become: yes
  tasks:
    - name: Task1 will install apache2 using apt
      apt:
        name: apache2
        #local cache of the package information available from the repositories configured on the system
        update_cache: yes
        state: latest
    - name: Task2 will upload custom index.html into all hosts
      copy:
        src: /home/ubuntu/ansible-labs/index.html
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
Save the file using `ESCAPE + :wq!`

Now lets create an index.html file to be used as the landing page for the web server.
In task2 of the above playbook, this 'index.html' will be copied to the document root of the 
apache2 server.
```
vi index.html
```

Add the given content, by pressing "INSERT" 
```
<html>
  <body>
  <h1>Welcome to Ansible Training</h1>
  </body>
</html>
```
Save the file using `ESCAPE + :wq!`

Now run the playbook so that it installs apache2 to all the hosts and index.html is copied from 
the ansible-control
```
ansible-playbook install-httpd.yml 
```

```
curl <private_ip of node1> 
```

```
curl <private_ip of node2>
```

### Task 2: Uninstall httpd web server

```
vi uninstall-httpd.yml
```
Add the given content, by pressing "INSERT"
```
---
- name: This play will uninstall apache web server on all target nodes
  hosts: all
  become: yes
  tasks:
    - name: Task1 will uninstall apache2 using apt
      apt:
        name: apache2
        #local cache of the package information available from the repositories configured on the system
        update_cache: yes
        state: latest
```
Save the file using `ESCAPE + :wq!`
Now run the playbook 
```
ansible-playbook uninstall-httpd.yml
```
```
curl <private_ip of node1> 
```

```
curl <private_ip of node2>
```
![image](https://github.com/user-attachments/assets/c8ffc3c9-9051-4bb2-a312-94b4ce50f0b8)

