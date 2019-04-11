# Direct Publisher Feed (JSON)

### Overview


This specification describes the feed required for your channel\'s
content catalog. A catalog is the set of movies, series, TV specials,
short-form videos or linear stream your channel provides, along with the
artwork that represents it in the UI. A catalog feed includes
the metadata that describes each asset so Roku can index it, identify
its streaming URL, categorize it, and present it to users.

This feed spec adheres to JSON-Schema Draft 4. All the properties in the
schema are case sensitive. 

Before submitting a feed, make sure it is a valid JSON file. You can do
that by using an IDE or free online tools, such as [JSON Schema
Validator](https://www.jsonschemavalidator.net/) or [JSON Schema
Lint](https://jsonschemalint.com).

For guidelines about presenting your channel, see [Content
Specification](https://developer.roku.com/publish/content-specifications/content-spec-index).

**Sections:**
* [Direct Publisher Feed Schema](#direct-publisher-feed-schema)
* [Artwork for All Content](#artwork-for-all-content)

**Content types:**(#content-types)
* [movie](#movie-content-type)
* [live feed](#liveFeed-content-type)
* [series](#series-content-type)
 * [season](#season-content-type)
 * [episode](#episode-content-type)
* [shortFormVideo](#shortformvideo-content-type)
* [tvSpecial](#tvspecial-content-type)

**Content categorization:**
* [category](#category)
* [playlist](#playlist)

**Content properties:**
* [content](#content)
 * [video](#video)
 * [caption](#caption)
 * [trickPlayFile](#trickplayfile)
* [genres](#genres)
* [externalId](#externalid)
* [rating](#rating)
 * [Parental Ratings](#parental-ratings)
 * [Rating Sources](#rating-sources)
* [credit](#credit)

---

## Direct Publisher Feed Schema
These are the properties for the root object of your feed. It contains basic information such as your company's name, when the feed was last updated, and other objects that will describe all your content such as TV Shows, Movies, etc.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| providerName | string | Required | The name of the feed provider. E.g.: “Acme Productions”.
| lastUpdated | string | Required | The date that the feed was last modified in the [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm) format: {YYYY}-{MM}-{DD}T{hh}:{mm}:{ss}+{TZ}. E.g.: 2015-11-11T22:21:37+00:00
| language | string | Required | The language the channel uses for all its information and descriptions. (e.g., “en”, “en-US”, “es”, etc.). ISO 639 alpha-2 or alpha-3 language code string.
| |
| movies | [Movie Object](#movie) | Required* | A list of one or more movies.
| series | [Series Object](#series) | Required* | A list of one or more series. Series are episodic in nature and would include TV shows, daily/weekly shows, etc.
| shortFormVideos | [ShortFormVideo Object](#shortformvideo) | Required* | A list of one or more short-form videos. Short-form videos are usually less than 20 minutes long and are not TV Shows or Movies.
| tvSpecials | [TV Special Object](#tvspecial) | Required* | A list of one or more TV Specials. TV Specials are one-time TV programs that are not part of a series.
| |
| categories | [Category Object](#category) | Optional | An ordered list of one or more categories that will show up in your Roku Channel. Categories may also be manually specified within Direct Publisher if you do not want to provide them directly in the feed. Each time the feed is updated it will refresh the categories.
| playlists | [Playlist Object](#playlist) | Optional | A list of one or more playlists. They are useful for creating manually ordered categories inside your channel.

> :information_source: *At least one of these content types is required

Direct Publisher Feed Root Object Example:

```json
{
    "providerName": "Acme Productions",
    "lastUpdated": "2015-11-11T22:21:37+00:00",
    "language": "en",
    "categories": [
        ...
    ],
    "playlists": [
        ...
    ],
    "movies": [
        ...
    ],
    "series": [
        ...
    ],
    "shortFormVideos":  [
        ...
    ],
    "tvSpecials": [
        ...
    ]
}
```
------------------------------------------------------------------------
## Artwork for All Content

For each piece of content Roku supports three types of artwork: key art, background art, and box cover. Each image type will be used in a different location within the Roku Channel.

Each image must be delivered in the JPEG format (.jpg file extension). 

Roku prefers to receive all art sizes whenever possible. All three image types are preferred. At a minimum, for a movie and a series Roku requires key art or box cover. 

Image Type|Description|Resolution|Aspect Ratio|Required/Optional
-----|-----|-----|-----|-----
key art|Image with title treatment|1920x1080|16:09|Movie: Required<br/>Series: Required
background|Textless image used in background of program page|1920x1080|16:09|Movie: Optional<br/>Series: Optional
box cover|Vertical poster image with title treatment|2000x3000|2:03|Movie: Optional<br/> Series: Optional

### Artwork Content Guidelines

- Key art (graphic with the full title of the asset visible)
- No sexually explicit or graphically violent artwork
- Artwork should be post-theatrical and not include language such as "In Theaters Now" or "Coming Soon"
- Artwork designed specifically for digital delivery is preferred
- Artwork for international territories should be localized for each territory

Object Example:
```xml
thumbnail: "http://thumbnail.jpg",
thumbnailBoxcover: "http://thumbnailBoxcover.jpg",
backgroundImage: "http://backgroundImage.jpg"
```
# Content Types

## movie Content Type
Child object of root property `movies`.

This object represents a movie object.

### **Movie Ad Guidelines**

- No adBreaks should be listed during the first 10 minutes of playback
  - No pre-roll
- Noted adBreaks should occur during natural scene breaks
- 10 mins or more between each adBreak
- No adBreaks within 8 minutes of end credits.

Field|Type|Required|Description
-----|-----|-----|-----
id|string|Required|Your immutable string reference ID for the movie. THIS CANNOT CHANGE. This should serve as a unique identifier for the movie across different locales.<br>**Note**: The ID for an asset must not exceed 50 characters.
title|string|Required|Movie title. Roku uses this value for matching in Roku Search. Please use plain text and don’t include extra information like year, version label, etc.
content|[Content Object](#content)|Required|The actual video content, such as the URL of the video file, subtitles, etc.
genres|string|Required|The genre(s) of the movie. Must be one of the values listed in genres.
thumbnail|string|Required|The URL of the thumbnail for the movie. This is used within your channel and in search results. Image dimensions must be at least 800x450 (width x height, 16x9 aspect ratio).<br/>Landscape key art and portrait box cover graphics with the full title of the asset visible in a JPEG file. Image dimensions should be 1920x1080 preferred (width x height, 16x9 aspect ratio) for landscape key art and 532px by 800px preferred (width x height, 2x3 aspect ratio) for portrait box cover.
releaseDate|string|Required|The date the movie was initially released or first aired. Used to sort programs chronologically and grouping related content in Roku Search. Conforms to the [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm) format: {YYYY}-{MM}-{DD}. E.g.: 2015-11-11
shortDescription|string|Required|A movie description that does not exceed 200 characters. The text will be clipped if longer.
longDescription|string|Required|A longer movie description that does not exceed 500 characters. The text will be clipped if longer. Must be different from shortDescription.
tags|string|Optional|One or more tags (e.g., “dramas”, “korean”, etc.). Each tag is a string and is limited to 20 characters. Tags are used to define what content will be shown within a [category](#category).
credits|[Credit Object](#credit)|Optional|One or more credits. The cast and crew of the movie.
rating|[Rating Object](#rating)|Required|A parental rating for the content.
externalIds|[External ID Object](|Optional|One or more third-party metadata provider IDs.

### Kids Directed Content

You may NOT submit or distribute content that is directed to children
unless you have Roku's express written approval. 

If Roku has provided such approval then the following additional
obligations apply:

Field|Type|Required|Description
-----|-----|-----|-----
tags|string|Required|Include a "kidsdirected" tag into the tags string *all lowercase*
rating|[Rating Object](#rating)|Required|A parental rating for the content. (MPAA or TV Rating).<br/>- 'Unrated' and "Not Rated" are not accepted ratings for kids directed content

Movie Object Example:

```json
{  
   "id":"1509428502952",
   "title":"Sample Movie",
   "content":{  
      ...
   },
   "genres":[  
      "drama",
      "comedy",
      "horror"
   ],
   "thumbnail":"https://example.org/cdn/thumbnails/1509428502952/1",
   "releaseDate":"2016-01-01",
   "shortDescription":"Incredible movie description",
   "longDescription":"Even more incredible and longer movie description",
   "tags":[  
      "amazing",
      "drama",
      "comedy",
      "horror"
   ]
}
```

---

## liveFeed Content Type

Child object of root property liveFeeds. 

This object represents a live linear stream. Please contact your Roku representative if you want to create a channel that streams live linear content.

Field|Type|Required|Description
-----|-----|-----|-----
id|string|Required|Your immutable string reference ID for the live linear stream. THIS CANNOT CHANGE. This should serve as a unique identifier for the live stream across different locales.<br/> **Note**: The ID for an asset must not exceed 50 characters.
title|string|Required|The live stream's title. Roku uses this value for matching in Roku Search. Please don’t include extra information like year, version label, etc.
content|[Content Object](#content)|Required|The stream's actual live HLS stream, in the form of a URL. 
thumbnail|string|Required|The URL of the primary thumbnail for the live stream. This is used within your channel and in search results. Image dimensions must be at least 800x450 (width x height, 16x9 aspect ratio).<br/>Landscape key art and portrait box cover graphics with the full title of the asset visible in a JPEG file. Image dimensions must be 1920x1080 (width x height, 16x9 aspect ratio) for landscape key art and 532px by 800px (width x height, 2x3 aspect ratio) for portrait box cover.
brandedThumbnail|string|Required|The URL of the secondary thumbnail for the live stream. This is used as a backup in the event that the primary image is not suitable. Image dimensions must be at least 800x450 (width x height, 16x9 aspect ratio). <br/>Landscape key art and portrait box cover graphics with the full title of the asset visible in a JPEG file. Image dimensions must be 1920x1080 (width x height, 16x9 aspect ratio) for landscape key art and 532px by 800px (width x height, 2x3 aspect ratio) for portrait box cover.
shortDescription|string|Required|A live stream description that does not exceed 200 characters. The text will be clipped if longer.
longDescription|string|Required|A longer live stream description that does not exceed 500 characters. The text will be clipped if longer. Must be different from shortDescription.
tags|string|Optional|One or more tags (e.g., “dramas”, “korean”, etc.). Each tag is a string and is limited to 20 characters. Tags are used to define what content will be shown within a category and to find content for content curation purposes.
validityPeriodStart|string|Optional|The date when the content should become available in the [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm)  format: {YYYY}-{MM}-{DD}T{hh}:{mm}:{ss}+{TZ}. E.g.: 2018-11-11T22:21:37+00:00
validityPeriodEnd|string|Optional|The date when the content is no longer available in the [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm)  format: {YYYY}-{MM}-{DD}T{hh}:{mm}:{ss}+{TZ}. E.g.: 2018-11-11T22:21:37+00:00
rating|Rating Object](#rating)|Optional|A parental rating for the content.
genres|string|Optional|The genre(s) of the event. Must be one of the values listed in genres.

liveFeed Object Example:

```json
{  
   "id":"1509428502953",
   "title":"Sample Stream",
   "content":{  
      ...
   },
   "language":{  
      "en"
   },
   "thumbnail":"https://example.org/cdn/thumbnails/1509428502952/1",
   "brandedThumbnail":"https://example.org/cdn/thumbnails/1509428502952/1",
   "shortDescription":"A live description",
   "longDescription":"A longer description of a live stream",
   "tags":[  
      "linear",
      "news",
      "celebrity"
   ]
}
```
Live Feed Stream Rendition Requirements:

Target| | 
-----|-----|-----
Rendition|Resolution|Bitrate
1|384 x 216|230 Kbps
2|384 x 216|440 Kbps
3|512 x 288|700 Kbps
4|720 x 404|1.2 Mbps
5|1280 x 720|2.5 Mbps
6|1920 x 1080|5 Mbps

Category|Required/ Recommended|Requirement
-----|-----|-----
Bitrate Ladder|Required|Minimum five different renditions 
Resolution|Required|Minimum three different resolutions 
Aspect Ratio|Required|Resolutions must remain the same across bit rates when switching / adapting
Segment Duration|Required|6 seconds each
Segment Live Window|Required|6 segments
Codec Standard|Required|H.264, Main Profile @ level 4.1 or lower
Audio Codec Standard|Required |AAC Two Channel Stereo
Audio Bitrate|Required|*Set the audio sampling at a single rate regardless of bitrate. The recommended rate is 48KHz.*
Failover / Backup Server|Recommended |A secondary / backup server for each bitrate

### Live Feed Ad Guidelines
- Partners must accept Roku ad tags, which are VAST/VMAP 2.0 and higher compliant
- Partners should not serve any ads in the channel other than the ones returned by the Roku tag, unless explicitly agreed in the contract.
- Partners should adhere to the below ad length requirements:
  - Maximum 8 minutes of ads per hour
  - Maximum 2 minutes pod length
  - No more than 4 ads per pod
  - Flexibility around the ad policy is provided during live sporting events. Please discuss this with your Roku rep.

#### Passing Ad Parameters

Roku requires the following parameters be sent in every SSAI ad request. Some of them are sent in the initial stream request as URL parameters but others needs to be extracted from the stream. Example URL with parameters:

http://<stream-url>?rdid=deviceid1234&is_lat=1
    
Parameter|Description|URL Key|Provided in the initial stream request?
-----|-----|-----|-----
Client IP|IP address of the client|ip|Yes, should be available in the request header
Device ID|ID of the device|rdid|yes
COPPA Flag|Flag to indicate children content|coppa|yes, but should be replaced with the information from the stream, if available
Limit Ad Tracking|Flag to limit ad tracking|Is\_lat|yes
User Agent|User Agent|ua|yes
Content ID|ID of the content being streamed|content|no
Genre|Comma separated value of the genre of the content|genre|yes, but should be replaced with the information from the stream, if available
Platform Type|Type of platform (Roku, Web, Samsung etc.)|platform|yes
Content Type|Type of content (Movies, Series etc.)|content\_type|yes, but should be replaced with the information from the stream, if available
Content Rating|Rating of the content (R, PG etc.)|content\_rating|yes, but should be replaced with the information from the stream, if available 

*\*\*Do not include pixels, third-party tags, or Software Development Kits of any kind without express prior written approval and certification by Roku.*

### Kids Directed Content

You may NOT submit or distribute content that is directed to children
unless you have Roku's express written approval. 

If Roku has provided such approval then the following additional
obligations apply:

Field|Type|Required|Description
-----|-----|-----|-----
tags|string|Required|Include a "kidsdirected" tag into the tags string *all lowercase*
rating|Rating Object|Required|A parental rating for the content. (MPAA or TV Rating).<br/>- 'Unrated' and "Not Rated" are not accepted ratings for kids directed content

---

## series Content Type
Child object of root property `series`.

This object represents a series, such as a season of a TV Show or a mini-series.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| id | string | Required | Your immutable string reference ID for the series. THIS CANNOT CHANGE. This should serve as a unique identifier for the movie across different locales.<br/>**Note**: The ID for an asset must not exceed 50 characters.
| title | string | Required | The title of the series. We use this field for matching in Roku Search.
| |
| seasons | [Season Object](#season) | Required* | One or more seasons of the series. Seasons should be used if episodes are grouped by seasons.
| episodes | [Episode Object](#episode) | Required* | One or more episodes of the series. Episodes should be used if they are not grouped by seasons (e.g., a mini-series).
| |
| genres | string | Required | The genre(s) of the series. Must be one of the values listed in [Genres](#genres).
| thumbnail | string | Required | The URL of the thumbnail for the series. This is used within your channel and in search results. Image dimensions must be at least 800x450 (width x height, 16x9 aspect ratio).<br/>Landscape key art and portrait box cover graphics with the full title of the asset visible in a JPEG file. Image dimensions should be 1920x1080 preferred (width x height, 16x9 aspect ratio) for landscape key art and 532px by 800px preferred (width x height, 2x3 aspect ratio) for portrait box cover.
| releaseDate | string | Required | The date the series first aired. Used to sort programs chronologically and grouping related content in Roku Search. Conforms to the [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm) format: {YYYY}-{MM}-{DD}. E.g.: 2015-11-11
| shortDescription | string | Required | A description of the series that does not exceed 200 characters. The text will be clipped if longer.
| longDescription | string | Optional | A longer movie description that does not exceed 500 characters. The text will be clipped if longer. Must be different from shortDescription.
| tags | string | Optional | One or more tags (e.g., “dramas”, “korean”, etc.). Each tag is a string and is limited to 20 characters. Tags are used to define what content will be shown within a [category](#category).
| credits | [Credit Object](#credit) | Optional | One or more credits. The cast and crew of the series.
| externalIds | [External ID Object](#externalid) | Optional | One or more third-party metadata provider IDs.

> :information_source: *Must have either `seasons` or `episodes`

### Kids Directed Content

You may NOT submit or distribute content that is directed to children
unless you have Roku's express written approval. 

If Roku has provided such approval then the following additional
obligations apply:

Field|Type|Required|Description
-----|-----|-----|-----
tags|string|Required|Include a "kidsdirected" tag into the tags string *all lowercase*
rating|Rating Object|Required|A parental rating for the content. (MPAA or TV Rating).<br/>- 'Unrated' and "Not Rated" are not accepted ratings for kids directed content

Series Object Example (seasons):

```json
{
  "id": "1509428502952",
  "title": "The Amazing Series with Seasons!",
  "seasons": [
    ...
  ],
  "genres": [
    "educational",
    "science fiction",
    "thriller",
  ],
  "thumbnail": "https://example.org/cdn/thumbnails/1509428502952/1",
  "shortDescription": "Wondrous series seasons."
}
```

Series Object Example (mini-series):

```json
{
  "id": "1509428502952",
  "title": "The Amazing Series with Episodes Only!",
  "episodes": [
    ...
  ],
  "genres": [
    "fashion",
    "romance",
    "technology",
  ],
  "thumbnail": "https://example.org/cdn/thumbnails/1509428502952/1",
  "shortDescription": "Unbelievables series episodes."
}
```

---

## season Content Type
Child object of property `series` -> `seasons`.

This object represents a single season of a series.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| seasonNumber | integer | Required | Sequential season number. E.g.: 3 or 2015.
| episodes | [Episode Object](#episode) | Required | One or more episodes of this particular season.
| thumbnail | string | Optional | The URL of the thumbnail for the season. Image dimensions must be at least 800x450 (width x height, 16x9 aspect ratio).

Season Object Example:

```json
{
  "seasonNumber": "1",
  "episodes": [
    ...
  ]
}
```

---

### episode
Child object of property:

* `series` -> `episodes`
* `series` -> `seasons` -> `episodes`

This object represents a single episode in a series or a season.

### **Episode Ad Guidelines**

_Content length longer than 20 minutes:_

- No adBreaks should be listed during the first 10 minutes of playback
  - No pre-roll
- Noted adBreaks should occur during natural scene breaks
- 10 mins or more between each adBreak
- No adBreaks within 8 minutes of end credits.

_Content length less than 20 minutes:_

- Please leave this filed blank
- Content less than 20 minutes with no adBreaks listed will automatically receive a single pre-roll ad

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| id | string | Required | Your immutable string reference ID for the episode. THIS CANNOT CHANGE. This should serve as a unique identifier for the movie across different locales.<br/>**Note**: The ID for an asset must not exceed 50 characters.
| title | string | Required | Episode title. We use this value for matching in Roku Search. Please don’t include extra information like year, version label, etc.
| content | [Content Object](#content) | Required | The actual video content, such as the URL of the video file, subtitles, etc.
| thumbnail | string | Required | The URL of the thumbnail for the episode. This is used within your channel and in search results. Image dimensions must be at least 800x450 (width x height, 16x9 aspect ratio).<br/>Landscape key art and portrait box cover graphics with the full title of the asset visible in a JPEG file. Image dimensions should be 1920x1080 preferred (width x height, 16x9 aspect ratio) for landscape key art and 532px by 800px preferred (width x height, 2x3 aspect ratio) for portrait box cover.
| episodeNumber | integer | Required | The sequential episode number. E.g.: 3.
| releaseDate | string | Required | The date the episode first aired. Used to sort programs chronologically and grouping related content in Roku Search. Conforms to the [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm) format: {YYYY}-{MM}-{DD}. E.g.: 2015-11-11
| shortDescription | string | Required | An episode description that does not exceed 200 characters. The text will be clipped if longer.
| longDescription | string | Optional | A longer episode description that does not exceed 500 characters. The text will be clipped if longer. Must be different from shortDescription.
| credits | [Credit Object](#credit) | Optional | One or more credits. The cast and crew of the episode.
| rating | [Rating Object](#rating) | Optional | A parental rating for the content.
| externalIds | [External ID Object](#externalid) | Optional | One or more third-party metadata provider IDs.



### Kids Directed Content

You may NOT submit or distribute content that is directed to children
unless you have Roku's express written approval. 

If Roku has provided such approval then the following additional
obligations apply:

Field|Type|Required|Description
-----|-----|-----|-----
tags|string|Required|Include a "kidsdirected" tag into the tags string *all lowercase*
rating|Rating Object|Required|A parental rating for the content. (MPAA or TV Rating).<br/>- 'Unrated' and "Not Rated" are not accepted ratings for kids directed content

Episode Object Example:

```json
{
  "id": "1509428502952",
  "title": "The Amazing First Episode Title",
  "content": {
    ...
  },
  "thumbnail": "https://example.org/cdn/thumbnails/1509428502952/1",
  "episodeNumber": 1,
  "shortDescription": "Marvelous episode description"
}
```

---

## shortFormVideo Content Type

Child object of root property `shortFormVideos`.

Short-form videos are generally less than 20 minutes long, and are not TV Shows or Movies.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| id | string | Required | Your immutable string reference ID for the video. THIS CANNOT CHANGE. This should serve as a unique identifier for the movie across different locales.<br/>**Note**: The ID for an asset must not exceed 50 characters.
| title | string | Required | Video title. We use this value for matching in Roku Search. Please don’t include extra information like year, version label, etc.
| content | [Content Object](#content) | Required | The actual video content, such as the URL of the video file, subtitles, etc.
| thumbnail | string | Required | The URL of the thumbnail for the video. This is used within your channel and in search results. Image dimensions must be at least 800x450 (width x height, 16x9 aspect ratio).<br/>Landscape key art and portrait box cover graphics with the full title of the asset visible in a JPEG file. Image dimensions should be 1920x1080 preferred (width x height, 16x9 aspect ratio) for landscape key art and 532px by 800px preferred (width x height, 2x3 aspect ratio) for portrait box cover.
| shortDescription | string | Required | A description of the video that does not exceed 200 characters. The text will be clipped if longer.
| releaseDate | string | Required | The date the video first became available. Used to sort programs chronologically and grouping related content in Roku Search. Optional but very important, we recommend that you provide this. Conforms to the [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm) format: {YYYY}-{MM}-{DD}. E.g.: 2015-11-11
| longDescription | string | Optional | A longer description that does not exceed 500 characters. The text will be clipped if longer. Must be different from shortDescription.
| tags | string | Optional | One or more tags (e.g., “dramas”, “korean”, etc). Each tag is a string and is limited to 20 characters. Tags are used to define what content will be shown within a [category](#category).
| genres | string | Optional | The genre(s) of the video. Must be one of the values listed in [Genres](#genres).
| credits | [Credit Object](#credit) | Optional | One or more credits. The cast and crew of the video.
| rating | [Rating Object](#rating) | Optional | A parental rating for the content.

 ### **shortForm Ad Guidelines**

- Please leave this filed blank
- Content less than 20 minutes with no adBreaks listed will automatically receive a single pre-roll ad

Short-form Video Object Example:

```json
{
  "id": "1509428502952",
  "title": "The Amazing Short-form Video",
  "content": {
    ...
  },
  "thumbnail": "https://example.org/cdn/thumbnails/1509428502952/1",
  "shortDescription": "Astonishing short-form video",
  "releaseDate": "2016-01-01"
}
```

---

## tvSpecial Content Type
Child object of root property `tvSpecials`.

### **tvSpecial Ad Guidelines**

_Content length longer than 20 minutes:_

- No adBreaks should be listed during the first 10 minutes of playback
  - No pre-roll
- Noted adBreaks should occur during natural scene breaks
- 10 mins or more between each adBreak
- No adBreaks within 8 minutes of end credits.

_Content length less than 20 minutes:_

- Please leave this filed blank
- Content less than 20 minutes with no adBreaks listed will automatically receive a single pre-roll ad

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| id | string | Required | Your immutable string reference ID for the TV Special. THIS CANNOT CHANGE. This should serve as a unique identifier for the movie across different locales.<br/>**Note**: The ID for an asset must not exceed 50 characters.| title | string | Required | Episode title. We use this value for matching in Roku Search. Please don’t include extra information like year, version label, etc.
| content | [Content Object](#content) | Required | The actual video content, such as the URL of the video file, subtitles, etc.
| thumbnail | string | Required | The URL of the thumbnail for the TV Special. This is used within your channel and in search results. Image dimensions must be at least 800x450 (width x height, 16x9 aspect ratio).<br/>Landscape key art and portrait box cover graphics with the full title of the asset visible in a JPEG file. Image dimensions should be 1920x1080 preferred (width x height, 16x9 aspect ratio) for landscape key art and 532px by 800px preferred (width x height, 2x3 aspect ratio) for portrait box cover.| genres | string | Required | The genre(s) of the movie. Must be one of the values listed in [Genres](#genres).
| releaseDate | string | Required | The date the TV Special first aired. Used to sort programs chronologically and grouping related content in Roku Search. Conforms to the [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm) format: {YYYY}-{MM}-{DD}. E.g.: 2015-11-11
| shortDescription | string | Required | A description of the special that does not exceed 200 characters. The text will be clipped if longer.
| longDescription | string | Optional | A longer episode description that does not exceed 500 characters. The text will be clipped if longer. Must be different from shortDescription.
| credits | [Credit Object](#credit) | Optional | One or more credits. The cast and crew of the TV special.
| rating | [Rating Object](#rating) | Optional | A parental rating for the content.
| tags | string | Optional | One or more tags (e.g., “dramas”, “korean”, etc). Each tag is a string and is limited to 20 characters. Tags are used to define what content will be shown within a [category](#category).
| externalIds | [External ID Object](#externalid) | Optional | One or more third-party metadata provider IDs.

TV Special Object Example:

```json
{
  "id": "1509428502952",
  "title": "The Amazing First Episode Title",
  "content": {
    ...
  },
  "genres": [
    "animals",
    "animated",
    "fantasy",
  ],
  "thumbnail": "https://example.org/cdn/thumbnails/1509428502952/1",
  "shortDescription": "Unusual episode description"
}
```

---

## category
Child object of root property `categories`.

The category object defines a new category your channel will display, and the content included in it based either on a playlist (see object description below), or a query containing one or multiple tags. You can also create them directly in Direct Publisher.

There are three default categories in every channel: "Continue Watching", "Most Popular", and "Recently Added".

Each category is displayed as a separate row to end-users.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| name | string | Required | The name of the category that will show up in the channel.
| |
| playlistName | string | Required* | The name of the playlist in this feed that contains the content for this category.
| query | string | Required* | The query that will specify the content for this category. It is a Boolean expression containing tags that you have provided in your content feed. The available operators are: <ul><li>AND</li><li>OR</li></ul>You cannot use both of them in the same query. You can use more than one. For example, if your feed has the tags "romance", "movie", "korean" and "dramas", you could do:<ul><li>movie AND korean</li><li>movie AND korean AND dramas</li><li>romance OR dramas</li></ul>The following is NOT supported:<ul></li><li>movie AND romance OR dramas</li></ul> |
| |
| order | enum | Required | The order of the category. Must be one of the following:<ul><li>manual – For playlists only</li><li>most_recent – reverse chronological order</li><li>chronological – the order in which the content was published (e.g., Episode 1, Episode 2, etc.)</li><li>most_popular – sort by popularity (based on Roku usage data).</li><ul>

> :information_source: Must have either `playlistName` or `query`

Category Object Example (query):

```json
{
    "name": "Cooking Shows",
    "query": "cooking AND reality shows",
    "order": "most_popular"
}
```

Category Object Example (playlist):

```json
{
    "name": "Featured",
    "playlistName": "featured content",
    "order": "manual"
}
```

---

## playlist
Child object of root property `playlists`.

A playlist is an **ordered** list of videos that may contain a mix of [Movies](#movie), [Series](#series), [Short-form videos](#shortformvideo), and [TV Specials](#tvspecial). It references a list of video IDs that are defined elsewhere in the feed. The same video can be referenced in multiple playlists.

Playlists are similar to tags: they help you define the content which your channel's categories will display. The main difference is that playlists let you manually specify the order of the content, and so they are perfect, for example, to create a "Featured" category in your channel.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| name | string | Required | The name of the playlist. The name is limited to 20 characters.
| itemIds | string | Required | An ordered list of one or more item IDs. An item ID is the ID of a movie/series/short-form video/TV special.

Playlist Object Example:

```json
{
    "name": "featured content",
    "itemIds": [
        "1509428502952",
        "1509428502953",
        "1509428502954"
    ]
}
```

---

## content
Child object of property:

* `movie`
* `series` -> `episodes` -> `episode`
* `series` -> `seasons` -> `episodes` -> `episode`
* `shortFormVideo`
* `tvSpecial`

This object represents the details about a single video content of a movie, episode, short-form video, or TV special.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| dateAdded | string | Required | The date the video was added to the library in the [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm) format: {YYYY}-{MM}-{DD}T{hh}:{mm}:{ss}+{TZ}. E.g.: 2015-11-11T22:21:37+00:00 This information is used to generate the “Recently Added” category.
| videos | [Video Object](#video) | Required | One or more video files. For non-adaptive streams, you can specify the same video with different qualities so the Roku player can choose the best one based on bandwidth.
| duration | integer | Required | Runtime in seconds.
| captions | [Caption Object](#caption) | Optional | Supported formats are described in [Closed Caption / Subtitle Support](https://sdkdocs.roku.com/display/sdkdoc/Closed+Caption+Support).
| trickPlayFiles | [Trickplay File Object](#trickplayfile) | Optional | The trickplay file(s) that displays images as a user scrubs through a video, in Roku’s BIF format. Trickplay files in multiple qualities can be provided.
| language | string | Optional | The language in which the video was originally produced (e.g., “en”, “en-US”, “es”, etc). ISO 639 alpha-2 or alpha-3 language code string.
| validityPeriodStart | string | Optional | The date when the content should become available in the [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm) format: {YYYY}-{MM}-{DD}T{hh}:{mm}:{ss}+{TZ}. E.g.: 2015-11-11T22:21:37+00:00
| validityPeriodEnd | string | Optional | The date when the content is no longer available in the [ISO 8601](http://www.iso.org/iso/home/standards/iso8601.htm) format: {YYYY}-{MM}-{DD}T{hh}:{mm}:{ss}+{TZ}. E.g.: 2015-11-11T22:21:37+00:00
| adBreaks | string | Optional | One or more time codes. Represents a time duration from the beginning of the video where an ad shows up. Conforms to the format: {hh}:{mm}:{ss}.

Content Object Example:

```json
{
  "dateAdded": "2015-11-11T22:21:37+00:00",
  "videos": [
    ...
  ],
  "trickPlayFiles": [
    ...
  ],
  "duration": 1290
}
```

---

### video
Child object of property `content` -> `videos`.

This object represents the details of a single video file.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| url | string | Required | The URL of the video itself. The video should be served from a CDN (Content Distribution Network). Supported formats are described in [Audio and Video Support](https://sdkdocs.roku.com/display/sdkdoc/Audio+and+Video+Support).
| quality | enum | Required | Must be one of the following:<ul><li>HD – 720p</li><li>FHD – 1080p</li><li>UHD – 4K</li></ul>If your stream uses an adaptive bitrate, set the quality to the highest available.
| videoType | enum | Required | Must be one of the following:<ul><li>HLS</li><li>SMOOTH</li><li>DASH</li><li>MP4</li><li>MOV</li><li>M4V</li></ul>
| bitrate | integer | Required only for non-ABR streams. | The bitrate in kbps. For non-adaptive streams, this must be provided. It is not needed for an ABR (e.g., HLS) stream.

Video Object Example:

```json
{
  "url": "https://example.org/cdn/videos/1509428502952",
  "quality": "UHD",
  "videoType": "HLS"
}
```

---

### caption
Child object of property `content` -> `captions`.

This object represents a single video caption file of a video content. The supported formats are described in [Closed Caption / Subtitle Support](https://sdkdocs.roku.com/display/sdkdoc/Closed+Caption+Support).

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| url | string | Required | The URL of the video caption file. Supported formats are described in [Closed Caption / Subtitle Support](https://sdkdocs.roku.com/display/sdkdoc/Closed+Caption+Support).
| language | string | Required | A language code for the subtitle (e.g., “en”, “es-mx”, “fr”, etc). [ISO 639-2 or alpha-3](https://www.loc.gov/standards/iso639-2/php/code_list.php) language code string.
| captionType | enum | Required | A string specifying the type of caption. Default is subtitle. Must be one of the following:<ul><li>CLOSED_CAPTION</li><li>SUBTITLE</li></ul>

Caption File Object Example:

```json
{
  "url": "https://example.org/cdn/subtitles/1509428502952/sub-fr.srt",
  "Language": "fr",
  "captionType": "CLOSED_CAPTION"
}
```

---

### trickPlayFile
Child object of property `content` -> `trickPlayFiles`.

This object represents a single trickplay file. Trickplay files are the images shown when a user scrubs through a video, either fast-forwarding or rewinding. The file must be in the Roku BIF format, as described in [Trick Mode Support](https://sdkdocs.roku.com/display/sdkdoc/Trick+Mode+Support).

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| url | string | Required | The URL to the image representing the trickplay file Must be in the Roku BIF format, more information in the Trick Mode Support article.
| quality | enum | Required | Must be one of the following:<ul><li>HD – 720p</li><li>FHD – 1080p</li></ul>

Trickplay File Object Example:

```json
{
  "url": "https://example.org/cdn/trickplayFiles/1509428502952/1",
  "quality": "FHD"
}
```

---

## Genres

The following genres are supported:

* action
* adventure
* animals
* animated
* anime
* children
* comedy
* crime
* documentary
* drama
* educational
* fantasy
* faith
* food
* fashion
* gaming
* health
* history
* horror
* miniseries
* mystery
* nature
* news
* reality
* romance
* science
* science fiction
* sitcom
* special
* sports
* thriller
* technology

---

## externalId
Child object of property:

* `movie`
* `series`
* `series` -> `episodes` -> `episode`
* `series` -> `seasons` -> `episodes` -> `episode`
* `shortFormVideo`
* `tvSpecial`

This object represents a third-party metadata provider ID (such as TMS, Rovi, IMDB, EIDR), that can provide more information about a specific video content. This information is used to optimize your content to be discovered within the Roku Search, and provide more details to users.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| id | string | Required | The third-party metadata provider ID for your video content. For example, in the case of IMDB you would use the last part of the URL of a movie such as "http://www.imdb.com/title/tt0371724".
| idType | enum | Required | Must be one of the following:<ul><li>TMS – A Tribune Metadata Service ID for the content</li><li>ROVI - A Rovi ID for the content</li><li>IMDB – An Internet Movie Database ID</li><li>EIDR – An Entertainment Identifier Registry ID</li></ul>

External ID Object Example:

```json
{
  "id": "tt0371724",
  "idType": "IMDB"
}
```

---

## rating
Child object of property:

* `movie`
* `series` -> `episodes` -> `episode`
* `shortFormVideo`
* `tvSpecial`

This object represents the rating for the video content. You can define the parental rating, as well as the source (USA Parental Rating, UK Content Provider, etc). See [Parental Ratings](#parental-ratings) and [Rating Sources](#rating-sources) for acceptable values.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| rating | enum | Required | Must be a value listed in [Parental Ratings](#parental-ratings)
| ratingSource | enum | Required | Must be one of the following:<ul><li>BBFC</li><li>CHVRS</li><li>CPR</li><li>MPAA</li><li>UK_CP</li><li>USA_PR</li></ul>See [Rating Sources](#rating-sources) for more information.

Rating Object Example:

```json
{
  "rating": "PG",
  "ratingSource": "USA_PR"
}
```

---

### Parental Ratings

The following parental ratings can be used to better help your viewers find age-appropriate content:

* 12
* 12A
* 14+
* 14A
* 15
* 18
* 18+
* 18A
* A
* AA
* C
* C8
* E
* G
* NC17
* PG
* PG13
* R
* R18
* TV14
* TVG
* TVMA
* TVPG
* TVY
* TVY14
* TVY7
* U
* Uc
* UNRATED

### Rating Sources

These are the accepted values for the `ratingSource` property followed by their meaning:

* BBFC - British Board of Film Classification
* CHVRS - Canadian Home Video Rating System
* CPR - Canadian Parental Rating
* MPAA - Motion Picture Association of America
* UK_CP - UK Content Provider
* USA_PR - USA Parental Rating

---

## credit
Child object of property:

* `movie`
* `series`
* `series` -> `episodes` -> `episode`
* `shortFormVideo`
* `tvSpecial`

This object represents a single person in the credits of a video content.

| Field | Type | Required | Description |
| ----- | ---- | -------- | ----------- |
| name | string | required | name of the person
| role | enum | required | role of the person - must be one of the following values:<br><ul><li>actor</li><li>anchor</li><li>host</li><li>narrator</li><li>voice</li><li>director</li><li>producer</li><li>screenwriter</li></ul>
| birthDate | string | required | birthdate of the person

**Credit Object Example:**

``` json
{
  "name": "Douglas N. Adams",
  "role": "screenwriter",
  "birthDate": "1952-03-11"
}
```

---

## Change Log

* **2017-05-02** - Updated enumerations for `role` within a `credit` object
* **2016-08-18** - Removed UHD as an option from trickPlayFile -> quality.
* **2016-08-11** - Added SD to the qualities available for `video` -> `quality`. Updated `shortFormVideo`, `releaseDate` is now a required property.
* **2016-07-28** - Fixed `playlist` -> `itemIds` property description to not include episodes.
* **2016-07-18** - Updated `thumbnail` property description.
* **2016-07-08** - Updated `shortDescription` limit from 60 to 200 characters. Added `adBreaks` property to `content`.
* **2016-05-17** - Updated objects and properties descriptions, added horizontal lines between objects to improve reading, added a few more examples.
* **2016-04-25** - All properties documented with Name, Type, Required, and Description fields.
* **2016-04-14** - Document created.

## References

* JSON Schema Draft 4: http://json-schema.org/latest/json-schema-core.html
* ISO 8601: http://www.iso.org/iso/home/standards/iso8601.htm
* JSON Schema Validator: http://www.jsonschemavalidator.net/
* JSON Schema Lint: http://jsonschemalint.com/draft4/
