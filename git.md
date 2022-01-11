# Git cheat sheet

## Concepts

1. Working area
1. Index (staging area)
1. Repository (permanent storage)
1. Stash

## Things to avoid always
1. Never use submodules. Use nuget packages and repo instead.

## Log and status
* `git status -s`
* `git log --oneline --graph -n 10`


## Undo changes / resetting

* Revert to previous commit, delete history: `git reset --hard`

* Mixed reset will only update index to usntage files from index: `git reset`

* Individual files, destructive
```
git reset menu.txt
git checkout HEAD menu.txt
```

* Clean everything, reset everything, both tracked, untracked, ignored, including directories
```
git reset --hard
git clean -f -x -d
```

## Branching
* Create local branch from current branch: `git switch -c <branchname>`
* Switch between two latest branches: `git switch -`
* Push to repo first time after creating: `git push --set-upstream <origin> <branchname>`
* Usually, `<origin> = origin`, but could be backup if I use backup repos
* List all branches local and remote: `git branch -a`
## Merging branches

* Cancel merge: `git merge --abort`

## Tagging
* Tag current commit on branch in work area: `git tag <tag-name>`
I've never used annotated tags
* Push tags to origin: `git push --tags`

## Safekeeping
* Stash everything: `git stash --include-untracked`
  
Note that stashes are only local. You need to set up a backup repo to store remotely, but that introduces complexity. See below.

## Backup a local repository remotely
IN PROGRESS
* Mirror the remote repo to your Google drive disk or similar (see backup a remote repository locally)
* Add as new remote under the name 'backup'
* `git push --set-upstream backup <branchname>`
 
  
## Backup a remote repository locally
* Set up the backup: `git clone --mirror <repo-url> <local-repo-dir>`
* Update backup: `git remote update`

## All the commands I will probably ever use

### Central ones
* add
* branch
* checkout (deprecate)
* cherry-pick
* clean
* clone
* commit
* diff
* fetch
* init
* log
* merge
* pull
* push
* rebase
* reset
* revert
* stash
* status
* switch
* tag

### Others
* config
* remote
* blame
* show-branch

## Git global aliases
```
git config --global alias.log1 'log --oneline --graph'
```
