---
layout: post
published: true
hidden: true
---

# What helped to make git rebase less confusing for me

When I was learning git, I remember struggling with the `git rebase` function.
I am sure I am not the only one, so hopefully an explanation of what I found confusing will be able to help somebody else too.

First, let's zoom out a bit. As a source control system, git makes it possible to integrate code changes into a source code repository. For simplicity, let's assume the git repository has a main branch for the main code base and a feature branch where a new feature is being developed. After completing the feature, the feature branch has to be integrated into the main branch. Git provides two main ways of achieving this: merging and rebasing. 

## Toy Example

Let's use the following toy example to discuss merging and rebasing. 

![](https://github.com/Overlytic/overlytic.github.io/blob/master/images/git-merge-rebase-v0.JPG?raw=true)

You can play with this example yourself by following this link: [learngitbranching toy example](https://learngitbranching.js.org/?NODEMO&command=importTreeNow%20%7B%22branches%22%3A%7B%22main%22%3A%7B%22remoteTrackingBranchID%22%3Anull%2C%22remote%22%3Afalse%2C%22target%22%3A%22C2%22%2C%22id%22%3A%22main%22%2C%22type%22%3A%22branch%22%7D%2C%22feature%22%3A%7B%22remoteTrackingBranchID%22%3Anull%2C%22remote%22%3Afalse%2C%22target%22%3A%22C4%22%2C%22id%22%3A%22feature%22%2C%22type%22%3A%22branch%22%7D%7D%2C%22commits%22%3A%7B%22C0%22%3A%7B%22type%22%3A%22commit%22%2C%22parents%22%3A%5B%5D%2C%22author%22%3A%22Peter%20Cottle%22%2C%22createTime%22%3A%22Mon%20Nov%2005%202012%2000%3A56%3A47%20GMT-0800%20%28PST%29%22%2C%22commitMessage%22%3A%22Quick%20Commit.%20Go%20Bears%21%22%2C%22id%22%3A%22C0%22%2C%22rootCommit%22%3Atrue%7D%2C%22C1%22%3A%7B%22type%22%3A%22commit%22%2C%22parents%22%3A%5B%22C0%22%5D%2C%22author%22%3A%22Peter%20Cottle%22%2C%22createTime%22%3A%22Mon%20Nov%2005%202012%2000%3A56%3A47%20GMT-0800%20%28PST%29%22%2C%22commitMessage%22%3A%22Quick%20Commit.%20Go%20Bears%21%22%2C%22id%22%3A%22C1%22%7D%2C%22C2%22%3A%7B%22type%22%3A%22commit%22%2C%22parents%22%3A%5B%22C1%22%5D%2C%22author%22%3A%22Peter%20Cottle%22%2C%22createTime%22%3A%22Wed%20Jul%2005%202023%2023%3A04%3A55%20GMT+0200%20%28South%20Africa%20Standard%20Time%29%22%2C%22commitMessage%22%3A%22Quick%20commit.%20Go%20Bears%21%22%2C%22id%22%3A%22C2%22%7D%2C%22C3%22%3A%7B%22type%22%3A%22commit%22%2C%22parents%22%3A%5B%22C1%22%5D%2C%22author%22%3A%22Peter%20Cottle%22%2C%22createTime%22%3A%22Wed%20Jul%2005%202023%2023%3A04%3A58%20GMT+0200%20%28South%20Africa%20Standard%20Time%29%22%2C%22commitMessage%22%3A%22Quick%20commit.%20Go%20Bears%21%22%2C%22id%22%3A%22C3%22%7D%2C%22C4%22%3A%7B%22type%22%3A%22commit%22%2C%22parents%22%3A%5B%22C3%22%5D%2C%22author%22%3A%22Peter%20Cottle%22%2C%22createTime%22%3A%22Wed%20Jul%2005%202023%2023%3A04%3A59%20GMT+0200%20%28South%20Africa%20Standard%20Time%29%22%2C%22commitMessage%22%3A%22Quick%20commit.%20Go%20Bears%21%22%2C%22id%22%3A%22C4%22%7D%7D%2C%22tags%22%3A%7B%7D%2C%22HEAD%22%3A%7B%22id%22%3A%22HEAD%22%2C%22target%22%3A%22main%22%2C%22type%22%3A%22general%20ref%22%7D%7D)

### Git merge

Git merge combines the changes of one branch (e.g. feature) and adds it to the target branch (e.g. main). 

![](https://github.com/Overlytic/overlytic.github.io/blob/master/images/git-merge-rebase-v0.JPG?raw=true)

Overall merging is simpler to perform than rebasing. It doesn't change the commit history of either of the branches, meaning nothing can be lost. This also makes it relatively easy to undo. However as can be seen in the above example, a merge commit was created during the process. On busy repositories, these merge commits can multiply and make the commit history quite cluttered.

For easy reference, these are the git commands used in the above example.

```
# run on main branch
git merge feature
```

### Git rebase

Git rebasing on the other hand recreates the commits of a branch (e.g. feature) as if they occurred at the end of another branch (e.g. main).

![](https://github.com/Overlytic/overlytic.github.io/blob/master/images/git-merge-rebase-v0.JPG?raw=true)

This makes it possible to have a more streamlined commit history that is easier to follow. However, the drawback is that the commit history is being rewritten. This can  which can cause some information to be lost (e.g. merge commits from other's  in your feature branch).  Conflicts during the process can be harder to resolve. It's also very important to not rewrite the history of public branches that other people rely on (more about this later).

For easy reference, these are the git commands used in the above example.

```
git switch feature
git rebase main
```

If I want the feature included in the main branch (i.e., a comparable result to the previous section), another merge operation has to be done. 

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

Shortened:
```
# Rebase onto the tip of the main branch the commits from the feature branch
git rebase main feature

# Rebase onto the tip of the feature branch the commits from the main branch
git rebase feature main
```

Golden rule:

*todo: golden rule of rebase ... mention ... from here:* 
*link: [atlassian merge and rebase tutorial](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)*

```
# Rebase onto the tip of the main branch the commits from the feature branch
git rebase main feature
git switch main
git merge feature
```
