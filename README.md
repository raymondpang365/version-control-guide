Cheat sheet guidelines for developers wanting to get better at version control.

This following guideline is written in the perspective of Agile Project Lifecycle Methodology,  aiming to increase productivity, speed and quality of the project.  

# Chapter 1 - Basic Guide

## 1.1) Gitflow

Gitflow adopts the methodology of &quot;long live master, short live feature branch&quot;.

*Feature branch*: This is a private branch. For every feature, a new feature has to be created. The branch name would be in the format of <dev_name>/<feature_name>. Feature branch is short lived.

*Dev branch*: This is a public branch and the common source of truth for the up to date development. The code of this branch must pass all the unit test and integrated test.

*Release branch*:  When development branch gather enough features, it will be fast-forward merged to Release Branch. Release branch This is used for staging.

*Main branch*: If end-to-end testing and acceptance test pass for release branch, it will be fast-forward merged to this branch. Main branch is used for production


## 1.2) Test-driven development

Test-driven development must be adopted. TDD means you have to first know the purpose of the code, then write the integration test and unit test. After that, write the actual code until all the tests pass.

The baseline is that, at least write tests immidiately after you write the code.  
Before commiting the code, you have to run and make sure you pass all the unit tests.

## 1.3) Pushing changes, Pull request, Handling Merge conflict

### 1.3.1) Commiting changes locally

Use ```git add .``` and ```git commit -m "useful message"``` to commit.   
  
**After that, it is a very good habit to first pull the code.**  

After using ```git commit -m "msg"``` to commit, pull remote master branch to local master branch.   To do this, type:   
  ```git pull --rebase --preserve-merges```. 
  
``` --rebase --preserve-merges``` This makes sure new local commits that hasn&#39;t been uploaded will be combined on top of the master branch codes,   
 so that the local commits will not be overwritten. 

Never use ```rebase``` without ```preserve merges```, otherwise, it will cause tangling codes among two branches.   

After pulling, there would be some merge conflicts.
This is normal. Do not panic.
Go to section **"1.3-E) Handling Merge Conflict"** to see how to resolve Merge Conflect

(To learn about an alternative strategy, see section 2.1 in the advanced guide)

### 1.3.2) Pushing changes 

> Never push your code to dev branch directly.  
  
Push your code from your local <personal_name> branch to your remote <personal_name> branch. 
For example, If your name is meow.   
i) Make sure you are in your personal branch:    
```git checkout meow-1 ```.  
ii) Push to your remote personal branch:  
```git push origin meow-1```.  
  
It is a good practice to keep pull requests small and frequent, so that resolving merge conflict becomes easier.  
Each pull requests should not have more than 250 lines change.

### 1.3.3) Making sure CI is successful.   
A github workflow is set up for our project with *.github/workflow/ci.yml* file.  
If you are using Github,  you can navigate to Actions tab of your repo and find that you are runnng a CI workflow automatically after pushing.
Make sure it is successful (green dot/ green tick) before making a pull request.  
Then go to Pull requests. Navigate to "Pull requests" tab and click "New Pull Request"

### 1.3.4) Making sure there is no conflict 

You go to a page which compares and tell you whether you can automatically merge.

For example, in github, there may be a message  
"Can’t automatically merge. Don’t worry, you can still create the pull request."  

Do not listen to it saying "Don't worry". It is always better to be careful.   
You should not create a pull request whenever it cannot automatically merge.   
    
So, cancel and exit the merging action page, so you still do not create the pull request yet.  
Get back to your terminal, we will resolve it in the terminal instead of web editor.  

Go to the next section *"1.3-E) Handling Merge Conflict"* to see how to resolve Merge Conflect

### 1.3.5) Handling Merge Conflict

Go to your terminal, type:  
```
git checkout dev
git pull
git checkout meow-1
git merge dev
```
Now, You can see whether there are conflicted files  
```git diff --name-only --diff-filter=U``` tot show all conflicted files
  
Or, I recommend you use IDE such as VS Code or IntelliJ, because viewing unresolved conflicts would be a lot easier.  
Some IDE, such as IntelliJ can even let you click a buton - "take yours" or "take theirs" - to resolve each conflicting file.  
After resolving the issues, commit your change:   
```git commit -m "Resolve conflict and good message to tell which codes you decide to take"```

Then, do again:  
```git push origin meow-1```
  
Then, make sure CI/CD is a success. After that, try to create a pull request
  
It should let you automatically merge this time.  
Wait for a code reviewer if you want to ask for advice or approval.  
After merging the branch, tell your team about it.  


## 1.4) Keeping local code up to date

The rule of thumb is keep code pulling (such as ```git pull```) as frequent as possible.  
At minimum, do it in one scenario –   
i) After commiting the code and before pushin changes (The Commit > Pull > Push steps) as abovementioned in section 1.3-A.  

And, it is highly recommended you also pull new changes:  
i) Before you start your day of coding job. 
ii) Immediately after there are approved pull requests from other team members. 

To do this, types the following commands:
```
git checkout dev
git pull origin dev
git checkout my-feature-branch
git merge dev
```
This is very important. Otherwise, when somebody pushed new commit to the remote repository, the local repo would not be in sync with the remote repo. If new changes are made in working directory without keeping it updated first, conflicts easily occur.

**Advanced:**

If you are lazy, you can
```
git checkout feature/foo
git pull --all
git rebase develop
```
but a word of warning, rebase is best used on local feature branches that haven&#39;t been pushed, see [atlassian.com/git/tutorials/…](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing)

## 1.5) Delivery, Deploy and Release

According to the description of the Git Flow, the release branch is a short lived one. It may branch off of develop only, and merged into master. In theory, release should be merged back into develop after your release is done, and then be removed. The only thing that you should be merging into release is hotfix. 

If there is something that has to be changed in the release, the team will have to make the change in hotfix branch.  

If end-to-end testing and acceptance test pass, the release branch will be fast-forward merged into the main branch;  
And at the same time, automatically deployed for production server / ready for users to install.


## Common Git F&Q

**Q: What is the difference between Merge vs Rebase?**

Both are for integrating new work new commit that are on separate branches. There are always at least 2 branches in play.

Merge preserves the branches tree, like a **gluing two branches**.

It is useful for combining branches that are already public

Rebase does not preserve branches tree like a **cutting a branch with a scissor and taping it elsewhere**.

Rebase is for combining private branches, not for public one.

<img src="https://github.com/psfr937/version-control-guide/blob/master/merge_and_rebase_illustration.jpg?raw=true" width="300" />

In rebase, no extra commits are created, but the exiting commits that you created are moved.

In rebase you can find that the commits has changed the hash because the timeline is changed, the hash has to be recalculated.
