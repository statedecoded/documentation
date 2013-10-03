---
title: Search
layout: default
---

*Note: This page is actively under development. At present, it's more of a collection of thoughts than actual documentation.*

##solr_home directory

The `/solr_home/` directory can be kept anywhere. There's no functional need for it to be available within the scope of the website. By default, it lives in parallel to `/htdocs/` and `/includes/`. Its location is only relevant when starting Solr, which is when the path is provided, e.g. `-Dsolr.solr.home=/var/www/example.com/solr_home/`.

## Tag weighting

In `/solr_home/statedecoded/conf/solrconfig.xml`, the request handler for standard searches (`<requestHandler name="/search" class="solr.SearchHandler">`) is configured to give tags the same weight as all other indexed fields. Depending on the source of your tags, you may be well served by adjusting this number.

For instance, if your tags are well written, accurate, and not over-broad, then you could increase its weight from the (implied) default of 1.0, and take it up to 2.0, 5.0, even 10.0, for Solr to value tags, in determining the relevance of a given law, that multiple of times above all other fields.

On the other hand, if your tags are good-not-great, then you'd want to decrease the weight of tags, down to .5 or .2 or even .1, to have Solr value other fields beyond tags, but still include tags in its index.

This is done by appending `^5` (to weight tags at 500% that of other fields) or `^.1` (to weight tags at just 10% of other fields) after `tags` within the `<str name="…">` tag, e.g.:

~~~
<str name="qf">
				 catch_line
					   tags^5
					   text
				  structure
		   section_ancestor^10
		 section_descendent^10
	   refers_to_descendent
		 refers_to_ancestor
  referred_to_by_descendent
	referred_to_by_ancestor
					   term
				 definition
</str>
~~~