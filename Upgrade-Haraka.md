# Install latest version of Haraka

```sh
npm install -g Haraka
```

# Update config files
Adjust HARAKA_DIR and HARAKA_CONF as necessary for your OS platform and install choice.

```sh
export HARAKA_DIR=/usr/local/lib/node_modules/Haraka
export HARAKA_CONF=/usr/local/haraka/config

cp $HARAKA_DIR/config/public_suffix_list $HARAKA_CONF
for t in "top-level-tlds two-level-tlds three-level-tlds extra-tlds";
do
   cp $HARAKA_DIR/config/three-level-tlds
done
```