# GIT 

# Info
## Links
[slingaccademy](https://www.slingacademy.com/article/git-commands-comprehensive-cheat-sheet-examples/)

## info commands
```bash
git help <command>
# show all brancheds
git branch -v
# show a changeset
git show <hash, ...>

# shows recent activities
git show log 
# show the head (latest commit)
git show HEAD  # commit ff8bb51f869eb043a80d772c27820fcb22beb3eb (HEAD -> dev, origin/dev, fix/abc) 

# Show parent use ^ or ~ (on any sha or HEAD or branch)
git show HEAD^
git log dev~
# Show ancestry use ^^ or ~2
git show HEAD^^
git show dev~2

# Show Tree content: git ls-tree <tree-ish>
git ls-tree dev
git ls-tree dev -r # recursive.. breaks the "tree" up into its fils
```
## Log
```bash
# last n records, e.g. 3
# use -p for patch to see details about each commit
# --stat for statistics on commits
# --format=oneline or --oneline
git log -3 --oneline# last three entries
git log --graph --all --oneline --decorate
git log --stat

git log --since=2024-08-26
git log --until=2024-08-20

git log --after=2.weeks --before=3.days
git log --after=1.week
git log --after=1.day
git log --auther="tom"

git log SHA..HEAD  #between two commits

git log <filename> # shows me all the commits on that file
```

# Commit
##
```bash
git add <filename>
# -m for message
git commit -m 'message'
# or in one step
git commit -am 'message

```

# Basics branching
## pull specific branch from remote
```bash
git pull origin <branch_name> # e.g. git pull origin dev
```
## Switching Branches (checkout old way)
```bash
# list all branches (local and remote)
git branch -a
####
# git checkout replaced by: switch and restore
### switching branchwes
git switch <branch name>  # switch branchs
#### restoring files
git restore <file_name> # reset the file to the last staged or repo
git restore --staged <file_name
```
## New Branch
```bash
git switch -c <new_branchname>
# or old way
git checkout -b iss53
# shorthand for
$ git branch <new_branchname>
$ git switch <branchname>

```
## Merge Branch
```
# go to target branch (e.g. git switch dev)
git merge <
```

## Push new branch to remote
```bash
git push -u origin <branch>
```

## Rename a branch
This 
```bash
git branch -m <old_name> <new_name>
```

## Delete branch
```bash
git push -d <remote_name> <branchname>   # Delete remote - usually <remote_name> will be origin
git branch -d <branchname>               # Delete local
```

# Tagging
```bash
# show all the tags
git tag
# show all the tag details
git tag -l -n

# adding an anootated Tag
# -a is for annotate 
git tag -a v0.9 -m "Initial release version"

# pusing all tags to origin
git push origin --tags
# pushing a specific tag
git push origin v0.9

# create a branch and checkout
git checkout -b tag/v0.9 v0.9
```

# Gneeral
Git can reference a point in time via
- SHA-1 hash (40 char) you can use a min of 4 or unambigous 8-10 - UI shows 7 for example
- HEAD pointer reference
- Branch reference
- Tag reference
- Ancestry

## HEAD Pointer
Points to the head of head `git show HEAD`. 
- .git/HEAD is checked first


# Starting from scratch
## Git init
```bash
# create empty git 
git init

# there is no branch yet

# but head points to a "main branch already"
# .git/HEAD :  
ref: refs/heads/main

```

## Create first readme file stage them and commit it
``#`bash
echo "# tinker_with_git" >> README.md
git add README.md
git commit -m "first commit"
```

## Create main branch 
```bash
git branch -M main
```
## Add remote and push main branch
`.git/config` before:
```
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
```



```bash
# add the remote origin
git remote add origin https://github.com/tomafischer/tinker_with_git.git
```
`.git/config` after:
```
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = https://github.com/tomafischer/tinker_with_git.git
        fetch = +refs/heads/*:refs/remotes/origin/*
```

### push main via -u
The `-u` in this command is a short-hand for `--set-upstream`. Using this flag you are basically telling git to automatically link your local master to the remote master. Therefore you only need to do this once. After that you can just write `git pull` and `git push` on master.

The -u flag sets the merge property in `.git/config` of the branch

```bash
git push -u origin main
```
`.git/config` after:
```
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = https://github.com/tomafischer/tinker_with_git.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
        remote = origin
        merge = refs/heads/main
```

## Branch
### Create dev branch
```bash
git switch -c  dev
#or
git branch dev
git switch dev
```

### Change readme file, stage and commit
```
git add README.md
git commit -m 'branch created'
```
### Push remote - first time use -u
```bash
git push -u origin dev
# => Branch 'dev' set up to track remote branch 'dev' from 'origin'
```
The -u flag sets the merge property in `.git/config` of the branch
`.git/config`
```
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[remote "origin"]
        url = https://github.com/tomafischer/tinker_with_git.git
        fetch = +refs/heads/*:refs/remotes/origin/*
[branch "main"]
        remote = origin
        merge = refs/heads/main
[branch "dev"]
        vscode-merge-base = origin/main
        remote = origin
        merge = refs/heads/dev
```
without the `-u` it looks like that (added test without `-u`):
```
[branch "dev"]
        vscode-merge-base = origin/main
        remote = origin
        merge = refs/heads/dev
[branch "test"]
        vscode-merge-base = origin/dev
```