# git-cleancommit-hooks
This is a repository of Git Hooks that work with TDD to keep your local repo clean.
- pre-commit ensures that the tests pass when you commit code
- pre-push ensures that all files (except ignored) are committed.
- post-checkout automatically cleans your PR branches.

The intent of these hooks is to keep your local repository clean.  All commits should pass tests, and all PR pushes should be complete (no untracked or modified files).

## pre-commit
This is a git pre-commit hook that invokes mvn test.  If tests fail, the commit is rejected.

```
$ git commit -m "testing"
[git-autotest] Running pre-commit tests...

[git-autotest] [OK] All tests.
[tmp 6fc4601] testing
 1 file changed, 1 insertion(+)
 create mode 100644 foo.txt

```

```
$ git commit -m "testing"
[git-autotest] Running pre-commit tests...

    [mvn test] [ERROR] Tests run: 1, Failures: 1, Errors: 0, Skipped: 0, Time elapsed: 0.001 s <<< FAILURE! - in org.foo.SomeTest
    [mvn test] [ERROR] Failures: 
    [mvn test] [ERROR]   SomeTest.test:15
    [mvn test] [ERROR] Tests run: 2, Failures: 1, Errors: 0, Skipped: 0

[git-autotest] [ABORT] There are test failures.
```
## pre-push
`pre-push` ensures that your local changes are fully committed before pushing.  Any untracked, unstaged, or staged files will cause pre-push to reject the action, and suggest a `git commit` command to fix the issue.  

This helps catch any edits that didn't make the earlier commit.

```
$ git push
[git-cleanpush] Uncommitted changes were found:
[git-cleanpush]   CODEOWNERS
[git-cleanpush]   README.md
[git-cleanpush]   foo.txt
[git-cleanpush] Try using: git add --all && git commit
error: failed to push some refs to 'git@github.com:aetion/authorization.git'
```

## post-checkout
`post-checkout` helps clean up local feature branches that have been merged to master.  It supports merge commits, and squash-merge or cherry-pick.

It doesn't remove remote branches - those can be deleted from the Github PR.

```
$ git checkout master
Already on 'master'
Your branch is up to date with 'origin/master'.
[git-autobranch] Stale branches have been detected: 
[git-autobranch] squashed 5b71f8f  myprbranch
[git-autobranch] Delete all? (Y/n) y
[git-autobranch] Deleted branch myprbranch (was 5b71f8f).
```
# Credits
pre-commit was based on https://gist.github.com/arnobroekhof/9454645
