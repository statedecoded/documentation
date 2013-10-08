---
title: Release Process
layout: default
---

* review all code to make sure no alpha-testing code has slipped in
* run a from-scratch test deployment
* run site with <code>E_NOTICE</code> errors enabled
* validate site template
* validate CSS
* update the docblock at the head of each document
* update <code>CHANGELOG.md</code> based on both the closed issues and the actual changes
* review <code>LICENSE</code> to make sure that any newly incorporated packages are credited
* review <code>README.md</code>
* tag the release on GitHub
* create a new Vagrant image
  * upload it and publish the URL
* write a blog entry announcing it
* tweet about it
* inform every known State Decoded user
