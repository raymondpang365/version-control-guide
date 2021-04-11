## CI / CD Guide

This following workflow suggestion is in regards to agile project lifecycle.

This adopts the methodology of &quot;long live master, short live feature branch&quot; (as known as gitflow), adopts test-driven development, and also continuous integration/development/deployment to increase project efficiency.

## Gitflow

Feature branch: This is a private branch. For every feature, a new feature has to be created. The branch name would be in the format of \&lt;dev\_name\&gt;/\&lt;feature\_name\&gt;. Feature branch is short lived.

Dev branch: This is a public branch and the common source of truth for the up to date development. The code of this branch must pass all the unit test and integrated test.

Release branch: This is used for staging. Once development branch gather enough features, it will be fast-forward merged to this branch.

Main branch: This is used for production. If end-to-end testing and acceptance test pass, it will be fast-forward merged to this branch.

## Before Coding or upon approved pull request

There are two scenarios you should update your feature branch with new updated code from development branch – 1) Before Coding; 2) Immediately after there are approved pull requests from other team members

To do this, types the following commands:

git checkout develop

git pull

git checkout feature/foo

git merge develop

git push

This is very important. Otherwise, when somebody pushed new commit to the remote repository, the local repo would not be in sync with the remote repo. If new changes are made in working directory without keeping it updated first, conflicts easily occur.

**Advanced:**

If you are lazy, you can

git checkout feature/foo

git pull --all

git rebase develop

but a word of warning, rebase is best used on local feature branches that haven&#39;t been pushed, see [atlassian.com/git/tutorials/…](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing)

## Test-driven development

Test-driven development is adopted. TDD means you have to first know the purpose of the code, then write the integration test and unit test. After that, write the actual code until all the tests pass.

Every time you write a feature, commit the code. Each commit is not have more than 250 lines change.

But before commit the code, you have to run and pass the unit test.

## Integration

Only when the feature is fully implemented, the dev have to

1. Commit your code locally. Type &quot;git commit -m &quot;msg&quot; to commit&quot;
2. pull remote master branch to local master branch. To do this, type &quot;git pull --rebase=preserve&quot;

--rebase=preserve makes sure new local commits that hasn&#39;t been uploaded will be combined on top of the master branch codes, so that the local commits will not be overwritten

1. Set a Github action prehook that will automatically do linting, unit testing, integration testing and test if it can build in local/remote environment. They pull request can be uploaded only if everything pass.
2. git merge feature master to merge feature branch new codes to master locally.

Never use rebase here, as the target merge branch is public. Use merge instead to avoid tangling codes from two branches

1. Post a pull request. (aka git push)

When you post a pull request, the Github action prehook should be in place that automatically do linting, unit testing, integration testing and test if it can build in local/remote environment. The pull request is allowed only if everything pass.

1. A code review will then be performed before pushing the commit remotely. If you are in a solo project, merge pull request directly
2. If change is needed after the review, repeat step 1 - 6 until it passes the review.

Optimally, there should be a continuous integration tool to automate step 1 - 5, such as rultor.com

On the development branch, integration testing has to be done daily with a cron job.

## Delivery

Once dev branch gather enough features, the dev branch will be fast-forward merged into the release branch. The release branch code is delivered to the staging server. End-to-end testing and acceptance test are done by beta testers or QA team.

If there is something that has to be changed, the team will have to repeat the above process to introduce new changes in development branch. And then, fast-forward merge into release branch again

## Deploy

If end-to-end testing and acceptance test pass, the release branch will be fast-forward merged into the main branch and at the same time automatically deployed for production server / ready for users to install.

## Common Git F&Q

**Q: What is the difference between Merge vs Rebase?**

Both are for integrating new work new commit that are on separate branches. There are always at least 2 branches in play.

Merge preserves the branches tree, like a **gluing two branches**.

It is useful for combining branches that are already public

Rebase does not preserve branches tree like a **cutting a branch with a scissor and taping it elsewhere**.

Rebase is for combining private branches, not for public one.

![](RackMultipart20210411-4-18zohr_html_4bb265d20787a9aa.gif)

![](RackMultipart20210411-4-18zohr_html_48a92eff6b6b2cbc.gif) ![](RackMultipart20210411-4-18zohr_html_7d08db99e12cef45.gif) ![](RackMultipart20210411-4-18zohr_html_80c8c01d330f43eb.gif) ![](RackMultipart20210411-4-18zohr_html_88c9e44f08876be9.gif)

![](RackMultipart20210411-4-18zohr_html_975ac3674c428fc6.gif)

![](RackMultipart20210411-4-18zohr_html_f0c194f27df1c3c7.gif)

In rebase, no extra commits are created, but the exiting commits that you created are moved.

In rebase you can find that the commits has changed the hash because the timeline is changed, the hash has to be recalculated.
