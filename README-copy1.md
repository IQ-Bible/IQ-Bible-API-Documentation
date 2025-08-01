# IQ Bible API Official Documentation

## Welcome

Welcome to the **IQ Bible API**, which has been built to enable developers to access a well-maintained and powerful Bible API with useful out-of-the-box tools and resources — like Strong's Concordance and the original texts in Hebrew or Greek — as well as advanced tools, such as professional audio narration for complete chapters.

---

## Overview

The IQ Bible API provides developers with a reliable and well-structured interface for accessing biblical texts, tools, and resources.

Features include:

- Strong's Concordance references
- Original Hebrew and Greek texts
- Professional audio narration for entire chapters
- Topic-based verse searches
- Bible reading plans by theme or duration

You can connect to the API via [RapidAPI](https://rapidapi.com/vibrantmiami/api/iq-bible/), obtain your API key, and start building quickly.

---

## Getting Started

To start using the **IQ Bible API**, follow these simple steps:

### 1. Sign up on RapidAPI

Go to the [IQ Bible API listing on RapidAPI](https://rapidapi.com/vibrantmiami/api/iq-bible/) and sign up for a free RapidAPI account if you don't have one.

### 2. Subscribe to the API

Click **"Subscribe to Test"** or choose a pricing tier (we offer a free plan for getting started).

Once subscribed, you'll receive an **X-RapidAPI-Key** that you'll use to authenticate your requests.

### 3. Make Your First Request

Use any HTTP client (like Postman, Insomnia, or your code) to make a GET request. Here's an example using `curl`:

```bash
curl --request GET \
  --url 'https://iq-bible.p.rapidapi.com/GetVerseById?verseId=John.3.16' \
  --header 'X-RapidAPI-Key: YOUR_API_KEY_HERE' \
  --header 'X-RapidAPI-Host: iq-bible.p.rapidapi.com'
```
---

## FAQ

**How do I get an API key?**  
Visit our [RapidAPI listing](https://rapidapi.com/vibrantmiami/api/iq-bible/), sign in or create an account, then click **Subscribe to Test** and choose a pricing tier (a free tier is available).  
Once subscribed, your API key will be available in your RapidAPI dashboard.

**Is the API free?**  
Yes, there's a free tier available for testing and small-scale use. Higher tiers are available for more extensive usage.

**What data formats are supported?**  
The API returns clean, consistent JSON — perfect for use in web apps, mobile apps, and integrations.

**Can I search by Strong's numbers?**  
Yes! Our endpoints support Strong's Concordance data, making it easy to perform lexical or original-language searches.

**Is audio included for all books?**  
Audio is available for entire chapters in supported translations. Use the `/GetChapterAudio` endpoint to see what's available.

---

## Feature Requests

We're actively improving the IQ Bible API and welcome your feedback.

If you'd like to request a new feature (e.g., a new Bible version, language, or integration), you can:

- Email us at [info@iqbible.com](mailto:info@iqbible.com)
- Or post your request in the Q&A section of our [RapidAPI page](https://rapidapi.com/vibrantmiami/api/iq-bible/)

Your suggestions help us improve the experience for the entire community.

---

## Authorship

The IQ Bible API and its documentation are maintained by [Jody Pike Méndez](https://jodypm.com), founder of [IQBible.com](https://iqbible.com) and senior AI/Web consultant and software engineer at [Websitie.com](https://websitie.com).

Contact:  
- Email: info@iqbible.com  | jody@websitie.com
- Phone: +1 (754) 800-4695  

For questions, contributions, or custom API integrations, contact: [info@iqbible.com](mailto:info@iqbible.com) or [jody@websitie.com](mailto:jody@websitie.com)

---

## Common Parameters

This section explains the key parameters used throughout the IQ Bible API. Understanding these parameters is essential for constructing valid requests and interpreting responses across multiple endpoints.

Parameters like `verseId`, `bookId`, and `versionId` provide standardized ways to reference specific books, chapters, verses, and Bible versions, enabling precise and consistent data retrieval.

### `verseId`

The `verseId` is an 8-digit numeric code used to uniquely identify a specific Bible verse.

**Format:** `BBCCCVVV`  
- `BB` – 2-digit Book ID (e.g., `01` = Genesis, `43` = John)  
- `CCC` – 3-digit Chapter number (e.g., `002` = Chapter 2)  
- `VVV` – 3-digit Verse number (e.g., `004` = Verse 4)

**Example:**  
`01002004` → Genesis 2:4

### `bookId`

A number from `1` to `66` representing the primary 66 books commonly used in Protestant Bibles.

**Example:**  
`1` → Genesis  
`43` → John

### `chapterId`

The chapter number within a book, starting at `1`.

**Example:**  
`1` → Chapter 1

> **Note:** Some endpoints support extrabiblical texts (e.g., 1 Enoch, 2 Esdras, Testaments of the Twelve Patriarchs), which are assigned IDs beyond the canonical 66 books.  
> Use the [`GetBooksExtraBiblical`](#getbooksextrabiblical) endpoint for a full list.

### `versionId`

The Bible version or translation to use.

**Examples:**  
- `kjv` → King James Version  
- `rv1909` → Reina-Valera - 1909

### `bookAndChapterId`

A 5-digit numeric code combining the book and chapter.

**Format:** `BBCCC`  
- `BB` – Book ID (e.g., `40` = Matthew)  
- `CCC` – Chapter (e.g., `001` = Chapter 1)  

**Example:**  
`40001` → Matthew 1

---

## Endpoints

All current IQ Bible API endpoints use the GET method. This section provides clear documentation for each, organized by category for easy navigation.

### 🔊 Audio

#### > GetAudioNarration

**Description:**
Returns the audio narration file for the Bible chapter in the version specified. Currently supported versions: KJV, RV1909 (Reina Valera 1909 (Spanish)), and SVD (Smith-Van Dyke (Arabic)).

The response can be utilized in an audio element to allow the users of your app to listen to complete chapters of the Bible professionally narrated.

Important note: Do not hard-code the URL of the audio file returned by this endpoint as the URLs change periodically. This endpoint should always be called first to ascertain the correct URL for the filename. If you hard-code the URLs and that URL changes later, your audio narration won't work and will thus plague your users with a poor User Experience (UX). Always call this endpoint first, and then use the responding URL in your implementation.

**Headers:**
Content-Type: application/json

**Parameters:**
- `bookId`: Integer
- `chapterId`: Integer
- `versionId`: String (e.g., 'rv1909')

*Example request:* 
`GetAudioNarration?bookId=01&chapterId=02&versionId=kjv`

*Example response:*
`{"fileName":"https:\/\/2-us.com\/media\/audio\/narrations\/obf-kjv\/kjv-dw-01002-0yFiZ1R8cv.mp3"}`

---

### Chapters

#### GetChapter

**Description:**
Returns a complete Bible chapter. Required **Parameters:** 'bookId', 'chapterId', and 'versionId'. For example, 'GetChapter?bookId=01&chapterId=02&versionId=kjv' would return the entire second chapter of Genesis in the King James Version.

**Headers:**
Content-Type: application/json

**Parameters:**
- `bookId`: Integer or String
- `chapterId`: Integer
- `versionId`: String (e.g., 'kjv')

*Example request:* 
`GetChapter?bookId=01&chapterId=01&versionId=kjv`

*Example response:*
`[{"id":"01001001","b":"1","c":"1","v":"1","t":"In the beginning God created the heaven and the earth."},{"id":"01001002","b":"1","c":"1","v":"2","t":"And the earth was without form, and void; and darkness was upon the face of the deep. And the Spirit of God moved upon the face of the waters."}, ... }]`



#### GetChapterByBookAndChapterId

**Description:**
Returns a complete Bible chapter according to the 'bookAndChapterId' and 'versionId' established. Required **Parameters:** 'bookAndChapterId', and 'versionId'. For example, 'GetChapterByBookAndChapterId?bookAndChapterId=40001&versionId=kjv' will return the entire first chapter of Matthew in the King James Version.

This endpoint enjoys great utility when used in conjunction with 'GetBibleReadingPlan'.

Also, see the following endpoints:
GetBibleReadingPlan
GetBookAndChapterNameByBookAndChapterId
GetChapter

**Headers:**
Content-Type: application/json

**Parameters:**
- `bookAndChapterId`: String (e.g., '40001')
- `versionId`: String (e.g., 'kjv')

*Example request:* 
`GetChapterByBookAndChapterId?bookAndChapterId=40001&versionId=kjv`

*Example response:*
`[{"id":"40001001","b":"40","c":"1","v":"1","t":"The book of the generation of Jesus Christ, the son of David, the son of Abraham."},{"id":"40001002","b":"40","c":"1","v":"2","t":"Abraham begat Isaac; and Isaac begat Jacob; and Jacob begat Judas and his brethren;"}, ... }]`

---

### 🔢 Counting

#### GetChapterCount

**Description:**
Returns simply the number of chapters in any book requested via the 'bookId' parameter. For example, 'GetChapterCount?bookId=66' would return '22' as there are twenty-two (22) chapters in Revelation the 66th book.

**Headers:**
Content-Type: application/json

**Parameters:**
- `bookId`: Integer

*Example request:* 
`GetChapterCount?bookId=66`

*Example response:*
`{"chapterCount":22}`



#### GetSearchCount

**Description:**
GetSearchCount will return the total results count for any search term (query) entered. For example, 'GetSearchCount?query=Jesus&versionId=kjv' will return 943 the number of times that the word, "Jesus" appears in the KJV.

**Headers:**
Content-Type: application/json

**Parameters:**
- `query`: String
- `versionId`: String (e.g., 'kjv')

*Example request:* 
`GetSearchCount?query=Messiah&versionId=kjv`

*Example response:*
`{"searchCount":2}`



#### GetVerseCount

**Description:**  
Returns the number of verses for a specified book and chapter. You can now optionally specify the Bible version to use. If no version is provided, the function defaults to the King James Version (KJV). This update allows you to account for differences in verse counts between Bible versions. For example, the 43rd chapter of Genesis contains 34 verses in the KJV, but only 33 verses in the Bible in Basic English (BBE).

**Headers:**  
`Content-Type: application/json`

**Parameters:**  
- `bookId`: Integer (The ID of the book)  
- `chapterId`: Integer (The chapter number)  
- `versionId` (optional): String (e.g., `"kjv"`, `"bbe"`) — Defaults to `"kjv"` if not provided.

*Example Request using KJV:*  
`GetVerseCount?bookId=1&chapterId=43&versionId=kjv`

*Example response using KJV:*
`{"verseCount":34}`

*Example Request using BBE:*  
`GetVerseCount?bookId=1&chapterId=43&versionId=bbe`

*Example response using BBE:*
`{"verseCount":33}`



#### GetWordCountOfBook

**Description:**
This response will provide a count of all of the words in the book ('bookId') and version ('versionId') submitted. Both parameters are required. For example, 'GetWordCountOfBook?bookId=01&versionId=kjv' would return a complete count of all the words in the book of Genesis ('bookId=01') in the KJV version ('versionId=kjv'). Both parameters are required.

**Headers:**
Content-Type: application/json

**Parameters:**
- `bookId`: Integer or String
- `versionId`: String (e.g., 'kjv')

*Example request:* 
`GetWordCountOfBook?bookId=01&versionId=kjv`

*Example response:*
`{"bookId":"01","versionId":"kjv","wordCount":38265}`



#### GetWordCountOfChapter

**Description:**
'GetWordCountOfChapter' returns the count of all of the words in any given chapter specified by the value of the 'bookAndChapterId' in the version ('versionId') requested. For example, 'GetWordCountOfChapter?bookAndChapterId=01001&versionId=kjv' would return a complete count of all the words in the first chapter of the book of Genesis ('bookAndChapterId=01001') in the KJV version ('versionId=kjv'). Both parameters are required.

**Headers:**
Content-Type: application/json

**Parameters:**
- `bookAndChapterId`: String (e.g., '01001')
- `versionId`: String (e.g., 'kjv')

*Example request:* 
`GetWordCountOfChapter?bookAndChapterId=01001&versionId=kjv`

*Example response:*
`{"bookAndChapterId":"01001","versionId":"kjv","wordCount":797}`



#### GetWordCountOfVerse

**Description:**
The response will contain a count of all of the words in any given verse specified by the value of the 'verseId' in the version requested ('versionId'). For example, 'GetWordCountOfVerse?verseId=01001001&versionId=kjv' would return a complete count of all the words in the first verse of the book of Genesis in chapter one ('verseId=01001001') of the KJV version ('versionId=kjv'). Both parameters are required.

**Headers:**
Content-Type: application/json

**Parameters:**
- `verseId`: String (e.g., '01001001')
- `versionId`: String (e.g., 'kjv')

*Example request:* 
`GetWordCountOfVerse?verseId=01001001&versionId=kjv`

*Example response:*
`{"verseId":"01001001","versionId":"kjv","wordCount":10}`

---

### ✡️ Greek and Hebrew Study

#### GetGreekCharactersAndUnicode

**Description:**
Returns the Unicode for the Greek alphabet in both lower and uppercase. Only one parameter is taken: 'language'.

**Headers:**
Content-Type: application/json

**Parameters:**
- `language`: String (e.g., 'english')

*Example request:* 
`GetGreekCharactersAndUnicode?language=english`

*Example response:*
`{"alpha":{"lowercase":"\\03b1","uppercase":"\\0391"},"beta":{"lowercase":"\\03b2","uppercase":"\\0392"},"gamma":{"lowercase":"\\03b3", ... }`



#### GetHebrewCharactersAndUnicodePoints

**Description:**
Returns the Unicode for the Hebrew alphabet and corresponding English transliterations (based on the Unicode standard). The response includes letters, points, accents, punctuation, marks, signs, and Yiddish ligatures. Only one parameter is taken: 'language'.

**Headers:**
Content-Type: application/json

**Parameters:**
- `language`: String (e.g., 'english')

*Example request:* 
`GetHebrewCharactersAndUnicodePoints?language=english`

*Example response:*
`{"letters":{"alef":"\\u05d0","bet":"\\u05d1","gimel":"\\u05d2","dalet":"\\u05d3","he":"\\u05d4","vav":"\\u05d5","zayin":"\\u05d6", ... "}`



#### GetOriginalText

**Description:**  
Returns the original Hebrew (Old Testament) or Greek (New Testament) text for a verse, determined from the `verseId`. There are thirty-nine (39) books in the Old Testament (Protestant) and twenty-seven (27) in the New Testament.

The response now includes each word's Strong's glossary definition to avoid the need for separate lookups.

**Headers:**  
Content-Type: application/json

**Parameters:**  
- `verseId`: String (e.g., `01001001`)

*Example request:*  
`GetOriginalText?verseId=01001001`

*Example response:*  
`[{"id":"1","verseID":"1001001","book":"1","chapter":"1","verse":"1","word":"\u05d1\u05bc\u05b0\u05e8\u05b5\u05d0\u05e9\u05c1\u05b4\u0596\u05d9\u05ea","pronun":"...","strongs":"7225","morph":"","orig_order":"1","connected":"0","parashah":"0","notes":"","glossary":"The first, in place, time, order or rank (Strong's H7225)"}]`

---

### 📚 Books

#### GetBibleBookAbbreviations

**Description:**
GetBibleBookAbbreviations will return an array of all the abbreviations for bible book names. Also see, GetParseCitation.

**Headers:**
Content-Type: application/json

**Parameters:**
- N/A

*Example request:* 
`GetBibleBookAbbreviations?`

*Example response:*
`[["Genesis","Gen.","Ge.","Gn."],["Exodus","Ex.","Exod.","Exo."],["Leviticus","Lev.","Le.","Lv."],["Numbers","Num.","Nu.","Nm.","Nb."], ... ]]`



#### GetBooks

**Description:**
Returns a list of all of the books of the Bible in the language specified with the '?language=[language]' parameter. Currently supported languages are English, Spanish, and Arabic; with more on the way.

**Headers:**
Content-Type: application/json

**Parameters:**
- `language`: String (e.g., 'english')

*Example request:* 
`GetBooks?language=english`

*Example response:*
`[{"b":"1","n":"Genesis"},{"b":"2","n":"Exodus"},{"b":"3","n":"Leviticus"},{"b":"4","n":"Numbers"}, ... }]`



#### GetBooksNT

**Description:**
Returns a list of only the New Testament books in the language specified with the '?language=[language]' parameter.

**Headers:**
Content-Type: application/json

**Parameters:**
- `language`: String (e.g., 'english')

*Example request:* 
`GetBooksNT?language=english`

*Example response:*
`[{"b":"40","n":"Matthew","t":"NT","g":"5"},{"b":"41","n":"Mark", ... }]`



#### GetBooksOT

**Description:**
Returns a list of only the Old Testament books in the language specified with the '?language=[language]' parameter.

**Headers:**
Content-Type: application/json

**Parameters:**
- `language`: String (e.g., 'english')

*Example request:* 
`GetBooksOT?language=english`

*Example response:*
`[{"b":"1","n":"Genesis","t":"OT","g":"1"},{"b":"2","n":"Exodus","t":"OT","g":"1"}, ... }]`



### Info

#### GetInfo

**Description:**
GetInfo will return information about this API.

**Headers:**
Content-Type: application/json

**Parameters:**
- N/A

*Example request:* 
`GetInfo`

*Example response:*
`{"appName":"IQ Bible API","appDescription":"The IQ Bible API provides an advanced structure for procuring not only Biblical passages but data from many other theological sources.","appType":"web application API","version":"v1.9.1","parentApplication":"IQ Bible - https:\/\/iqbible.com","authors":"https:\/\/websitie.com"}`



#### GetEndpoints

**Description:**
Retrieves a list of all available public endpoints in the API. It's an essential tool for developers to understand the API's structure and the parameters each endpoint expects.

**Headers:**
Content-Type: application/json

**Parameters:**
- N/A

*Example request:* 
`GetEndpoints`

*Example response:*
`{"GetAudioNarration":{"params":["bookId","chapterId","versionId"]},"GetAudioMusic":{"params":["genre"]}, ...}`



### Parsing and Extracting

#### GetBookAndChapterNameByBookAndChapterId

**Description:**
Returns the name of the book and chapter as specified in the 'bookAndChapterId' and 'language' parameter values. Both parameters are required.

This endpoint will now validate any submissions (e.g., if an incorrect 'bookAndChapterId' is submitted, it will return empty; whereas, previously, it would return the correct book name with an and incorrect chapter number).

**Headers:**
Content-Type: application/json

**Parameters:**
- `bookAndChapterId`: String (e.g., '01001')
- `language`: String (e.g., 'english')

*Example request:* 
`GetBookAndChapterNameByBookAndChapterId?bookAndChapterId=01001&language=english`

*Example response:*
`"Genesis 1"`



#### GetBookIdByBookName

**Description:**
Returns the 'bookId' of the 'bookName' submitted. At this time, abbreviations, such as with the 'GetParseCitation' endpoint are not supported but will be in the future. English, Spanish, and Arabic book names are supported.

**Headers:**
Content-Type: application/json

**Parameters:**
- `bookName`: String
- `language`: String (e.g., 'english')

*Example request:* 
`GetBookIdByBookName?bookName=John&language=english`

*Example response:*
`"43"`



#### GetBookNameByBookId

**Description:**
Returns the name of the book per the number sent in the language specified. For example, 'GetBookNameByBookId?bookId=01&language=english' would return 'Genesis'.

**Headers:**
Content-Type: application/json

**Parameters:**
- `bookId`: Integer or String
- `language`: String (e.g., 'english')

*Example request:* 
`GetBookNameByBookId?bookId=01&language=english`

*Example response:*
`[{"n":"Genesis"}]`



#### GetBookNameByVerseId

**Description:**
Returns the name of the book per the 'verseId' sent in the language specified. For example, 'GetBookNameByVerseId?verseId=40001001&language=english' would return 'Matthew'.

**Headers:**
Content-Type: application/json

**Parameters:**
- `verseId`: String (e.g., '02001001')
- `language`: String (e.g., 'english')

*Example request:* 
`GetBookNameByVerseId?verseId=02001001&language=english`

*Example response:*
`[{"n":"Matthew"}]`



#### GetParseCitation

**Description:**
Returns all of the 'verseIds' for the citation submitted. You can use abbreviations (e.g., Ex., or Exod. for Exodus) and multiple references from the same book within the same chapter. Also, see GetBibleBookAbbreviations for an array of abbreviations.

Separate individual verses by commas and use a hyphen for a range of verses. So, if we want the 2nd, 3rd, and 7th verses of Matthew chapter 1 as well as a range of verses from 10 to 14, our call would be: 'GetParseCitation?citation=Matt.1:2, 3, 7, 10-14' or 'Matthew 1:2, 3, 7, 10-14', or even, 'Mt. 1:2, 3, 7, 10-14'.

Note: This endpoint does not validate the returned 'verseIds' or book name ('bookName'), nor can it be used to parse multiple verses from different books and chapters.

**Headers:**
Content-Type: application/json

**Parameters:**
- `citation`: String (e.g., 'Matt.1:2,3,7,10-14')

*Example request:* 
`GetParseCitation?citation=Matt.1:2,3,7,10-14`

*Example response:*
`{"bookName":"Matthew","bookId":"40","chapterId":"001","verseIds":["40001002","40001003","40001007","40001010","40001011","40001012","40001013","40001014"]}`



#### GetParseVerseId

**Description:**
Parses any 'verseId' to return the 'bookId', 'bookAndChapterId', 'chapterNumber', and the verse number ('verseNumber'). This endpoint will check the value of the 'verseId' submitted, so ensure that it is a valid 'verseId' and it's correctly formatted (8 digits), otherwise an empty response will be returned. For example, 'GetParseVerseId?verseId=66001001' is valid, and will return the JSON, whereas, 'GetParseVerseId?verseId=66050001' will not since there is not a chapter 50 in the book of Revelation (the 66th book (Protestant)).

**Headers:**
Content-Type: application/json

**Parameters:**
- `verseId`: String (e.g., '01001001')

*Example request:* 
`GetParseVerseId?verseId=43003016`

*Example response:*
`{"verseId":"43003016","bookId":"43","bookAndChapterId":"43003","chapterNumber":"003","verseNumber":"016"}`



#### GetWords

**Description:**
Will return a word-by-word JSON array of the specified verse, along with the word count.

Also, see the following endpoints:
GetWordCountOfBook
GetWordCountOfChapter 
GetWordCountOfVerse

**Headers:**
Content-Type: application/json

**Parameters:**
- `verseId`: String (e.g., '01001001')
- `versionId`: String (e.g., 'kjv')

*Example request:* 
`GetWords?verseId=01001001&versionId=kjv`

*Example response:*
`{"verseId":"01001001","versionId":"kjv","wordCount":10,"0":"In","1":"the","2":"beginning","3":"God","4":"created","5":"the","6":"heaven","7":"and","8":"the","9":"earth."}`



### Reading Plan Generation

#### GetBibleReadingPlan

**Description:**
GetBibleReadingPlan will return a Bible reading plan dividing the Bible into chapters according to the days specified. There are optional parameters that you can set after the required 'days' parameter, namely, 'requestedStartDate', 'sections', and 'requestedAge'.

The initial values of the response are the days requested ('daysRequested') of the plan and the days actually filled ('daysFilled'). This is to denote a disparity in the requested days and actually filled days for an event such as requesting a plan for 365 days of the New Testament only (the 'sections' parameter): As there are only 260 chapters in the New Testament, we can't fill all 365 days with 260 chapters.

The 'sections' parameter can be set to 'OT' or 'NT' for either the Old or New testaments. If this is not set, the default is both, signified by the 'OT+NT' value.

The sub-array 'datesInfo' contains today's date ('todaysDate'), the date the plan starts ('startDate') (if set), how many days until the start date ('daysUntilStartDate'), and the ending date of the plan ('endDate').

The 'chaptersInfo' key holds the values for the total number of chapters of the plan ('totalChapters'), the average number of chapters that will be read per day ('averageChaptersPerDay'), and the most ('mostChapters') and least chapters ('leastChapters') that will be encountered; each having an additional sub-array of information such as what days and dates the most and least chapters will be read on.

The 'readingTime' key holds arrays concerning the average time, based on the reader's age ('readersAge') of the plan. The 'requestedAge' is the value of the parameter 'requestedAge', and the 'readersAge' may be the same as long as the 'requestedAge' is valid and above the value of '6', which is the default age. This array gathers its date from the 'GetReadingTimeByAge' endpoint, q.v., differentiating between silent and oral reading. For the Bible reading plan endpoint, only averages will be returned, but the 'GetReadingTimeByAge' if called separately is more precise.

Note: Please see the description of the 'GetReadingTimeByAge' endpoint for calculations of reading times beyond the age of 18, where we humans normally reach our peak.

After these preliminary arrays, the response is then divided into a key for each day of the plan encompassing the day number ('day'), the 'date' of that day, and all of the chapters ('bookAndChapterIds') to be read on that day. These 'bookAndChapterIds' can be called with the 'GetChapterByBookAndChapterId' endpoint, which will return the entire chapter - just as with the 'GetChapter' endpoint (which uses different parameters).

Lastly, if you want to also pull the names (such as 'Genesis 1') instead of '01001', this can be achieved via the 'GetBookAndChapterNameByBookAndChapterId', which see.

Also, see the following endpoints:
GetReadingTimeByAge
GetChapterByBookAndChapterId
GetBookAndChapterNameByBookAndChapterId
GetWordCountOfChapter

**Headers:**
Content-Type: application/json

**Parameters:**
- `days`: Integer
- `requestedStartDate`: Date (format: YYYY-MM-DD)
- `sections`: String (e.g., 'nt')
- `requestedAge`: Integer

*Example request:* 
`GetBibleReadingPlan?days=180&requestedStartDate=2023-01-01&sections=nt&requestedAge=17`

*Example response:*
`[{"daysRequested":"180","daysFilled":"180","sections":"nt","datesInfo":{"todaysDate":"2022-11-28","startDate":"2023-01-01","daysUntilStartDate":34,"endDate":"2023-06-30"},"chaptersInfo":{"totalChapters":260, ...`



#### GetReadingTimeByAge

**Description:**
This response contains information about how long, on average (with a range of 'high', 'average', and 'low'), it should take a reader of the specified age ('requestedAge') to read the number of words in the 'wordCount' parameter value. Both 'requestedAge' and 'wordCount' are required parameters.

The response is divided into 'silent' and 'oral' (silent reading is the process of reading without speaking the words, while 'oral' reading is the converse (i.e., reading aloud)). Each division of the response is further sub-divided into 'high', 'average', and 'low' ranges ('high' is a fast reader, whereas 'low' is a slow reader); each containing the amount of time the reader, by the age set in the 'requestedAge' parameter, should be able to read the number of words in 'seconds', 'minutes', 'hours', and 'days'.

This endpoint complements the 'GetBibleReadingPlan' endpoint by ascertaining more precise average values for the time in reading chapters (q.v.), but can also be easily coupled with the 'GetWordCountOfVerse', 'GetWordCountOfChapter'. and 'GetWordCountOfBook' endpoints to enable your app to display estimated reading times for your users when formulating reading plans or schedules in diverse feature-sets.

Although not represented in the resulting JSON of this endpoint, the calculations for ages above 18 - 25 are formulated internally with the data obtained from research. 

Interestingly, as we humans sail past the age of 25, on average, our reading speeds begin to decline - whereas there had been a steady increase from the default reading age of 6 until the age range of 18 - 25.

This decline is formulated thusly:

26 - 30 years of age = -20% decline
26 - 30 years of age = -50% decline
46 - 60 years of age = -57.89% decline
60+ years of age = -66.66% decline

References:
- https://onlinelibrary.wiley.com/doi/abs/10.1002/pits.10152
- https://wordsrated.com/reading-speed-by-age/
- https://irisreading.com/average-reading-speed-by-age-are-you-fast-enough/
- https://scholarwithin.com/average-reading-speed

Also, see the following endpoints:
GetBibleReadingPlan
GetWordCountOfVerse
GetWordCountOfChapter
GetWordCountOfBook

**Headers:**
Content-Type: application/json

**Parameters:**
- `requestedAge`: Integer
- `wordCount`: Integer

*Example request:* 
`GetReadingTimeByAge?requestedAge=29&wordCount=650`

*Example response:*
`{"readersAge":"29","readersAgeRequested":"29","wordCount":"650","wordsPerMinuteReadingSpeed":{"low":176,"average":228,"high":280},"timeToRead":{"silent":{"low":{"seconds":221.4,"minutes":3.69,"hours":0.06,"days":0},"average":{"seconds":171, ...}`



#### GetBibleReadingPlanByTopic

**Description:**
This endpoint will return a Bible Reading Plan specific to the value of the 'topic' parameter. Please note that when using this endpoint, not as many days can be assigned per the 'days' parameter as with the 'GetBibleReadingPlan' endpoint, but you can easily ascertain how many 'verseIds' any topic will produce by using the 'GetTopicVerseCount' endpoint (e.g., GetTopicVerseCount?topic=angels). If the 'days' parameter is not set, the value will default to '30'. If the 'topic' parameter is not set, the default will be the first topic (addiction).

Topics (97): addiction, adultery, afterlife, alcohol, animals, anger, angels, anxiety, baptism, birth, business, charity, children, church, compassion, courage, dating, death, deliverance, devil, depression, discipleship, divorce, drugs, endurance, eternal life, evil, faith, faithfulness, family, fasting, fear, forgiveness, friendship, food, generosity, good, gospel, gossip, government, grace, gratitude, guidance, healing, health, holiness, homosexuality, hope, humility, idolatry, ignorance, integrity, jealousy, joy, justice, kindness, kingdom of god, life, love, marriage, medicine, money, miracles, obedience, patience, peace, perseverance, politics, praise, prayer, predestination, prophecy, purpose, racism, redemption, renewal, repentance, resurrection, righteousness, sacrifice, satan, salvation, science, second coming, servanthood, sex, stress, suffering, suicide, temptation, trust, truth, unity, wealth, wisdom, worrying, and worship.

Also, see the following endpoints:
GetTopics
GetTopicVerseCount
GetBibleReadingPlan
GetChapterByBookAndChapterId
GetBookAndChapterNameByBookAndChapterId

**Headers:**
Content-Type: application/json

**Parameters:**
- `topic`: String
- `days`: Integer (optional)
- `requestedStartDate`: Date (format: YYYY-MM-DD) (optional)

*Example request:* 
`GetBibleReadingPlanByTopic?topic=love&days=30&requestedStartDate=2025-01-01`

*Example response:*
`{"topicInfo":{"topic":"love","daysRequested":"30","daysFilled":26},"readingPlan":[{"day":1,"date":"2025-01-01","verseIds":["46013004","46013005","46013006","46013007","46013008","46016014"]},{"day":2,"date":"2025-01-02","verseIds":["43003016","62004008","60004008","51003014","46013013","43015013"]} ...`



### Study Tools

#### GetBookInfo

**Description:**
This endpoint retrieves detailed information for a specified canonical biblical book based on the provided `bookId` and language setting. The data includes both general and theological information about the book, offering an in-depth resource for applications focused on biblical studies, teaching, theological, and thematic exploration. The information spans various aspects such as book structure, themes, historical context, theological significance, and major characters, with additional insights on covenantal themes, symbolism, and eschatological perspectives. 

The endpoint currently supports English (`language=english`) as the default and only available language.

**Headers:**
- `Content-Type`: `application/json`

**Parameters:**
- `bookId` (required, integer): Specifies the biblical book ID. Ranges from `01` (Genesis) to `66` (Revelation).
- `language` (optional, string): Specifies the language for the response data. Currently, only `"english"` is available.

*Example request:*  
`GetBookInfo?bookId=40&language=english`

*Example response:*
`[{"bookId":"40","chapterCount":28,"introduction":"The Gospel of Matthew presents Jesus as the promised Messiah...","introduction_long":"The Gospel of Matthew is a foundational book in the New Testament...","author":"Traditionally attributed to Matthew, the tax collector","date":"Circa a.d. 60-70","word_origin":"The title 'Gospel' comes from the Greek 'euangelion', meaning 'good news'.","genre":"Gospel"}...]`



#### GetCommentary

**Description:**
Returns all of the commentaries for the 'verseId' and 'commentaryId' parameters specified. The 'commentaryId' is currently limited to 'gills'. Both parameters are required.

**Headers:**
Content-Type: application/json

**Parameters:**
- `verseId`: String (e.g., '01001001')
- `commentaryName`: String (e.g., 'gills')

*Example request:* 
`GetCommentary?verseId=01001001&commentaryName=gills `

*Example response:*
`"\nIn the beginning God created the heaven and the earth. By the heaven some understand the supreme heaven, the heaven of heavens, the habitation of God, and of the holy angels; and this being made perfect at once, no mention is ... "`



#### GetCrossReferences

**Description:**
Returns all of the complete cross-references (with starting and ending verse, if applicable) for the 'verseId' received.

**Headers:**
Content-Type: application/json

**Parameters:**
- `verseId`: String (e.g., '66001001')

*Example request:* 
`GetCrossReferences?verseId=66001001`

*Example response:*
`[{"verseId":"66001001","r":"10","sv":"66022016","ev":"00000000"},{"verseId":"66001001","r":"11","sv":"66022006","ev":"00000000"}, ... }]`



#### GetDefinitionBiblical

**Description:**
Returns the dictionary for the 'dictionaryId' parameter specified. The 'dictionaryId' is currently limited to 'smiths'.

**Headers:**
Content-Type: application/json

**Parameters:**
- `query`: String
- `dictionaryId`: String (e.g., 'smiths')

*Example request:* 
`GetDefinitionBiblical?query=solomon&dictionaryId=smiths`

*Example response:*
`{"word":"Solomon","definition":"(peaceful). I. Early life and occasion to the throne . --Solomon was the child of ... "}`



#### GetGenres

**Description:**  
This endpoint retrieves the genres of book groupings within the Bible, providing the starting and ending bookId and verseId for each genre. The genres are categorized according to the section of the Bible (Old Testament, New Testament, or both). It supports genre groupings like "Law", "History", "Poetry", "Prophecy", "Gospels", and "Epistles", enabling easy, programmatic navigation for users. This functionality makes it simple to explore the Bible's traditional literary divisions and facilitates seamless access to genre-based content for applications, studies, or Bible exploration tools.

The `section` parameter allows users to filter the genres by the Old Testament (OT), New Testament (NT), or both (OT + NT).

**Headers:**  
- `Content-Type`: `application/json`

**Parameters:**  
- `language` (optional, string): Specifies the language for the response data. Currently, only `"english"` is available.
- `section` (optional, string): Specifies the section of the Bible for the response. Accepted values are:
  - `"ot"`: Old Testament genres only.
  - `"nt"`: New Testament genres only.
  - `"ot+nt"` or `"all"`: Both Old and New Testament genres.

**Example request:**  
`GetGenres?section=ot+nt&language=english`

**Example response:**
`[{"genre":"Law","startingBookId":"01","endingBookId":"05","startingVerseId":"01001001","endingVerseId":"05034028"}] ...`



#### GetParables

**Description:**
Produces a list of parables in the Bible and their relative citations (e.g., 'GetParables?language=english'). The verses of the parables can be retrieved in your app by using the 'GetParseCitation' call (q.v.). For example, for the last parable, 'The Sheep and the Goats', we can retrieve the 'verseIds' for that parable with this call: 'GetParseCitation?citation=Matthew 25:31-46', and then use the 'GetVerse' endpoint to pull the text.

**Headers:**
Content-Type: application/json

**Parameters:**
- `language`: String (e.g., 'english')

*Example request:* 
`GetParables?language=english`

*Example response:*
`{"Salt of the Earth":["Matthew 5:13","Mark 9:50"],"Lamp Under a Bowl":["Matthew 5:14-16","Mark 4:21-22","Luke 8:16","Luke 11:33"],"Wise and Foolish Builders":["Matthew 7:24-27","Luke 6:47-49"], ...}`



#### GetPropheciesFulfilledInJesus

**Description:**
Returns the 351 prophecies of the Old Testament that were fulfilled in Jesus. Includes the scripture references in the Old and New Testaments.

**Headers:**
Content-Type: application/json

**Parameters:**
- `language`: String (e.g., 'english')

*Example request:* 
`GetPropheciesFulfilledInJesus?language=english`

*Example response:*
`{"Gen 3:15":"He will bruise Satan's head. Fulfilled in: Heb 2:14; 1 John 3:8","Gen 5:24":"The bodily ascension to heaven illustrated. Fulfilled in: Mark 16:19", ... }`



#### GetSemanticRelations

**Description:**  
Returns a list of words semantically related to the input word specified, based on the vocabulary found in the King James Version (KJV) of the Bible. This endpoint helps to identify synonyms or closely associated words, which can be useful in applications involving natural language processing, lexical analysis, or semantic search.

The lookup is case-insensitive, meaning the word can be provided in any capitalization format. The endpoint dynamically fetches the semantic relations from the corresponding JSON file based on the first letter of the input word.

**Headers:**  
Content-Type: application/json

**Parameters:**  
- `word`: String (The word for which you want to retrieve semantic relations)

*Example request:*  
`GetSemanticRelations?word=faith`

*Example response:*
`{"word":"Faith","related":["Belief","Trust","Conviction","Confidence","Loyalty"]}`



#### GetStories

**Description:**
Will return a JSON containing all of the stories by their starting and ending verses of the Bible. Currently, only in English (?language=english), but more are in active development.

**Headers:**
Content-Type: application/json

**Parameters:**
- `language`: String (e.g., 'english')

*Example request:* 
`GetStories?language=english`

*Example response:*
`[{"id":"1","verse_id":"1001001","story":"God Creates the World","verses_1":"Genesis 1:1","verses_2":"","verses_3":"","verses_4":""}, ... }]`



#### GetStrongs

**Description:**
Returns Strong's in Hebrew or Greek. The [id] in the JSON returned corresponds to the 'H' for Hebrew and 'G' for Greek that Strong's uses to precede the id. Thus, an [id] of 3 in our table is equivalent to Strong's Hebrew H3 as well as Strong's Greek G3. The Strong's needed can be ascertained after using a GetOriginalText call (e.g. 'GetOriginalText?verseId=01001001') wherein we can receive back the Strong's Ids for any word in Hebrew or Greek respectively.

**Headers:**
Content-Type: application/json

**Parameters:**
- `lexiconId`: String (e.g., 'H')
- `id`: Integer

*Example request:* 
`GetStrongs?lexiconId=H&id=21`

*Example response:*
`[{"id":"21","strongs_id":"H21","language":"H","word":"\u05d0\u05b2\u05d1\u05b4\u05d9","glossary":"Abiy (ab-ee') n\/p.\n1. fatherly\n2. Abi, Hezekiah's mother\n[from H1]\nKJV: Abi. \nRoot(s): H1 ","occurences":"1","first_occurence":"12018002","root":"\u05d0\u05d1", ... }]`



#### GetWordsOfJesus

**Description:**
This endpoint will return an array of all the verses ('verseIds') wherein Jesus spoke - just as you would find in a 'red letter edition' bible. This endpoint does not take any parameters. Those verses can then be called via the 'GetVerse' endpoint.

**Headers:**
Content-Type: application/json

**Parameters:**
- N/A

*Example request:* 
`GetWordsOfJesus`

*Example response:*
`{"[0]":"40003015","[1]":"40004004","[2]":"40004007","[3]":"40004010","[4]":"40004017","[5]":"40004019","[6]":"40005003","[7]":"40005004", ... }`



### Search

#### GetSearch

**Description:**
Will return all the results of the query entered. For example, 'GetSearch?query=Jesus&versionId=kjv ' will return all the results of the Bible wherein 'Jesus' is found. This is a basic search request, for advanced search needs, see 'GetSearchAdvanced'.

**Headers:**
Content-Type: application/json

**Parameters:**
- `query`: String (e.g., 'jesus')
- `versionId`: String (e.g., 'kjv')

*Example request:*
`GetSearch?query=jesus&versionId=kjv`

*Example response:*
`[{"id":"40001001","b":"40","c":"1","v":"1","t":"The book of the generation of Jesus Christ, the son of David, the son of Abraham."},{"id":"40001016","b":"40","c":"1","v":"16","t":"And Jacob begat Joseph the husband of Mary, of whom was born Jesus, ... }]`



#### GetSearchAdvanced

**Description:**
Advanced Search is a more powerful search than the 'GetSearch' GET request. For example, 'GetSearchAdvanced?query=David&versionId=kjv&matchType=exact&excludeString=urias&limitToBookId=40&limitToChapterId=1' will search for 'David' in the KJV and exclude any mention of 'Urias' within the book of Matthew ('limitToBookId=40') and within chapter one (1) ('limitToChapterId=1') only.

When using the 'limitToBookId' parameter, you can also specify 'ot' or 'nt' instead of the numeric 'bookId' to limit the search to the Old or New Testament. Note that nothing should follow the 'limitToBookId' when using 'ot' or 'nt' values.

The 'matchType' parameter can be set to 'broad' or 'exact'. A broad setting will include any general matches, for example, 'GetSearchAdvanced?query=da&versionId=kjv&matchType=broad' will return all instances wherein the string, 'da', is found. So, a verse containing, 'darkness' would be considered correct. However, if we limit the search to exact with the 'matchType' parameter, only an exact match would return. For example, 'GetSearchAdvanced?query=da&versionId=kjv&matchType=exact' would return empty as the word 'da' does not exist.

Note: Queries must be a minimum of two (2) characters.

**Headers:**
Content-Type: application/json

**Parameters:**
- `query`: String
- `versionId`: String (e.g., 'kjv')
- `matchType`: String (e.g., 'exact')
- `excludeString`: String
- `limitToBookId`: Integer
- `limitToChapterId`: Integer

*Example request:*
`GetSearchAdvanced?query=David&versionId=kjv&matchType=exact&excludeString=urias&limitToBookId=40&limitToChapterId=1`

*Example response:*
`[{"id":"40001001","b":"40","c":"1","v":"1","t":"The book of the generation of Jesus Christ, the son of David, the son of Abraham."},{"id":"40001017","b":"40","c":"1","v":"17","t":"So all the generations from Abraham to David are fourteen generations; and from David until the carrying away into Babylon are fourteen generations; and from the carrying away into Babylon unto Christ are fourteen generations."},{"id":"40001020","b":"40","c":"1","v":"20","t":"But while he thought on these things, behold, the angel of the LORD appeared unto him in a dream, saying, Joseph, thou son of David, fear not to take unto thee Mary thy wife: for that which is conceived in her is of the Holy Ghost."}]`



### Random

#### GetRandomChapter

**Description:**
Returns a random chapter in its entirety. 

**Headers:**
Content-Type: application/json

**Parameters:**
- `versionId`: String (e.g., 'kjv')

*Example request:* 
`GetRandomChapter?versionId=kjv`

*Example response:*
`[{"id":"07017001","b":"7","c":"17","v":"1","t":"And there was a man of mount Ephraim, whose name was Micah."}, ... }]`



#### GetRandomVerse

**Description:**
Returns a random verse from the Bible (Old Testament or New Testament). The 'GetRandomVerse' has been newly updated and now features advanced filter parameters, such as limiting the random verse to the New or Old Testament, limiting the random verse to a specific book, or a specific book and chapter. 

For example, 'GetRandomVerse?versionId=kjv&limitToBookId=20&limitToChapterId=1' will limit to the book of Proverbs, chapter '1'. Or, 'GetRandomVerse?versionId=kjv&limitToBookId=20' will limit to the book of Proverbs, any chapter. If no parameters are added after 'versionId', a random verse from the entire Bible will be requested.

You can also limit the return to a specific query. For example, 'GetRandomVerse?versionId=kjv&limitToBookId=nt&limitToQuery=faith' will limit the randomized return to a verse wherein the word 'faith' is found. This is a useful option for targeting specific subjects while still maintaining randomization.

**Headers:**
Content-Type: application/json

**Parameters:**
- `versionId`: String (e.g., 'kjv')
- `limitToBookId`: Integer
- `limitToChapterId`: Integer
- `limitToQuery`: String (e.g., 'faith')

*Example request:* 
`GetRandomVerse?versionId=kjv&limitToBookId=20&limitToChapterId=1`

*Example response:*
`[{"id":"20001015","b":"20","c":"1","v":"15","t":"My son, walk not thou in the way with them; refrain thy foot from their path:"}]`



### Verses

#### GetParallelVerses

**Description:**
Returns the verse according to the 'verseId' sent in all of the versions available.

**Headers:**
Content-Type: application/json

**Parameters:**
- `verseId`: String (e.g., '01001001')

*Example request:* 
`GetParallelVerses?verseId=01001001`

*Example response:*
`[[{"id":"01001001","b":"1","c":"1","v":"1","t":"In the beginning God created the heavens and the earth.","versionId":"t_asv","versionAbbreviation":"ASV","versionName":"American Standard \r\n - 1901"}], ... }]]`



#### GetVerse

**Description:**
Will return a single verse from the Bible using the 'verseId', which is composed of eight (8) digits. The first two (2) are the book number, the second three (3) are the chapter number, and the last three (3) are the verse number.

**Headers:**
Content-Type: application/json

**Parameters:**
- `verseId`: String (e.g., '01001001')
- `versionId`: String (e.g., 'kjv')

*Example request:* 
`GetVerse?verseId=01001001&versionId=kjv`

*Example response:*
`[{"id":"01001001","b":"1","c":"1","v":"1","t":"In the beginning God created the heaven and the earth."}]`



### Versions

#### GetVersions

**Description:**
Will return an array of the bible versions available in the API.

*Because Bible versions are standardized across the world, we do not attempt to translate versions programmatically.*

1. *English versions: ASV (American Standard 1901), BBE (Bible in Basic English), DBY (Darby English Bible), KJV (King James Version), KJV1611 (King James Version 1611), WBT (Webster Bible), WEB (World English Bible), YLT (Young Literal Translation)*

2. *Spanish version: RV1909 (Reina-Valera 1909)*

3. *Arabic version: SVD (Smith-Van Dyke)*

**Headers:**
Content-Type: application/json

**Parameters:**
- N/A

*Example request:* 
`GetVersions`

*Example response:*
`[{"table":"t_asv","version_id":"1","abbreviation":"ASV","version":"American Standard \r\n - 1901","language":"english"},{"table":"t_bbe", ... }]`



### Extra Biblical

#### GetBooksExtraBiblical

**Description:**
This endpoint retrieves a list of all the extra-biblical books available in our database. 

These works are not part of the standard biblical canon but are valued for their historical and religious insights. They include various genres like prophecies, wisdom literature, and additional narratives that offer a broader understanding of ancient religious and cultural contexts. While not considered canonical in many religious traditions, they are often studied for their historical significance and their influence on religious thought and tradition.

These books are: 
- 1st Enoch ("Ethiopic" Book of Enoch)
- 2nd Enoch ("Slavonic" "Secrets of Enoch")
- 3nd Enoch ("Hebrew" Book of Enoch)
- 1st and 2nd Book of Adam and Eve
- Jashar
- Jubilees
- Tobit
- Judith
- Esther (Greek)
- Wisdom
- Ecclesiasticus
- Song of Three
- Susanna
- Bel and the Dragon
- 1 Maccabees
- 2 Maccabees
- 1 Esdras
- 2 Esdras
- Manasseh
- 1 Hermas
- 2 Hermas
- 3 Hermas
- Testament of Solomon
- Apocalypse of Peter
- Testaments of the Twelve Patriarchs
- The Ascension of Isaiah
- The Apocalipse of Sedrach
- 1 Baruch
- 2 Baruch
- 3 Baruch
- 4 Baruch

The chapters for any book can be retrieved using the 'GetChapterExtraBiblical' endpoint with the 'bookId' and 'chapterId' parameters.

Only English is currently supported.

Also see:
GetChapterExtraBiblical

**Headers:**
Content-Type: application/json

**Parameters:**
- N/A

*Example request:* 
`GetBooksExtraBiblical`

*Example response:*
`[{"id":"1","weight":"1","name":"1 Enoch","info_text":"","info_url":"https:\/\/en.wikipedia.org\/wiki\/Book_of_Enoch","path":"1-enoch\/1-enoch"}...}]`



#### GetChapterExtraBiblical

**Description:**
This endpoint provides access to individual chapters from a wide range of extra-biblical texts. These texts, not included in the standard biblical canon, encompass a diverse array of writings such as apocryphal, deuterocanonical, and other ancient religious documents. They offer valuable insights into religious thought, history, and cultural practices from periods not covered in traditional biblical narratives. To retrieve a specific chapter, two parameters are required: 'bookId' and 'chapterId'.

For instance, a request like 'GetChapterExtraBiblical?bookId=01&chapterId=02' fetches the second chapter of the extra-Biblical book of 1 Enoch.

Also see:
GetBooksExtraBiblical

**Headers:**
Content-Type: application/json

**Parameters:**
- `bookId`: Integer
- `chapterId`: Integer

*Example request:* 
`GetChapterExtraBiblical?bookId=01&chapterId=03`

*Example response:*
`[{"b":"1","c":"3","cs":"3","v":"1","vs":"1","t":"Observe and see how (in the winter) all the trees \u2308\u2308seem as though they had withered and shed all their leaves, except fourteen trees, which do not lose their foliage but retain the old foliage from two to three years till the new comes.\n\n"}]`



#### GetBookNameExtraBiblicalByBookId

**Description:**  
Returns the name of an extra-biblical book per the number sent in the request. For example, sending a request to 'GetBookNameExtraBiblicalByBookId?bookId=01' would return the name of the first extra-biblical book in the configured list.

**Headers:**  
Content-Type: application/json

**Parameters:**  
- `bookId`: Integer or String. The ID of the extra-biblical book whose name is to be returned.

*Example request:*  
`GetBookNameExtraBiblicalByBookId?bookId=01`

*Example response:*  
`[{"n":"1 Enoch"}]`



#### GetChapterCountExtraBiblical

**Description:**  
Returns the number of chapters in any extra-biblical book requested via the 'bookId' parameter. This endpoint specifically caters to the collection of extra-biblical texts. For example, a request like 'GetChapterCountExtraBiblical?bookId=1' would return the total number of chapters (107) in the extra-biblical book of 1 Enoch.

**Headers:**  
Content-Type: application/json

**Parameters:**  
- `bookId`: Integer. The ID of the extra-biblical book whose chapter count is being requested.

*Example request:*  
`GetChapterCountExtraBiblical?bookId=1`

*Example response:*  
`{"chapterCount":107}`



### Topics

#### GetTopic

**Description:**
This endpoint will return JSON containing all of the citations (e.g., 1 Corinthians 13:4-8) as well as the verseIds (if more than one, they will be arrayed) that apply to the value of the topic parameter entered (e.g., 'love').

Also see:
GetTopicVerseCount
GetTopics

**Headers:**
Content-Type: application/json

**Parameters:**
- `topic`: String (e.g., 'love')

*Example request:* 
`GetTopic?topic=love`

*Example response:*
`[{"citation":"1 Corinthians 13:4-8","verseIds":["46013004","46013005","46013006","46013007","46013008"]},{"citation":"1 Corinthians 16:14", ...}]`



#### GetTopics

**Description:**
This endpoint will return the JSON array for all of the available topics that can be used with the 'GetTopic' endpoint, q.v.

Also see:
GetTopic
GetTopicVerseCount

**Headers:**
Content-Type: application/json

**Parameters:**
- N/A

*Example request:* 
`GetTopics`

*Example response:*
`["addiction","adultery","afterlife","alcohol","animals","anger","angels","anxiety","baptism","birth","business","charity","children", ...]`

<br/>

---