# Chapter 1 - Basic Concept

## 1.1) The common misunderstanding of branch and commit

## 1.2 Difference between Working directory, Staging area & Commit history

## 1.3) What is the difference between Merge vs Rebase?

Both are for integrating new work new commit that are on separate branches. There are always at least 2 branches in play.

Merge preserves the branches tree, like a **gluing two branches**.

It is useful for combining branches that are already public

Rebase does not preserve branches tree like a **cutting a branch with a scissor and taping it elsewhere**.

Rebase is for combining private branches, not for public one.

In rebase, no extra commits are created, but the exiting commits that you created are moved.

In rebase you can find that the commits has changed the hash because the timeline is changed, the hash has to be recalculated.
