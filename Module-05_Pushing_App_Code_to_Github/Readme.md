# Pushing the Maven project code to VCS

## Prerequisites

- App code (created & validated using maven)
- Git

## Step-01: Create a new Github repository

- Navigate to https://github.com/ >> Sign-in
- Select **Repositories** tab >> click **New** button

  - **Repository Name**: mvn-sample-project
  - **Description**
  - **Scope**: Public

- Click **Create repository** button

## Step-02: Push the maven project to Github repository

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
