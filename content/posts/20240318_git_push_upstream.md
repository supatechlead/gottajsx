--- 
draft : false
date : 2024-03-18T14:37:18+01:00
title : "Git Push Upstream Explanation"
tags : ["git"]
showTableOfContents : true
type: "post"
---

Trying to do a simple `git push` on a project leads to the error message `"fatal: Current branch 'master' has no upstream branch"`

## Explanation 

The error message `"fatal: Current branch 'master' has no upstream branch"` indicates that your local `master` branch doesn't have a designated upstream branch to track changes from. This can be easily fixed by following the steps below:

1. Understand Upstream Branches:

An upstream branch is a remote branch that your local branch tracks and updates from. By setting an upstream branch, you can easily pull changes from the remote repository and push your local changes to it.

2. Set Upstream Branch:

To set your local `master` branch to track the remote 'origin/master' branch, run the following command:
```
git push --set-upstream origin master
```
This command will push your local `master` branch to the remote `origin/master` branch and set it as the upstream branch.

3. Benefits of Setting Upstream Branch:

Simplified Workflow: Setting an upstream branch simplifies your workflow by enabling you to use shorter commands for pushing and pulling changes. For example, you can simply use:

* `git pull` to fetch and merge changes from the remote `origin/master` branch into your local `master` branch.

* `git push` to push your local `master` branch changes to the remote `origin/master` branch.

* Automatic Tracking: With an upstream branch set, Git can automatically detect when the remote branch has been updated and prompt you to pull the changes.

## BreakDown

Here's a breakdown of when to use each approach:

* Use `git push` if:

    * This is a one-time push and you don't plan to interact with the remote branch frequently.

* Use `git push --set-upstream origin master` if:
    * You plan to work with the remote branch regularly (pushing and pulling changes). This sets up a more convenient workflow for future operations.

Remember, you can always set the upstream branch later using `git branch --set-upstream-to origin/master master` if you decide to use `git push` initially.