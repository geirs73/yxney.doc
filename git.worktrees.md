# Git worktrees like I like them

I like my worktrees in my development directory like this:

![git worktree figure](git.worktrees.drawio.png)

The following works at any repo. It works by creating an empty detached branc named `root`. Remove all local files (but not the .git dir of course)

```text
> git switch --orphan root
> git commit --allow-empty -m "Initial commit on orphan branch"
> git push -u origin root
```

Then we just add the worktrees in subfolders. I forego the feature directory as part of the structure, 
as I never have same name for features or bugfixes or similar, and it makes it much easier to compare

```text
> git worktree add main main
> git worktree add release release
> git worktree add feature1 feature/feature1
> git worktree add feature2 feature/feature2
```




