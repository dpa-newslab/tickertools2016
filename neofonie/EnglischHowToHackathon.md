# The unofficial News-Stream API 

At Tickertools hackathon, the unofficial News-Stream API is presented for the first time. The API grants access to the news index compiled by the [News-Stream](http://newsstreamproject.org) research project. The search index (Apache Solr) contains annotated news:

 - **dpa newswire** (all channels including the regional channels and the economy service) 
 - **Neofonie's news feed** containing more than 1200 German and international news sources 
 - Annotated **Persons, Places Organisations, Concepts** extracted by [Neofonie's TXT Werk API](http://txtwerk.de)
 - **departments and industries** (automatically classified)

All code, data and news is copyrighted by dpa, Neofonie GmbH, it's partners or third parties. Usage is permitted in the context of Tickertools Hackathon and related research projects. Please do ask if you intend to use it in other contexts, we will be welcoming.

**Direct contact:** 

mvirtel at dpa-newslab dot com (present at the hackathon)

peter.adolphs at neofonie dot de

Solr is [well documented](https://cwiki.apache.org/confluence/display/solr/Searching). We have prepared some queries which are listed below. We have also prepared a [Banana Dashboard](https://nstr.neofonie.de/dev/#/dashboard/solr/Hackathon) which helps you exploring our news collection. Next to each widget in the Dashboard you finde an information icon which will show you the query parameters used in the widget. 

*All Documents:*  
[https://nstr.neofonie.de/solr-dev/hackathon/select?q=\*:\*](https://nstr.neofonie.de/solr-dev/hackathon/select?q=*:*)

*Documents about Berlin (JSON):* 
[https://nstr.neofonie.de/solr-dev/hackathon/select?q=Berlin&wt=json&indent=on](https://nstr.neofonie.de/solr-dev/hackathon/select?q=Berlin&wt=json&indent=on)
Available result formats: json, php, python, csv u.a. (default XML)

*English documents about Berlin from Neofonie&#39; news crawl:* 
[https://nstr.neofonie.de/solr-dev/hackathon/select?q=Berlin&fq=sourceId:neofonie&fq=language:en&wt=json&indent=on&wt=json&indent=on](https://nstr.neofonie.de/solr-dev/hackathon/select?q=Berlin&fq=sourceId:neofonie&fq=language:en&wt=json&indent=on&wt=json&indent=on)

*Documents about Berlin from Neofonie&#39;s news crawl not older than 24 hours:*
[https://nstr.neofonie.de/solr-dev/hackathon/select?q=Berlin&fq=sourceId:neofonie&fq=publicationDate:[NOW/HOUR-24HOUR%20TO%20NOW/HOUR%2B1HOUR]&wt=json&indent=on](https://nstr.neofonie.de/solr-dev/hackathon/select?q=Berlin&fq=sourceId:neofonie&fq=publicationDate:%5BNOW/HOUR-24HOUR%20TO%20NOW/HOUR%2B1HOUR%5D&wt=json&indent=on)

*Hourly count  Documents about Berlin from Neofonie&#39;s news crawl not older than 24 hours:* 
[https://nstr.neofonie.de/solr-dev/hackathon/select?q=Berlin&wt=json&indent=on&rows=0&fq=publicationDate:[NOW/HOUR-24HOUR%20TO%20NOW/HOUR%2B1HOUR]&fq=sourceId:neofonie&facet=true&facet.range=publicationDate&facet.range.start=NOW/HOUR-24HOUR&facet.range.end=NOW/HOUR%2B1HOUR&facet.range.gap=%2B1HOUR#](https://nstr.neofonie.de/solr-dev/hackathon/select?q=Berlin&wt=json&indent=on&rows=0&fq=publicationDate:%5BNOW/HOUR-24HOUR%20TO%20NOW/HOUR%2B1HOUR%5D&fq=sourceId:neofonie&facet=true&facet.range=publicationDate&facet.range.start=NOW/HOUR-24HOUR&facet.range.end=NOW/HOUR%2B1HOUR&facet.range.gap=%2B1HOUR#)





## Field documentation Solr index##

The majority of fields are available in all documents. 

| **Field** | **Value** | **Description** |
| --- | ------ | -------------- |
| sourceId | string (dpa, neofonie) | News source. &quot;dpa&quot; contains all dpa services (see: dpaServices),&quot;neofonie&quot; is a large news crawl of 1200 news sources. |
| id | string | document id |
| publicationDate | Datetime, e.g. 2016-11-03T15:00:18Z  | date and time |
| title | text | article title |
| text | text | article text |
| language | string, e.g. &quot;de&quot; | language (de, en, …) |
| mlRessort | string, e.g. &quot;vm&quot; | department (automatic classification) |
| mlIndustries | list von strings, z.B &quot;CHM&quot; | industry (automatic classification) |
| knownSurfaceforms | list of strings, e.g. &quot;dpa&quot; | surface form of named entities in text |
| entityLabels | list of strings, e.g. &quot;Deutsche Presse-Agentur&quot; | labels (main identifier) of named entities in text |
| unknownPersonsSurfaceforms | list of strings, e.g. &quot;Tracy Coifman&quot; | person names without wikidata entry (= unknown persons) found in text (may contain wrong entries like &#39;Chrystal Meth&#39;) |
| entityUris | list of wikidata-IDs, e.g. Q312653 (dpa) [https://www.wikidata.org/wiki/Q312653](https://www.wikidata.org/wiki/Q312653) | wikidata IDs of all entities in text  |
| knownTypes | PERSON, PLACE, CONCEPT, EVENT, ORGANISATION | entity types of all entities with wikidata entry |
| unknownTypes | PERSON | PERSON singals that unknown persons were found in text |
| entityRfc4180 | text (csv format) | named entity annotationen including position in text and confidence value |

The following fields are only avaiable for Neofonie's news feed.

| **Neofonie Newsfeed** |   |   |
| --- | ------ | -------------- |
| neoDocId | number | Neofonie&#39;s article id |
| neoApplication | bz, focus, spiegel, faz, stern (eine Auswahl findet sich in untenstehender Liste) | short name of the news source |
| neoPublicationId | int, e.g. 10 | publication id (for a list of English and German publications see below) |
| neoPublicationName | string, e.g. &quot;Berliner Morgenpost&quot; | publication |
| neoUrl | URL, e.g. [http://www.morgenpost.de/sport/hertha/article207495143/Herthas-Suche-nach-der-letzten-Reserve.html](http://www.morgenpost.de/sport/hertha/article207495143/Herthas-Suche-nach-der-letzten-Reserve.html) | article url |
| neoBaseUrl | URL, e.g. [http://www.morgenpost.de/](http://www.morgenpost.de/)  | publication url |
| neoTeaser | Text | teaser text |
| neoTeaserGenerated | boolean | False if teaser text was extracted from original document. True if teaser was generated from the first lines of the article ... |
| neoBody | text | text body. Starts with  … if teaser was generated. |

The following fields are only avaiable for dpa documents.


| **dpa** |   |   |
| --- | ------ | -------------- |
| dpaId | string | news id |
| dpaKeywords | list of strings, e.g. &quot;Champions League&quot; | keywords (tagged manually) |
| dpaIndustries | list of strings, e.g. &quot;TRN&quot; | industries (tagged manually)Only available for dpa-AFX |
| dpaRessort | string, e.g. &quot;rs&quot; | department |
| dpaServices | list of strings, e.g. &quot;dpasrv:bdt&quot; | dpa service, e.g. gerneral service (dpasrv:bdt) |






## German news sources

| **neoPublicationId** | **neoApplication** | **Bezeichnung** |
| --- | --- | --- |
| 130 | bz | B.Z. |
| 10 | berlmopo | Berliner Morgenpost |
| 1794 | buzzfeed\_com | BUZZFEEDNEWS |
| 75 | tagesspiegel | Der Tagesspiegel |
| 112 | focus | FOCUS Online |
| 1312 | hna | [hna.de](http://hna.de) |
| 4 | faz | Frankfurter Allgemeine Zeitung |
| 245 | frankfurterrundschau | Frankfurter Rundschau |
| 189 | handelsblatt | Handelsblatt |
| 299 | ksta | Kölner Stadt Anzeiger |
| 48 | leipzigervz\_css | LVZ-Online |
| 904 | mainpost | Mainpost |
| 246 | mitteldeutschezeitung | Mitteldeutsche Zeitung |
| 63 | neuewestf | Neue Westfälische |
| 465 | passauernp | Passauer Neue Presse |
| 99 | rponline | RP Online |
| 1 | spiegel | SPIEGEL ONLINE |
| 1796 | sputniknews\_com | Sputniknews |
| 2 | stern | Stern |
| 13 | stn | Stuttgarter Nachrichten |
| 93 | stz | Stuttgarter Zeitung |
| 132 | suedkurier | Südkurier |
| 1311 | suedwestpresse | Südwest Presse Online |
| 1797 | huffingtonpost\_de | THE HUFFINGTON POST |
| 5 | welt | WELT ONLINE |
| 248 | waz | Westdeutsche Allgemeine |


## American news sources
| **neoPublictionId** | **neoApplication** | **Name** |
| --- | --- | --- |
|  864 | chron | Houston News |
| 638 |  sf\_chronicle | San Francisco Chronicle |
| 1113 | latimes | Los Angeles Times |
| 1101 | washingtontimes | Washington Times |
| 138 | bloomberg | Bloomberg |
| 190 | usatoday | USA Today |
| 244 | nypost | New York Post |
| 1100 | seattletimes | Seattle Times |
| 1112 | chroniclejournal | Chronicle Journal |




> Interested in using TXT Werk directly? Get a 30 day test account for free. [Registration](https://services.neofonie.de/ws/account/register?lang=en)

