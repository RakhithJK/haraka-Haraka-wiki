

- [ ] log.elasticsearch plugin #906
- [ ] why blocked?  [example](https://mail.theartfarm.com/haraka/)
    - [ ] return URL as block message
    - [ ] display `Why Blocked` page when URL visited
    - [ ] page shows logs for that UUID
        - [ ] log.search (think: web enabled grep for haraka logs)
    - [ ] shows ordered list of karma rules that caused block
        - [ ] karma rules need to be indexed
    - [ ] shows resolution steps
- [ ] penalty plugin (the resurrection of karma's penalty feature)
    - [ ] rate limit asn_all_bad (think: 1 conn/hour or per day)
    - [ ] rate limit all_bad IP
    - [ ] rate limit unknown senders who send really bad stuff
- [ ] bayes training
    - [ ] spamassassin
    - [ ] rspamd
    - [ ] dspam
- [ ] content scanning server baudehlo/haraka#1032