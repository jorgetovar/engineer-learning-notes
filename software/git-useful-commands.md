# Git useful commands

Our brain is better at processing data than storing it. So that's why I'm creating this document with one of the useful but maybe not so common commands to get the best for this wonderful tool

- Unstage all files you might have staged with git add
```shell
git reset
```
- Revert all local uncommitted changes
```shell
git checkout .
```
- Revert all uncommitted changes
```shell
git reset --hard HEAD
```
- Remove all local untracked files, so only git tracked files remain

- Delete all branches except the main one
```shell
git branch | grep -v "main" | grep -v "master" | xargs git branch -D
```
- Delete the most recent commit, keeping the work you've done
```shell
git reset --soft HEAD~1
```
- Remove .git tracking
```shell
rm -rf .git
```
- Remove a file from a Git repository without deleting it from the local filesystem
```shell
git rm --cached .classpath
```

- If you have a sequence of commits, to squash commit2 to commit5 into a single commit, you can reset your branch to commit1 and then commit again [ commit1, commit2, ..., commit5, HEAD] 


```shell
git reset --soft commit1
git commit
```

- How to revert to the origin's master branch's version of the file
```shell
git checkout origin/master filename
```

- Resolve Git merge conflicts in favor of their changes during a pull
In a conflicted state, and you want to just accept all of theirs

```shell
git checkout --theirs .
git add .
# or in the pull
git pull -X theirs
# In a conflicted state by file
git checkout --theirs path/to/file
```