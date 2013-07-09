Duplicate `includes/templates/default.inc.php` (to, say, `/includes/templates/kansas.inc.php`), then edit `TEMPLATE` in `includes/config.inc.php` to read `kansas` instead of `default`. `kansas.inc.php` is now your site's page template.

Note that if you're running Varnish, APC, or mod_pagespeed on your server, you'll need to clear the cache, both to remove cached pages with the old design and to remove the cached config options from `config.inc.php`.
