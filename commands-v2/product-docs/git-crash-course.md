---
description: git stuff I keep forgetting
icon: octopus-deploy
---

# Git crash course

## What is Git?

Git is a computer software that helps keep track of code changes. Like a video game, we can use Git to 'save' progress. Windows needs to download Git Bash.

## Set up Git

Download Git Bash

Can use this command to check if it works

{% code overflow="wrap" %}
```
git --version
```
{% endcode %}

## Saving robotics code

I want to 'save' my robotics code folder. First I need to turn it into a local Git repository.

Go to the folder that I want to 'save' don't forget quotes

{% code overflow="wrap" %}
```
cd "C:\Users\myusername\Documents\_CommandsV2Study"
```
{% endcode %}

or even easier, bypass that by opening the folder in file explorer and Shift + right clicking the empty folder space >> Git Bash Here

type in

{% code overflow="wrap" %}
```
git init
```
{% endcode %}

Now this folder called \_CommandsV2Study has a local Git repository.

Check what changed with

{% code overflow="wrap" %}
```
git status
```
{% endcode %}

Choose what to save AKA stage the file AKA add from local repo to the Index (add . adds everything)

{% code overflow="wrap" %}
```
git add .
```
{% endcode %}

Save changes AKA commit AKA add from Index to Head

{% code overflow="wrap" %}
```
git commit -m "Helpful Commit message"
```
{% endcode %}

## 'Saving' to GitHub

There is now a remote repository (my robot code backup on my computer) but I would like to have an online backup.

### Online repo setup

Go to GitHub.com, log in and make a new repo

Go back to Git Bash and connect remote computer to my GitHub account with:

{% code overflow="wrap" %}
```
git config --global user.email "myemail@email.com"
```
{% endcode %}

Go to the repo to 'save' don't forget quotes (can bypass by going to the folder, shift + right click >> Git Bash Here)

{% code overflow="wrap" %}
```
cd "C:\Users\myusername\Documents\_CommandsV2Study"
```
{% endcode %}

check to make sure there is a local repo with the command

{% code overflow="wrap" %}
```
git status
```
{% endcode %}

#### First time doing a push to remote repo

Connect local repo to remote repo

{% code overflow="wrap" %}
```
git remote add origin https://github.com/mygithubusername/_CommandsV2Study.git
```
{% endcode %}

Need to log in and authenticate to authorize application

If you are having issues you might have to temporarily pause adblockers

Can check the connection with the command:

{% code overflow="wrap" %}
```
git remote -v
```
{% endcode %}

Add:

{% code overflow="wrap" %}
```
git add .
```
{% endcode %}

commit.

{% code overflow="wrap" %}
```
git commit -m "First commit to online repo"
```
{% endcode %}

Then use this command for sending the local repo to remote repo

{% code overflow="wrap" %}
```
git push -u origin main
```
{% endcode %}

#### After connecting the remote repo the first time

only need to type this after each add & commit

{% code overflow="wrap" %}
```
git push
```
{% endcode %}

## ⭐ Git Daily Routine

Check what changed (did you save in VS Code??)

{% code overflow="wrap" %}
```
git status
```
{% endcode %}

Choose what to save AKA stage the file AKA add from local to the Index (add . adds everything)

{% code overflow="wrap" %}
```
git add .
```
{% endcode %}

Save changes AKA commit AKA add from Index to Head

{% code overflow="wrap" %}
```
git commit -m "Helpful Commit message"
```
{% endcode %}

Save changes in remote repo

{% code overflow="wrap" %}
```
git push
```
{% endcode %}

## When should you commit?

* Often
* After you fix something that wasn't working before
* Before you try something risky
* Frequent backups

## When should you make a branch?

* Less often than commit
* About to make a big change with several commits that might not work out
* You want to keep a stable backup on hand
* There are multiple people working on the same project. Each has their own branch

## Create a branch

We can create a branch away from main. Create a new branch with

{% code overflow="wrap" %}
```
git branch nameOfNewBranch
```
{% endcode %}

Rename a branch with

{% code overflow="wrap" %}
```
git branch -M whatYouWantToRenameBranchTo
```
{% endcode %}

Check which branch you are on with:

{% code overflow="wrap" %}
```
git branch
```
{% endcode %}

or

{% code overflow="wrap" %}
```
git status
```
{% endcode %}

To change to a branch to edit, use

{% code overflow="wrap" %}
```
git switch nameOfBranchYouWantToGoTo
```
{% endcode %}

### Merge branches

Let's say you're satisfied with the changes you made to the branch, so you're ready to merge those changes with the main branch.

Choose the branch to make changes to

{% code overflow="wrap" %}
```
git switch theBranchToReceiveChangesUsuallyMain
```
{% endcode %}

Merge the branch into the branch we are making changes to (usually main). The branch we merge from is left untouched.

{% code overflow="wrap" %}
```
git merge theBranchWithChangesToMerge
```
{% endcode %}

#### Managing Conflicts

If there are changes to the same line in a merge, the file will want you to resolve those changes

{% code overflow="wrap" %}
```
<<<<<<< HEAD
int speedCurrentVersion = 5;
=======
int speedIncomingVersion = 6;
>>>>>>> theBranchWithChangesToMerge
```
{% endcode %}

Or you can start over from the beginning

{% code overflow="wrap" %}
```
git merge --abort
```
{% endcode %}

## Revert a 'save'

Use the command:

{% code overflow="wrap" %}
```
git log --oneline
```
{% endcode %}

(Now you can see why helpful commit descriptions are so important)

Find the number of the specific commit to revert

{% code overflow="wrap" %}
```
git revert e497ffd
```
{% endcode %}
