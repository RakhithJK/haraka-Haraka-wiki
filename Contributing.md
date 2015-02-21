We love and appreciate contributions to Haraka.

### To contribute, use the Github "Pull Request" mechanism

#### Overview

1. fork, by clicking the Fork button on the [GitHub Project Page](https://github.com/baudehlo/Haraka)
2. checkout a copy
3. create a branch
4. make changes
5. push changes to your fork
6. submit Pull Request

#### Detailed Example

```sh
export GHUSERNAME=CHANGE_THIS
git clone https://github.com/$GHUSERNAME/Haraka.git
cd Haraka
git checkout -b new_branch
$EDITOR server.js
git add server.js
git commit -m 'fixed bug in server.js'
git commit -m 'added test coverage for last commit'
git commit -m 'fixed bug discovered in testing'
git rebase -i origin
git push origin new_branch
```

The `git commit` step(s) will launch you into `$EDITOR` where the first line should be a summary of the change(s) in less than 50 characters. Additional paragraphs can be added starting on line 3.

If you have a more than one commit, squash them into a single commit during the `git rebase` step. Alternatively, with each subsequent commit, append the --amend flag to `git commit`.

To submit new_branch as a Pull Request, visit the [Haraka project page](https://github.com/baudehlo/Haraka) where your recently pushed branches will appear with a green "Pull Request" button.

### General Guidelines

* New features **must** be documented
* New features **should** include tests

### Style conventions

* 4 spaces for indentation (no tabs)
* Semi-colons on the end of statements are preferred
* Use underscores\_to\_separate\_names (yes this goes against JS conventions - it's the way it has always been done)
* Do not [cuddle elses](http://c2.com/cgi/wiki?CuddledElseBlocks)
* Use whitespace between operators - we prefer `if (foo > bar)` over `if(foo>bar)`
* Don't comment out lines of code, remove them as they will be in the revision history.
* Use boolean true/false instead of numeric 0/1
* See [Editor Settings](Editor-Settings)

## Tests

* run all tests: ./run_tests  (or "npm test")
* run tests for a single plugin: ./run_tests tests/plugins/bounce.js