# how_to_create_rep
this will guide user on how to create, pull, clone, push repo on Github from/to  their local repository

# Quick setup — if you’ve done this kind of thing before

Get started by ``creating a new file`` or ``uploading an existing file``. Github recommend every repository include a ``README, LICENSE, and .gitignore``.


## For those who created a new repository on the command line.

make sure you are in a folder  that will have working project on ``terminal for mac and linux`` and ``cmd or power-shell or gitbash`` then run the following command.


## steps

```powershell   

git init
git add README.md
git commit -m "any message you want to add as commit"
git branch -M master
git remote add origin <Your github rep URL> e.g https://github.com/deesynertz/how_to_create_rep.git
git push -u origin master

but you can use -f flag instead of -u during push activity

```


## push an existing repository from the command line

```bash
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

# BRANCH
See What Branch You're On
Run this command:

```powershell
git status
```
List All Branches

NOTE:  <p>The current local branch will be marked with an asterisk (<span style="color:green; font-size: 15px;">*</span>)</p>

To see local branches, run this command:

```bash
git branch
```

To see remote branches, run this command:
```bash 
git branch -r
```
To see all local and remote branches, run this command:
```bash
git branch -a
```

Create a New Branch
Run this command (replacing my-branch-name with whatever name you want):

```bash
git checkout -b <my-branch-name>
```


## SOME RULES TO FOLLOW BEFORE PUSH SOME CHANGES TO GITHUB

This is very important rules for team work.

- stage your changes

  ```bash
  git add .
- Commit your work first.

  ```bash
  git commit -m "<any description >"
- Pull first before push your changes
  ```bash
  git pull
  ```
- Now push your changes to git
  ```bash
  git push
  ```

Note: make sure you are in a project directory on your local computer.  

```powershell
x:/-directory/project-directory > 

```

<h2 align="left">Support:</h2>
<p><a href="https://www.buymeacoffee.com/deesynertz"> <img align="left" src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" height="50" width="200" alt="deesynertz" /></a></p><br><br><br><br>

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

