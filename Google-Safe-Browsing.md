### Use Google Safe Browsing with Haraka

The way to use GSB with Haraka is via the clamd plugin. ClamAV optionally includes the GSB database. To enable it you must set `SafeBrowsing yes` in `freshclam.conf` and manually run a `freshclam` to download the database.

To avoid rejecting based on GSB hits, add an entry such as `/Heuristics\.Safebrowsing/` to haraka/config/clamd.excludes.

### karma usage

Karma will prevent clamd from rejecting virus and instead assign spam scores. The following settings are recommended for config/karma.ini:

    results.clamd.fail@virus  = -16 if match
    results.clamd.fail@phish  =  10 if match

That results in -16 karma point for virus infected messages and -6 for GSB hits, which tend to have more false positives.