# Chatper 4 - Dangerous & Alternative Workflow


## 4.1) Make commits locally on DEV branch

Use```git pull origin dev --rebase --preserve-merges```. 
``` --rebase --preserve-merges``` makes sure new local commits that hasn&#39;t been uploaded will be combined on top of the master branch codes,   
 so that the local commits will not be overwritten.   
**!!!Never use ```rebase``` without ```preserve-merges```!!!, otherwise, it will cause tangling codes among two branches.**

