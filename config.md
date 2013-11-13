---
title: Config file
layout: default
---
# Configuration Options

## SITE_TITLE
What is the title of the website?

## PLACE_NAME
What is the name of the place that these laws govern?

## LAWS_NAME
What does this place call its laws? e.g., `The Kansas Statutes`, `The Code of Virginia`, etc.

## SECTION_SYMBOL
What is the prefix that indicates a section? In many states, this is `§`, but in others it might be `s `. This would be formatted as `§ 1.23-45` or `s. 123`.

## WEB_ROOT
The path to the contents of the website, e.g. the location of `index.php`.

## IMPORT_DATA_DIR
The directory in which the importer can find the laws to be imported.

## CUSTOM_FUNCTIONS
The file in the `/includes/` directory that contains functions custom to this installation. By convention, this is named for the place that these laws govern, e.g. `class.Kansas.inc.php`.

## TEMPLATE_DIR
The directory in which templates are found.

## THEME_NAME
The name of the theme that's in use.

## THEME_DIR
The filesystem path to the directory that contains the theme. This defaults to `TEMPATE_DIR/THEME_NAME` (e.g., `/var/www/example.com/themes/StateDecoded2013/`.)

## THEME_WEB_PATH
The public URL of the directory that contains the theme (e.g., `http://example.com/themes/StateDecoded2013/`.)

## CURRENT_API_VERSION
Define the default version of the API to send requests to, if a version isn't othewise specified.

## EDITION_ID
Establish which version of the code that's in effect sitewide. This is the database ID in the `editions` table. (On a new installation, this will always be `1`.)

## EDITION_YEAR
Establish which version of the code that's in effect sitewide. This is the label for the present revision. Although the word "year" appears in the title, if setting up a site for a code that is updated more frequently than annually, another terse label will suffice here (e.g., `2015-02-01` or `2014-Q1`). This label will appear in URLs.

## INCLUDES_REPEALED
Does this state's code include laws that have been repealed formally, and that are marked as such? This will suppress their display, so that visits don't have to wade through empty, repealed laws. If your legal code isn't full of vast wastelands of long-abandoned chapters, then you can leave this set to `FALSE`.

## LONG_LAW_URLS
Should we use short URLs or long URLs for laws? Short URLs are the default (e.g., `http://example.com/12.3-45:67/`), but if laws have non-unique identifiers, then you'll need to use long URLs (e.g. `http://example.com/56/21/12.3-45:67/`), which are URLs that incorporate the hierarchical structures (e.g., laws, titles, parts, etc.) that contain each law.

## PDO_DSN
[PHP Data Object's Data Source Name](http://php.net/manual/en/ref.pdo-mysql.connection.php)—tells PHP how to connect to the MySQL database. (Only MySQL will work.) This looks like this:

~~~
mysql:dbname=statedecoded;host=localhost;charset=utf8
~~~

`dbname` is the name of the database that you created for The State Decoded. `host` is the name of the database server. This will most often be `localhost`—check with your host for details, if that doesn't work. And `charset` must always be `utf8`.

## PDO_USERNAME
The database username.

## PDO_PASSWORD
The database password.

## GLOBAL_DEFINITIONS
Specify the structural identifier ancestry for the unit of the code that contains definitions of terms that are used throughout the code, and thus should have a global scope. Separate each identifier with a comma. If all global definitions are found in Title 15A, Part BD, Chapter 16.2, that would be identified as `15A,BD,16.2`. If all global definitions are found in Article 36, Section 105, that would be identified as `36,105`. This must be the *complete path* to the container for global definitions, and not a standard citation.

Not all legal codes need this. This only needs to be specified for those legal codes that list globally applicable definitions but that don't specify within the list of definitions that they are globally applicable. For instance, a legal code might set aside a chapter to list all global definitions, and use the first law in the chapter to say "all following laws apply globally," and then have 100 more laws, each containing a single definition. This is a legal code for which this configuration option is necessary.

## STRUCTURE
Create a list of the hiearchy of the code, from the top container to the name of an individual law. A legal code's top structural unit might be "titles," and each title might be divided into "chapters," each chapter might be divided into "parts," and each part might be divided into "sections." That would be rendered here as `title,chapter,part,section`.

## SECTION_REGEX
Define the regular expression that identifies section references. It is best to do so without using the legal code's symbol (e.g., `§`), since section references are frequently made without its presence. [A growing collection of per-state regular expressions is available](https://github.com/statedecoded/law-identifier)—even if the regular expression that you need isn't found there, you should be able to find something to repurpose.

## ERROR_PAGE_DB
The path, relative to the webroot, to an error page to be displayed if the database connection is not available. (Like the fail whale.) Do not begin this path with a slash. If this is undefined, a bare database connection error will be displayed.

## EMAIL_ADDRESS
When there is cause for the website to send an e-mail (e.g., API registration), what "From" address should be used?

## EMAIL_NAME
When sending e-mail, what name should appear in the "From" field?

## RECORD_VIEWS
Record each view of each law in the laws_views table? Doing so provides a corpus of data that can be useful for analysis, data that will be drawn on in future releases of The State Decoded, but that at present is not used for anything. This is done via MySQL's `INSERT DELAYED`, so it will not slow down page rendering time, but it does require a certain amount of system resources and storage.

## API_KEY
The site uses its own API extensively, such as to display inline definitions and inline cross-references. That API is stored here, although you don't have to do it—this is populated automatically at the time that the parser is run.

## DISQUS_SHORTNAME
If you want to enable [Disqus](http://www.disqus.com/) commenting for every law, register for Disqus, create a new site, and enter the assigned Disqus shortname here.

## VARNISH_HOST
If you're running a Varnish server, and you want The State Decoded to automatically purge expired content, provide the URL (including the port number) here.

## GOOGLE_ANALYTICS_ID
If you want to track traffic stats with Google Analytics, provide your site's web property ID here.

## TYPEKIT_ID
The State Decoded can optionally render the site using several very nice, commercial fonts, calling them from Adobe's [Typekit](https://typekit.com/) service. This requires [paid registration at the "Portfolio" level](https://typekit.com/plans), which costs $4/month. The provided Typekit site ID goes here. See [the "Typekit typefaces" section of the installation guide](http://statedecoded.github.io/documentation/installation.html#typekit-typefaces) for details about where to find that ID.

## COURTLISTENER_USERNAME / COURTLISTENER_PASSWORD
 * If you want to display court decisions that affect each law using [CourtListener's REST API](https://www.courtlistener.com/api/rest-info/), you must register for an account and enter your username and password here. See the `get_court_decisions()` method in `class.State-sample.inc.php` to actually enable the display of these decisions.

# Caching
If you are running APC on your web server, note that all of these configuration constants are cached aggressively. You will need to clear APC's cache after making any changes to them, or else those changes will not take effect for hours or days. This can be done in your site’s admin section. (Note that the option is only displayed if you are running APC.)