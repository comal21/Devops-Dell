# Lab: Create a maven project in Jenkins

### Task-0: Fork the repository
1. Open the URL in the browser: "https://github.com/Sruti1512/my-app"
2. Click on Fork repository in your github.

### Task-1: Create a maven project
1. Click on **Manage Jenkins** > **Plugins** > **Available Plugins** tab and search for **Maven**.
2. Select the "**Maven Integration**" Plugin and "**Unleash Maven**" Plugin and click **Install** (without restart).
3. Once the installation is completed, click on **Go back to the top page**.
4. On Home Page select **Manage Jenkins** > **Tool**.
5. Inside Tool Configuration, look for **Maven installations**, click **Add Maven**. 
6. Give the **Name as "Maven"**, choose **Version as 3.9.5**, and **Save** the configuration.
7. Now you need to make a project for your application build, that selects **New Item** from the Home Page of Jenkins
8. Enter an item name as **hello-world** and select the project as **Maven Project** and then **click OK.**
   ( You will be prompted to the configure page inside the hello-world project.)
9. Go to the "**Source Code Management**" tab, and select Source Code Management as **Git**, Now you need to provide the GitHub Repository **Master Branch URL** and **GitHub Account Credentials**.
10. In the Credentials field, you have to click **Add** and then click on **Jenkins**.
11. Then you will be prompted to the **Jenkins Credentials Provider** page. Under Add Credentials, you can add your **GitHub Username**, **Password**, and **Description**. Then click on **Add**.
12. After returning to the Source Code Management Page, click on Credentials and Choose your **GitHub Credentials**.
13. Keep all the other values as default and select the "**Build**" Tab and inside Goals and options write "**clean package**" and **save** the configuration.

#### Note: The 'clean package' command clears the target directory, Builds the project, and packages the resulting WAR file into the target directory.

14. Now, you will get back to the Maven project "**hello-world**" and click on "**Build Now**" to build the **.war** file for your application

* You can go to **Workspace** > **dist** folder to see that the **.war** file is created there.
* war file will be created in **/var/lib/jenkins/workspace/hello-world/target/**
