---
title: Config file
layout: default
---

# BASE_PATH


# INCLUDE_PATH


# WEB_ROOT


# CUSTOM_FUNCTIONS
The file in the /includes/ directory that contains functions custom to this installation.

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
Does this state's code include laws that have been repealed formally, and that are marked as such?

# PDO_DSN
[PHP Data Object's Data Source Name](http://php.net/manual/en/ref.pdo-mysql.connection.php)—tells PHP how to connect to the database.

# PDO_USERNAME
The database username.

# PDO_PASSWORD
The database password.

# GLOBAL_DEFINITIONS
Specify the structural identifier ancestry for the unit of the code that contains definitions of terms that are used throughout the code, and thus should have a global scope. Separate each identifier with a comma. If all global definitions are found in Title 15A, Part BD, Chapter 16.2, that would be identified as '15A,BD,16.2'. If all global definitions are found in Article 36, Section 105, that would be identified as '36,105'. This must be the COMPLETE PATH to the container for global definitions, and not a standard citation.

# STRUCTURE
Create a list of the hiearchy of the code, from the top container to the name of an individual law.

# SECTION_PCRE
Define the regular expression that identifies section references. It is best to do so without using a section symbol (e.g., §), since section references are frequently made without its presence. [A growing collection of per-state regular expressions is available](https://github.com/statedecoded/law-identifier).

# SECTION_PCRE_STRUCTURE
Map the above PCRE's stanzas to its corresponding hierarchical labels. It's OK to have duplicates. For example, if the PCRE is broken up like (title)(title)-(part)-(section)(section), then list "title,title,part,section,section".

# ERROR_PAGE_DB
The path, relative to the webroot, to an error page to be displayed if the database connection is not available. Do not begin this path with a slash. If this is undefined, a bare database connection error will be displayed.

# EMAIL_ADDRESS


# EMAIL_NAME


# RECORD_VIEWS


# API_KEY


# DISQUS_SHORTNAME


# VARNISH_HOST


# GOOGLE_ANALYTICS_ID

