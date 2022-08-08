# GIT useful commands

### Delete all branches except the main one

git branch | grep -v "main" | grep -v "master" | xargs git branch -D

### Delete the most recent commit, keeping the work you've done:

git reset --soft HEAD~1

### Remove file from .git tracking

 rm -rf .git
 
### Remove archive from commit
 
git rm --cached .classpath
