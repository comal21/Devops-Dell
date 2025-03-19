# Lab: Setting up automated CI pipeline using Jenkins, Maven & GitHub WebHooks 

### Task-0: Fork the repository
1. Open the URL in the browser: "https://github.com/comal21/my-app.git"
2. Click on Fork repository in your github.

### Task-1: Create a maven project
1. Click on **Manage Jenkins** > **Plugins** > **Available Plugins** tab and search for **Maven**.
2. Select the "**Maven Integration**" Plugin, "**Github Integration**" and "**Unleash Maven**" Plugin and click **Install** (without restart).
3. Once the installation is completed, click on **Go back to the top page**.
4. On Home Page select **Manage Jenkins** > **Tool**.
5. Inside Tool Configuration, look for **Maven installations**, click **Add Maven**. 
6. Give the **Name as "Maven"**, choose **Version as 3.9.5**, and **Save** the configuration.
7. Now you need to make a project for your application build, that selects **New Item** from the Home Page of Jenkins
8. Enter an item name as **hello-world** and select the project as **Maven Project** and then **click OK.**
   ( You will be prompted to the configure page inside the hello-world project.)
9. Go to the "**Source Code Management**" tab, and select Source Code Management as **Git**, Now you need to provide the GitHub Repository **Master Branch URL**
![image](https://github.com/user-attachments/assets/5c3a4c0c-70d5-4652-bdcc-082cbd61f1d7)
![image](https://github.com/user-attachments/assets/6e6b92e0-a12d-47be-a0d5-e0e48365b74a)

11. Go to **Build Triggers** and enable the **GitHub hook trigger for GITScm polling**.
![image](https://github.com/user-attachments/assets/80122eb0-efb1-478a-b58c-dd4a17f8178f)

13. Keep all the other values as default and select the "**Build**" Tab and inside Goals and options write "**clean package**" and **save** the configuration.
#### Note: The 'clean package' command clears the target directory, Builds the project, and packages the resulting WAR file into the target directory.
![image](https://github.com/user-attachments/assets/85c3ac8a-4783-4884-b104-731f6bdd4516)

### Task-2: Create a github webhook
1. Go to your **GitHub website**, and inside the **hello-world** repository > **Settings** > **Webhooks** and Click on the **Add Webhook**.
2. Now fill in the details as below.
#### Payload URL Example: 
* http://< jenkins-PublicIP >:8080/github-webhook/ (**Example:** http://184.72.112.155:8080/github-webhook/)
* **Content type:** application/JSON
 ![image](https://github.com/user-attachments/assets/fdefc23b-7a56-4c3a-aef3-5d177f355c06)

Then, Click on **Add Webhook**.
3. Now, Make a minor change and commit in GitHub's **hello-world repository** by editing **hello.txt** file.
4. As the source code gets changed, Jenkins gets triggered by the WebHook and starts building the new source code.
5. Go to Jenkins, and you can see a build is happening automatically.
6. Observe the successful build on the Jenkins page.

* You can go to **Workspace** > **dist** folder to see that the **.war** file is created there.
* war file will be created in **/var/lib/jenkins/workspace/hello-world/target/**
![image](https://github.com/user-attachments/assets/e9ff0483-e9f8-4a7b-939e-295f37d37c81)
