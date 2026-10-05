## Address review comments in a PR with multiple commits

When a PR has several commits, put each review fix into the commit it belongs to.
For example, PR #2 has these commits:

| Commit | ID |
| --- | --- |
| First | `2c1ac11` |
| Second | `fca3ef8` |
| Third | `8c2e8dd` |
| Fourth | `9134992` |

1. Check the working tree and identify the target commits:

   ```bash
   git status
   git log --oneline origin/main..HEAD
   ```

2. Make the requested changes. Stage only the changes for **one** target commit,
   then create a fixup commit:

   ```bash
   git add -p
   git commit --fixup=2c1ac11
   ```

   Use `git add path/to/new-file` for a new file. Repeat for other target commits,
   using their respective IDs. Keep unrelated review fixes in separate fixup commits.

3. Once all changes are committed, fold the fixups into their target commits:

   ```bash
   git fetch origin
   git rebase -i --autosquash origin/main
   ```

   Git places each fixup after its target in the editor. Save and close the editor.
   If a conflict occurs, resolve it, stage the resolved files, and run
   `git rebase --continue`.

4. Check the resulting history and changes:

   ```bash
   git log --oneline origin/main..HEAD
   git diff origin/main...HEAD
   ```

5. Update the existing PR:

   ```bash
   git push --force-with-lease
   ```

   Rewriting commits changes their IDs. Coordinate first if someone else is
   pushing to the same branch, and never use a plain `--force` here. After the
   push, reply to the review comments with what changed.

If the team does **not** require fixes in their original commits, a single new
review-fix commit followed by a normal `git push` is simpler and avoids
rewriting history.