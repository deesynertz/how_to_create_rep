# how_to_create_rep

<!-- [![N|DEESYNERTZ](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid) -->

This will guide user on how to create, pull, clone, push repo on Github from/to  their local repository

## Quick setup — if you’ve done this kind of thing before

Get started by ``creating a new file`` or ``uploading an existing file``. Github recommend every repository include a ``README, LICENSE, and .gitignore``.

## For those who created a new repository on the command line

make sure you are in a folder  that will have working project on ``terminal for mac and linux`` and ``cmd or power-shell or gitbat`` then run the following command.

## steps

```bat
git init
```

```bat
git add README.md
```

```bat
git commit -m "any message you want to add as commit"
```

```bat
git branch -M master
```

```bat
git remote add origin <Your github rep URL> e.g https://github.com/deesynertz/how_to_create_rep.git
```

***in case you have remote origin but in any sucmstance you need to change the remote URL the you have to use below code as commted by @shebyclassic2018***

```bat
git remote set-url origin <Your new github rep URL>
```

```bat
git push -u origin master
```

but you can use -f flag instead of -u during push activity

## push an existing repository from the command line

```bat
git remote add origin <Your github rep URL> e.g https://github.com/deesynertz/how_to_create_rep.git
git branch -M master
git push -u origin master
```

Some basic Git commands are:

```
git status
git add
git commit "any message you want to add as commit"
git clone <Your github rep URL> e.g https://github.com/deesynertz/how_to_create_rep.git
git pull
```

```powershell
Note: 
# Some beginner as i was before they are facing problem of quit the git log, and i realised that is quite simple to quit the log by pressing one character in your keyboard just one );

q
```

## BRANCH

See What Branch You're On Run this command:

```bat
git status
```

List All Branches

NOTE:  
<p>The current local branch will be marked with an asterisk (<span style="color:green; font-size: 15px;">*</span>)</p>

To see local branches, run this command:

```bat
git branch
```

To see remote branches, run this command:

```bat
git branch -r
```

To see all local and remote branches, run this command:

```bat
git branch -a
```

Create a New Branch
Run this command (replacing my-branch-name with whatever name you want):

```bat
git checkout -b <my-branch-name>
```

How do I delete a local branch in Git?
<p> Git makes managing branches really easy - and deleting local branches is no exception:</p>

```bat
git branch -d <local-branch>
```

## Try to synchronize your branch list using

>The -p flag means "prune". After fetching, branches which no longer exist on the remote will be deleted.

```powershell
git fetch -p
```

<p>In some cases, Git might refuse to delete your local branch: when it contains commits that haven't been merged into any other local branches or pushed to a remote repository.
This is a very sensible rule that protects you from inadvertently losing commit data.</p>

```bat
  git branch -D <local-branch>
```

<p>The quqestion is How do I delete a remote branch in Git ?</p>

```bat
git push origin --delete <remote-branch-name>
```

```powershell
Note: 
# If you have deleted a branch on GitHub and it still appears when you run the command to view all branches, it's possible that the local repository still has a reference to the deleted branch. To remove the deleted branch permanently, you can try the following steps:

Update your local repository with the latest changes from GitHub by running the following command:
```

```bat
git fetch --prune
```

The above command will remove any references to remote branches that no longer exist on GitHub.

## SOME RULES TO FOLLOW BEFORE PUSH SOME CHANGES TO GITHUB

This is very important rules for team work.

- stage your changes

  ```bat
  git add .
  ```

- Commit your work first.

  ```bat
  git commit -m "<any description >"
  ```

- Pull first before push your changes

  ```bat
    git pull
  ```

- Now push your changes to git

  ```bat
    git push
  ```

Note: make sure you are in a project directory on your local computer.  

```powershell
x:/-directory/project-directory >
```

## THE GOLDEN RULES OF VERSION CONTROL

according to [TOWER](https://www.git-tower.com/learn/git/ebook/en/desktop-gui/branching-merging/working-with-branches#start)

### No. 4: Never Commit Half-Done Work

> You should only commit code when it’s completed. This
> doesn’t mean you have to complete a whole, large
> feature before committing. Quite the contrary: split
> the feature’s implementation into logical chunks and
> remember to commit early and often. But don’t commit
> just to get half-done work out of your way when you
> need a "clean working copy". For these cases,
> consider using Git’s “Stash” feature instead.

## Support:

<p><a href="https://www.buymeacoffee.com/deesynertz"><img align="left" src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" height="50" width="200" alt="deesynertz" /></a></p><br>

### LISENCE AND CONTACT

[![General badge](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/deesynertz/how_to_create_rep)

[![General badge](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)](MailTo:deesynertz@gmail.com)

[![General badge](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/deogratias-alison/)

<!-- https://img.shields.io/badge/Facebook-1877F2?style=for-the-badge&logo=facebook&logoColor=white -->

<!-- https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white -->

<!-- https://img.shields.io/badge/Skype-00AFF0?style=for-the-badge&logo=skype&logoColor=white -->

<!-- https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white -->

<!-- https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white -->
<!-- https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white -->
<!-- https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white -->
<!-- https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black -->

@deesynertz :+1: Enjoy using Github for best software version control - it's ready to merge! :shipit:

![This is an image](https://myoctocat.com/assets/images/base-octocat.svg)
