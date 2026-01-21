# Merge vs Rebase 

This document explains **when to use merge vs rebase** in real DevOps workflows.

---

## The Core Difference

Both commands integrate changes from one branch into another.

The difference is **history behavior**.

---

## git merge (SAFE & TEAM-FRIENDLY)

### What it does
- Combines branches
- Preserves full history
- Creates a merge commit

### When to use merge
- Shared branches
- Pull Requests
- main / develop branches
- Production repositories

### Rule
If others may have pulled the branch → **MERGE**

---

## git rebase (CLEAN & LOCAL)

### What it does
- Rewrites commit history
- Replays commits on top of another branch
- No merge commit

### When to use rebase
- Local feature branches
- Before opening a PR
- Cleaning up commits

### Rule
Rebase local → merge shared

---

## The Golden Rule (Memorize This)

❌ Never rebase a branch that others are already using.

Why?
- History rewrite
- Teammates break
- CI failures
- Production incidents

---

## Real DevOps Workflow

### Feature development
```bash
git switch feature-x
git rebase main
```

### Integration
- Open Pull Request
- Merge into main (no rebase)

---

## Summary

| Scenario | Use |
|--------|-----|
| Local cleanup | rebase |
| Shared branch | merge |
| Production fix | revert |
