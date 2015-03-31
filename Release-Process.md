# Checklist

For rolling new Haraka releases:

1. Start a N-week quiet period
1. Update [Changes](Changes)
1. Update Public Suffix List
    * curl -o config/public-suffix-list https://publicsuffix.org/list/effective_tld_names.dat
1. Update TLDs files (for URIBL plugin and utils.split_hostname)
    * config/update_tld_files
1. For major version numbers, check for PRs with the versions # attached.
1. Bump version in package.json
1. git tag v<release_number>; git push --tags
1. npm publish
1. update [haraka's wikipedia page](https://en.wikipedia.org/wiki/Haraka_(software))

## Automation

Most of these steps are currently manual. Documenting the process is the first step towards automating it.

### Automation Ideas
* https://github.com/skywinder/github-changelog-generator
* GitHub Releases [haraka](https://github.com/baudehlo/Haraka/releases)