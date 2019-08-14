
# Working with GIT 

> Clone the repository to download the sample-project.zip file. This file will be used to understand the entire concepts around working with Git commands . 

##  Install and Configure Git 

* Install git on Ubuntu 

`   apt-get install -y git ` 

* Configure Git once installed 

> To configure git, you will have to register the username and email address of the developers. The configuration can be done on either one of the below three modes - 

* --global - This configuration is for all git repository for a single username on a machine / VM / laptop etc. This configuration will be stored under USER_HOME_DIRECTORY/.gitconfig 

* --system - This configuration becomes global systemwide. all users in that machine will have the same username & email 

* --local - This configuration is local to a single repository 

For our demo we will use --global - i.e. configuration for a user on a machine. This is how you will work with git in your organization too. 

` git config --global user.name "Developer" `

` git config --global user.email "test@xyz.com" `

> Execute `git config -l` to list all the configurations or alternatively execute ` cat ~/.gitconfig` to view the same configuration

##  Working with Git - Basic commands 

> On your local machine clone this repo and get the sample-project-git.zip. Once downloaded - 

` mkdir git_demo` 

` cp sample-project-git.zip git_demo/`

` cd git_demo` 

`apt-get install -y unzip` 

` unzip sample-project-git.zip` 

` mv sample-project project ` 

` cd project` 

* Initialize git project 

` git init` 

* Add index.html to the git repo 

` git add index.html` 

* Check Status 

` git status ` 

* Add whole directory to git 

` git add . ` 

` git status` 

* Create a new file 

` touch test ` 

` git status ` 

* Create first commit 

` git commit -m " First commit `

` git status` 

* Edit index.html 

` echo "test" >> index.html` 

` git status` 

`git add . ` 

* Unstage test file by reseting HEAD 

` git reset HEAD test ` 

` git status ` 

* Unstage ALL tracked files 

` git reset HEAD . ` 

* Add back all files and commit 

` git add . ` 

` git commit -m "Second Commit " ` 

* Use git rm to remove test file from being tracked 

` git rm test ` 

` git status`

> The above command also removes the test file from your FS 

* Check the logs 

` git log`

* For shorter logs execute 

` git log --oneline ` 

* Commit the changes for delete 

` git commit -m " Deleted Test File `

` git status` 

* Perform a Filesystem remove of index.html file 

` rm index.html` 

` git status` 

* Checkout the last state of index.html file from git object database 

` git checkout -- index.html` 

* Check the latest state of the repository to ensure you are matching the last commit 

` git status` 


##  Working with Git - gitignore file 

* Create a test file 

` touch test ` 

` git status ` 


> Gitignore file is used to ignore some selected files from getting pushed to github repo. This is a useful function when developers commit a bunch of source code from their IDE which also contains IDE specific files like .vscode .project etc. 

* Create a file .gitignore 

` vi .gitignore` 

* Add the content `*test*` inside the file and save 

* Check the status to ensure test file is not trached anymore 

` git status` 

* create a directory temp

` mkdir temp ` 

` touch temp/test ` 

` git status`

* Add one more file that doesnt match test 

` touch temp/mytst` 

` git status` 

` git add . ` 

` git status ` 

* Ignore temp directory completely 

` vi .gitignore` 

* Add ` temp/` inside .gitignore and save 

` git status` 

` git add .` 

` git commit -m " Adding gitignore" ` 


##  Working with git - Branching 

* Create a new branch - feature 

` git branch feature` 

* Checkout the branch feature

` git checkout feature` 

` git branch` 

* Go back to master 

` git checkout master` 

* Create and checkout branch inline 

` git checkout -b feature2`

` git branch` 

* Move back to master 

` git checkout master `

* Delete branch

` git branch -d feature2`

` git branch -d feature1`


##  Working with git - Cloning (Local) & merging 

* go back one directory by executing `cd ..`

* Clone the project repository

` git clone project project-clone` 

` cd project-clone` 

` git status` 

* Check git log to verify the origin 

` git log ` 

* make changes to the cloned branch 

` echo "testclone" >> index.html ` 

` git add . `

` git commit -m "Making changes to clone branch " ` 

* Check latest logs 

` git log` 

* Check origin details 

` git config -l ` 

> Local git branches doesnt allow master of clone to be directly merged with master of origin. We have to create a branch in the cloned repository and push it to the origin as a new branch. This branch can then be merged to the master. This is similar to how we create pull requests in github. 

* Create a branch on the cloned repository

` git checkout -b mybranch`

` git branch` 

* Push the changes to origin repository as a new branch with the same or different name 

` git push -u origin mybranch` 

* Go to the project repository (origin) 

` cd ../project` 

* Check all branches available in the repo 

` git branch` 

* Checkout mybranch branch 

` git checkout mybranch` 

* Verify the changes made in index file 

* Checkout master once again 

` git checkout master` 

* Merge mybranch 

` git merge mybranch` 

> Fast forwarding will forward the current HEAD of master branch to the latest commit of the mybranch branch. This will ensure that all changes made by mybranch are now merged with master 

* Verify index.html to ensure changes are merged 

* Check latest logs to view commits 

` git log --oneline` 


##  Working with git -- Tagging 

> Tags are certain sticky notes to commits that might signify some attributes to your CI cycle. Examples of tags can be version number , minor version, major version etc which might signify that the code is now ready for the release stage. There are 2 types of tags 

* Annotated tags - This the the most commonly used tag that denotes usually that the code is ready and can be released or some automation can be done on top of the code. This tag contains an entire object including tagger, db, checksums etc. 

* Lightweight tags - This are internally used tags between developers which also acts as private labels. This tag can be used to indicate that work for partA of the code is finished by the developerA and developerB can now start integrating code to his builds. This tag doesnt contain the entire object, the tagger details is usually not furnished as a part of this tag. 

* Create annotated tag 

` git tag -a version0 -m "First Tag" `

* List tag

` git tag` 

* Get details of tag version0

` git show version0` 

* Delete tag

` git tag -d version0` 

* Create Lightweight tag 

` git tag v0.1.dev1 ` 

` git show v0.1.dev1` 

* Delete tag 

` git tag -d v0.1.dev1` 


##  Working with Git -- Merge Conflicts 


> Merge conflicts usually happens when different developer work on the same piece of code on multiple branches and send merge requests to the main branch. This is a conflict which needs to be resolved manually. Lets create a merge conflict to understand how it works

* Go to the `project` repository

` git checkout master` 

* delete any other branches for the demo

* Create 2 new branches 

` git branch feature1 ` 

` git branch feature2` 

* Checkout feature1 and make changes to index.html

` git checkout feature1 ` 

` echo "feature1 changes" >> index.html` 

` git add . ` 

` git commit -m "feature1 changes " ` 

* Checkout feature2 and make changes to index.html file 

` git checkout feature2` 

` echo "feature2 changes" >> index.html` 

` git add . ` 

` git commit -m "feature2 changes" ` 

* Merge feature1 changes to feature2 

` git merge feature1` 

> Check the merge conflict on index file. Developer will have to collaborate now and resolve the conflict to add both the codes. We will remove the additional comments added by git and resolve the conflict to add both the changes. 

` git add . ` 

` git commit -m "resolving conflicts" ` 

` git checkout master` 

` git merge feature2` 


##  Working with Git -- reverting commits 

* Revert back to the last commit 

` git revert HEAD` 

> This creates a new commit and changes the state to the last commit 

* Revert back to nth commit from head  

` git revert HEAD~n` 

##  Working with Git -- diff 

> diff is used to get differences between 2 commits 

* Get the latest diff 

` git diff` 

* Get diff between current and previous commit 

` git diff HEAD^ HEAD `

* Get diff between any 2 commits 

` git log --oneline` 

> Get the shortened hash of any 2 commits 

` git diff hash1 hash2` 


##  Working with git -- Merge vs Rebase 

> Merge fasts forwards your state to the latest commit of branch that you want to merge. Rebase rewinds your current base to the latest commit of the branch you want to merge and provides you with a linear progression of your progress with the branch. Rebase should not be used with public repositories or while working with other developers. Rebase should be done on your own individual branch. Lets do a demo to understand difference between rebase and merge 

* Working with merges 

* Create a clean repository

` cp -R project project_new` 

` cd project_new` 

` rm -fr .git ` 

` git init ` 

` git status` 

` git add . ` 

` git commit -m " Initial Master Commit " ` 


* Checkout a feature branch 

` git checkout -b feature` 

` git branch`

` echo "test" >> index.html` 

` git add . ` 

` git commit -m " Initial feature branch commit "`

` git log ` 


* Checkout a hotfix branch 

` git checkout -b hotfix master` 

` echo "hotfix" >> hotfixfile `

` git add . ` 

` git commit -m " Initial hotfix branch commit" ` 

` git log --oneline` 

* Checkout master and merge hotfix 

` git checkout master` 

` git log --oneline` 

` git merge hotfix` 

` git log --oneline` 

* Checkout feature and merge master changes to feature

` git checkout feature` 

` git merge master` 

` git log --oneline` 

> Note the commits - commits are stacked over feature branch and feature has fast forwarded to the latest commit at master 

> Lets perform the same steps again to work with rebase 

* Checkout feature and make changes to index.html

` git checkout feature` 

` echo "feature2" >> index.html` 

` git commit -a -m "Second feature branch changes " `

* Delete the hotfix branch and create a new one 

` git branch -d hotfix` 

` git checkout -b hotfix master` 

` git branch` 

` echo "newhotfix" >> newhotfixfile` 

` git add .` 

` git commit -m "Second hotfix commit " ` 


* Checkout master and merge hotfix 

` git checkout master` 

` git merge hotfix` 

` git log --oneline`

* Checkout feature branch and rebase master 

` git checkout feature ` 

` git log --oneline` 

` git rebase master` 

` git log --oneline` 

* Verify the new base before the last commit of feature


























