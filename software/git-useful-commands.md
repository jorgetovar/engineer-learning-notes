# Git useful commands

Our brain is better at processing data than storing it. So that's why I'm creating this document with one of the useful but maybe not so common commands to get the best for this wonderful tool

- Unstage all files you might have staged with git add:
```shell
git reset
```
- Revert all local uncommitted changes:
```shell
git checkout .
```
- Revert all uncommitted changes:
```shell
git reset --hard HEAD
```
- Remove all local untracked files, so only git tracked files remain:
```shell
git clean -fdx
```
*WARNING*: -x will also remove all ignored files, including ones specified by .gitignore! You may want to use -n for a preview of files to be deleted.

- Delete all branches except the main one
```shell
git branch | grep -v "main" | grep -v "master" | xargs git branch -D
```
- Delete the most recent commit, keeping the work you've done:
```shell
git reset --soft HEAD~1
```
- Remove .git tracking
```shell
rm -rf .git
```
- Remove archive from commit
```shell
git rm --cached .classpath
```