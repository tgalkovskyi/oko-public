# Introduction to OKO DB

TODO

## Core Tables

### Pages
- Table is a catalog of news articles collected by the system. Articles are uniquely indexed by their HTTP URL. In addition to basic metadata information, a number of fields are available detailing basic information about the article, timing of publishing and crawling and the source of information. Other tables offer complimentary information for a subset of articles.
- Time Range: differs by language. Earliest articles in English date back to October 2014.
- Update Interval: every 15 minutes several data sources are crawled for new articles.
- Source: Google News Search, Bing News Search, GDELT, manually added articles.
- Extraction APIs: [IBM AlchemyLanguage](http://www.ibm.com/watson/developercloud/alchemy-language/api/v1/), [Mashape Natural Language Processing API by Joanfihu](https://market.mashape.com/joanfihu/natural-language-processing), Facebook Graph API.
- Columns:
  * **url** Unique HTTP URL of the web document that represents news article. Note that once crawled by the system web pages may expire, change URL or go offline altogether.
  * **title** Title of the news article as reported by a Source, or in case it was unavailable as detected by one of APIs.
  * **pub\_at** Timestamp when article was published as reported by the Source or as detected by one of APIs. Note that this maybe different from the published date/time on the article page itself (print newspapers often put date in the future) or it may approximate and reflect the time when article was first crawled by the software.
  * **synced\_at** Timestamp when article was added to the OKO DB.
  * **social\_fb** Latest known sum of social interactions on Facebook with this URL: sum of number of likes, comments, shares. 7 days after article was added to the OKO DB we stop updating this metric.
  * **social\_tw** Similar to **social\_fb** this metric used to indicate sum of social interactions on Twitter, but it is no longer updated since Oct 2015 due to being deprecated by Twitter.
  * **author** Name or list of names of authors of the news article. While we make best effort to extract this information, available APIs have low accuracy so this field usually requires preprocessing before it can be used for aggregation.
  * **crawl\_id** Key to the **Crawl** table that indicates the exact query source how this article was crawled.
  * **duplicate\_page\_id** If not null, means that article with a high degree of certainty is a full duplicate of another article in the OKO DB.
  
  * Columns **id**, **language\_id**, **publisher\_id**, **social\_sync\_at** are self explanatory.
  * Columns **url\_hash**, **nlapi\_\*** are internal and maybe remove/change without notice.

### Page\_texts
- Table is a catalog of full article text as extracted by one of the APIs consumed by the OKO DB. Each row from **page** table has either 0 or 1 matching rows in **page\_texts** table. While we make best effort to collect
- **Disclaimer**: access to the full text of news articles is governed by the copyright law. OKO DB only provides web archiving services and is not resposible for possible copyright breach by the users of the DB.
- Extraction APIs: [IBM AlchemyLanguage](http://www.ibm.com/watson/developercloud/alchemy-language/api/v1/), [Mashape Natural Language Processing API by Joanfihu](https://market.mashape.com/joanfihu/natural-language-processing), Internal API based on the [PhantomJS](phantomjs.org).
- Columns:
  * **text** Full text of the page, up to 65536 bytes long in 4 bytes UTF8 encoding.
  * **page\_id** Unique ID of the corresponding row in **pages** table.
  * **api** API identifier that was used to extract the full text.

### Topics
- Table is a flat taxonomy of high-level topics which news articles are getting classified. Topics have no hierarchy and are decided by editors of OKO DB.

## Supplementary Tables

* Countries
* Crawls
* Keyword\_objs
* Languages
* Social_stats
* Publishers
* Publishers\_meta
* Users

## Development-only Tables
Tables in the list below are only used for development and may be deleted without advance notice.

* Gdelt\_infos
* Gdelt\_syncs
* Gdelt\_unknown\_pages
* Publisher\_audiences
* Publisher\_dests
* Publisher\_referers
* Publisher\_socials
* Publisher\_traffic\_sources
* Semantic\_links
* Social\_providers
* Text\_frequency\_datas
* Topics\_map
* Topics\_meta
* Website\_meta