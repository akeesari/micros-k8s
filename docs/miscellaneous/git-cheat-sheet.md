Here is a cheat sheet of common Git commands:

## Initializing git

Initializing a Git repository:
```
git init
```
## Git clone

Cloning a Git repository:
```
git clone <repository-url>
```
## Git add

Adding files to the Git staging area:
```
git add <file-name>
git add . # Add the whole directory changes to staging (no delete files).
```

## Committing changes

Committing changes to the repository:

```
git commit -m “(message)”	// Commits the changes with a custom message.
git commit -am “(message)”	// Adds all changes to staging and commits them with a custom message.

```

## Git log

Viewing the commit history:

```
git log
press q any time to quit
```


## Git status
Checking the status of your repository:

```
git status
```

Undoing changes:
```
git checkout -- <file-name>
```

To undo all changes in the repository, use git reset --hard HEAD.
## Viewing differences 
Viewing differences between versions:

```
git diff
```
## Pushing changes 
Pushing changes to a remote repository:
```

git push origin <branch-name>
git push origin (branch_name)	// Push branch to the Origin.

git push --set-upstream origin aspnet-api

```
## Pulling changes
Pulling changes from a remote repository:

```
git pull origin <branch-name>
```

## Git fetch

Get the latest changes from the origin but not merge.
```
git fetch
```
## Creating a new branch:

```
git branch	// Shows the list of all branches.

git branch <branch-name>

example:

# Creates a new branch, `aspnet-api` is name of the branch here
git branch aspnet-api

```

## Switching branch:

Switching to a different branch:

```
git checkout <branch-name>

# Switched to branch 'aspnet-api'
git checkout aspnet-api


```

## Merging branches
```
git merge <branch-name>

example, merging from develop to main
# cd to the folder
git checkout main
git merge develop

git status
git logs

```

## Delete branch
```
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

Create the branch using a commit hash:


```
git branch branch_name <commit-hash>
# step-1

git branch new_branch 07615d50afde24d21e2180b90d3a0a58ec131980

this will create the local branch

# step-2 commit the branch 

git commit -am “(message)” 

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



## Reference

<!-- - https://www.theserverside.com/blog/Coffee-Talk-Java-News-Stories-and-Opinions/The-global-Git-config-files-key-settings-and-usages#:~:text=How%20to%20do%20a%20git,It's%20that%20easy. -->
