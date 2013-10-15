---
title: Installation
layout: default
---

# Contents
{:.no_toc}

* Will be replaced with the ToC, excluding the "Contents" header
{:toc}

# Requirements

The following programs and modules are required to run The State Decoded, in addition to a basic LAMP/WAMP stack (MySQL 5+, PHP 5.2+, Apache 2+). Nearly all of them are almost certainly already installed on any standard server. The State Decoded’s installer will automatically check to see whether these are installed and running.

* [Make sure that Apache has `mod_rewrite` enabled](http://stackoverflow.com/questions/9021425/how-to-check-if-mod-rewrite-is-enabled-in-php) and that [`.htaccess` files can use `RewriteRule`](https://help.ubuntu.com/community/EnablingUseOfApacheHtaccessFiles).
* Make sure that Apache has `mod_env` enabled:
	* Red Hat/Fedora/CentOS/Windows: [instructions](http://serverfault.com/questions/56394/how-do-i-enable-apache-modules-from-the-command-line-in-redhat/56435#56435)
	* SuSE: [instructions](http://johannesluderschmidt.de/lang/en-us/django-invalid-command-setenv-in-opensuse/291/)
	* Debian/Ubuntu: `sudo a2enmod env`
* Make sure that [PHP's PDO extension](http://php.net/manual/en/book.pdo.php) has MySQL support included:
	* Red Hat/Fedora/CentOS: `sudo yum install php-mysql`
	* SuSE: `yast2 --install php5-mysql`
	* Debian/Ubuntu: `sudo apt-get install php5-mysql`
	* Windows: `add to php.ini: extension=php_pdo_mysql.dll`
* Make sure that you have `php-curl` installed:
	* Red Hat/Fedora/CentOS: `sudo yum install php-curl`
	* SuSE: `yast2 --install php5-curl`
	* Debian/Ubuntu: `sudo apt-get install php5-curl`
* Make sure that you have either `php-tidy` or `tidy` installed:
	* Red Hat/Fedora/CentOS: `sudo yum install php-tidy`
	* SuSE: `yast2 --install php5-tidy`
	* Debian/Ubuntu: `sudo apt-get install php5-tidy`
* Make sure that you have `zip` installed (you almost certainly do, except on Amazon EC2):
	* Red Hat/Fedora/CentOS `sudo yum install zip`
	* SuSE: `yast2 --install zip`
	* Debian/Ubuntu: `sudo apt-get install zip`
* Install Apache Solr 4.0 or newer:
	* At present, this rules out any package installations (via `yum`, `yast2`, `apt-get`, etc.), which are all version 1.X.
	* Simply [download Solr](http://lucene.apache.org/solr/downloads.html) to your server, and in the `examples/` directory, run `java -jar start.jar -Dsolr.solr.home=/var/www/example.com/solr_home/`, replacing `/var/www/example.com/solr_home/` with the actual path to the `solr_home` directory, which is provided as part of the State Decoded download.
	* For anything other than a test installation, you'll need to follow [Apache's guide to enabling Solr as a standard system service that will start at boot time](http://wiki.apache.org/solr/SolrJetty#Init_script_to_run_the_Solr_example).

# Basic configuration

Here is the process of configuring the beta version of The State Decoded. (Most of these steps will be automated for the v1.0 release.)

1. Upload the `htdocs`, `includes`, and `solr_home` directories to your web server (e.g., all within `/var/www/example.com/`), with `htdocs` serving as the web server's document root (e.g. `/var/www/example.com/htdocs/`).
1. Create a new MySQL database (e.g., `mysqladmin create statedecoded`) and make sure that the web server has access to it.
1. Rename `config-sample.inc.php` to `config.inc.php`
1. Rename `class.State-sample.inc.php` to `class.[Placename].inc.php` (e.g., `class.Kansas.inc.php`).
1. Go through `config.inc.php` and configure each setting. Additional details about each setting can be found within [the configuration file documentation](config.html).
1. Prepare the parser.
	* Straightforward route: With all laws in [the State Decoded XML format](xml-format.html), copy all XML files to `htdocs/admin/import-data/`.
	* Custom route: Modify `class.[Statename].inc.php`—specifically `Parser::iterate`, `Parser::parse`, and `Parser::store`—to support the legal code that you will be importing. See "[How the Parser Works](parser.html)" for details.
1. Load http://example.com/admin/ in your browser and follow the prompts in the "Import Data" section. Wait while the parser runs, which could require anywhere from 5-60 minutes to run, depending on the power of the server and the length of the legal code. This is iterating through the XML, loading it into the database, and creating the website. When the parser finished, you have a complete site for your legal code.


# Advanced configuration

## Typekit typefaces

The State Decoded has been designed to look best with three specific fonts: [FF Meta Serif Web Pro](https://typekit.com/fonts/ff-meta-serif-web-pro), [Adobe Jenson Pro](https://typekit.com/fonts/adobe-jenson-pro), and [Proxima Nova](https://typekit.com/fonts/proxima-nova). In order to use these fonts on the site, it's necessary to [pay for an account on Typekit](https://typekit.com/plans), Adobe's website font service. These three fonts require their "Portfolio" plan, which costs $4/month.

If you do not sign up for Typekit, the site will work perfectly well. It simply won't look as nice as it otherwise would.

## Custom functions

Within the `State` class, in `class.[Statename].inc.php`, there exist several methods, commented out, that can be written for your specific implementation, to provide additional functionality on your site. To implement these, it is only necessary to write the methods—all of the glue is already in place to include their output within the law object, return the data via the API, and display it on the law pages.

This class can also be used to house additional functionality not envisioned within The State Decoded.

### Official URL

Method `official_url()`. Turns a law's section number into the URL for the law on its official government website. For example, `10-2.301.4` might become `http://example.gov/laws/10-2.301.4/`. This URL will be provided on the sidebar of every law page and within the API's law method.

### Translate history

Method `translate_history()`. Turns a law's history text into a plain-English version of the same text. For example, `Code 1950, c. 118; 1972, c. 825` might become:

> This law was first codified in 1950, as recorded in chapter 118 of the Acts of Assembly, and was amended in 1972, by chapter 825.

The translated history will be displayed after each law on the law page, and will be provided within the API's law method.

### Citations

Method `citations()`. Turns a law's section number and history into one or more citation methods. For example, it might turn a law's section number (10-2.301.4) and the most recent year in which that law was amended (1997) into `Ne. Code § 10-2.301.4 (1997)`. All created citation methods will be displayed in the sidebar of the law page and within the API's law method.

## Definition parsing

Within `class.[Statename].inc.php`, the `extract_definitions()` method does its best to determine the *scope* of definitions. “Scope” is how broadly that a term is to be defined. Some terms are defined only for the purpose of a single law:

> As used in this section, unless the context requires a different meaning:
> 
> “Costs” means the reasonable and customary charges for goods and services incurred or to be incurred in major information technology projects.

Others might be defined for an entire structural unit—a chapter, title, part, or other named structural unit. And still others might be global, defined for the entire legal code. Every legal code has its own terminology, and many are inconsistent in how a definition's scope is described. "As used in this chapter," "for the purpose of this chapter," and "for purposes of this chapter" are all viable phrases indicating scope. These candidate phrases are stored in the `$scope_indicators` array. The scope of a list of definitions is gathered from the first paragraph in that list.

Then there are the phrases that indicate an actual definition:

* "mean"
* "means"
* "shall include"
* "includes"
* "has the same meaning as"
* "shall be construed"
* "shall also be construed to mean"

These are all terms that can connect a defined term to its definition. These are stored in the `$linking_phrases` array. If your legal code uses different terminology, you need only add its linking phrases to the list.

Finally, the terms themselves are located based on the presence of quotation marks. (Either straight quotation marks—`U+0022` in Unicode—or angled double quotation marks—`U+201C` and `U+201D` in Unicode.) If the terms within your legal code are not stored within double quotation marks, then `extract_definitions()` will need to be modified to be able to isolate those terms, such as locating them within `<em></em>` tags. For those rare legal codes that do not offset defined terms in any way (italics, quotation marks, or otherwise; e.g., Nebraska), modifications to the code will be necessary in order to isolate the term being defined.

The State Decode does its best to allow for the many terms and phrases employed by different legal codes, but this may need to be tuned to accommodate the legal code that you are parsing. Survey how it indicates the scope of definitions, and how it connects a term to its definition, and modify `extract_definitions()` to suit those circumstances.

## Varnish caching

If you are running a Varnish server, and you want The State Decoded to automatically purge expired content, provide the URL for the Varnish server as the value of `VARNISH_HOST` within `config.inc.php`. Make sure that your [Varnish VCL](https://www.varnish-cache.org/docs/2.1/tutorial/vcl.html) [supports a `BAN`](https://www.varnish-cache.org/docs/3.0/tutorial/purging.html#bans).

## Autolinking cross-references

Many legal codes use coherent, compact, unique citation methods, assigning every law an identifier that is never used elsewhere. With the regular expression in the config file (`SECTION_PCRE`), The State Decoded can identify all cross-references and turn these into links. But for those legal codes that do not use these identifiers, some custom autolinking code will need to be written.

The first step to doing this is to copy the `replace_sections()` method found within `includes/class.Autolinker.inc.php` and paste it into your `class.[Statename].inc.php` file, within a `State_Autolinker` class, like such:

~~~
class State_Autolinker extends Autolinker
{
  function replace_sections()
    {
        [paste method contents here]
    }
}
~~~

The function of the existing `replace_sections()` method is to process a single match from `SECTION_PCRE`. Normally this is as simple as `return '<a class="law" href="/'.$match.'/">'.$match.'</a>';`, because the section number is the same as the URL slug. But for something like, say, [the Maryland Code's citations](https://github.com/statedecoded/law-identifier/blob/master/Maryland.md), it's necessary to be able to generate a URL for "§ 9-301 of the State Government Article" (say, `sg-9-301`). That might mean maintaining a lookup table within `replace_sections()` to turn every article name ("State government Article") into a URL prefix ("sg-"). Methods will vary between legal codes.
