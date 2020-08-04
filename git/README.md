# git

## branch

 - **create remote**
   - local branch `git checkout -b <branch>`
   - push to origin `git push -u origin <branch>`
 - **delete remote** `git push origin --delete <branch>`
 - **list remote branches** `git branch -r`
 - **list merged branches** `git branch --merged`
 - **clean up stale branches** `git remote prune origin` or `git fetch -p`
 - **create branch from commit** `git branch <branch> <commit>` with checkout `git checkout -b <branch> <commit>`

## revert changes
 - **unstaged**
   - all changes `git checkout -- .`
   - single file `git checkout <path/to/file/to/revert>`
 - **pushed**
   - checkout commit `git checkout <commit> .`
   - commit and push `git commit -m "..." && git push`

## tags
Push only annotated tags, lightweight tags are for local development
 - `git tag -m <msg> <tag> && git push --follow-tags`
