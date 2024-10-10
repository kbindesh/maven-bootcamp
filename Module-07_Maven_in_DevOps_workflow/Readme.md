## Understanding Maven in DevOps Workflow

## Prerequisites

- A Linux machine (EC2 Instance)
- Java
- Jenkins
- Maven
- Git
- IDE (in this lab, Visual Studio Code)

## Step-xx: Create a new Maven based Java project using IDE (locally)

- You may refer [Module-04_Create a Maven Project](../Module-04_Create_a_Maven_Project/Readme.md)

## Step-xx: Compile & Validate the Application using Maven

```
# Validate the maven project | Ensure all the required files & folders are present
mvn validate

# Compile the source code | Check for any syntactical errors and convert the code (.java) to .class
mvn compile
```

## Step-xx: Create a new GitHub Repository for Maven project

- Navigate to https://GitHub.com/ >> Sign-in
- Select **Repositories** tab >> click **New** button

  - **Repository Name**: mvn-sample-project
  - **Description**
  - **Scope**: Private

- Click **Create repository** button

## Step-xx: Push the Maven project code to GitHub

### GitHub `Username & Password based Authentication` to push code

- Launch **VS Code** >> Open your maven project >> **View** menu -> **Terminal** >> Get inside the project folder path.
- **Initialize the project folder as a git repo**

```
git init
```

- Stage the changes

```
git add .
```

- **Commit the changes (on default (master) branch)**

```
git commit -m "Application version 1.0"
```

- **Set the remote Github repo origin**

```
# Syntax
git remote add <alias> <your_github_repo_url>

# Example
git remote add origin https://github.com/kbindesh/mvn-webproject.git
```

- **(Optional) Rename a branch**

```
git branch -M <new_branch_name>

# Example
git branch -M main
```

- **Push the changes to Github (remote origin)**

```
git push -u <remote_origin_alias> <branch_name>

# Example
git push -u origin main
```

- If prompted, enter the username and password of your GitHub account.

- Verify the application code from GitHub repository.

### (Alternative approach) GitHub `Token based authentication` to push code to GitHub

#### Generate GitHub Personal Access Token (PAT)

- Sign in to your GitHub account.
- Click on your profile picture in the upper-right corner >> **Settings** >> **Developer settings** >> **Personal Access Token**.
- Click Generate token.
- Copy the token to your clipboard.

**Note**: You won’t be able to see this token again, so store it securely somewhere.

#### Configure git to use your GitHub token

- To authenticate Git operations with your token, you need to update the URL of your GitHub repository to include the token.

```
# Navigate to your project directory
cd <maven_project_folder>

# Update the remote repo (origin) URL
git remote set-url origin https://<TOKEN>@github.com/<USERNAME>/<REPOSITORY>.git

[In the preceding command, replace <TOKEN> with your actual Github token, USERNAME with your GitHub username, and REPOSITORY with the name of your repository.]
```

## Step-xx: Setup Jenkins Server

### Create an EC2 Instance for `Jenkins server`

- Sign-in to AWS Account (https://console.aws.amazon.com/).
- Navigate to EC2 service >> **Launch Instance**.
- **Name**: Jenkins-Server
- **AMI**: Amazon Linux 2 (Kernel 5.10)
- **Instance Type**: t2.micro
- **Key Pair**: <create_new_keypair>
- **VPC/Subnet**: Default
- **Elastic IP**: Enable
- **Security Group**: <create_new_sg>
  - **Ingress**: Allow - 22 (SSH), 8080 (Jenkins)
- **Storage**: 20 GB, GP2 (for this lab)
- Click on **Launch Instance** button

### Install & Configure Java (prerequisite for Jenkins)

- Official link for java download: https://www.oracle.com/in/java/technologies/downloads/

```
# Install Java as a pre-requisites for jenkins installation
sudo yum install -y java-17-amazon-corretto.x86_64

# To verify java installation
java --version
```

### Install & Configure Jenkins

- Official website link for Jenkins download: https://www.jenkins.io/doc/book/installing/linux/

```
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

# Update all the installed packages
sudo yum -y upgrade

# Install Jenkins
sudo yum install -y jenkins

# Start the Jenkins service
sudo systemctl start jenkins

# Enable the Jenkins service
sudo systemctl enable jenkins

# To check Jenkins service status
sudo systemctl status jenkins

# To check the current installed version of Jenkins
jenkins --version
```

### Accessing Jenkins Dashboard

- Launch browser (Chrome) on your local system >> Navigate to http://<your_jenkinsserver_public_ip>:8080
- Unlock Jenkins by entering temporary password

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

- Setup and Install suggested Jenkins plugins
- Create the first Admin user

  - Username
  - Passwsord
  - Confirm Password
  - Full Name
  - E-mail Address

### Install Git

```
sudo yum install -y git
```

### Install Jenkins Plugins

- Navigate to the Jenkins Dashboard >> **Manage Jenkins** >> **Plugins** >> Select **Available** tab
- Search following plugins and install it:

  - **Maven Integration**
  - **Maven Invoker**
  - **Github Plugin**

- Once all the required plugins as installed, verify it from the **Installed** tab of plugin manager.

## Step-xx: Setup Maven Server

### Create an EC2 Instance for `Maven Build server`

- Sign-in to AWS Account (https://console.aws.amazon.com/).
- Navigate to EC2 service >> **Launch Instance**.
- **Name**: maven-build-server
- **AMI**: Amazon Linux 2 (Kernel 5.10)
- **Instance Type**: t2.micro
- **Key Pair**: <create_new_keypair>
- **VPC/Subnet**: Default
- **Elastic IP**: Enable
- **Security Group**: <create_new_sg>
  - **Ingress**: Allow - 22 (SSH) from Jenkins server
- **Storage**: 10 GB, GP2 (for this lab)
- Click on **Launch Instance** button

### Install & Configure `Java`

```
# Switch to sudo user
sudo su -

# Install Java as a pre-requisites for jenkins installation
sudo yum install -y java-17-amazon-corretto.x86_64

# To verify java installation
java --version
```

- **Setup home path for Java**

```
find /usr/lib/jvm/java* | head -n 3
[From the preceding command, copy "/usr/lib/jvm/java-17-amazon-corretto.x86_64" path]

vi ~/.bash_profile

# Create a new variable JAVA_HOME
JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto.x86_64

# Add JAVA_HOME to the existing path
PATH=$PATH:$HOME/bin:$JAVA_HOME
```

- **Verify the Java path**

```
echo $PATH
[The preceding command will give you the updated PATH]

# In order to refresh the path
source ~/.bash_profile

# Again, display the PATH to get the updated values
echo $PATH
```

### Install & Configure `Apache Maven`

- Download Apache maven from the official website: https://maven.apache.org/download.cgi

- **Download and configure Maven**

```
# Move to /opt directory
cd /opt

# Download the maven binary
sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.8/binaries/apache-maven-3.9.8-bin.tar.gz

# Unzip the downloaded maven tarball
sudo tar -xvzf apache-maven-3.9.8-bin.tar.gz

# List all the file to see the unzipped maven directory
sudo ls -l apache-maven-3.9.8

# Get inside the maven home directory
sudo cd apache-maven-3.9.8
```

- **Setup home path for Maven**

```
# Update the bash profile with maven path
vi ~/.bash_profile

# Create M2_HOME and M2 variable with maven location specs
M2_HOME=/opt/apache-maven-3.9.8
M2=/opt/apache-maven-3.9.8/bin

# Update the PATH variable | Add Maven path
PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2

[Save the file and exit]

source ~/.bash_profile

# Verify the PATH with java and maven variables
echo $PATH
```

## Step-xx: Add Maven Build Server as a Jenkins Agent

### Create a new user on Maven build server for Jenkins communication

- Connect to your maven server

```
# Switch to sudo user
sudo su -

# List all the existing users
cat /etc/passwd

# Create a new user
useradd jenkins

# Set the password for jenkins user
passwd jenkins

# Add the jenkins user to the sudoers file
visudo

[Press "G" to go to the end of the file and press "i" to go in insert mode]

## Allow root to run any command anywhere
root    ALL=(ALL)     ALL
jenkins ALL=(ALL)     NOPASSWD: ALL
```

- Enable password based authentication

```
vi /etc/ssh/sshd_config

[Search for PasswordAuthentication]

# Uncomment the line
PasswordAuthentication yes

# Refresh sshd service
service sshd reload
```

### Create a new node in Jenkins

- Open Jenkins server's Dashboard >> Manage Nodes and Cloud >> New Node
- **Node Name**: maven_build_server
- **Permanent Agent**: Enable
- **# of executors**: 5
- Remote Root Directory: /home/jenkins
- Launch Method: Launch Agent via SSH
  - Host: <private_ip_of_the_maven_server>
  - Credentials >> Add
    - Username: jenkinsuser
    - Password: <your_jenkins_user_passwd>
    - ID: jenkins
  - Select the created credentials from the dropdown list.
- Host key verification strategy: Non verifying verification strategy

### Verify the connection with Maven build agent

- Jenkins Dashboard >> Manage Jenkins >> Nodes >> maven_build_server
- You should see agent added without a warning sign. Also check in the logs.

## Step-xx: Configure GitHub Webhook with Jenkins server for CI

## Step-xx: Build a Java project on Maven server (Jenkins agent)

### Configure Global Configuration settings for Java, Maven and Git

- Navigate to Jenkins server dashboard >> **Manage Jenkins** >> **Tools**

- **JDK**

  - Name: Java-17
  - JAVA_HOME: /usr/lib/jvm/java-17-amazon-corretto.x86_64

- **Git**

  - Name: Default
  - Path to git executable: /usr/bin/git

- **Maven** - location of maven installation on maven build agent (not on jenkins server)
  - Name: Maven_3.9.8
  - MAVEN_HOME: /opt/apache-maven-3.9.8

### Create a Jenkins job

- **Name**
- **Type**: Maven project
- **Restrict where this project can be run**: Enable and mention the name of maven build agent "maven_build_server"
- **Source Code Management**
  - Select Git
    - Repository URL
    - Branch to build: <branch_name>
- **Build**
  - Maven Version
  - Root POM: pom.xml
  - Goals and Options: clean install

## Step-xx: Build a war file on Maven server (Jenkins agent)
