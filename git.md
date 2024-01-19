# Git cheat sheet

## Concepts

1. Working area
1. Index (staging area)
1. Repository (permanent storage)
1. Stash

## Things to avoid always

1. Never use submodules. Use packages, packagemanagers and repo instead.

## Log and status

```text
> git status -s
> git log --oneline --graph -n 10
```

## Undo changes / resetting

Revert to previous commit, delete history:

```text
> git reset --hard
```

Mixed reset will only update index to usntage files from index:

```text
> git reset
```

Reset individual files, destructive:

```text
> git reset menu.txt
> git checkout HEAD menu.txt
```

Clean everything, reset everything, both tracked, untracked, ignored, including directories:

```text
> git reset --hard
> git clean -f -x -d
```

## Branching

Create local branch from current branch:

```text
> git switch -c <branchname>
```

Create local branch from origin main branch as start-point but do not track it (push -u later)
```text
> git switch --no-track -c <branchname> origin/main
```
This one is great for creating new feature branch on a worktree feature branch location where you work with your current feature branches (if you have a main branch checked out in parallel somewhere else using worktree)

Switch between two latest branches:

```text
> git switch -
```

Push to repo first time after creating:

```text
> git push -u <origin> <branchname>
```

Usually, `<origin> = origin`, but could be backup if I use backup repos

List all branches local and remote:

```text
> git branch -a
```

Delete branches locally and remotely. If you have done a pull request on github or similar, you need to use the -D flag instead of -d.

```text
> git branch -d <branchname>
> git branch -D <branchname>   -- typically after a PR
> git push origin -d <branchname>
```

## Merging branches

Cancel merge:

```text
> git merge --abort
```

Continue merge after resolving (you may want to edit commit message):

```text
git merge --continue --no-edit
```

## Tagging

Tag current commit on branch in work area:

```text
> git tag <tag-name>
```

I've never used annotated tags

Push tags to origin:

```text
> git push --tags
```

Delete remote tags matching a pattern with Powershell (DANGER):

```powershell
(git tags) | Select-String -Pattern 'v.*' | ForEach-Object { [string]$str = $_; $str = $str.Trim(); (Write-Host "git push --delete origin $str") }
```

Prune tags deleted on remote:
```text
> git fetch  --prune --prune-tags
```


## Stashing

Stash everything, e.g. before a pull.

```text
> git stash --include-untracked
```

Note that stashes are only local. You need to set up a backup repo to store remotely, but that introduces complexity. See below.

## Backup a local repository to a directory

Typically a directory that is either on a shared disk with backup or to a cloud drive

* Mirror the remote repo to your Google drive disk or similar (see backup a remote repository locally)
* Add as new remote under the name 'backup'
* Use `git push -u` a.k.a. `git push --set-upstream` to set whatever branch to track a backup remote repo branch
* Now you can have ad-hoc commits that will never ever show up if you squash merge them into one of the origin-tracked repos before pushing upwards to origin.
* Try to avoid having multiple remotes for a specific branch. Rather create scratch branches that only tracks the backup location, then squash merge. At least if you want to avoid long commit histories and keep mess away from your main repo.

Example (Windows cmd/powershell):

```text
> cd <backupdirlocation>
> git clone c:\Development\SomeProject SomeProject --mirror
> cd c:\Development\SomeProject
> git remote add backup <backupdirlocation>
> git switch -c scratch/somebackupbranch
> git push -u backup scratch/somebackupbranch
```
  
## Backup a remote repository locally

Set up the backup:

```text
> git clone --mirror <repo-url> <local-repo-dir>
```

Update backup:

```text
> git remote update
```

## My most used commands

Here are the central ones, categorized:

| Setup, repository <br> and status | Branch work | File work |
| --------------------------------- | ----------- | --------- |
| clone                             | commit      | add       |
| init                              | switch      | clean     |
| log                               | push        | diff      |
| status                            | pull        | reset     |
| [worktree](git.worktrees.md)      | fetch       | restore   |
| config                            | tag         | stash     |
| remote                            | revert      | blame     |
| branch                            | merge       | rm        |
| show-branch                       | rebase      | mv        |
|                                   | cherry-pick | difftool  |
|                                   | revert      | apply     |
|                                   | checkout    |           |
|                                   |             |           |

Full list of git commands: <https://git-scm.com/docs>

Useless commands (for me):

| Name      | Reason                                                       |
| --------- | ------------------------------------------------------------ |
| submodule | Use packaging and package managers and avoid a world of pain |
|           |                                                              |

## Git global aliases

```text
> git config --global alias.log1 'log --oneline --graph'
> git config --global alias.status1 'status -b -s'
```

## List all existing aliases

```text
> git config --get-regexp alias
```
