# Chatper 2 - Advanced Guide 

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
