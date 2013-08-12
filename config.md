---
title: Config file
layout: default
---

# BASE_PATH
The directory that contains the web root (e.g., `/htdocs/`) and the include files (e.g. `/includes/`).

# INCLUDE_PATH
The path to the files found in `/includes/`.

# WEB_ROOT
The path to the files found in `/htdocs/`.

# CUSTOM_FUNCTIONS
The file in the `/includes/` directory that contains functions custom to this installation.

# TEMPLATE
Which template to use.

# SITE_TITLE
What is the title of the website?

# PLACE_NAME
What is the name of the place that these laws govern?

# LAWS_NAME
What does this state call its laws?

# SECTION_SYMBOL
What is the prefix that indicates a section? In many states, this is `§`, but in others it might be `s `.

# EDITION_ID
Establish which version of the code that's in effect sitewide. This is the database ID in the `editions` table.

# EDITION_YEAR
Establish which version of the code that's in effect sitewide. This is the label for the present revision.

# INCLUDES_REPEALED
Does this state's code include laws that have been repealed formally, and that are marked as such? This will suppress their display, so that visits don't have to wade through nearly empty, repealed
laws.

# PDO_DSN
[PHP Data Object's Data Source Name](http://php.net/manual/en/ref.pdo-mysql.connection.php)—tells PHP how to connect to the database.

# PDO_USERNAME
The database username.

# PDO_PASSWORD
The database password.

# GLOBAL_DEFINITIONS
Specify the structural identifier ancestry for the unit of the code that contains definitions of terms that are used throughout the code, and thus should have a global scope. Separate each identifier with a comma. If all global definitions are found in Title 15A, Part BD, Chapter 16.2, that would be identified as '15A,BD,16.2'. If all global definitions are found in Article 36, Section 105, that would be identified as '36,105'. This must be the COMPLETE PATH to the container for global definitions, and not a standard citation.

# STRUCTURE
Create a list of the hiearchy of the code, from the top container to the name of an individual law. A legal code's top structural unit might be "titles," and each title might be divided into "chapters," each chapter might be divided into "parts," and each part might be divided into "sections." That would be rendered here as `title,chapter,part,section`.

# SECTION_PCRE
Define the regular expression that identifies section references. It is best to do so without using the legal code's symbol (e.g., `§`), since section references are frequently made without its presence. [A growing collection of per-state regular expressions is available](https://github.com/statedecoded/law-identifier)—even if the regular expression that you need isn't found there, you should be able to find something to repurpose.

# SECTION_PCRE_STRUCTURE
Map the above PCRE's stanzas to its corresponding hierarchical labels. It's OK to have duplicates. For example, if the PCRE is broken up like (title)(title)-(part)-(section)(section), then list "title,title,part,section,section".

# ERROR_PAGE_DB
The path, relative to the webroot, to an error page to be displayed if the database connection is not available. (Like the fail whale.) Do not begin this path with a slash. If this is undefined, a bare database connection error will be displayed.

# EMAIL_ADDRESS
When there is cause to send an e-mail (e.g., API registration), what "From" address should be used?

# EMAIL_NAME
When sending e-mail, what name should appear in the "From" field?

# RECORD_VIEWS
Record each view of each law in the laws_views table? Doing so provides a corpus of data that can be useful for analysis, data that will be drawn on in future releases of The State Decoded, but that at present is not used for anything. This is done via MySQL's `INSERT DELAYED`, so it will not slow down page rendering time, but it does require a certain amount of system resources and storage.

# API_KEY
The site uses its own API extensively, such as to display inline definitions and inline cross-references. That API is stored here, although you don't have to do it—this is populated automatically at the time that the parser is run.

# DISQUS_SHORTNAME
If you want to enable [Disqus](http://www.disqus.com/) commenting for every law, register for Disqus, create a new site, and enter the assigned Disqus shortname here.

# VARNISH_HOST
If you're running a Varnish server, and you want The State Decoded to automatically purge expired content, provide the URL (including the port number) here.

# GOOGLE_ANALYTICS_ID
If you want to track traffic stats with Google Analytics, provide your site's web property ID here.
