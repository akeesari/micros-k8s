# **Git Commands**

In this article, I am going to present a comprehensive cheat sheet of commonly used Git commands with examples. 

## Installing git

Here are the commands to install Git on different operating systems:

```sh
# Ubuntu/Debian:
sudo apt-get install git

# MacOS (using Homebrew):
brew install git

# Windows OS (using choco)
choco install git
```

## Setting up git configuration:

To begin, it's important to configure your Git settings, associating your name and email with your commits. Use the following commands to set your name and email respectively:

``` sh
git config --global user.name "anji.keesari"
git config --global user.email "anjkeesari@gmail.com"
```
## Caching credentials:

Typing in login credentials repeatedly can be time consuming. To streamline this process, you can store your credentials in the cache using the command:

``` sh
git config --global credential.helper cache
```

## Enable automatic coloring of Git output  

This command is used to enable automatic coloring of Git output in the command line interface. Enabling this option enhances the readability of Git's output by applying different colors to various elements.

```sh
git config --global color.ui auto
```
## Checking git configuration:

To verify your Git configuration, including your username and email, use the following command:

``` sh
git config -l
```

## Initializing git

Before diving into Git commands, you need to initialize a new Git repository locally in your project's root directory. Execute the command:

```
git init
```
## Git clone

To work on an existing Git repository, you can clone it using the command

```
git clone <repository-url>
```
## Adding files to the staging area:

To stage changes and prepare them for commit, use the git add command. You can add specific files or entire directories to the staging area using the following commands:


``` sh
git add <file-name>             # Add a specific file
git add .                       # Add all changes in the current directory (excluding deletions)
git add test*                   # Add all files starting with 'test' in the current directory

```

## Committing changes:

Committing changes captures a snapshot of your code at a specific point in time. Use the following commands to commit your changes:

``` sh
git commit -m "(message)"       # Commits the changes with a custom message
git commit -am "(message)"      # Adds all changes to staging and commits them with a custom message

```

## Git log

To view the commit history of a repository, use the `git log` command. It provides you with an overview of past commits and their respective details. Additionally, you can use `git log -p` to see the commit history along with the changes made to each file.

``` sh
#  shows the commit history for the current repository:
git log
# commit's history including all files and their changes:
git log -p

press q any time to quit
```

## Commit details

Use this command to see a specific commit in details

``` sh
git show commit-id
```
Note: replace `commit-id` with the id of the commit that you can find in the git log

## Git status

This command will show the status of the current repository including staged, unstaged, and untracked files.

``` sh
git status
```

## Undoing changes:

If you have already pushed a commit to a remote repository and want to undo it, you need to create a new commit that undoes the changes. The following command will create a new commit that undoes the changes introduced by the specified commit:

``` sh
git revert <commit-id>
```
Replace <commit-id> with the ID of the commit you want to undo.

If you have already committed changes and want to undo the most recent commit, you have a few options depending on your desired outcome:
- Undo the commit and keep the changes as unstaged modifications:
```sh
git reset HEAD^
```
Undo the commit and completely discard the changes:
```
git reset --hard HEAD^
```

## Viewing differences 

To compare the differences between versions, you can use the git diff command. It displays the changes made to files since the last commit. 

```sh
git diff
```

This will show the line-by-line differences between the current state of the files and the last committed version.

## Pushing changes 

To push your local commits to a remote repository, you need to use following command.

``` sh
git push origin <branch-name>

# if you haven't set the upstream branch yet, you can use this

git push --set-upstream origin aspnet-api
```
## Pulling changes

Use this command to incorporate the latest changes from a remote repository into your local repository.

``` sh
git pull origin <branch-name>
```

## Git fetch

To fetch the latest changes from the remote repository without merging them into your local branches.

``` sh
git fetch
```
## Creating a new branch:

To create a new branch in Git, you can use the git branch command followed by the name of the branch you want to create. 

```sh
git branch <branch-name>

# Creates a new branch, `aspnet-api` is name of the branch here
git branch aspnet-api

```

## Switching branch:

To switch to a different branch in your Git repository, you can utilize the `git checkout` command followed by the name of the branch you want to switch to. 


```sh
git checkout <branch-name>

# Switched to branch 'aspnet-api'
git checkout aspnet-api
```
## List branches

It will show a list of all branches and mark the current branch with an asterisk and highlight it in green.

```sh
# Shows the list of all branches.
git branch	
# List all local branches in repository. With -a: show all branches (with remote).
git branch -a 

# press q to quit
```
## Get remote URLs

You can see all remote repositories for your local repository with this command:

``` sh
git remote -v
```
## More info about a remote repo 

How to get more info about a remote repo in Git:

``` sh
git remote show origin
```
## Merging branches

In Git, merging allows you to combine the changes from one branch into another. To merge a branch into another branch, you can use the git merge command followed by the name of the branch you want to merge. Here's an example:

``` sh
git merge <branch-name>

# For instance, if you want to merge the changes from the develop branch into the main branch
# cd to the folder
git checkout main
git merge develop
```

After performing the merge, it's a good practice to check the status of your repository using git status to ensure that the merge was successful and there are no conflicts to resolve. Additionally, you can view the commit history using git log to see the merged commits and their details.

```sh
git status
git logs
```

## Delete branch

To delete a branch in Git, you can use either of the following commands:

``` sh
git branch --delete <branch-name>
git branch -d <branch-name>

example
# git branch to see list of branches before delete
git branch
# delete the branch
git branch --delete <branch-name>
# git branch again to see list of branches after delete
git branch
```


## Branch from a previous commit

To create a new branch in Git using a specific commit hash, you can use the git branch command followed by the name of the branch and the commit hash


``` sh
git branch branch_name <commit-hash>
# Step 1: Create the branch from the commit hash

git branch new_branch 07615d50afde24d21e2180b90d3a0a58ec131980

# this will create the local branch

# Step 2: Switch to the new branch & commit

git commit -am “(message)” 
```

## Rollback an old commit

You can revert an old commit using its commit id. 

``` sh
git revert comit_id
```
## How to resolve merge conflicts using git commands

Resolving merge conflicts in Git involves editing the conflicted files to choose which changes to keep and which to discard, and then committing the resolved changes. Here's a step-by-step guide:

1. Check the status of your repository to see if there are any merge conflicts:

    ```
    git status
    ```
    If there are merge conflicts, you will see a message indicating which files have conflicts.

1. Open the conflicted files in a text editor and look for the conflict markers. The markers will look something like this:

    ```
    <<<<<<< HEAD
    This is the content from the current branch.
    =======
    This is the content from the branch you are merging.
    >>>>>>> <branch-name>

    ```
1. Decide what changes you want to keep and remove the conflict markers and any unnecessary content. The final content should only include the changes you want to keep.

1. Stage the changes using git add:
    ```
    git add <file-name>
    ```
1. Commit the changes to the repository:
    ```
    git commit -m "Resolved merge conflicts"

    ```
1. Push the changes to the remote repository if necessary:
    ```
    git push origin <branch-name>
    ```

## Temporary commits

In Git, you can use temporary commits to store modified, tracked files temporarily, allowing you to switch branches without losing your changes. This is a useful technique when you want to work on a different branch but are not ready to commit your changes yet.

- Stash your changes: 
This will create a temporary commit that stores your modifications, allowing you to switch branches.
```sh
git stash
```
- Git stash list
Running this command will show you the stash ID, along with a description that includes the branch name and commit message.
```sh
git stash list
```
- Git stash pop:
his command is used to apply the changes from the top of the stash stack and remove that stash from the stack.
```sh
git stash pop:
```
- Git stash drop: 
This command allows you to discard a stash from the stash stack. It permanently removes a stash and its changes, freeing up space in the stack.
```sh
git stash drop: 
```
<!-- 
## How to set global git config settings?

There are a number of ways to edit the global git config file. One way is to add properties through the command line. The global git config email and username properties are often set in the following way:

```
git config –global user.name anjkeesari	# Set the username to be used for all actions
git config –global user.email anjkeesari@gmail.com	# Set the email to be used for all the actions.
```

## git config global edit


The global git config is simply a text file, so it can be edited with whatever text editor you choose. Open, edit global git config, save and close, and the changes will take effect the next time you issue a git command. It’s that easy.

```
git config --global --edit
```

These are some of the most commonly used Git commands. However, Git is a powerful tool with many more features and commands. For more information, you can refer to the official Git documentation.



## Reference -->

<!-- - https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/The-global-Git-config-files-key-settings-and-usages#:~:text=How%20to%20do%20a%20git,It's%20that%20easy. -->
