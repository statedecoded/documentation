---
title: How the Parser Works
layout: default
---

# Overview

`include.State-sample.inc.php` (which, if you have already installed The State Decoded, you’ve renamed to something like `include.Kansas.inc.php`) contains several hollowed-out methods and several populated methods. These are used to gather up the available raw materials of the laws in question (whether thousands of XML files, hundreds of web pages, or a single SGML file) and load them into The State Decoded's database in the format expected by the program.

Essentially, it works like this, as invoked within `parser-controller.inc.php`:

```
while ($section = $parser->iterate())
{
	$parser->section = $section;
	$parser->parse();
	$parser->store();
}
```

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

When fed a section that contains a list of definitions, this atomizes them into a list of definitions, and then extracts the term being defined from each definition. Because every state stores these definitions differently (and each state can store them in different ways within the same legal code), this requires customization.

## extract_references()

This method should not require any customization. Its job is to scan each section for mentions of other sections to create a cross reference list.

## extract_history()

Turns the often-cryptic history sections within nearly every law into standardized data. For instance:

```
1995, c. 837, § 18.2-340.32; 1997, cc. 777, 838
```

This tell us that in the 1995 Acts of Assembly, the instant law was modified (or perhaps created), as recorded in chapter 837, and the law was then known as § 18.2-340.32, though it as since been renumbered. And then in the 1997 Acts of Assembly, it was modified twice, in chapters 777 and 838. The job of `extract_history()` is to turn this data into an object, rather than storing it as a single string of text. Inevitably, it will require extensive modification to work with each state's unique method of storing history data, although this is both technically and logistically straightforward.

## create_structure

The only customization that may be helpful here is to populate the `order_by` field in the `structure` table. Many legal codes are ordered in such a way that no standard SQL-based ordering can display them properly (e.g., 4.8, 4.9, 4.10, 4.11, rather than the mathematically proper 4.10, 4.11, 4.8, 4.9, or 4.8, 4.8:A, 4.8:B, 4.8:C). For such codes it may be helpful to populate `order_by` within the SQL query in this method, by whatever ordering mechanisms are available.
