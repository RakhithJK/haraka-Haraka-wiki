# Checklist

For rolling new Haraka releases:

1. Start a N-week quiet period
1. Update [Changes](Changes)
1. Update Public Suffix List
1. ...
1. ...
1. Bump version in package.json
1. git tag v<release_number>; git push --tags
1. npm publish

## Automation

Most of these steps are currently manual. Documenting the process is the first step towards automating it.

### Automation Ideas
* https://github.com/skywinder/github-changelog-generator
* GitHub Releases [haraka](https://github.com/baudehlo/Haraka/releases)