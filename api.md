# Table of Contents

* [How to...](#how-to)
* [Methods](#methods)
  * [Law](#method-law)
  * [Structure](#method-structure)
  * [Dictionary](#method-dictionary)
* [Errors](#errors)

# <a id="how-to"></a> How to...

## ...get an API key.

Register for a key on the site in question (e.g., [Virginia](http://vacode.org/api-key/)).

## ...browse the Code starting at the root.

`http://vacode.org/api/structure/?key=[api_key]`

## ...get information about a given law.

`http://vacode.org/api/law/[section_number]?key=[api_key]`

## ...get a information about a given title/chapter/part/etc.

`http://vacode.org/api/structure/[identifier]?key=[api_key]`

## ...get all definitions for a word or phrase.

`http://vacode.org/api/dictionary/[term]?key=[api_key]`

## ...get the definition for a word or phrase as it's used in a specific law.

`http://vacode.org/api/dictionary/[term]?section=[section_number]&key=[api_key]`

## ...restrict the fields that are returned.

Append `&fields=field1,field2,field3` to the query, substituting the desired field names for "field1," etc.

# <a id="methods"></a> Methods

## <a id="method-law"></a> Law
When provided with a section number identifying an individual law, returns everything that The State Decoded knows about that law.

### Query format
```
http://vacode.org/api/law/[section_number]?key=[api_key]
```

### Optional parameters
#### Callback
Pass the parameter `callback` and a callback string to have results returned as JSONP.

```
http://vacode.org/api/law/[section_number]?key=[api_key]&callback=d9f3lIb013
```

#### Specified fields
Pass the parameter `fields` and a comma-separated listing of fields to limit the response to those fields.

```
http://vacode.org/api/law/[section_number]?key=[api_key]&fields=catch_line,url_official_url,citation
```

### Fields
<table>
<thead>
<tr>
<th>Name</th>
<th>Type</th>
<th>Description</th>
<th>Notes</th>
</tr>
</thead>
<tbody>

<tr>
<td>section_number</td>
<td>string</td>
<td>The unique identifier for this section.</td>
<td></td>
</tr>

<tr>
<td>section_id</td>
<td>integer</td>
<td>Internal unique identifier for this law.</td>
<td></td>
</tr>

<tr>
<td>structure_id</td>
<td>integer</td>
<td>Internal unique identifier for the law's containing structural unit.</td>
<td></td>
</tr>

<tr>
<td>catch_line</td>
<td>string <em>or null</em></td>
<td>The title of this section.</td>
<td>Not all legal codes have catch lines. California's laws, for instance, lack them.</td>
</tr>

<tr>
<td>history</td>
<td>string <em>or null</em></td>
<td>The acts of the legislature that created this law.</td>
<td>Only some legal codes include histories.</td>
</tr>

<tr>
<td>full_text</td>
<td>string</td>
<td>The complete text of the law as a single string of text.</td>
<td>This duplicates the collective contents of `text`, but concatenates all subsections into a single string.</td>
</tr>

<tr>
<td>repealed</td>
<td>true/false</td>
<td>Indicates whether this law is still in force.</td>
<td>Only some legal codes continue to publish laws that have been repealed.</td>
</tr>

<tr>
<td>text</td>
<td>array</td>
<td>Contains the subsections of text that comprise this law.</td>
<td>There is one child element for each subsection. Some codes may be broken up not by labelled subsection, but by paragraphs that lack any labels or unique identifiers.</td>
</tr>

<tr>
<td colspan="4">

<table>
<tr>
<td>text</td>
<td>string</td>
<td>The text of the subsection.</td>
<td></td>
</tr>
<tr>
<td>type</td>
<td>string</td>
<td>What type of subsection this is—`section` (the default), `table`, `illustration`, or possibly others, as dictated by the needs of each legal code.</td>
<td></td>
</tr>
<tr>
<td>prefixes</td>
<td>array</td>
<td>A list of of the complete prefix path that uniquely identifies this subsection.</td>
<td></td>
</tr>
<tr>
<td>prefix</td>
<td>string <em>or null</em></td>
<td>This subsection's identifying prefix.</td>
<td></td>
</tr>
<tr>
<td>entire_prefix</td>
<td>string <em>or null</em></td>
<td>The complete, uniquely identifying prefix for this subsection.</td>
<td>The same data that's in `prefixes`, only collapsed into a single string.</td>
</tr>
<tr>
<td>prefix_anchor</td>
<td>string <em>or null</em></td>
<td>The complete, uniquely identifying prefix for this subsection, ready to be used in a URL.</td>
<td>This is `entire_prefix`, URL encoded.</td>
</tr>
<tr>
<td>level</td>
<td>integer</td>
<td>How deeply nested that this level is, in the hierarchy of subsections. 1 is the top level.</td>
<td></td>
</tr>
</table>

</td>
</tr>

<tr>
<td>ancestry</td>
<td>array</td>
<td>Contains a subsection for each hierarchically nested structural unit that contains this law.</td>
<td></td>
</tr>

<tr>
<td colspan="4">
<table>

<tr>
<td>id</td>
<td>integer</td>
<td>Internal unique identifier for this structural unit.</td>
<td></td>
</tr>
<tr>
<td>name</td>
<td>string</td>
<td>The title of this structural unit.</td>
<td></td>
</tr>
<tr>
<td>identifier</td>
<td>string</td>
<td>The identifer for this structural unit.</td>
<td>This will often—but not always—be a number.</td>
</tr>
<tr>
<td>label</td>
<td>string</td>
<td>The name of this level of structural unit.</td>
<td></td>
</tr>
<tr>
<td>url</td>
<td>string</td>
<td>The public (non-API) URL for this structural unit.</td>
<td></td>
</tr>

</table>
</td>
</tr>

<tr>
<td>structure_contents</td>
<td>array</td>
<td>Contains a subsection for each law that is part of the immediate structural unit (siblings of the requested section).</td>
<td></td>
</tr>

<tr>
<td colspan="4">
<table>

<tr>
<td>id</td>
<td>integer</td>
<td>Internal unique identifier for this law.</td>
<td></td>
</tr>

<tr>
<td>structure_id</td>
<td>integer</td>
<td>Internal unique identifier for the containing structural unit.</td>
<td></td>
</tr>

<tr>
<td>section_number</td>
<td>string</td>
<td>The unique identifier for this section.</td>
<td></td>
</tr>

<tr>
<td>catch_line</td>
<td>string <em>or null</em></td>
<td>The title of this section.</td>
<td>Not all legal codes have catch lines. California's laws, for instance, lack them.</td>
</tr>

<tr>
<td>url</td>
<td>string</td>
<td>The public (non-API) URL for this law.</td>
<td></td>
</tr>

<tr>
<td>api_url</td>
<td>string</td>
<td>The API URL for this law.</td>
<td></td>
</tr>

</table>
</td>
</tr>

<tr>
<td>previous_section</td>
<td>array</td>
<td>The prior law within the containing structural unit, as ordered.</td>
<td></td>
</tr>

<tr>
<td colspan="4">
<table>

<tr>
<td>id</td>
<td>integer</td>
<td>Internal unique identifier for this law.</td>
<td></td>
</tr>

<tr>
<td>structure_id</td>
<td>integer</td>
<td>Internal unique identifier for the containing structural unit.</td>
<td></td>
</tr>

<tr>
<td>section_number</td>
<td>string</td>
<td>The unique identifier for this section.</td>
<td></td>
</tr>

<tr>
<td>catch_line</td>
<td>string <em>or null</em></td>
<td>The title of this section.</td>
<td>Not all legal codes have catch lines. California's laws, for instance, lack them.</td>
</tr>

<tr>
<td>url</td>
<td>string</td>
<td>The public (non-API) URL for this law.</td>
<td></td>
</tr>

<tr>
<td>api_url</td>
<td>string</td>
<td>The API URL for this law.</td>
<td></td>
</tr>

</table>
</td>
</tr>

<tr>
<td>next_section</td>
<td>array</td>
<td>The following law within the containing structural unit, as ordered.</td>
<td></td>
</tr>

<tr>
<td colspan="4">
<table>

<tr>
<td>id</td>
<td>integer</td>
<td>Internal unique identifier for this law.</td>
<td></td>
</tr>

<tr>
<td>structure_id</td>
<td>integer</td>
<td>Internal unique identifier for the containing structural unit.</td>
<td></td>
</tr>

<tr>
<td>section_number</td>
<td>string</td>
<td>The unique identifier for this section.</td>
<td></td>
</tr>

<tr>
<td>catch_line</td>
<td>string <em>or null</em></td>
<td>The title of this section.</td>
<td>Not all legal codes have catch lines. California's laws, for instance, lack them.</td>
</tr>

<tr>
<td>url</td>
<td>string</td>
<td>The public (non-API) URL for this law.</td>
<td></td>
</tr>

<tr>
<td>api_url</td>
<td>string</td>
<td>The API URL for this law.</td>
<td></td>
</tr>

</table>
</td>
</tr>

<tr>
<td>metadata</td>
<td>array, <em>or null</em></td>
<td>Contains all metadata about this law that is stored separately in the metadata table.</td>
<td>By default, this contains nothing, but any State Decoded implementation that wants to store additional metadata about laws—data beyond the structure otherwise described here—would emit that metadata here.</td>
</tr>

<tr>
<td>court_decisions</td>
<td>array, <em>or null</em></td>
<td>Every court decision that cites this law.</td>
<td>Only available within State Decoded implementations that have chosen to ingest court rulings.</td>
</tr>

<tr>
<td>official_url</td>
<td>string, <em>or null</em></td>
<td>The URL for this law on the official website for this legal code.</td>
<td></td>
</tr>

<tr>
<td>history_text</td>
<td>string, <em>or null</em></td>
<td>A narrative version of the `history` field.</td>
<td></td>
</tr>

<tr>
<td>references</td>
<td>array</td>
<td>A listing of all other laws that refer to this one.</td>
<td></td>
</tr>

<tr>
<td colspan="4">
<table>

<tr>
<td>id</td>
<td>integer</td>
<td>Internal unique identifier for this law.</td>
<td></td>
</tr>

<tr>
<td>section_number</td>
<td>string</td>
<td>The unique identifier for this section.</td>
<td></td>
</tr>

<tr>
<td>catch_line</td>
<td>string <em>or null</em></td>
<td>The title of this section.</td>
<td>Not all legal codes have catch lines. California's laws, for instance, lack them.</td>
</tr>

<tr>
<td>url</td>
<td>string</td>
<td>The public (non-API) URL for this law.</td>
<td></td>
</tr>

</table>
</td>
</tr>

<tr>
<td>related</td>
<td>array</td>
<td>A list of all other laws that are have been determined through lexical analysis to be similar to this one.</td>
<td>This list is generated by [Solr's MoreLikeThis handler](http://wiki.apache.org/solr/MoreLikeThis).</td>
</tr>

<tr>
<td colspan="4">
<table>

<tr>
<td>id</td>
<td>integer</td>
<td>Internal unique identifier for this law.</td>
<td></td>
</tr>

<tr>
<td>catch_line</td>
<td>string <em>or null</em></td>
<td>The title of this section.</td>
<td>Not all legal codes have catch lines. California's laws, for instance, lack them.</td>
</tr>

<tr>
<td>section_number</td>
<td>string</td>
<td>The unique identifier for this section.</td>
<td></td>
</tr>

<tr>
<td>url</td>
<td>string</td>
<td>The public (non-API) URL for this law.</td>
<td></td>
</tr>

</table>
</td>
</tr>

<tr>
<td>amendment_years</td>
<td>array</td>
<td>A list of every year in which this law was amended.</td>
<td></td>
</tr>

<tr>
<td>url</td>
<td>string</td>
<td>The public (non-API) URL for this law.</td>
<td></td>
</tr>

<tr>
<td>citation</td>
<td>array</td>
<td>A list of formal citation methods for this law.</td>
<td></td>
</tr>

<tr>
<td colspan="4">
<table>

<tr>
<td>official</td>
<td>string</td>
<td>A formal citation for this law, in <em>The Bluebook</em> format.</td>
<td></td>
</tr>
<tr>
<td>universal</td>
<td>string</td>
<td>A formal citation for this law, in the <a href="http://universalcitation.org/">AALL <em>Universal Citation Guide</em></a> format.</td>
<td></td>
</tr>

</table>
</td>
</tr>

<tr>
<td>api_version</td>
<td>integer</td>
<td>The version of the API that is returning this result.</td>
<td></td>
</tr>

</tbody>
</table>

### Sample query and response

Abbreviated for brevity.

```
http://vacode.org/api/law/15.2-627?key=[api_key]
```

```json
{
	"section_number": "15.2-627",
	"section_id": "7508",
	"structure_id": "1980",
	"catch_line": "Department of education.",
	"history": "Code 1950, § 15-324; 1956, c. 153; 1962, c. 623, § 15.1-644; 1980, c. 559; 1981, c. 246; 1982, cc. 32, 75; 1995, c. 8; 1996, c. 873; 1997, c. 587.",
	"full_text": "<p>The department of education shall consist of the county school board, the division superintendent of schools and the officers and employees thereof. Except as herein otherwise provided, the county school board and the division superintendent of schools shall exercise all the powers conferred and perform all the duties imposed upon them by general law. Except for the initial elected board which shall consist of five members, the county school board shall be composed of not less than three nor more than nine members; however, there shall be at least one school board member elected from each of the county’s magisterial or election districts. The members shall be elected by popular vote from election districts coterminous with the election districts for the board of county supervisors. The exact number of members shall be determined by the board of county supervisors. Elections of school board members shall be held to coincide with the elections of members of the board of county supervisors at the regular general election in November. The terms of office for the county school board members shall be the same as the terms of the members of the board of county supervisors and shall commence on January 1 following their election.<\/p><p>A vacancy in the office of school board member shall be filled pursuant to §§ 24.2-226 and 24.2-228.<\/p><p>In order to have their names placed on the ballot, all candidates shall be nominated only by petition as provided by general law pursuant to § 24.2-506.<\/p><p>The county school board may also appoint a resident of the county to cast the deciding vote in case of a tie vote of the school board as provided in § 22.1-75. The tie breaker, if any, shall be appointed for a four-year term whether appointed to fill a vacancy caused by expiration of term or otherwise.<\/p><p>The chairman of the county school board, for the purpose of appearing before the board of county supervisors, shall be considered head of this department, unless some other person in the department shall be designated by the school board for such purpose.<\/p>",
	"repealed": "n",
	"text": {
		"0": {
			"text": "The department of education shall consist of the county school board, the division superintendent of schools and the officers and employees thereof. Except as herein otherwise provided, the county school board and the division superintendent of schools shall exercise all the powers conferred and perform all the duties imposed upon them by general law. Except for the initial elected board which shall consist of five members, the county school board shall be composed of not less than three nor more than nine members; however, there shall be at least one school board member elected from each of the county’s magisterial or election districts. The members shall be elected by popular vote from election districts coterminous with the election districts for the board of county supervisors. The exact number of members shall be determined by the board of county supervisors. Elections of school board members shall be held to coincide with the elections of members of the board of county supervisors at the regular general election in November. The terms of office for the county school board members shall be the same as the terms of the members of the board of county supervisors and shall commence on January 1 following their election.",
			"type": "section",
			"prefixes": [
				""
			],
			"prefix": "",
			"entire_prefix": "",
			"prefix_anchor": "",
			"level": 1
		},
		"1": {
			"text": "A vacancy in the office of school board member shall be filled pursuant to §§ 24.2-226 and 24.2-228.",
			"type": "section",
			"prefixes": [
				""
			],
			"prefix": "",
			"entire_prefix": "",
			"prefix_anchor": "",
			"level": 1
		},
		"2": {
			"text": "In order to have their names placed on the ballot, all candidates shall be nominated only by petition as provided by general law pursuant to § 24.2-506.",
			"type": "section",
			"prefixes": [
				""
			],
			"prefix": "",
			"entire_prefix": "",
			"prefix_anchor": "",
			"level": 1
		},
		"3": {
			"text": "The county school board may also appoint a resident of the county to cast the deciding vote in case of a tie vote of the school board as provided in § 22.1-75. The tie breaker, if any, shall be appointed for a four-year term whether appointed to fill a vacancy caused by expiration of term or otherwise.",
			"type": "section",
			"prefixes": [
				""
			],
			"prefix": "",
			"entire_prefix": "",
			"prefix_anchor": "",
			"level": 1
		},
		"4": {
			"text": "The chairman of the county school board, for the purpose of appearing before the board of county supervisors, shall be considered head of this department, unless some other person in the department shall be designated by the school board for such purpose.",
			"type": "section",
			"prefixes": [
				""
			],
			"prefix": "",
			"entire_prefix": "",
			"prefix_anchor": "",
			"level": 1
		}
	},
	"ancestry": {
		"1": {
			"id": "1980",
			"name": "County Manager Form of Government",
			"identifier": "6",
			"label": "chapter",
			"url": "/15.2/6/"
		},
		"2": {
			"id": "27",
			"name": "Counties, Cities and Towns",
			"identifier": "15.2",
			"label": "title",
			"url": "/15.2/"
		}
	},
	"structure_contents": {
		"0": {
			"id": "7481",
			"structure_id": "1980",
			"section_number": "15.2-600",
			"catch_line": "Designation of form; applicability of chapter.",
			"url": "http://vacode.org/15.2-600/",
			"api_url": "http://vacode.org/api/law/15.2-600/"
		},
		"1": {
			"id": "7482",
			"structure_id": "1980",
			"section_number": "15.2-601",
			"catch_line": "Adoption of county manager form.",
			"url": "http://vacode.org/15.2-601/",
			"api_url": "http://vacode.org/api/law/15.2-601/"
		},
		"2": {
			"id": "7483",
			"structure_id": "1980",
			"section_number": "15.2-602",
			"catch_line": "Powers vested in board of supervisors; election and terms of members; vacancies.",
			"url": "http://vacode.org/15.2-602/",
			"api_url": "http://vacode.org/api/law/15.2-602/"
		},
		"3": {
			"id": "7484",
			"structure_id": "1980",
			"section_number": "15.2-603",
			"catch_line": "Referendum on election of supervisors by districts or at large.",
			"url": "http://vacode.org/15.2-603/",
			"api_url": "http://vacode.org/api/law/15.2-603/"
		},
		"4": {
			"id": "7485",
			"structure_id": "1980",
			"section_number": "15.2-604",
			"catch_line": "General powers of board.",
			"url": "http://vacode.org/15.2-604/",
			"api_url": "http://vacode.org/api/law/15.2-604/"
		},
		"5": {
			"id": "7486",
			"structure_id": "1980",
			"section_number": "15.2-605",
			"catch_line": "Prohibiting misdemeanors and providing penalties.",
			"url": "http://vacode.org/15.2-605/",
			"api_url": "http://vacode.org/api/law/15.2-605/"
		},
		"6": {
			"id": "7487",
			"structure_id": "1980",
			"section_number": "15.2-606",
			"catch_line": "Investigation of county officers.",
			"url": "http://vacode.org/15.2-606/",
			"api_url": "http://vacode.org/api/law/15.2-606/"
		},
		"7": {
			"id": "7488",
			"structure_id": "1980",
			"section_number": "15.2-607",
			"catch_line": "Organization of departments.",
			"url": "http://vacode.org/15.2-607/",
			"api_url": "http://vacode.org/api/law/15.2-607/"
		},
		"8": {
			"id": "7489",
			"structure_id": "1980",
			"section_number": "15.2-608",
			"catch_line": "Designation of officers to perform certain duties.",
			"url": "http://vacode.org/15.2-608/",
			"api_url": "http://vacode.org/api/law/15.2-608/"
		},
		"9": {
			"id": "7490",
			"structure_id": "1980",
			"section_number": "15.2-609",
			"catch_line": "Appointment of county manager.",
			"url": "http://vacode.org/15.2-609/",
			"api_url": "http://vacode.org/api/law/15.2-609/"
		},
		"10": {
			"id": "7491",
			"structure_id": "1980",
			"section_number": "15.2-610",
			"catch_line": "Tenure of office; removal.",
			"url": "http://vacode.org/15.2-610/",
			"api_url": "http://vacode.org/api/law/15.2-610/"
		},
		"11": {
			"id": "7492",
			"structure_id": "1980",
			"section_number": "15.2-611",
			"catch_line": "Disability of county manager.",
			"url": "http://vacode.org/15.2-611/",
			"api_url": "http://vacode.org/api/law/15.2-611/"
		},
		"12": {
			"id": "7493",
			"structure_id": "1980",
			"section_number": "15.2-612",
			"catch_line": "Manager responsible for administration of affairs of county; appointment of officers and employees.",
			"url": "http://vacode.org/15.2-612/",
			"api_url": "http://vacode.org/api/law/15.2-612/"
		}
	},
	"previous_section": {
		"id": "7507",
		"structure_id": "1980",
		"section_number": "15.2-626",
		"catch_line": "Department and board of social services.",
		"url": "http://vacode.org/15.2-626/",
		"api_url": "http://vacode.org/api/law/15.2-626/"
	},
	"next_section": {
		"id": "7509",
		"structure_id": "1980",
		"section_number": "15.2-628",
		"catch_line": "Terms of school boards.",
		"url": "http://vacode.org/15.2-628/",
		"api_url": "http://vacode.org/api/law/15.2-628/"
	},
	"metadata": {
		"history": {
			"1": {
				"year": "1956",
				"chapter": "153"
			},
			"2": {
				"year": "1962",
				"chapter": "623",
				"section": "1962, c. 623, § 15.1-644"
			},
			"3": {
				"year": "1980",
				"chapter": "559"
			},
			"4": {
				"year": "1981",
				"chapter": "246"
			},
			"5": {
				"year": "1982",
				"chapter": [
					"32",
					"75"
				]
			},
			"6": {
				"year": "1995",
				"chapter": "8"
			},
			"7": {
				"year": "1996",
				"chapter": "873"
			},
			"8": {
				"year": "1997",
				"chapter": "587"
			}
		}
	},
	"court_decisions": false,
	"official_url": "http://lis.virginia.gov/cgi-bin/legp604.exe?000+cod+15.2-627",
	"history_text": "This law has been modified 7 times since it was first created in 1956. Those modifications are cataloged by “The Acts of Assembly,” a state publication, by year and chapter. Those modifications that can be read on the General Assembly’s website will be linked accordingly. Those modifications are as follows: in 1962, chapter 623; in 1980, chapter 559; in 1981, chapter 246; in 1982, chapters 32, 75; in 1995 chapter <a href=\"http://leg1.state.va.us/cgi-bin/legp504.exe?951+ful+CHAP0008\">8<\/a>; in 1996 chapter <a href=\"http://leg1.state.va.us/cgi-bin/legp504.exe?961+ful+CHAP0873\">873<\/a>; in 1997 chapter <a href=\"http://leg1.state.va.us/cgi-bin/legp504.exe?971+ful+CHAP0587\">587<\/a>.",
	"references": [
		{
			"id": "12528",
			"section_number": "22.1-29.1",
			"catch_line": "Public hearing before appointment of school board members.",
			"url": "http://vacode.org/22.1-29.1/"
		},
		{
			"id": "12533",
			"section_number": "22.1-34",
			"catch_line": "Application of article.",
			"url": "http://vacode.org/22.1-34/"
		},
		{
			"id": "12542",
			"section_number": "22.1-41",
			"catch_line": "Application of article.",
			"url": "http://vacode.org/22.1-41/"
		},
		{
			"id": "12596",
			"section_number": "22.1-75",
			"catch_line": "Procedure in case of tie vote.",
			"url": "http://vacode.org/22.1-75/"
		}
	],
	"related": {
		"0": {
			"id": "7469",
			"catch_line": "Department of education.",
			"section_number": "15.2-531",
			"url": "/15.2-531/"
		},
		"1": {
			"id": "7618",
			"catch_line": "Department of education.",
			"section_number": "15.2-837",
			"url": "/15.2-837/"
		},
		"2": {
			"id": "12566",
			"catch_line": "Election of school board members; appointment of tie breaker.",
			"section_number": "22.1-57.3",
			"url": "/22.1-57.3/"
		},
		"3": {
			"id": "7428",
			"catch_line": "County school board and division superintendent of schools.",
			"section_number": "15.2-410",
			"url": "/15.2-410/"
		},
		"4": {
			"id": "7583",
			"catch_line": "Powers of county vested in board of supervisors; membership, election, terms, etc., of board; vacancies; powers of chairman.",
			"section_number": "15.2-802",
			"url": "/15.2-802/"
		}
	},
	"amendment_years": {
		"0": "1950",
		"1": "1956",
		"2": "1962",
		"3": "1980",
		"4": "1981",
		"5": "1982",
		"6": "1995",
		"7": "1996",
		"8": "1997"
	},
	"url": "http://vacode.org/15.2-627/",
	"citation": {
		"official": "Va. Code §&nbsp;15.2-627",
		"universal": "VA Code §&nbsp;15.2-627"
	},
	"api_version": "0.1"
}
```

## <a id="method-structure"></a> Structure
When provided with a structural identifier identifying an individual structural unit (e.g., a title, a chapter, a part, etc.), returns everything that The State Decoded knows about that structural unit.

### Query format
`http://vacode.org/api/structure/[identifier]?key=[api_key]`

Leave [identifier] blank to get a top-level listing of structural units—that is, the major structural units that make up the basic divisions of the code (e.g., titles).

### Optional parameters
#### Callback
Pass the parameter `callback` and a callback string to have results returned as JSONP.

```
http://vacode.org/api/structure/[identifier]?key=[api_key]&callback=d9f3lIb013
```

#### Specified fields
Pass the parameter `fields` and a comma-separated listing of fields to limit the response to those fields.

```
http://vacode.org/api/structure/[identifier]?key=[api_key]&fields=catch_line,url_official_url,citation
```

### Fields
<table>
<thead>
<tr>
<th>Name</th>
<th>Type</th>
<th>Description</th>
<th>Notes</th>
</tr>
</thead>
<tbody>

<tr>
<td>ancestry</td>
<td>array</td>
<td>Information about every structural unit included in the path specified in the request.</td>
<td></td>
</tr>

<tr>
<td colspan="4">
<table>

<tr>
<td>id</td>
<td>integer</td>
<td>Internal unique identifier for this structural unit.</td>
<td></td>
</tr>
<tr>
<td>identifier</td>
<td>string</td>
<td>The identifer for this structural unit.</td>
<td>This will often—but not always—be a number.</td>
</tr>
<tr>
<td>name</td>
<td>string</td>
<td>The title of this structural unit.</td>
<td></td>
</tr>
<tr>
<td>label</td>
<td>string</td>
<td>The name of this level of structural unit.</td>
<td></td>
</tr>
<tr>
<td>parent_id</td>
<td>integer</td>
<td>Internal unique identifier for this structural unit’s parent.</td>
<td></td>
</tr>
<tr>
<td>level</td>
<td>integer</td>
<td>How deeply nested that this level is, in the hierarchy of subsections. 1 is the top level.</td>
<td></td>
</tr>
<tr>
<td>url</td>
<td>string</td>
<td>The public (non-API) URL for this structural unit.</td>
<td></td>
</tr>

</table>
</td>
</tr>

<tr>
<td>children</td>
<td>array</td>
<td>A list of every structural unit that is a child of the requested one.</td>
<td></td>
</tr>

<tr>
<td colspan="4">
<table>


<tr>
<td>id</td>
<td>integer</td>
<td>Internal unique identifier for this structural unit.</td>
<td></td>
</tr>
<tr>
<td>label</td>
<td>string</td>
<td>The name of this level of structural unit.</td>
<td></td>
</tr>
<tr>
<td>name</td>
<td>string</td>
<td>The title of this structural unit.</td>
<td></td>
</tr>
<tr>
<td>identifier</td>
<td>string</td>
<td>The identifer for this structural unit.</td>
<td>This will often—but not always—be a number.</td>
</tr>
<tr>
<td>url</td>
<td>string</td>
<td>The public (non-API) URL for this structural unit.</td>
<td></td>
</tr>
<tr>
<td>api_url</td>
<td>string</td>
<td>The API URL for this structural unit.</td>
<td></td>
</tr>

</table>
</td>
</tr>

<tr>
<td>laws</td>
<td>array</td>
<td>A list of every law that is a child of the requested structural unit.</td>
<td></td>
</tr>

<tr>
<td colspan="4">
<table>

<tr>
<td>id</td>
<td>integer</td>
<td>Internal unique identifier for this law.</td>
<td></td>
</tr>

<tr>
<td>structure_id</td>
<td>integer</td>
<td>Internal unique identifier for the containing structural unit.</td>
<td></td>
</tr>

<tr>
<td>section_number</td>
<td>string</td>
<td>The unique identifier for this section.</td>
<td></td>
</tr>

<tr>
<td>catch_line</td>
<td>string <em>or null</em></td>
<td>The title of this section.</td>
<td>Not all legal codes have catch lines. California's laws, for instance, lack them.</td>
</tr>

<tr>
<td>url</td>
<td>string</td>
<td>The public (non-API) URL for this law.</td>
<td></td>
</tr>

<tr>
<td>api_url</td>
<td>string</td>
<td>The API URL for this law.</td>
<td></td>
</tr>

</table>
</tr>
</td>

<tr>
<td>api_version</td>
<td>integer</td>
<td>The version of the API that is returning this result.</td>
<td></td>
</tr>

</tbody>
</table>

### Sample query and response
```
http://vacode.org/api/structure/18.2?key=[api_key]
```

```json
{
	"ancestry": {
		"0": {
			"id": "30",
			"identifier": "18.2",
			"name": "Crimes and Offenses Generally",
			"label": "title",
			"parent_id": "",
			"level": 1,
			"url": "http://vacode.org/18.2/"
		}
	},
	"children": {
		"0": {
			"id": "2082",
			"label": "chapter",
			"name": "In General",
			"identifier": "1",
			"url": "http://vacode.org/18.2/1/",
			"api_url": "http://vacode.org/api/structure/18.2/1/"
		},
		"1": {
			"id": "2083",
			"label": "chapter",
			"name": "Principals and Accessories",
			"identifier": "2",
			"url": "http://vacode.org/18.2/2/",
			"api_url": "http://vacode.org/api/structure/18.2/2/"
		},
		"2": {
			"id": "2084",
			"label": "chapter",
			"name": "Inchoate Offenses",
			"identifier": "3",
			"url": "http://vacode.org/18.2/3/",
			"api_url": "http://vacode.org/api/structure/18.2/3/"
		},
		"3": {
			"id": "2085",
			"label": "chapter",
			"name": "Crimes Against the Person",
			"identifier": "4",
			"url": "http://vacode.org/18.2/4/",
			"api_url": "http://vacode.org/api/structure/18.2/4/"
		},
		"4": {
			"id": "2086",
			"label": "chapter",
			"name": "Crimes Against Property",
			"identifier": "5",
			"url": "http://vacode.org/18.2/5/",
			"api_url": "http://vacode.org/api/structure/18.2/5/"
		},
		"5": {
			"id": "2087",
			"label": "chapter",
			"name": "Crimes Involving Fraud",
			"identifier": "6",
			"url": "http://vacode.org/18.2/6/",
			"api_url": "http://vacode.org/api/structure/18.2/6/"
		},
		"6": {
			"id": "2088",
			"label": "chapter",
			"name": "Crimes Involving Health and Safety",
			"identifier": "7",
			"url": "http://vacode.org/18.2/7/",
			"api_url": "http://vacode.org/api/structure/18.2/7/"
		},
		"7": {
			"id": "2089",
			"label": "chapter",
			"name": "Crimes Involving Morals and Decency",
			"identifier": "8",
			"url": "http://vacode.org/18.2/8/",
			"api_url": "http://vacode.org/api/structure/18.2/8/"
		},
		"8": {
			"id": "2090",
			"label": "chapter",
			"name": "Crimes Against Peace and Order",
			"identifier": "9",
			"url": "http://vacode.org/18.2/9/",
			"api_url": "http://vacode.org/api/structure/18.2/9/"
		},
		"9": {
			"id": "2091",
			"label": "chapter",
			"name": "Crimes Against the Administration of Justice",
			"identifier": "10",
			"url": "http://vacode.org/18.2/10/",
			"api_url": "http://vacode.org/api/structure/18.2/10/"
		},
		"10": {
			"id": "2092",
			"label": "chapter",
			"name": "Offenses Against the Sovereignty of the Commonwealth",
			"identifier": "11",
			"url": "http://vacode.org/18.2/11/",
			"api_url": "http://vacode.org/api/structure/18.2/11/"
		},
		"11": {
			"id": "2093",
			"label": "chapter",
			"name": "Miscellaneous",
			"identifier": "12",
			"url": "http://vacode.org/18.2/12/",
			"api_url": "http://vacode.org/api/structure/18.2/12/"
		},
		"12": {
			"id": "2094",
			"label": "chapter",
			"name": "Virginia Racketeer Influenced and Corrupt Organization Act",
			"identifier": "13",
			"url": "http://vacode.org/18.2/13/",
			"api_url": "http://vacode.org/api/structure/18.2/13/"
		}
	},
	"laws": false
}
```

## <a id="method-dictionary"></a> Dictionary
When provided with a term, returns all definitions for that term found within the code.

### Query format
```
http://vacode.org/api/dictionary/[word_or_phrase]?key=[api_key]
```

### Optional parameters
#### Section number
Pass the parameter `section` and a section number, and get only the definition of the term that is relevant to that law. (Most terms have a defined scope of a given structural unit or law.) *N.B.: Because only one definition is returned, rather than providing an array of definitions, just a single definition’s fields are returned. They are not stored as the lone element in an array, but are at the top level of the JSON.*

```
http://vacode.org/api/dictionary/person?section=12.1-14&key=[api_key]
```

#### Callback
Pass the parameter `callback` and a callback string to have results returned as JSONP.

```
http://vacode.org/api/dictionary/[word_or_phrase]?key=[api_key]&callback=d9f3lIb013
```

#### Specified fields
Pass the parameter `fields` and a comma-separated listing of fields to limit the response to those fields.

```
http://vacode.org/api/dictionary/word_or_phrase]?key=[api_key]&fields=definition
```

### Fields

<table>
<thead>
<tr>
<th>Name</th>
<th>Type</th>
<th>Description</th>
<th>Notes</th>
</tr>
</thead>
<tbody>

<tr>
<td>term</td>
<td>string</td>
<td>The term that is being defined.</td>
<td></td>
</tr>

<tr>
<td>definition</td>
<td>string</td>
<td>The text of the definition.</td>
<td></td>
</tr>

<tr>
<td>scope</td>
<td>string</td>
<td>How broadly that this definition applies.</td>
<td>This is based on the structural unit labels, which vary between implementations. Possible values might be `title`, `chapter`, `part`, or others. A definition that applies to just one law would have a value of `section`, while a definition that applies to the whole code would have a value of `global`.</td>
</tr>

<tr>
<td>section_number</td>
<td>string</td>
<td>The unique identifier for the section in which the definition appears.</td>
<td></td>
</tr>

<tr>
<td>url</td>
<td>string</td>
<td>The URL of the section in which the definition appears.</td>
<td></td>
</tr>

<tr>
<td>formatted</td>
<td>string</td>
<td>The definition, with display-ready formatting and a citation.</td>
<td>This is the same as the content of `definition`, with typographic niceties and a parenthetical citation.</td>
</tr>

<tr>
<td>api_version</td>
<td>integer</td>
<td>The version of the API that is returning this result.</td>
<td></td>
</tr>

### Sample query and response
```
http://vacode.org/api/dictionary/person?section=12.1-14&key=[api_key]
```

```json
{
	"term": "person",
	"definition": "“Person” includes any individual, corporation, partnership, association, cooperative, limited liability company, trust, joint venture, government, political subdivision, or any other legal or commercial entity and any successor, representative, agent, agency, or instrumentality thereof.",
	"section_number": "1-230",
	"scope": "global",
	"url": "http://vacode.org/1-230/",
	"formatted": "“Person” includes any individual, corporation, partnership, association, cooperative, limited liability company, trust, joint venture, government, political subdivision, or any other legal or commercial entity and any successor, representative, agent, agency, or instrumentality thereof. (<a href=\"http://vacode.org/1-230/\">§&nbsp;1-230<\/a>)",
	"api_version": "0.1"
}
```

# <a id="errors"></a>Errors
When a request cannot be completed, the API will return a JSON-formatted error. For example, if an invalid key is provided in a request, the response would be as follows:

```json
"error"
{
	"message": "An Error Occurred",
	"details": "Invalid key."
}
```
