# Basic Git Usage

## Global Configuration: 

- git config --global user.name "your username"
- git config --global user.email "your email address"
- git config user.name: check global config user name
- git config user.email: check global config user email

## Simple Usage Example In Windows:

**Assume:** You have a directory **D:\GitProjects\learn-basic-git** with 2 files (**file1.txt**, **file2.txt**). 

Open Command Prompt, enter the command below

```bash
pushd D:\gitprojects\learn-basic-git
```

Type **ls** list all files inside

```bash
ls
```

By default, your directory is not tracked by git, type **git init** to initialize an empty git repository in your machine.

```bash
git init
```

Now you can check the status in your directory by type **git status**

```bash
git status
```

Add **file1.txt** to **git staging**, type **git add file1.txt** (a single file)

```bash
git add file1.txt
```

You can also add all file in the directory to **git staging** by **git add -a** or **git add .**

```bash
git add .
```

You can also remove the file from staging by **git rm —cached file1.txt**

```git
git rm --cached file1.txt
```

When your files have been added to **git staging**, you can commit it to your **git repository** by **git commit -m "message of your commit".

```git
git commit -m "Initial Commit"
```

### Summary

```git
git init
git status
git add .
git commit -m "Initial Commit"
```



## Add Remote To Your Git Repository To Github or Any Git Server

Go to [Github](https://github.com/join) and sign up your **git account**
Create a new repository with any name
Find **remote url**

Use **git remote add origin your_github_remote_url**, to remote to your repository in Github with **origin** name(could be any name)

```git
git remote add origin your_github_remote_url
```

Use **git remote** to show remote name

```git
git remote
```

Use **git remote -v** to show the remote of origin's urls

```git
git remote -v
```

Use **git push origin master** to push request to repository branch 'master' to your remote git server

```git
git push origin master
```



## Pull Back To Your Machine, If You Already Have Your Remote Repository

Use **git pull origin master** to pull request to your local repository branch 'master'

```git
git pull origin master
```

Use **git checkout -b [branch_name]** switch branch in repository

```git
git checkout -b new_branch
```


## use git reset –hard <commit-hash> to set the current branch HEAD to the commit you want
```git
git reset --hard cedc856
git push --force origin master
```

