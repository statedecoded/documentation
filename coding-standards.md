---
title: Coding Standards
layout: default
---

* Generally, <a href="http://pear.php.net/manual/en/standards.php">PEAR standards</a>.
* Indentation: tabs, not spaces.
* Wrap at 100 columns, unless the result would be confusing.
* Variable names in lowercase (e.g., <code>$section_number</code>), multiple words separated with underscores, never camelCase or StudlyCaps.
* Generally, give everything space to breathe. Whitespace is free.
* Braces on a new line:~~~
if ($x == $y)
{
  ...
}
~~~
* Save for very terse (>~5 lines?) clauses, use an extra carriage return after an opening brace and before a closing brace: ~~~
while ($reference = $statement->fetch(PDO::FETCH_OBJ))
{

	$reference->catch_line = stripslashes($reference->catch_line);
	$reference->url = 'http://' . $_SERVER['SERVER_NAME']
		. ( ($_SERVER['SERVER_PORT'] == 80) ? '' : ':' . $_SERVER['SERVER_PORT'] )
		. '/' . $reference->section_number . '/';
	$references->$i = $reference;
	$i++;
	
}
~~~

* Don't use the ternary operator (`($size > 2) ? 'small' : 'large'`).
* The results of conditionals should always go within braces, rather than expressed on a single line, e.g.:~~~
if ($a == TRUE)
{
  echo 'true';
}
~~~
* Spaces before conditionals (`if ($a == TRUE)`, not `if($a == TRUE)`)
* Boolean terms in all caps (e.g., `TRUE`).
* Comments as complete sentences, offset like such:~~~
/*
 * Example comment here.
 */
~~~
* Comments preceded with `//` indicate temporary, working comments, generally indicating problems.
* If a bit of code is anything short of perfectly obvious, provide a comment explaining it.
* Use `echo`, not `print`.
* Use HTML 5, meaning self-terminating tags (e.g., `&lt;br /&gt;`.
