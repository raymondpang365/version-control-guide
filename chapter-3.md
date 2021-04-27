# Chatper 3 - Advanced Trick, Strategy & Discussion

## 3.2) Change a Commit Message that Hsn't Been Pushed Yet 

```git commit -m "wrong message"```  
you have commited a wrong message, what should you do?  
```git commit -m "new message" --amend```   
```git push origin meow-1 --fast-forward```  


## 3.3) Remove Files from Staging

Image you staged a wrong file, for example, the wrong file is lib.js
```git add lib.js```   
You want to remove it, what should you do?  

```git log --oneline```   

It will display your current logs, each line is a commit, for example:  
```
85da456 ( HEAD -> XXX) Adding index.html and app.js
45ca772 ...
... ...
... ...
```
Find your latest commit, which is ```85a456```, do:  
```git reset HEAD lib.js```.  
Now, check your staging area with ```git status```.   
You would find that lib.js is removed from ```git add``` (staging area).  
Removing file from staging area is called ```unstaging```.  

## 3.4) Remove Files from Commit Before Pushing

Image you commited a wrong file, what should you do?  

```git log --oneline```   

It will display your current logs, each line is a commit, for example:  
```
85da456 ( HEAD -> XXX) Adding index.html and app.js
45ca772 ...
... ...
... ...
```
Find your latest commit, which is ```85a456```, do:  
```git reset HEAD~1```
Now, your commit is rollbacked to when you have not commited yet.  
You will find all the commited files are back to staging area,  



## 3.5) Remove Files from Commit Before Pushing

The following tip is only suitable if you have NOT pushed the commit.
if you have pushed the commit, do not use ```reset```, use ```revert``` instead,  
which is mentioned in section 3.7.

Image you commited a wrong file, what should you do?  

```git log --oneline```   

It will display your current logs, each line is a commit, for example:  
```
85da456 ( HEAD -> XXX) Adding index.html and app.js
45ca772 ...
... ...
... ...
```
Find your latest commit, which is ```85a456```, do:  
```git reset HEAD~1```
Now, your commit is rollbacked to when you have not commited yet.  
You will find all the commited files are back to staging area


## 3.6) Difference between git reset ```--hard```, ```--mixed``` and ```--soft```

**SOFT:  **.=  
1) Undo commit, moving files from commit back to staging area.  
**MIXED: **   
1) Undo commit
2) Also unstages the files from staging area.  
HARD: 
**HARD: **  
1) Undo commit
2) Also unstages the files from staging area. 
3) Also removes local changes in working directory 


## 3.7) Undo a commit that has already been pushed

Do not use ```git reset``` because others may alrady have pull your commit.  
So, it is pointless to rewrite history.   

instead:   

```git log --oneline``` and check the hash of the
commit that you want to revert.   
if you want to revert ```A1B27D```

Use ```git revert A1B27D```.

Different from ```git reset```, this does not remove the commit.

Instead, it just creates a new commit on top that reverts the difference


## 3.8) Copy a commit to another branch

For example, we made a wrong commit in ```master``` branch and we mean to ```commit``` it in meow-1 branch

```git log --oneline``` and check the hash of the  
commit that you want to move.   
if you want to move ```A1B27D```    
```git checkout meow-1```
```git cherry-pick A1B27D```


## 3.9) Move the change even if worked on and commited on the wrong branch

For example, we worked on the wrong branch made a wrong commit in ```master``` branch.
And, we want to move all works and ```commit``` it in meow-1 branch instead
First, copy the commit with the method mentioned in section 3.8

Next, go back to master branch
```git checkout master```
```git reset --HARD~1```


## 3.10) git Update the branch to the latest code  

```git pull```    
Merge your local changes into the latest code:  

```git stash apply```  
Add, commit and push your changes. 
```
git add
git commit
git push
```  
## 3.11) Explore old commit with detach HEAD, then recover
Let's say you are now in ```meow-1``` branch...   
You can look into old commits with hash ```C1B6D2```   
with ```git checkout C1B6D2```.  

But when you change anything, you will lose the change because you have detached HEAD.   
To working on old commits...    
use ```git branch -b new-branch```, now you can work on the commit in history with a new branch.  
When you want to get back to the current branch, simply ```git branch meow-1```.  



## 3.2) If commits are made locally on DEV branch

Use```git pull origin dev --rebase --preserve-merges```. 
``` --rebase --preserve-merges``` makes sure new local commits that hasn&#39;t been uploaded will be combined on top of the master branch codes,   
 so that the local commits will not be overwritten.   
**!!!Never use ```rebase``` without ```preserve-merges```!!!, otherwise, it will cause tangling codes among two branches.**



## 3.3) CI / CD Automation Tool
1. Set a Github action prehook that will automatically do linting, unit testing, integration testing and test if it can build in local/remote environment. They pull request can be uploaded only if everything pass.   
2. On the development branch, integration testing can be done daily with a cron job.
3. Optimally, there should be a continuous integration tool to automate, such as rultor.com
