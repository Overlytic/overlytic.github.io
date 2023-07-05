---
layout: post
published: true
hidden: true
---

# What helped to make git rebase less confusing for me

When I was learning git, I remember struggling with the `git rebase` function.
I am sure I am not the only one, so hopefully an explanation of what I found confusing will be able to help somebody else too.

First, let's zoom out a bit. As a source control system, git makes it possible to integrate code changes into a source code repository. For simplicity, let's assume the git repository has a main branch for the main code base and a feature branch where a new feature is being developed. After completion, the feature branch has to be integrated into the main branch. Git provides two main ways of achieving this namely merging and rebasing. 

*todo: high level description of merging vs rebasing?*

There are advantages and disadvantages to both approaches, and developers can be quite quite polarised strongly prefer one over the other. ...
...

*todo: setup of previous example*

```
git branch feature # create branch
git commit #
git switch feature
git commit
git commit
```

**todo: merging explanation + revisit previous example gif + permalink**

```
git switch main
git merge feature
```

*todo: explain what happened*

**todo: rebasing comparison + similar example gif + permalink**

```
git switch feature
git rebase main
```

*todo: explain what happened*

What I found confusing here is that though is that the main branch was not updated at all. If I want the feature included in the main branch (i.e., a comparable result to the previous section), another merge operation has to be done. 

```
git switch main
git merge feature
```

Now the feature is included in the main branch as well, making the main branch comparable with the first approach (simple git merge). I don't know if it's just me, but I did find it strange that this way still depended on the `merge` command though. Based on the discussions developers had I was under the impression that `rebase` and `merge` were independent workflows, and that developers preferring to use `rebase` avoided `merge` where possible in order to keep their history cleaner.

I checked to see if there was a way to accomplish this last step only using `rebase`, and in fact there is. This is the alternative way:

```
git switch main
git rebase feature
```

This second rebase felt quite confusing to me initially though. However, the below visualisation makes it clear what is actually happening and why is it equivalent to the merge.  

*todo: visualise merge/rebase of feature.*

Since the feature branch is a sub-branch of the main branch, there are no additional commits that need to be created by the rebase function. The main branch is therefore just fast-forwarding to the last commit of the feature branch (i.e, a fast-forward merge).

Putting all of the commands together. 

```
git switch feature
git rebase main
git switch main
git rebase feature
```

More compact approaches:

Approach 1:
```
# Merge the feature branch into the main branch
git merge feature main
```

Approach 2:
```
# Rebase the feature branch into the main branch
git rebase main feature
git rebase feature main
```

Golden rule:

*todo: golden rule of rebase ... mention ... from here:* 
*link: [atlassian merge and rebase tutorial](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)*

```
git rebase main feature
git merge feature main
```
