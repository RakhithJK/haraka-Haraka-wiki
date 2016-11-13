# Checklist

For rolling new Haraka releases:

- [ ] Start a N-week quiet period
- [ ] Update [haraka-tld](https://github.com/haraka/haraka-tld)
- [ ] Draft a release on the [releases page](https://github.com/haraka/Haraka/releases) (Save draft)
- Create a PR with:
    - [ ] Updated [Changes](https://github.com/haraka/Haraka/blob/master/Changes)
    - [ ] Bumped version in [package.json](https://github.com/haraka/Haraka/blob/master/package.json)
    - [ ] Updated dependencies identified with `grunt versioncheck`
- For major version releases:
    * [ ] check for waiting PRs for that milestone version.
    * [ ] Grep the source for: `Sunset v3`, where 3 is the new major version.
- [ ] Update release date in [Changes](https://github.com/haraka/Haraka/blob/master/Changes)
- [ ] `npm publish`
- [ ] Post `Changes` file on the draft release and publish the release 
- [ ] update [haraka's wikipedia page](https://en.wikipedia.org/wiki/Haraka_(software))
- [ ] Email the mailing list
- [ ] Publish a blog post
- [ ] Edit the /topic on #haraka IRC channel

## Automation

Most of these steps are currently manual. Documenting the process is the first step towards automating it. There are some [related comments here](https://github.com/haraka/Haraka/pull/1468#issuecomment-220100505).

### Automation Ideas
* https://github.com/skywinder/github-changelog-generator
* GitHub Releases [haraka](https://github.com/haraka/Haraka/releases)