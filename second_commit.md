## Fix review comments in a multi-commit PR

Put each fix in the commit it belongs to. For PR #2, the first commit is
`2c1ac11`.

```bash
git add first_commit.md
git commit --fixup=2c1ac11
```

Repeat for other fixes, using the ID of the relevant commit. Then combine the
fixes with their commits:

```bash
git fetch origin
git rebase -i --autosquash origin/main
git push --force-with-lease
```

Save and close the rebase editor when it opens. If Git reports a conflict,
resolve it, stage the file, and run `git rebase --continue`.

Only force-push if nobody else is working on the PR branch. If clean individual
commits are not required, make one new review-fix commit and push normally.