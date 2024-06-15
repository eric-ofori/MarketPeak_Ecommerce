# Capstone Project: E-Commerce Platform Deployment with Git, Linux and AWS

### Scenario:

You have been assigned to develop an e-commerce website for a new marketplace named "MarketPeak". This platform will feature product listings, a shopping cart and user authentication. Your objective is to utilized Git for version control, develop the platform in a Linux environment and deploy it on an AWS EC2 instance.

## 1. Implementing Version Control with Git

### 1.1. Initialize Git Repository

+ Begin by creating a project directory name **_"MarketPeak_Ecommerce"_**
+ Inside your **_"MarketPeak_Ecommerce"_** directory, initialize a Git repository to manage your version control

```
mkdir MarketPeak_Ecommerce
cd MarketPeak_Ecommerce
git init
```

![Git_Initialize](/Img/Git_Initialize.png)

### 1.2. Obtain and Prepare the E-Commerce Website Template
+ Download the specific template from here: https://www.tooplate.com/view/2130-waso-strategy 
+ Extract the downloaded template into your project directory, **_"MarketPeak_Ecommerce"_**

![MarketPeak_Template](/Img/MarketPeak_Template.png)

### 1.3. Stage and Commit the Template to Git

+ Add your website files to the Git repository.
+ Set your Git global configuration with your username and email.
+ Commit your changes with clear and descriptive message

```
git add .
git config --global user.name "YourUsername"
git config --global user.email "youremail@example.com"
git commit -m "Initial commit with basic e-commerce site structure"

# Make sure to replace "YourUsername" with your actual GitHub username and "youremail@example.com" with your actual email used for your GitHub account
```
![Git_add_config_commit](/Img/Git_add_config_commit.png)

### 1.4. Push the code to your GitHub repository

***For versioning and collaboration purposes we need to push the code to a remote repository on GitHub***

+ Create a remote repository on GitHub by loggining into your GitHub account and creating a new repository name **_"MarketPeak_Ecommerce"_**. Leave the repository empty without initializing it with a README, .gitignore or license.

![MarketPeak_GitHub_Repo_1](/Img/MarketPeak_GitHub_Repo_1.png)

![MarketPeak_GitHub_Repo_2](/Img/MarketPeak_GitHub_Repo_2.png)

![MarketPeak_GitHub_Repo_3](/Img/MarketPeak_GitHub_Repo_3.png)

+ Link your local repository to GitHub by issuing the following command in your terminal within your project directory, add the remote reposity URL to your local repository configuration

```
git remote add origin https://github.com/your-git-username/MarketPeak_Ecommerce.git 		# Make usre to replace "your-git-username" with your actual git username
```
![Link_LocalRepo_To_GitHub](/Img/Link_LocalRepo_To_GitHub.png)

+ Push your code by uploading your local repository content to GitHub using this command:

```
git push -u origin main
```
![Link_LocalRepo_To_GitHub](/Img/Link_LocalRepo_To_GitHub.png)

You will realise that you get an error message like the one below:

**error: failed to push some refs to 'https://github.com/"YourUsername"/MarketPeak_Ecommerce.git'**
This is because the main branch does not exist locally and we need to correct that. Instead we are on a master branch which is not same as the main branch in our GitHub remote repository.

+ To Resolve this issue you need to verify the branch you are on using the following command:

```
git branch
```

![Verify_Branch](/Img/Verify_Branch.png)

+ Rename master branch to main using this command:

```
git branch -m master main
```
![Rename_Branch](/Img/Rename_Branch.png)

+ Now you can push your code by uploading your local repository content to GitHub using this command:

```
git push -u origin main
```

![GitPush_Main](/Img/GitPush_Main.png)


## 2. AWS Deployment

To deploy **_"MarketPeak_Ecommerce"_** platform, you'll start by setting up an Amazon EC2 instance:

### 2.1. Set Up an AWS EC2 Instance

+ Log in to the AWS Management Console

![AWS_Login](/Img/AWS_Login.png)

+ Launch an EC2 instance using an Amazon Linux AMI.

![EC2_Launch_1](/Img/EC2_Launch_1.png)

![EC2_Launch_2](/Img/EC2_Launch_2.png)

+ Connect to the instance using SSH

![SSH_Connect](/Img/SSH_Connect.png)

### 2.2 Clone the repository on the Linux Server

**Before deploying your e-commerce platform, you need to clone the GitHub repository to your AWS EC2 instance. This process involves authenticating with GitHub and choosing between two primary methods of cloning a repository: SSH and HTTPS. We will use the HTTPS method in cloning our repository**

1. Navigate to your repository in github console

2. Select the 'code' as in the image below:

![Link_LocalRepo_To_GitHub](/Img/Link_LocalRepo_To_GitHub.png)

HTTPS Method:

For repositories that you plan to clone without setting up SSH key, use HTTPS URL. GitHub will prompt for your username and password:

```
git clone https://github.com/yourusername/MarketPeak_Ecommerce.git
```

![Git_Clone_1](/Img/Git_Clone_1.png)

![Git_Clone_2](/Img/Git_Clone_2.png)

**After issuing the git clone command you realise there is an error. The error tells us that no such directory exists. This is because the git command cannot be executed. Since GIT as program has not been install the command cannot be executed. As such we need to install git before we can git clone the remote repository. We can do that by first updating our dependencies and then installing git using the following commnds:

```
sudo yum update -y
```

![Update](/Img/Update.png)

```
sudo yum git install -y
```

![Git_Install](/Img/Git_Install.png)

```
git -v
```

Now we can git clone our remote repository

```
git clone https://github.com/yourusername/MarketPeak_Ecommerce.git
```
![Git_Clone_Success](/Img/Git_Clone_Success.png)

![Git_Clone_Succes_Verification](/Img/Git_Clone_Succes_Verification.png)

### 2.3 Install a Web Server on EC2

**_Apache HTTP Server (httpd)_** is a widely used web server that serves HTML files and content over the internet. Installing it on Linux EC2 server allows you to host **_"MarketPeak_Ecommerce"_** site:

+ Install Apached web server on the EC2 instance. Note that httpd is the software name for Apache on systems using yum package manager:

```
sudo yum update -y
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```
![HTTPD_Install_1](/Img/HTTPD_Install_1.png)

![HTTPD_Install_2](/Img/HTTPD_Install_2.png)

### 2.4 Configure httpd for Website

+ To serve the website from the EC2 instance, **_httpd_** to point to the directory on the Linux server where the websier code files are stored, which is usually in **_/var/wwww/html

+ Prepare the web directory by clearing the default httpd web directory and copy MarketPeak_Ecommerce website to it

```
sudo rm -rf /var/www/html/*
sudo cp -r ~/MarketPeak_Ecommerce/* /var/www/html/
```

![MarketPeak_Standard_Directory](/Img/MarketPeak_Standard_Directory.png)

+ The directory **_/var/www/html/_** is a standard directory structure on Linux systems that host web content, particularly the **_Apache HTTPD Server_**

+ Reload httpd to apply the changes by reloading the httpd service

```
sudo systemctl reload httpd
```

![httpd_reload](/Img/httpd_reload.png)

### 2.5 Access Website from Browser

+ With **_httpd_** configured and website files in place, **_MarketPeak Ecommerce_** platform is now live on the internet:

	- Open a web browser and access the public IP of your EC2 instance to view the deployed website

+ You realize that the website is not connecting and as such you get "a site cannot be reached message".

+ To solve this issue we need to edit the inbound rules of the security groug for our AWS EC2 instance to allow http communication.

![Editing_Inbound_Rules_1](/Img/Editing_Inbound_Rules_1.png)

![Editing_Inbound_Rules_2](/Img/Editing_Inbound_Rules_2.png)

+ After editing the inbound rules of the AWS EC2 instance you can refresh your web browser and having the MarketPeak Ecommerce site up and running

![First_Deployed_Website](/Img/First_Deployed_Website.png)


## 3. Continuous Integration and Deployment Workflow

### Step 1: Developing New features and Fixes

+ Create a Developmnet Branch: Begin your development work by creating a separate branch. This isolates new features and bug fixes from the stable version of your website

```
git branch development
git checkout development
```
![Dev_branch](/Img/Dev_branch.png)

+ Implement changes on the development branch by adding new features or bug fixes. This might include updating web pages, adding new products or fixing known issues.

### Step 2: Version Control with Git

+ Stage the changes made to the staging area in Git by using this command:

```
git add .
```
![Dev_Git_Staging](/Img/Dev_Git_Staging.png)

+ Commit the changes by using the command below:

```
git commit -m "Add new features or fix bugs"
```

![Dev_Commit](/Img/Dev_Commit.png)

+ Push changes to the remote repository on GitHub:

```
git push origin development
```

![Dev_Push](/Img/Dev_Push.png)

### Step 3: On GitHub create a pull request to merge the development branch into the main branch. This is crucial for code review and maintaining code quality

![Pull_Request_1](/Img/Pull_Request_1.png)

![Pull_Request_2](/Img/Pull_Request_2.png)

+ Review the changes for any potential issues. Once you are satisfied that there are not conflicts or issues, merge the pull request into the main branch to incorporate the new features or fixes into the production codebase.

![Dev_Merge](/Img/Dev_Merge.png)

```
git checkout main
git merge development
```

+ Checkout of development branch and switch to the main branch

![Queer](/Img/Queer.png)

+ Push the merged changes to GitHub to ensure that your local branch, now containing the updates is pushed to the remote repository on GitHub

```
git push origin main
```

![Push_Reject](/Img/Push_Reject.png)

+ You realize that there is an error message that some refs failed to be pushed to the remote GitHub repository. This is because you have changes the remote repository that are not in the local repository. As such we have to pull or fetch the contents of the remote main branch to the local before pushing. You can do this in two ways:

1. Fetch and merge changes from remote origin
2. Rebase origin and main

We will go with Fetching and merging changes. You can do this by:

```
git fetch origin
```

If there are not conflicts then you can go ahead and merge origin with main. But if there are you will need to solve them.

![Git_Fetch_Main](/Img/Git_Fetch_Main.png)

```
git merge origin/main
```

![Git_Merge_Origin_Main](/Img/Git_Merge_Origin_Main.png)

```
git push origin main
```

![GitHub_Push_local_Main](/Img/GitHub_Push_local_Main.png)

### Step 4: Deploying Updates to the Production Server

+ Pull the latest chages to your production server by SSH into your AWS EC2 instance. Navigate to the website's directory and pull the latest changes from the main branch

```
git pull origin main
```

![Updates_Production_Server](/Img/Updates_Production_Server.png)

+ Restart the web server to apply the changes

```
sudo systemctl reload httpd
```

![Update_Server_Reload](/Img/Update_Server_Reload.png)

### Step 5: Testing the New Changes

Open a web browser and navigate to the EC2 instance public IP. Test the new features or fixes to ensure they work or have taken effect.

You will realize that the changes did not take event. This is because we have not copied the changes to the /var/www/html directory where the contents of our website is hosted. We need to do that by:

```
sudo rm -rf /var/www/html/*
sudo cp -r ~/MarketPeak_Ecommerce/* /var/www/html/
```

![Recopy_Var_www_html](/Img/Recopy_Var_www_html.png)

Then now you can restart the web server and open your web browser to see if the changes have taken effect

![site_change_1](/Img/site_change_1.png)

![site_change_2](/Img/site_change_2.png)

![site_change_3](/Img/site_change_3.png)

![site_change_4](/Img/site_change_4.png)

![site_change_5](/Img/site_change_5.png)


## 4. Capstone Submission

+ Provide a detailed README.md file documenting the steps you took to deploy the entire flow.

+ Include any troubleshooting or challenges you faced and how you overcame them.

![README_1](/Img/README_1.png)

![README_2](/Img/README_2.png)
