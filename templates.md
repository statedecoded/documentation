---
title: Templates
layout: default
---

# Master template file
Duplicate `includes/templates/default.inc.php` (to, say, `/includes/templates/kansas.inc.php`), then edit `TEMPLATE` in `includes/config.inc.php` to read `kansas` instead of `default`. `kansas.inc.php` is now your site's page template.

# Images
There is a [State Decoded Assets repository](https://github.com/statedecoded/statedecoded-assets/) that contains PSD files of page mockups and a favicon. These may be modified to customize the site's images as you see fit.

# CSS
You will be happiest if you do not CSS files directly. Instead, create a new CSS file (e.g., `/static/css/kansas.css`) and start it with the following line:

```html
@import url("application.css");
```

Then add your CSS declarations on top of that. This way, as subsequent releases of The State Decoded add modify the bundled CSS to add new functionality, you will gain that functionality in your own design. Then, in your template (e.g., `includes/templates/kansas.inc.php`), have it load `/static/css/kansas.css` instead of `/static/css.application.css`.

Better still, if you're comfortable with SCSS, do the same, but by creating a new SCSS file (e.g., `/static/scss/kansas.scss`) that leads off by importing `application.scss`, and then use that to generate `/static/css/application.css`.

# Caching
Note that if you're running Varnish, APC, or mod_pagespeed on your server, you'll need to clear the cache, both to remove cached pages with the old design and to remove the cached config options from `config.inc.php`.
