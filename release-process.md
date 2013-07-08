---
title: Release process
---

* review all code to make sure no alpha-testing code has slipped in
* run a from-scratch test deployment
* run site with `E_NOTICE` errors enabled
* update the docblock at the head of each document
* update `CHANGELOG.md` based on both the closed issues and the actual changes
* review `LICENSE` to make sure that any newly incorporated packages are credited
* review `README.md`
* tag the release on GitHub
* create a new Vagrant image
  * upload it and publish the URL
* write a blog entry announcing it
* tweet about it
* inform every known State Decoded user
