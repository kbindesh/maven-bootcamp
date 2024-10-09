# Setup Apache Maven on Amazon Linux 2023

## Step-01: Prerequisites

- Java
- Git

### 1.1 Installing and Configuring JDK

```
# Installing JDK 17
yum install -y java-17-amazon-corretto.x86_64

# Check java version
java --version
```

- Now, set the Java home path

```
# Set JAVA_HOME path
find /usr/lib/jvm/java* | head -n 3
[Copy the java path from the preceding command as we'll need it in the next step]

# Update the java home path in bash_profile file
vi ~/.bash_profile

# Create a new variable JAVA_HOME
JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto.x86_64

# Add JAVA_HOME to the existing path
PATH=$PATH:$HOME/bin:$JAVA_HOME
```

- To verify the java path

```
source ~/.bash_profile

echo $PATH
[The preceding command should display java path along with other program paths]
```

### 1.2 Installing Git

```
sudo yum install -y git
```

## Step-02: Setting-up Apache Maven

### 2.1 Download Apache Maven

```
# Move to /opt directory
cd /opt

# Download the maven binary
wget https://dlcdn.apache.org/maven/maven-3/3.9.8/binaries/apache-maven-3.9.8-bin.tar.gz

# Unzip the downloaded maven tarball
tar -xvzf apache-maven-3.9.8-bin.tar.gz

# List all the file to see the unzipped maven directory (apache-maven-3.9.8)
cd apache-maven-3.9.8
ls -l
```

### 2.2 Setup home path for Apache Maven

```
# Update the bash profile with maven path
vi ~/.bash_profile

# Create M2_HOME and M2 variable with maven location specs
M2_HOME=/opt/apache-maven-3.9.8
M2=/opt/apache-maven-3.9.8/bin

# Update the PATH variable | Add Maven path
PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_HOME:$M2

[Save the file and exit]
```

- To verify the apache maven path

```
source ~/.bash_profile

echo $PATH
[The preceding command should display maven path along with other program paths]
```

### 2.3 Check Maven version

```
# Navigate to any other random directory and check version
mvn --version

[The preceding command should display the current installed version of maven]
```
