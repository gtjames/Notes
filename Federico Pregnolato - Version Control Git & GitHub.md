# Version Control: Git & GitHub
[GitHub Repository](https://github.com/fedpre/master-git-class)

## Introduction
Lessons divided into three clusters:
1. L1 -> L3: Intro to version control and Git
2. L4 -> L8: Project
3. L9 -> L10: GitHub Pages & Features
4. L11 -> Conclusions

Learn the basics of version control, Git, work on a project, be comfortable with Git and GitHub

### L1: What is version control?
Version control is a method of recording changes made to a file or files. Recording changes allows you to go back to a specific versions on the record. 
This is extremely useful when multiple changes on files have been made to implement a new feature and you want to go back to a previous release.

Two types of version control:
1. Centralized
	+ The codebase lives on a server and you and your teammates make changes to the files on the server
2. Distributed
	+ The codebase is on a server but can be cloned by everyone on the team. You work on the code on your computer and then send the changes to the codebase on the server once you are ready

### L1: Intro to Git
+ Git is a distributed version control system
+ Developed byt Linus Torvald (same developer as Linux)
+ GitHub is a code sharing & publishing service - LinkedIn for code!

#### Git vs. GitHub

| Git | GitHub |  
| ----------- | ----------- |  
| Runs on the Terminal | Collection of repos from developers |  
| Keep tracks locally | Clone, pull requests, and fork features |

### L2 - Installing Git
Visit the [Git website](https://git-scm.com/)
Click on "Downloads"
Choose your operative system:
+ [MacOS](https://git-scm.com/download/mac)
+ [Windows](https://git-scm.com/download/win)
+ [Linux](https://git-scm.com/download/linux)

### L2 - Configure Git
Have your GitHub username and email address associated with your GitHub account with you as you configure Git on your computer
```bash
git config --global user.name "fede" 
```
```bash
git config --global user.email "fede@gmail.com"
```
```bash
git config --global color.ui "auto"
```

#### Configure the SSH for GitHub**
What is [[SSH]]?

SSH stands for Secure Shell. It uses **RSA public key encryption**. . 

If you setup up an SSH key, you don't have to put your password each time you send a change to your GitHub account. Setting SSH for GitHub involves generating a public/private key pair and adding the **PUBLIC KEY** to GitHub.

##### For Windows:
Download [Putty](https://www.putty.org/)

##### On MacOs and Linux, type this command: 

```bash
ssh-keygen -t rsa -C fede@gmail.com
```

Follow the instructions and choose a passphrase (password).
Now we need to store our private key to an SSH agent. Type the following command to start the SSH agent:
```bash
eval "$(ssh-agent -s)"
```

Add the key to the agent with this command:
```bash
ssh-add ~/.ssh/id_rsa
```
Type in the passphrase that you chose earlier.
Congrats! We just generated a public/private key pair.

##### Add the public key to the GitHub account
Go to your root folder -> Open the .ssh folder (press cmd+shift+. to show hidden folders)
Open the *id_rsa.pub* file
Copy everything (make sure not to have whitespaces)

Go to your GitHub account and click on *Settings* -> *SSH and GPG Keys*
Click on *New SSH key*
Add the title of the key (ex. Adding Public Key) and paste the content of the .pu file into the Key section.
After completed, click on *Add SSH key*

### L3- Git Theory
Three main **file states** in Git
1. Modified state
	+ State of files in the working directory that you are currently working on. Some changes have happened and more could happen.
2. Staged state
	+ State of files in the stages area that you have modified and added to the staging area for Git to take a spanshot of. 
3. Commited state
	+ State of files when Git has taken a snapshot of the current state and are stored in the repo now

Three main **section** of the Git Project (folder)
1. Working Directory (Modified)
2. Staging Area (Staged)
3. Repository (Commited)

Structure of Git commands
`Git <action keyword>`

For example:
```bash
git add
git commit
git push
```

#### Basic Git workflow
**Workflow**: Sequence of steps to complete a task

1. Modify files in the working directory
```bash
git add
```
3. Add files to the staging area
```bash
git commit
```
5. Commit files to the repository
```bash
git push
```

### L4 - Add, Commit, and Push
Open the terminal and navigate to the folder you want to initialize as a Git repository

Initialize the Git repo:
```bash
git init 
```

Sync your local repo with the one on GitHub -> Go to the GitHub website and copy the link of your repo.
Add the remote origin to your local Git repo
```bash
git remote add origin https://github.com/fedpre/master-git-class.git
```

Check if you the remote origin has been added
```bash
git remote -v
```

Check the current status of your working directory
```bash
git status
```

Add all the files to the staging area
```bash
git add .
```

Take the snapshot of the working folder
```bash
git commit -m 'Initial commit. pushing theme over to GitHub'
```

Push the current state of the file to GitHub
```bash
git push -u origin main
```

Congrats! Everything is on GitHub

Check the history of your commits
```bash
git log
```

If you want to have everything on one line
```bash
git log --pretty=oneline
```


### L5 - Undoing a Staged Change

What if I staged a file (or multiple files) but I want to undo the action? Type:

```bash
git reset HEAD index.html
```

### L5 - Undoing a Commited Change

How to undo a commit:

```bash
git reset --soft HEAD~1
```
The `--soft` deletes the commit but leaves the file unchanged
The `HEAD~1` refers to the commit before our first mistake. It resets our HEAD to one commit before our mistake

After you undo the commit, the file is still staged, let's unstage it also
```bash
git reset HEAD index.html
```

### L5 - Undoing a Pushed Change

How to revert a mistaken push:
```bash
git revert HEAD 
```

It opens an editor where you can change the commit message. 
+ If vim press `:q!`

What this option does is that it creates a second commit on the commit before you commited the mistake. 

After that, to change the message on GitHub type:

```bash
git push -u origin main
```

### L5 - Git Diff

`git diff` will display the differences between the working directory and the index

```bash
git diff index.html
```

### L6 - Fetch and Merge Changes

To get changes from the remote repo, run
```bash
git fetch
```

to apply the changes, type:
```bash
git merge origin/main
```

### L6 - Git Pull

`git pull` merges the two previous commands (fetch & merge) in one command. 

```bash
git pull origin main
```

### L6 - Avoid Rejected Push Requests

Commons reasons why a git push/pull can be rejected. For example, your collegue has pushed changes to the repo but you haven't pulled before making your changes. When you try to push, you get an error that says that you need to pull first.

*Hint*: If you only do `git pull origin main` it won't work. That's the command you need to run to get the change and keep your commit:

```bash
git pull origin main --rebase
```
This way, you will pull the latest version and you are ready to push your change.
Run this command to check that everything is on order:

```bash
git log --pretty=oneline
```

### L6 - Work through a Failed Pull Request

Common reason why it happens is if you have conflicting changes on your files - two of you have edited the same lines of code.

After you do your git commit, in order to push you always need to pull first to see if there are any changes. If you get an error, run this command:

```bash
git pull origin main --rebase
```

and you will get the error in your editor.
After you talk to your teammate and fix the issue, save the file and type:

```bash
git rebase --continue
```

After this command, you only need to push again by typing:

```bash
git push -u origin main
```

### L7 - Introduction to Branches in Git

#### Branches
+ A Git branch represents an independent line of development
+ It allows you to try out new things in a separate environment
+ Each branch is associated with its own unique changes
+ **Main** is the main branch
+ **HEAD** refers to the tip of the branch you are on

### L7 - Organazing Your Branches
List all the branches on your repository
```bash
git branch -a
```

Add a new branch
```bash
git branch featureA
```

Switch to a different branch
```bash
git checkout featureA
```

Combine the creation of a new branch and switching to it
```bash
git checkout -b featureC
```

Rename a branch
```bash
git branch -m Extra BranchToDelete
```

Delete a branch (make sure to switch to a different branch from the one you want to delete)
```bash
git branch -d BranchToDelete
```

### L7 - Pushing to Branch featureA

Make sure to switch to the branch before making any edits by typing:
```bash
git checkout featureA
```

After running add and commit, make sure to push to the name of the branch and not main
```bash
git push -u origin featureA
```

### L7 - Pulling from Branch featureA

Make sure to pull from the branch and not main
```bash
git pull origin featureA
```

### L7 - Merging Branches

Git `merge` command merges the branch passed as argument into your current branch.

Make sure to checkout on the main (or branch receiver) of the marge.
For example, if you want to merge `featureB` to `main`, run the comand `git checkout main` before running the `git merge` command.

Differnt types of merges:
1. Fast forward merge
	1. This happens if the main branch hasn't diverged - no further commits have occurred on the main branch - git will point to the latest commit on the branch after the merge. All the commits from the branch are present in `main`
2. No fast forward merge `git merge --no-ff`
	1. Instead of pointing to the latest commits, when `git merge -- no -ff` is passed a merge commit occurs. No commitments from the branch are present in `main`.
3. Three way merge
	1. Two commits are generated for the two branches and they are merged. Then another merge occurs on a common ancestors
	2. We use this modality when simultanous work has been done on both branches

**Merge Fast-forward**
```bash
git checkout main
git merge featureC
```

**Merge No Fast-forward**
```bash
git merge --no-ff featureC
```
Add a merge message, then commit the changes. You will see that the first commit is connected to featureC

### L7 - Deleting branches

Delete the branch locally
```bash
git branch -d featureC
```

Delete the branch on the remote repo
```bash
git push origin --delete featureB
```

### L7 - Git Rebase

Git rebase:
+ Moves a branch up to a new base commit
+ It will go to the common ancestor of both branches and apply commits on top of the other to generate a linear commit history
+ It creates a linear branching history
+ Rebasing should be limited to local changes and not repositories that are shared with other (not good practice to change the history)

#rebaseQuestion How does rebase work? How to use it? Explore more

### L8 - View Your Git Log

Several ways to personalize how to view the `git log`

By author
```bash
git log --author=fedpre
```

As a tree
```bash
git log --graph --decorate --oneline
```

Limit number of commits to display
```bash
git log -n 1
```

Show a specific change on a commit (use the merge/commit number)
```bash
git show 11fe546 d7da838
```

Show differences in branches
```bash
git diff main featureA
```

### L9  - GitHub pages
Great way to showcase your projects to customers or potential employers
+ Webpages hosted by GitHub
+ Two types:
	1. User
	2. Project
+ The full url is: username.github.io/reponame

To make our project for GitHub pages, we will create a new branch and push the brach

```bash
git checkout -b gh-pages
```

Push the repository to GitHub

```bash
git push -u origin gh-pages
```

It can takes up to ten minutes but then you project will be online! [Check it out](https://fedpre.github.io/master-git-class/)

### L10  - Git clone

Get the url of a repo online and then use the command
```bash
git clone https://github.com/fedpre/master-git-class.git
```

### L10  - GitHub Forking
Makes a copy of someone else's repo inside your GitHub (not locally)

### L10  - Pull request
+ Propose a change to a project
+ Easier to contribute on open source projects
+ Make sure to check your work from a manager in work environment