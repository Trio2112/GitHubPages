# Git


## Basic Commands
- https://confluence.atlassian.com/bitbucketserver/basic-git-commands-776639767.html
- ```$git```  It all starts with “git”
- ```$git config```  Configure the tooling
- ```$git init```  Initialize a git local repo
- ```$git clone```  Download a project from remote
- ```$git add```  Prepare a file (to staging)
  - ```$git add .```  Adds all files not being tracked
- ```$git commit```  Commit a file to the repo
  - ```$git commit -m “My message”```
- ```$git status```  Gets the status of the current location
- ```$ls -la```  List files in the directory
- ```$git remote -v```  Determine if a remote configured currently
- ```$git remote add origin https://github.com/Trio2112/firstrepo.git```  Links the current directory to the specified remote repository
- ```$git push -u origin master```  Push local content to the remote reference. The -u is used to create a tracking relation between what the local master and the origin


## Locations in git
- Working directory
- Staging area
- .git repo (local)
- GitHub repo (remote)


## Configure SSH Key
- Generate the SSH key on the client  by:

   ```ssh-keygen -t rsa -b 4096 -C “brent_muhlestein@yahoo.com”```

   Be sure to note the directory where the generated key is stored.
- Go to the directory where the key was generated. The file with the .pub extension is the public key. Open the public key with a text editor and copy it to the clipboard.
- In GitHub, go to your profile settings and add the SSH key. Paste in the key that you copied.
- Now test that everything is working correctly by:

   ```ssh -T git@github.com```
