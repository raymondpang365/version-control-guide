Cheat sheet guidelines for developers wanting to get better at version control.

This following guideline is written in the perspective of Agile Project Lifecycle Methodology,  aiming to increase productivity, speed and quality of the project.  

# Chapter 2 - Version Control Guide

## 2.1) Gitflow

Gitflow adopts the methodology of &quot;long live master, short live feature branch&quot;.

*Feature branch*: (or personal branch) This is a private branch. For every feature, a new feature branch has to be created.  
Branch name does not matter. However, to make it easier to be recognized. It is better to follow a convention.
For example it can be in the format of <dev_name>/<feature_name>, or simply <dev_name>-<issue_id>, or <dev_name>-< incremental id>.  
For example, if you name is Meow. And, ifthis is your first feature branch, you can name it ```meow-1```, and so on.
Feature branch is short lived, which means after the remote feature branch merged into the remote dev branch, feature branch can be safely deleted.

*Dev branch*: This is a public branch and the common source of truth for the most up-to-date codes. The code of this branch must pass all the unit test and integrated test.

*Release branch*:  When development branch gather enough features, it will be fast-forward merged to Release Branch. Release branch This is used for staging.

*Master branch*: If end-to-end testing and acceptance test pass for release branch, it will be fast-forward merged to this branch. Master branch is used for production


## 2.2) Test-driven development

Test-driven development must be adopted. TDD means you have to first know the purpose of the code, then write the integration test and unit test. After that, write the actual code until all the tests pass.

The baseline is that, at least write tests immidiately after you write the code.  
Before commiting the code, you have to run and make sure you pass all the unit tests.

## 2.3) Pushing changes, Pull request, Handling Merge conflict

### Step 1) Commiting changes locally


Use ```git add . -A``` to add all files to staged area.  
Then, use ```git commit -m "useful message"``` to commit all staged files.   
  
*After that, it is a very good habit to first pull the code, which I will explain in the next section* 

### Step 2) Keeping local code up to date


After commiting the code but before pushin changes,  
types the following commands:  
___
1. Checkout dev branch:  
```git checkout dev```  
2. Pull<sup>[1]</sup> from remote dev branch to local dev branch:  
```git pull origin dev```
3. Checkout personal branch:  
```git checkout meow-1```.  
4. Include changes of dev branch into personal branch:  
```git merge dev```.  
___

These steps will included all the most updated codes in the team to your personal branch.  
After pulling, there might be some merge conflicts.  
This is normal<sup>[2]</sup>. Do not panic.  
Go to the next section to see how to resolve Merge Conflict.  


### Step 3) Handling Merge Conflict
 
You can show all the conflicted files by typing:    
```git diff --name-only --diff-filter=U```
  
Or, I recommend you use IDE such as VS Code or IntelliJ, because viewing unresolved conflicts would be a lot easier.  
Some IDE, such as IntelliJ can even let you click a buton - "take yours" or "take theirs" - to resolve each conflicting file.  
After resolving the issues, commit your change:   
```git commit -m "Resolve conflict and good message to tell which codes you decide to take"```

Then, you are ready to push to code.

### Step 4) Pushing changes 

> Never push your code to dev branch directly.  
  
Push your code from your local <personal_name> branch to your remote <personal_name> branch. 
For example, If your name is meow.   
i) Make sure you are in your personal branch:    
```git checkout meow-1 ```.  
ii) Push to your remote personal branch:  
```git push origin meow-1```.  
  
It is a good practice to keep pull requests small and frequent, so that resolving merge conflict becomes easier.  
Ideally, each ```push``` should not have more than 250 lines change.


### Step 5) Making sure CI is successful.   
A github workflow is set up for our project with *.github/workflow/ci.yml* file.  
If you are using Github,  you can navigate to Actions tab of your repo and find that you are runnng a CI workflow automatically after pushing.
Make sure it is successful (green dot/ green tick) before making a pull request.  
Then go to Pull requests. Navigate to "Pull requests" tab and click "New Pull Request"

### Step 6) Making a pull request

You go to a page which compares and tell you whether you can automatically merge.

For example, in github, there may be a message  
"Can’t automatically merge. Don’t worry, you can still create the pull request."  

Do not listen to it saying "Don't worry". It is always better to be careful.   
You should not create a pull request whenever it cannot automatically merge.   
    
So, cancel and exit the merging action page, so you still do not create the pull request yet.  
Get back to your terminal, we will resolve it in the terminal instead of web editor.  

Go back to Step 3 to see how to resolve Merge Conflect

## 2.4) Delivery, Deploy and Release

According to the description of the Git Flow, the release branch is a short lived one. It may branch off of develop only, and merged into master. In theory, release should be merged back into develop after your release is done, and then be removed. The only thing that you should be merging into release is hotfix. 

If there is something that has to be changed in the release, the team will have to make the change in hotfix branch.  

If end-to-end testing and acceptance test pass, the release branch will be fast-forward merged into the master branch;  
And at the same time, automatically deployed for production server / ready for users to install.


## Footnotes
  
[1]. To make your code cleaner, you can use another merging strategy, such as ```git pull origin dev --rebase --preserve-merges```. 
``` --rebase --preserve-merges``` makes sure new local commits that hasn&#39;t been uploaded will be combined on top of the master branch codes,   
 so that the local commits will not be overwritten.   
**!!!Never use ```rebase``` without ```preserve-merges```!!!, otherwise, it will cause tangling codes among two branches.**


If you are lazy, you can also use
```
git checkout feature/foo
git pull --all
git rebase develop
```
but a word of warning, rebase is best used on local feature branches that haven&#39;t been pushed, see [atlassian.com/git/tutorials/…](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing)

[2] Make your your development is still relevant to other collaborators' effort, so you can avoid wasted efforts or merge conflicts.    
To achieve that, it is highly recommended you also pull new changes immediately after there are approved pull requests.  
However, remember to **```git commit``` or ```git stash```** your code before pulling, otherwise all your local changes will be lost!

