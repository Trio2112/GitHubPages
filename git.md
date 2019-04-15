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
   
   
## Special files
- readme.md
  - Can be located in root, .github, or docs folder
  - Rendered automatically on landing page
  - Typically written in markdown (.md)
- license.md
  - Contains the open source license letting aspiring contributors know the open source license of the project
- contributing or contributors
  - Contains a listing of people allowed to contribute to the project
- changelog
  - Contains a list of major changes between different versions
- support
  - Informs people of possible ways to get support for the project
- code_of_conduct
  - Contains rules people need to boey when interacting with the project
- Files are written in markdown. Full syntax is available at:

  https://daringfireball.net/projects/markdown/syntax
  
  
## Working with repositories
- ```$git clone``` Creates a full, local copy on your machine of the remote/upstream repository, includes branches and the default, active branch.

  Example: ```$git clone <<paste URL copied from the clone button in github>>```
- ```$git fetch``` Brings down the changes made in the remote/upstream repository, which you don’t have locally yet. However, it will stop there. No merge is done. Essentially, updates the local git database with info from the remote git database.
- ```$git merge``` Merges changes fetched from the remote/upstream repository into your local code.
- ```$git pull``` A shortcut that performs a git fetch and git merge together.
- ```$git push``` Pushes your changes to the remote/upstream repository.

  Example: ```$git push origin master```


## Bringing in more people
- Collaborators: A fixed group of people comprising the core development team working on a project. They typically have more permission on the repository, such as the ability to commit code in the main repository.
- Contributors: Everyone outside of the core team, such as someone who uses your projects in their daily work and is proposing a change or improvement. They can’t commit to your main branch. Their changes come in the form of pull requests that can then be reviewed.


## The commands for branching
- ```$git branch [branch-name]```
- ```$git checkout -b [branch-name]``` will create a new branch and automatically switch you to that branch.
- ```$git push -u [origin] [branch]```
- Steps:
  - To create the test branch locally:
    
    ```$git branch "my-test-branch"```
  - To push the locally-created branch up to the remote (github) server:
    
    ```$git push -u origin my-test-branch```
  - To check the branch out to a pre-created, separate folder on your local pc:
    
    ```$git clone -b my-test-branch --single-branch https://github.com/Trio2112/firstrepo.git my-test-branch```


## Tags
- ```$git log```  Shows a log of commits for the repository you’re pointing to. Type q to quit/exit out of log mode.
- ```$git tag```  Shows the tags for the repository you’re pointing to. No output means there’s no tags.
- ```$git tag my-tag-name branch-to-use```  Adds a tag named “my-tag-name” to the “branch-to-use” branch.
- ```$git log --oneline --graph --decorate --all```  Shows a log in a format easier to read.
- ```$git tag -a v0.1 -m “0.1 release” <<revision number>>```  Adds an annotated tag with the given message at the given revision number.
- ```$git push --tags```  Pushes tag changes up to GitHub.
- ```$git tag -d “v0.2”```  Deletes the tag “v0.2”


## Gists
- A simple way to share snippets and notes with others, similar to Pastebin.
- Access by going to https://gist.github.com.


## GitHub Pages
- Static site hosting without server-side code
- Could be a personal blog or website
- Site is effectively a repository
- Can be created online or offline
- Enable by going to Settings > GitHub Pages


## Notifications
- Types of notifications
  - Participating notifications - notifications that come from repos your participating in or mentioned in
  - Watching notifications - notifications for repos that you have explicitly set to watch
- Go to https://github.com/watching to get an overview of all the repos you are currently watching. You can also unwatch repos from here. 
- Notifications can go out via GitHub or via email. You set this in your account setting page under “Notifications”.

