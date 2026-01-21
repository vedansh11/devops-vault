# Git Cheatsheet (DevOps)

## Setup
git init
git clone repo_url

## Daily Workflow
git status
git add .
git commit -m "message"
git pull
git push

## Branching
git branch
git switch branch
git checkout branch
git merge branch
git rebase branch

## History & Diff
git log --oneline --graph
git diff

## Undo
git reset --soft HEAD~1
git reset --hard HEAD~1
git revert commit_id

## Remote
git remote -v
git fetch
git pull
git push

## CI/CD Tips
- Never rebase shared branches
- Use revert in production
