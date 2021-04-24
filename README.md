Cheat sheet guidelines for developers wanting to get better at version control.

This following guideline is written in the perspective of Agile Project Lifecycle Methodology,  aiming to increase productivity, speed and quality of the project.

# 1 Basuc Guide

## 1.1) Gitflow

Gitflow adopts the methodology of &quot;long live master, short live feature branch&quot; (as known as gitflow),

Feature branch: This is a private branch. For every feature, a new feature has to be created. The branch name would be in the format of <dev_name>/<feature_name>. Feature branch is short lived.

Dev branch: This is a public branch and the common source of truth for the up to date development. The code of this branch must pass all the unit test and integrated test.

Release branch: This is used for staging. Once development branch gather enough features, it will be fast-forward merged to this branch.

Main branch: This is used for production. If end-to-end testing and acceptance test pass, it will be fast-forward merged to this branch.


## 1.2) Test-driven development

Test-driven development is adopted. TDD means you have to first know the purpose of the code, then write the integration test and unit test. After that, write the actual code until all the tests pass.

But before commit the code, you have to run and pass all the unit tests.

## 1.3) Pushing changes, Pull request, Handling Merge conflict

### 1.3-A) Commiting changes locally

Use ```git add .``` and ```git commit -m "useful message"``` to commit
After that, it is a very good habit to pull the code first.

After using ```git commit -m "msg"``` to commit, pull remote master branch to local master branch.   To do this, type:   
  ```git pull --rebase --preserve-merges```
  
 This makes sure new local commits that hasn&#39;t been uploaded will be combined on top of the master branch codes,   
 so that the local commits will not be overwritten. 
 
(To use an alternative strategy with git stash, see section 5)

Never use rebase here, as the target merge branch is public.  Use merge instead to avoid tangling codes from two branches

After pulling, there would be some merge conflicts.
This is normal. Do not panic.
Go to section **"1.3-E) Handling Merge Conflict"** to see how to resolve Merge Conflect

### 1.3-B) Pushing changes 

Never push your code to dev branch directly.  
Push your code from your local <personal_name> branch to your remote <personal_name> branch. 
For example, If your name is meow.   
i) Make sure you are in your personal branch:    
```git checkout meow-1 ```.  
ii) Push to your remote personal branch:  
```git push origin meow-1```.  
  
Keep pull requests small and frequent.  
Each pull requests should not have more than 250 lines change.

### 1.3-C) Making sure CI is successful.   
A github workflow should have been set up.  
Go to Actions tab in the repository. 
Find that you are running a CI workflow.  
Make sure it is successful (green dot/ green tick) before making a pull request. 

Successful? Go to Pull requests.  
Click "New Pull Request. 

### 1.3-D) Making sure there is no conflict 

You go to a page which compares and tell you whether you can automatica merge.

For example, in github, there may be a message  
"Can’t automatically merge. Don’t worry, you can still create the pull request."  

Do not listen to it saying "Don't worry". It is always better to be careful.   
You should not create a pull request whenever it cannot automatically merge.   
  
    
So, cancel and exit the merging action page, so you still do not create the pull request yet.  
Get back to your terminal, we will resolve it in the terminal instead of web editor.  

Go to the next section **"1.3-E) Handling Merge Conflict"** to see how to resolve Merge Conflect

### 1.3E) Handling Merge Conflict

Go to your terminal
```
git checkout dev
git pull
git checkout meow-1
git merge dev
```
This command merge meow-1 to dev
Now, use your favorite IDE such as VS Code or IntelliJ.  
There should be options to let you view all the files that has unresolved conflicts.  
Some IDE, such as IntelliJ even can let you simply one click away to choose "take your own code" or "take their code" for each conflicting file.  
After resolving the issues, commit your change:   
```git commit -m "Resolve conflict and good message to tell which codes you decide to take"```

Then, do again:  
```git push origin meow-1```
  
(Remember, as I said in section 3B, you have to be patient check to make sure the githab action shows the CI/CD is a success before creating a pull request.  If the CI/CD is successful, create a pull request again.)   
  
Since you have resolved the conflict, it should let you automatically merge this time.  
Wait for a code reviewer if you want to ask for advice or approval.  
After merging the branch, tell your team about it with a communication software that your team choose to use. 


## 1.4) Keeping local code up to date

There are two scenarios you should update your feature branch with new updated code from development branch –   
i) Before Coding.  
2) After commiting the code (The Commit > Pull > Push steps)
ii) Immediately after there are approved pull requests from other team members

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

## `1.5) Delivery, Deploy and Release (to be confirmed)

Once dev branch gather enough features, the dev branch will be fast-forward merged into the release branch. The release branch code is delivered to the staging server. End-to-end testing and acceptance test are done by beta testers or QA team.

If there is something that has to be changed, the team will have to repeat the above process to introduce new changes in development branch. And then, fast-forward merge into release branch again

If end-to-end testing and acceptance test pass, the release branch will be fast-forward merged into the main branch and at the same time automatically deployed for production server / ready for users to install.

# 2) Advanced Guide 

## 2.1) Using git stash in merging (to be confirmed)  

Stash your local changes:  

```git stash```      
Update the branch to the latest code  

```git pull```    
Merge your local changes into the latest code:  

```git stash apply```  
Add, commit and push your changes. 
```
git add
git commit
git push
```  
  

## 2.2) CI / CD Automation Tool
1. Set a Github action prehook that will automatically do linting, unit testing, integration testing and test if it can build in local/remote environment. They pull request can be uploaded only if everything pass.   
2. On the development branch, integration testing can be done daily with a cron job.
3. Optimally, there should be a continuous integration tool to automate, such as rultor.com




## 3) Common Git F&Q

**Q: What is the difference between Merge vs Rebase?**

Both are for integrating new work new commit that are on separate branches. There are always at least 2 branches in play.

Merge preserves the branches tree, like a **gluing two branches**.

It is useful for combining branches that are already public

Rebase does not preserve branches tree like a **cutting a branch with a scissor and taping it elsewhere**.

Rebase is for combining private branches, not for public one.

<img src="https://github.com/psfr937/version-control-guide/blob/master/merge_and_rebase_illustration.jpg?raw=true" width="300" />

In rebase, no extra commits are created, but the exiting commits that you created are moved.

In rebase you can find that the commits has changed the hash because the timeline is changed, the hash has to be recalculated.
