This following workflow suggestion is in regards to agile project lifecycle.

This adopts the methodology of "long live master, short live feature branch" (as known as github flow), adopts test-driven development, and also continuous integration/development/deployment to increase project efficiency. 

## Git branching structure

Feature branch: For every feature, a new feature has to be created. The branch name would be in the format of <dev_name>/<feature_name>.
Feature branch is short lived.

Dev branch: The code of this branch must pass all the unit test and integrated test, and is sitting in the staging server.

Release branch: Once development branch gather enough features, it will be fast-forward merged to this branch. The is used for staging. 

Master branch: If end-to-end testing and acceptance test pass, it will be fast-forward merged to this branch. The production code is in here. 

## Before Coding or upon approved pull request

If there are new coming approved pull requests, get feature branch updated:
1) git checkout master and git pull origin master to pull the code from master branch of the remote repo to the master branch of local repo
2) then, git merge master feature to merge local master branch into feature branch in order to keep the feature branch in sync with the latest updated

(Otherwise, when somebody pushed new commit to the remote repository, the local repo would not be in sync with the remote repo. If new changes are made in working directory without keeping it updated first, conflicts easily occur.)

## Coding

Test-driven development is adopted. TDD means you have to first know the purpose of the code, then write the integration test and unit test. After that, write the actual code until all the tests pass.

It is recommended that when all unit & integration tests are passed, devs should commit to local repo immediately.

Unit testing before local commit is a recommended practice, though not compulsory.

## Integration

Only when the feature is fully implemented, the dev have to

1) git commit -m "msg" to commit locally
2) If pass,
git pull --rebase=preserve to pull remote master branch to local master branch.
--rebase=preserve makes sure new local commits that hasn't been uploaded will be combined on top of the master branch codes,
so that the local commits will not be overwrited
3) (pre-hook) Enforce automatic linting, unit testing and integration testing before they can post a pull request
4) git merge feature master to merge feature branch new codes to master locally. (Never use rebase here, as the target merge branch is public. Use merge instead to avoid tangling codes from two branches)
5) post a pull request. (aka git push) A code review will then be performed before pushing the commit remotely.
6) If change is needed after the review, repeat step 1 - 5 until it passes the review.

Optimally, there should be a continuous integration tool to automate step 1 - 5, such as rultor.com

On the development branch, integration testing has to be done daily with a cron job.

## Delivery

Once dev branch gather enough features, the dev branch will be fast-forward merged into the release branch. The release branch code is delivered to the staging server. End-to-end testing and acceptance test are done by beta testers or QA team.

## Deploy

If end-to-end testing and acceptance test pass, the release branch will be fast-forward merged into the master branch and at the same time automatically deployed for production server / ready for users to install.
