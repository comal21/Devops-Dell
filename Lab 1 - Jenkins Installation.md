# Lab 1: Install Jenkins

### Task-0: The first step is to `Manually Launch an EC2 Server` with the below configuration:

* **Name:** `CICDLab`
* **AMI Type and OS Version:** `Ubuntu 24.04 LTS`
* **Instance type:** `t2.micro`
* Create a new Keypair with the Name `CICDLab-Keypair`
* In security groups, include ports `22 (SSH)` , `80 (HTTP)` and `8080 (jenkins)`
* **Configure Storage:** 10 GiB
* Click on `Launch Instance.`

### Task-1: Install Java
Once the Anchor EC2 server is up and running, SSH into the machine using `Instance Connect` with the username `ubuntu` and do the following:
```
sudo hostnamectl set-hostname CICDLab
bash
```
```
sudo apt update
```
```
sudo apt install wget unzip -y
```
```
sudo apt install fontconfig openjdk-17-jre
```
```
java -version
```
### Task-2: Install Jenkins

```
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc   https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```
```
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]"   https://pkg.jenkins.io/debian-stable binary/ | sudo tee   /etc/apt/sources.list.d/jenkins.list > /dev/null
```
```
sudo apt-get update
```
```
sudo apt-get install jenkins
```
```
sudo systemctl enable jenkins
```
```
sudo systemctl start jenkins
```
```
sudo systemctl status jenkins
```
### Task-3: Configure Jenkins Server:


Get the **Initial Password** for Jenkins from the below path.
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
   (**Example:** "afbe8d33e25b4b908c0b9f91546f09e6")

1. Now, go to the **Web Browser** and enter the Jenkins URL as shown: **http://< Jenkin's Public IP>:8080/**
2. Under Unlock Jenkins, enter the above **initialAdminPassword** & click **Continue**.
3. Click on **Install suggested Plugins** on the Customize Jenkins page.
4. Once the plugins are installed, it gives you the page where you can create a New **Admin User**. 
5. Enter the **User Id** and **Password** followed by **Name and E-Mail ID** then click on **Save & Continue**.
6. In the next step, on the Instance Configuration Page, verify your **Jenkins Public IP** and **Port Number** then click on **Save and Finish**
