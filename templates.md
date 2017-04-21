---
title: Templates
layout: default
---

# Contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Master template file
Duplicate `htdocs/themes/StateDecoded2013/` (to, say, `htdocs/themes/Kansas/`), then edit `THEME_NAME` in `includes/config.inc.php` to read `Kansas` instead of `StateDecoded2013`. The contents of the new `Kansas` directory are now your site template.

# Images
There is a [State Decoded Assets repository](https://github.com/statedecoded/statedecoded-assets/) that contains PSD files of page mockups and a favicon. These may be modified to customize the site's images as you see fit.

# CSS
If you choose to duplicate the default “State Decoded 2013“ theme, then you will be happiest if you do not edit the existing CSS files directly. Instead, create a new CSS file (e.g., `htdocs/themes/Kansas/static/css/kansas.css`) and start it with the following line:

~~~
@import url("application.css");
~~~

Then add your CSS declarations on top of that. This way, as subsequent releases of The State Decoded add modify the bundled CSS to add new functionality, you will gain that functionality in your own design by simply copying over the new `application.css`. Then, in your template (e.g., `htdocs/themes/Kansas/default.inc.php`), have it load `kansas.css` instead of `application.css`.

Better still, if you're comfortable with SCSS, do the same, but by creating a new SCSS file (e.g., `htdocs/themes/Kansas/static/scss/kansas.scss`) that leads off by importing `application.scss`, and then use that to generate `htdocs/themes/Kansas/static/css/application.css`.

# Caching
If you're running Varnish or mod_pagespeed on your server, you'll need to clear out your cache after making design changes. That's important for Varnish, which caches whole pages. mod_pagespeed should automatically expire its cached assets (Javascript, CSS, and images) when you change them, but if it doesn't, you'll need [flush its cache manually](https://developers.google.com/speed/pagespeed/module/system#flush_cache).

# Home Page Layout
The home page is found at `home.php`, not `index.php`. (`index.php` is the file through which all page requests on the site are piped.)

If you have an introductory video that you want to display on the home page, uncomment the `<div class="nest video">[…]</div>` stanza from `htdocs/home.php` and modify appropriately to link to your video.

# Favicons, Apple Touch Icons

Each of these icons should somehow correlate to the political entity's legal code. With Virginia, we used the seal icon that was created for the site. The larger touch icons have bitmapped 0 and 1s fading across, representing digital bits being "decoded" as it were. Feel free to edit the master images in [the State Decoded Assets repository](https://github.com/statedecoded/statedecoded-assets) to suit your needs. We recommend running any images through [ImageOptim](http://imageoptim.com) to squeeze out excess file size.

# Sass, Compass, etc.

This site was built with future flexibility and reuse in mind. To that end, the site basically is a style guide as well as a design. We use Sass/SCSS to allow the power of variables in CSS. This makes it easy to rapidly change colors and fonts without having to do massive find-and-replace or worry about color math variations in Photoshop.

This does create a layer of complexity, but a worthwhile one. You, as an end user, can write regular CSS in an SCSS file and it will compile normally.

To compile the SCSS into CSS, you need to run CodeKit, Scout, CompassApp, or do a manual setup with Ruby on your system. This is documented on [the Compass site](http://compass-style.org/install/).

# Media Queries

The site is defined with three breakpoints:

* Handheld/iPhone
* Tablet/iPad (Portrait)
* Everything else

# Creating New Pages

You may add new pages to the site by simply including `HTML` or `PHP` files inside of the `htdocs` directory of the site.  You can then reference those files by name.  Similarly, any images or other assets in that directory can be used on the site.

If you would like to use the template engine of The State Decoded instead of having to include an entire page, simply create a new PHP file with at least the following contents, replacing `$body` with your content.

```
/*
 * Create a container for our content.
 */
$content = new Content();

$body = 'Hello World';

/*
 * Put the shorthand $body variable into its proper place.
 */
$content->set('body', $body);

/*
 * Parse the template, which is a shortcut for a few steps that culminate in sending the content
 * to the browser.
 */
$template = Template::create();
$template->parse($content);
```

There are other fields available on a page, including the page title and heading. Please have a look at the other pages on the site and the theme's `default.inc.php` template file for specific fields that are available to change via the `$content->set` method.

To have the page appear without the file extension, as the About page and others do, you'll need to add it to the routing system.  The routing system uses simple [regular expressions](http://www.regular-expressions.info/) to match routes.  Simply add a line to the `Specific Routes` section of the `includes/routes.inc.php` file with your page's name, route structure, and filename like this:

```
$router->addRoute('contact', '^/contact/$', 'contact.php');
```


# Editing Help Content

To customize the help content of any section, you'll need to edit the `htdocs/content/help.json` file.  There, you'll need to give the help content a title, a brief description, and the full content that should appear in the modal popup.
