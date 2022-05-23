# git

## branch

 - create remote
   - local branch `git checkout -b <branch>`
   - push to origin `git push -u origin <branch>`
 - delete remote `git push origin --delete <branch>`
 - list remote branches `git branch -r`
 - list merged branches `git branch --merged`
 - clean up stale branches `git remote prune origin` or `git fetch -p`
 - create branch from commit `git branch <branch> <commit>` with checkout `git checkout -b <branch> <commit>`

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

## github pull request
- clone original repo
- fork repo to personal repo
- add remote to point to personal repo e.g. `git remote add forked-repo git@github.com:<personal>/<repo>.git`
- push to forked/personal repo e.g. `git push -u forked-repo <branch-name>`
- create PR

## github update forked repo
- check remotes `git remote`
- `git fetch <remote>` e.g. `git fetch forked-repo`
- `git pull <remote> <branch>` e.g. `git pull forked-repo main`

## github gh-pages
- create branch `git checkout --orphan gh-pages`
- clean up (prepare for github pages) `git rm -rf .`
- commit `git commit -m "Initial commit" --allow-empty`
- push `git push -u origin gh-pages`

