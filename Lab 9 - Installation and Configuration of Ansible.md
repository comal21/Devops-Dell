## Installation and Configuration of Ansible 

### Task 0: Connect to the existing CICD machine

### Task 2: Configure the Ansible Control Node

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
Install latest version of Python. 
```
sudo apt install python3-pip -y 
```
```
python3 --version
```
```
sudo pip3 install --upgrade pip
```
Install awscli, boto, boto3 and ansible. Boto/Boto3 are AWS SDK which will be needed while accessing AWS APIs
```
sudo pip3 install awscli boto boto3
```
```
sudo pip3 install ansible
```
```
pip show ansible
```
### Verify AWS CLI configuration
```
aws s3 ls
```
If you see an error, re configure using below steps or ignore.
Configure AWS CLI
```
aws configure
```
Use existing keys if not found create using following steps.
### Steps to Create and Download AWS Access Keys

* Go to the AWS Management Console.
* Click on your Profile Name (top right corner) → Select Security credentials.
* Scroll down to the Access keys section.
* Click Create access key.
* Select the use case : CLI.
* Click Next and then Create access key.

AWS provides:
Access Key ID (visible)
Secret Access Key (hidden)
* Click Download .csv file to save the keys securely.
⚠️ You cannot retrieve the secret key again after this step.

Configure AWS CLI
```
aws configure
```
Enter:
* AWS Access Key ID
* AWS Secret Access Key
* Default region - Hit Enter
* Output format - Hit Enter 

### Task 3: Automate Infrastructure Provisioning with Ansible

Create the Script file to launch 2 more instances.
```
vi ansible_script.yaml
```
Copy paste the below code & Save the file using "ESCAPE + :wq!"
```
---
- hosts: localhost
  connection: local

  tasks:
    - name: Execute curl command to get token
      shell: "curl -X PUT 'http://169.254.169.254/latest/api/token' -H 'X-aws-ec2-metadata-token-ttl-seconds: 21600'"
      register: TOKEN

    - name: Get region of instance
      shell: "curl -H 'X-aws-ec2-metadata-token:{{ TOKEN.stdout }}' http://169.254.169.254/latest/meta-data/placement/region/"
      register: region

    - name: Get AMI ID of instance
      shell: "curl -H 'X-aws-ec2-metadata-token:{{ TOKEN.stdout }}' http://169.254.169.254/latest/meta-data/ami-id"
      register: ami_id

    - name: Get keypair of instance
      shell: "curl -H 'X-aws-ec2-metadata-token:{{ TOKEN.stdout }}' http://169.254.169.254/latest/meta-data/public-keys/| cut -c 3-100 "
      register: kp

    - name: Get Instance Type of instance
      shell: "curl -H 'X-aws-ec2-metadata-token:{{ TOKEN.stdout }}' http://169.254.169.254/latest/meta-data/instance-type"
      register: instance_type


    - name: Get subnet id of instance
      shell: "curl -H 'X-aws-ec2-metadata-token:{{ TOKEN.stdout }}' -v http://169.254.169.254/latest/meta-data/network/interfaces/macs/$(curl -H 'X-aws-ec2-metadata-token:{{ TOKEN.stdout }}' -v http://169.254.169.254/latest/meta-data/network/interfaces/macs)/subnet-id"
      register: subnet

    - name: Get security group of instance
      shell: "curl -H 'X-aws-ec2-metadata-token:{{ TOKEN.stdout }}' -v http://169.254.169.254/latest/meta-data/network/interfaces/macs/$(curl -H 'X-aws-ec2-metadata-token:{{ TOKEN.stdout }}' -v http://169.254.169.254/latest/meta-data/network/interfaces/macs)/security-group-ids/"
      register: secgrp


    - name: Generate SSH keypair
      openssh_keypair:
        force: yes
        path: /home/ubuntu/.ssh/id_rsa

    - name: Get the public key
      shell: cat /home/ubuntu/.ssh/id_rsa.pub
      register: pubkey

    - name: Create EC2 instance
      ec2_instance:
        key_name: "{{ kp.stdout }}"
        security_group: "{{ secgrp.stdout }}"
        instance_type: "{{ instance_type.stdout }}"
        image_id: "{{ ami_id.stdout }}"         # "ami-0931978297f275f71"
        wait: true
        region: "{{ region.stdout }}"
        tags:
          Name: "{{ item }}"
        vpc_subnet_id: "{{ subnet.stdout }}"
        network:
         assign_public_ip: yes
        user_data: |
           #!/bin/bash
           echo "{{ pubkey.stdout }}" >> /home/ubuntu/.ssh/authorized_keys
      register: ec2var
      loop:
          - managed-node1
          - managed-node2

    - name: Make ansible directory
      file:
        path: /etc/ansible
        state: directory
      become: yes

    - debug:
        msg: "{{ ec2var.results[0].instances[0].private_ip_address }}"

    - debug:
        msg: "{{ ec2var.results[1].instances[0].private_ip_address }}"

```

Execute the script
```
ansible-playbook ansible_script.yaml
```
Verify the creation of the managed nodes on the AWS Management Console.

### Task 4: Set up the static inventory file

Once you get the ip addresses, do the following:

```
sudo vi /etc/ansible/hosts
```

Add the private IP addresses, by pressing "INSERT" 
```
managed-node1   ansible_ssh_host=<private-ip-managed-node1>  ansible_ssh_user=ec2-user

managed-node2   ansible_ssh_host=<private-ip-managed-node2>  ansible_ssh_user=ec2-user
 
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

