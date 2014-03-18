We love and value outside contributions to Haraka.

### If you wish to contribute, please use the Github "Pull Request" mechanism

1. fork the project (git clone https://github.com/baudehlo/Haraka.git)
1. create a branch for your changes (git checkout -b new_branch)
1. make changes and commit (git commit)
1. push changes to github (git push)
1. click on "Create Pull Request" for that branch in Github.

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

## Tests

* run all tests: ./run_tests  (or "npm test")
* run tests for a single plugin: ./run_tests tests/plugins/bounce.js
