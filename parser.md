---
title: How the Parser Works
layout: default
---

# Contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Overview

`class.State-sample.inc.php` (which, if you have already installed The State Decoded, you’ve renamed to something like `class.Kansas.inc.php`) contains several hollowed-out methods and several populated methods. These are used to gather up the available raw materials of the laws in question (whether thousands of XML files, hundreds of web pages, or a single SGML file) and load them into The State Decoded's database in the format expected by the program.

Essentially, it works like this, as invoked within `class.ParserController.inc.php`:

~~~
while ($section = $parser->iterate())
{
	$parser->section = $section;
	$parser->parse();
	$parser->store();
}
~~~

`iterate()` retrieves each law, one by one, from the source material, and hands each one of them off to `parse()`. The job of `parse()` is to turn that raw material into a normalized PHP object, which is then passed to `store()`, which puts that data into the database.

# By Method

## iterate()

The first method to be called, its job is to step through the source of the entirety of the state's laws and extract each law, one at a time. It might be retrieving raw HTML off a web server, stepping through a single XML file on disk, or collecting JSON via an API. Its sole job is to encapsulate the raw material for *just one law* to pass along to `parse()`. This loops over and over again, returning one law with each loop, until finally the entire legal code has been iterated over.

## parse()

The second method to be called, this is where the heavy lifting is done. Its job is to distill each component of a law from the raw material provided by iterate and load it into an object. This might be easy (such as if the data source is well-structured XML or JSON), or might be tricky (such as if the source is HTML or poorly structured XML or JSON). At a minimum this is:

* name: the "catch line"—the title of the law (e.g. "Proceeds exempt from local taxation")
* text: the complete text of the law
* section_number: the alphanumeric identifier for the law (e.g. "15.2-912.2")
* structure->identifier: the alphanumeric identifier for the law's container—such as chapter or title number (e.g. "16")
* structure->name: the title of the structural container (e.g. "Industrial Development Corporations")

Optionally, this can be included:

* history: the legislative history of the law's creation and amendments (e.g. "1995, c. 837, § 18.2-340.32; 1997, cc. 777, 838")

## store()

This method should require minimal customization. The data should have been well massaged within parse(). The one clear exception is the stanza that identifies whether the current law is a list of definitions, generally based on its catch line (e.g., "Definitions," or "Meaning of certain terms"). This list will need to include all catch lines that can indicates that the instant law is a glossary. (See extract_definitions() for more.)

## extract_definitions()

When fed a section that contains a list of definitions, this atomizes them into a list of definitions, and then extracts the term being defined from each definition. Because every state stores these definitions differently (and each state can store them in different ways within the same legal code), this is likely to require some very simple customization.

The two customizations to be made are to "scope indicators" and to "linking phrases."

#### Scope indicators

The first set of phrases to be customized are the scope indicators, stored as `$scope_indicators`. These are the phrases that indicate that a list of definitions is about to follow, and tell the reader what the scope of those definitions is. That is, do these terms apply only within this law? Or they apply to the whole chapter, part, title, article, etc? In context, these phrases might look like this:

> The following terms are to be defined as such when used in this chapter:

Or this: 

> In this section, these terms shall be defined as follows:

“Scope indicators” are the phrases that appear immediately prior to the name of the structural unit (e.g., "title," "chapter," etc.) to which the definitions apply. By default, scope indicators look like this:

~~~
$scope_indicators = array(	' are used in this ',
							' when used in this ',
							' for purposes of this ',
							' for the purpose of this ',
							' in this ',
						);
~~~

Your legal code is unlikely to use this exact set of scope indicators. You'll need to read through a bunch of laws to get a handle on the candidate phrases, and list those here. Again, you must use the phrase that appears *immediately* prior to the name of the structural unit. If this does not fit how your legal code describes scope, then you'll need to make some modifications to `extract_definitions()`. Specifically this bit:

~~~
/*
 * Now figure out the specified scope by examining the text that appears
 * immediately after the scope indicator. Pull out as many characters as the
 * length of the longest structural label.
 */
$phrase = substr( $paragraph, ($pos + strlen($scope_indicator)), $longest_label );
~~~

You'll just need to grab a different chunk of text surrounding `$scope_indicator`.

If you get this wrong, it's not disasterous. If you include a scope indicator erroneously, almost certainly the only effect will be that the parser does a bit more work that it has to. If you fail to include a scope indicator, the definitions will still be detected, extracted, and stored, but their scope will be limited to the law in which they appear, as that is the most cautious default.


#### Linking phrases

The second and final set of phrases to be customized are the linking phrases, which is the word, words, or characters that connect a term to its definitions. For example:

> “Person” shall include individuals, a trust, an estate, a partnership, an association, an order, a corporation, or any other legal or commercial entity.

Or:

> “Decree”: Shall be used interchangeably with “judgment,” and shall include orders or awards.

In the former example, "shall include" is the linking phrase. In the latter, ": " is the linking phrase. (Admittedly, ": " is not a phrase, but it's also not a common construct within legal codes.)

We use these linking phrases to separate a term from its definition. They're stored in an array, named `$linking_phrases`, which looks like this by default:

~~~
$linking_phrases = array(	' mean ',
							' means ',
							' shall include ',
							' includes ',
							' has the same meaning as ',
							' shall be construed ',
							' shall also be construed to mean ',
						);
~~~

As with scope indicators, it is necessary to survey your legal code, to see how it links terms to their definitions, in order to populate your list. Odds aren't bad that it'll look rather like the default list.

If your linking phrases list is overly broad, the only effect will be to imperceptably slow down The State Decoded's parser. If it's overly narrow, the effect will be to omit affected terms and definitions from your dictionary—The State Decoded will simply have no idea that it's looking at a definition.

### Quotation marks

There is a third attribute of which to be aware, although it's not a trivial customization like scope indicators and linking phrases. The State Decoded assumes that defined terms are contained within quotes, like this:

> “Decree”: Shall include orders or awards.

Those can be straight quotes or angled quotes (aka "smart quotes"), but the terms must be within quotes. We use quotation marks for two reasons. The first is to determine whether to examine a paragraph to see if it contains a definition. No quotes, no definition. The second is to determine which words are the term being defined.

If your legal code does not insert defined terms within quotes, don't panic. While there is no friendly configuration option to address this, it's not a tough customization.

The first change you'd need to make is to the conditional that checks whether a quotation mark is present within a paragraph:

~~~
/*
 * All defined terms are surrounded by quotation marks, so let's use that as a criteria
 * to round down our candidate paragraphs.
 */
if (strpos($paragraph, $quote_sample) !== FALSE)
{
~~~

Best-case, there will be some other wrapper around each definition (e.g., `<b>Decree</b>: Shall include orders or awards.`), in which case you might turn the conditional into this:

~~~
if (strpos($paragraph, '<b>') !== FALSE)
~~~

Worst-case, you'd have this examine every single paragraph (e.g., `if (1 == 1)`, or just remove the `if(…){…}` wrapper entirely.)

Then, throughout the contents of `if (strpos($paragraph, $quote_sample) !== FALSE) { … }`, you'd need to replace references to quotation marks to the applicable characters, including in the regular expression that extracts the defined term:

~~~
preg_match_all('/("|“)([A-Za-z]{1})([A-Za-z,\'\s-]*)([A-Za-z]{1})("|”)/', $paragraph, $terms);
~~~

Specifically, it's the two instances of `("|”)` that would need to be replaced with the new character or characters used to enclose the defined term.

If there are no characters used to enclose the defined term, this is still very fixable, although a bit outside of the scope of this guide.


## extract_references()

This method should not require any customization. Its job is to scan each section for mentions of other sections to create a cross reference list.

## extract_history()

Turns the often-cryptic history sections within nearly every law into standardized data. For instance:

~~~
1995, c. 837, § 18.2-340.32; 1997, cc. 777, 838
~~~

This tell us that in the 1995 Acts of Assembly, the instant law was modified (or perhaps created), as recorded in chapter 837, and the law was then known as § 18.2-340.32, though it as since been renumbered. And then in the 1997 Acts of Assembly, it was modified twice, in chapters 777 and 838. The job of `extract_history()` is to turn this data into an object, rather than storing it as a single string of text. Inevitably, it will require extensive modification to work with each state's unique method of storing history data, although this is both technically and logistically straightforward.

## create_structure

The only customization that may be helpful here is to populate the `order_by` field in the `structure` table. Many legal codes are ordered in such a way that no standard SQL-based ordering can display them properly (e.g., 4.8, 4.9, 4.10, 4.11, rather than the mathematically proper 4.10, 4.11, 4.8, 4.9, or 4.8, 4.8:A, 4.8:B, 4.8:C). For such codes it may be helpful to populate `order_by` within the SQL query in this method, by whatever ordering mechanisms are available.

# Working with Metadata

You may have additional properties, data, and fields in your legal code that The State Decoded does not know about out-of-the-box.  To include these, you'll need to store them in the law's `metadata`.  For example, for the Virginia code, we include court cases via The Court Listener which reference a given law.

This data is stored in the `laws_meta` database table.  However, instead of directly adding records to this table, you can use an existing instance of the Law class, add your metadata to the object, and call `store_metadata()` to save it.  Here's an example, from the `class.State-sample.inc.php` code:

```
// Store these decisions in the metadata table.
$law = new Law();
$law->section_id = $this->section_id;

$law->metadata->{0} = new stdClass();
$law->metadata->{0}->key = 'court_decisions';
$law->metadata->{0}->value = json_encode($this->decisions);

$law->store_metadata();
```

You can create metadata at any point after the law has been created in the parser (the call to the `Parser->store()`), but in most cases you'll want to store it before the laws are exported (`ParserController->export()`), so that the metadata can appear in the exported data files.

To show this data on a law's page, you'lll need to edit the `htdocs\law.php` file to format the data appropriately.  Again, refer to the `court_decisions` example in that file for reference – in most cases, your data will be available on the law as `$law->metadata->your_field`.

You may also store metadata on a structure, but you will need to manage this manually.  The metadata for structures is stored in the `structure.metadata` field as an object that has been run through the `serialize()` function.  Be careful to load the structure's previous metadata when adding your new metadata, so as not to overwrite previously-created data.
