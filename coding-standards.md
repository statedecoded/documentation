* Generally, [PEAR standards](http://pear.php.net/manual/en/standards.php).
* Indentation: tabs, not spaces.
* Wrap at 100 columns, unless the result would be confusing.
* Variable names in lowercase (e.g., `$section_number`), multiple words separated with underscores, never camelCase or StudlyCaps.
* Braces on a new line:
```php
if ($x == $y)
{
  ...
}
```
* Don't use terse expressions (e.g., the ternary operator)
* The results of conditionals should always go within braces, rather than expressed on a single line, e.g.:
```php
if ($a == TRUE)
{
	echo 'true';
}
```
* Boolean terms in all caps (e.g., `TRUE`).
* Comments as complete sentences, offset like such:
```php
/*
 * Example comment here.
 */
```
* Comments preceded with `// ` indicate temporary, working comments, generally indicating problems.
* If a bit of code is anything short of perfectly obvious, provide a comment explaining it.
