
# Changes

The idea is to put **significant** upgrade issues here, to save folks from having to trawl through the contents of [Changes](https://github.com/haraka/Haraka/blob/master/Changes).  When you run into items that *should* be listed here, please add them!

# Haraka v2.8

Following an upgrade to 2.8 we recommend deleting any config files in your personal Haraka config folder that you have not modified. The reason for this is that Haraka will now load non-modified config data from the core Haraka folder. This change makes future upgrades much easier.

# 2016.02.13

* updated for compat with eslint 2. Developers will likely need to:

    cd haraka && npm install && sudo npm install -g eslint

# 2016.01.21

* smtp\_proxy & qmail-queue: default to enabled for outbound deliveries
  (previously used Outbound), to better matches user expectations. To get
  previous behavior, add a config file with `enable_outbound=false`. 


# 2013.12.27

* new plugin: data.headers
    * deprecates data.rfc5322_header_checks.js
    * deprecates data.noreceived.js
    * deprecates data.nomsgid.js