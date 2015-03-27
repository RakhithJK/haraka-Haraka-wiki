# Install latest version of Haraka

```sh
npm install -g Haraka
```

# Update config files
Adjust HARAKA_DIR and HARAKA_CONF as necessary for your OS platform and install choice.

```sh
export HARAKA_DIR=/usr/local/lib/node_modules/Haraka
export HARAKA_CONF=/usr/local/haraka/config

cp $HARAKA_DIR/config/public-suffix-list $HARAKA_CONF
for t in "top-level-tlds two-level-tlds three-level-tlds extra-tlds"
do
   cp $HARAKA_DIR/config/$t $HARAKA_CONF/$t
done
```

## Update your config
Check for updates in your config files, and manually apply any changes that suite your preferences.
```sh
diff -r $HARAKA_DIR/config $HARAKA_CONF
```