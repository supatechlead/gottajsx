--- 
draft : false
date : 2024-03-25T14:37:18+01:00
title : "Create a New Branch From a Particular Commit in Git"
tags : ["git"]
showTableOfContents : true
type: "post"
---

To create a new branch from a particular commit in Git, follow these steps:

1. Identify the Commit ID: Ensure that you have the commit ID you want to create the new branch from. You’ve already provided the commit ID: `2ee3e2ebc459f777e16882433dbfc01a84d9d805.`

2. Checkout the Commit: Use the `git checkout` command to temporarily move to the specific commit:
```
git checkout 2ee3e2ebc459f777e16882433dbfc01a84d9d805
```

This command puts you in a “`detached HEAD`” state, which means you’re no longer on any branch. You’re now at the specified commit.

3. Create a New Branch: Now that you’re at the desired commit, create a new branch from it using the git branch command:
```
git branch new-branch-name
```
Replace `new-branch-name` with the name you want to give to your new branch.

4. Checkout the New Branch: To switch to the newly created branch, use the `git checkout` command again:
```
git checkout new-branch-name
```

Now you are on the new branch, and it is created from the specified commit.

5. Optional: Push the New Branch to `Remote` (if needed): If you want to push the new branch to a remote repository, use the `git push` command:
```
git push origin new-branch-name
```

This step is optional and depends on whether you want to share the new branch with others.
Now you have successfully created a new branch in Git starting from the specific commit with the given Commit ID.


