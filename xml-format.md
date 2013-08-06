The State Decoded parser can either be modified to use bespoke PHP to parse a legal code in its native format, or it can parse a prescribed XML format. This is a totally invented XML format. There is no legal code that is provided in this format. It is provided as an option because, via [XSLT](http://en.wikipedia.org/wiki/XSLT), it can be substantially easier to convert an existing XML to this format than to write a parser for it in PHP.

Note that this is *not* an effort to create a standard interchange or storage format for legal codes. This exists solely as a convenience for important data into The State Decoded.

# Descriptive Specifications

* There must be one file per law.
* All files must be in the same directory.
* The names of the files are irrelevant.
* Every file contains both structural data about its container—which chapter/title/part/article/etc. that it's within—and data about the law itself.
* Text must be broken up by subsection, rather than provided as a single block of text, unless the text of the law does not have subsections.
* Subsections may be nested.

This being open source software, some relatively simple tweaks to `Parser::iterate` and `Parser::parse` make all of these specifications malleable.

# Fields
* law: the container for all other fields—required
  * structure: the container for structural units (chapters, titles, articles, parts, etc.)—required
    * unit: a single structural unit that contains this law—required
      * label: the type of structural unit that this is (chapter, title, article, part, etc.)—required
      * identifier: the unique identifier for this structural unit, almost always a number (e.g., 18.2, 6)—required
      * order_by: when listing all structural units of this label, in what ordinal position this structural unit should appear—optional
      * level: a whole number, starting at one, indicating the depth of this structural unit—required
  * section_number: the unique identifier for this law—required
  * catch_line: the title of this law—required
  * order_by: when listing all laws within this structural unit, in what ordinal position this law should appear—optional
  * text: the container for the text of the law itself—required
    * section: a single labelled section of a law—optional
      * prefix: the identifying label for this section (e.g., A, 6, vi)—required
      * type: the type of material within this section; default is text, other options are "table" and "image"—optional
  * history: the textual description of the history of amendments to this law—optional
  * metadata: a container for any additional fields, which will be stored as key:value in the `laws_meta` table—optional; values of `true` and `false` will be stored in the database as `y` and `n`, respectively, but converted back to `true` and `false` within the internal and external APIs

# Sample XML

## Structure

### With All Fields

### Bare Minimum

## Populated
Here is a complete, populated sample XML file that includes every available parameter.

