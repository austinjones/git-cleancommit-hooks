# precommit-mvntest
This is a git pre-commit hook that invokes mvn test.  If tests fail, the commit is rejected.

I built this because sometimes I switch tasks, and commit unfinished changes.  Sometimes those changes cause test failures.  It's better to fix them now when everything is fresh, then have to dig back in later.

# example output:
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

# Credits
This script was based on https://gist.github.com/arnobroekhof/9454645
