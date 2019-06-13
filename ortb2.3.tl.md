# TripleLift OpenRTB 2.3 Integration Specs

## 0. Overview
TripleLift Native RTB is the largest exchange for DSPs to bid on native advertising impressions. As is standard for ad exchanges, the winning bid will pay the second price. The protocol is based on the native component of the OpenRTB 2.3 specification.

Bidders may submit up to 10 bids total (not per seatbid) in the entire bid response. This means that the only the first 10 bids in lexical parse order across seatbids will be considered. Additional bids will not be processed. Only the highest eligible bid response for a particular bidder will be used (meaning a bidder's losing bids will not be counted for price reduction).

### 0.1. Basic Auction on the TripleLift Exchange
1. TripleLift receives a native ad request from a partner.
2. TripleLift sends a bid request via HTTP POST to all partner bidders http endpoints. Each bidder responds within 100ms.
3. TripleLift holds a second price auction, subject to the publishers price floors and brand constraints.
4. TripleLift creates a native ad through the assets specified in the winning bid, and sends a notification to the bidder's notify endpoint. See [Notifys](notifys.md).
5. The native ad is rendered

## 1. General
### 1.1. Technical
The TripleLift Native RTB exchange is implemented via HTTP (not HTTPS). Connections should be established with HTTP Keep-Alive.

The appropriate mime type is:

```json
Content-Type: application/json
```

The OpenRTB header is:

```json
x-openrtb-version: 2.3
```

### 1.2. Internationalization
It is important that your bidder properly does encoding. This is especially important if you plan on ever submitting bids containing non-Roman or non-English characters. Failure to properly encode will cause the foreign characters to be misinterpreted and rendered improperly on the end-user's page. Additionally, creative and brand audits will likely fail.

Per the JSON specification (https://www.ietf.org/rfc/rfc4627.txt): "JSON text SHALL be encoded in Unicode. The default encoding is UTF-8." For uniformity, consistency, and stability, we strongly advise that you use the default encoding of UTF-8. Most JSON libraries provide UTF-8 support by default.

UTF-8: This response will work assuming 'ü' in München is encoded properly as the UTF-8 hex bytes 'c3 bc'. Although, HTTP/1.1 states that the default charset is ISO-8859-1, the default charset of UTF-8 is used here since this is JSON.

```json
Content-Type: application/json
{"city":"München"}
```

The "\uXXXX" encoding scheme can also be used. This is very explicit and will completely remove any doubts about character encodings, since only simple hex characters will be transmitted across the wire.

```json
Content-Type: application/json
{"city":"M\u00FCnchen"}
```

You may explicitly specify a charset. This is optional if you use UTF-8 since that is the default. However, it is required if you use any charset besides UTF-8.

```json
Content-Type: application/json; charset=utf-8
{"city":"München"}
```

### 1.3. Business
All transactions are in US Dollars. No other currencies are currently supported. 

## 2. Bid Request
The following describes the various attributes that are supported in the TripleLift OpenRTB 2.3 implementation. 

Please note that values marked "Required" will always be sent. "Optional" means they may or may not be sent.

### 2.1. Native Object 
Note: The native object is contained in the imp field as specified below. The request element in the native object, specifying the native request parameters, is an encoded string. Thus, as specified in OpenRTB 2.3, every request will require serial JSON parsing. 

```json
"imp" : "{\"native\" : { ... }}"
```

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|request|Required|string|JSON-encoded Native Request Object.|
|ver|Required|integer|This will always be 1.|
|ext|Optional|object|Optional extension object to carry extra request parameters. See Native Ext Object.|

### 2.2. Native Request Object
#### 2.2.1. Native Parameters
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|ver|Required|integer|This will always be 1.|
|layout|Required|integer|The Layout ID of the native ad unit. See Layout Types.|
|adunit|Required|integer|The Ad unit ID of the native ad unit. See Ad Unit Types.|
|plcmtcnt|Required|integer|This will always be 1.|
|assets|Required|Array of Asset objects|Array of AssetObjects. The bid response must follow the specifications of the Assets array.|

##### 2.2.1.1. Layout Types
|Value|Description|
|--- |--- |
|1|Content Wall|
|2|App Wall|
|3|News Feed|
|4|Chat List|
|5|Carousel|
|6|Content Stream|
|7|Grid adjoining the content|

##### 2.2.1.2. Ad Unit Types
|Value|Description|
|--- |--- |
|1|Paid Search Units|
|2|Recommendation Widgets|
|3|Promoted Listings|
|4|In-Ad (IAB Standard) with Native Element Units|
|5|Custom / "Can't Be Contained"|

#### 2.2.2. Asset Parameters
The asset object will have one of title, img, video, or data.

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|integer|The Asset ID, as assigned by TripleLift. See Bid Request Asset Types.|
|required|Required|integer|Set to 1 if required, 0 if not required|
|title|Optional|Title object|See Title Object|
|img|Optional|Image object|See Image Object|
|video|Optional|Video object|See Video Object|
|data|Optional|Data object|See Data Object|

##### 2.2.2.1. Bid Request Asset Types
|Value|Description (Item - Object)|
|--- |--- |
|1|Title - Title|
|2|Image - Image|
|3|Logo - Image (Optional)|
|4|Caption (Description) - Data|
|5|Advertiser Name - Data|
|6|Video (Optional)|

##### 2.2.2.2. Title Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|len|Required|integer|The maximum length of the text in the title element.|

##### 2.2.2.3. Image Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|type|Required|integer|Type of the image element. See Image Object Types.|
|w|Optional|integer|Width of the image in pixels.|
|wmin|Required|integer|Absolute minimum width of image that will be accepted|
|h|Optional|integer|Height of the image in pixels.|
|hmin|Required|integer|Absolute minimum height of the image that will be accepted|

##### 2.2.2.4. Image Object Types
|Value|Name|Description|Used by TripleLift|
|--- |--- |--- |--- |
|1|Icon|Image image|no|
|2|Logo|Logo for the brand|yes|
|3|Main|Main image used for the ad|yes|

##### 2.2.2.5. Video Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|mimes|Required|array of strings|Content MIME types supported. Can be empty. Flash is not supported currently.|
|minduration|Required|integer|Minimum video ad length|
|maxduration|Required|integer|Maximum video ad length|
|protocols|Required|array of integers|Corresponds to video response Section 5.8 of OpenRTB 2.3. TripleLift supports values 1 to 6.|
|ext|Optional|object|TL custom video options. See Video Ext Object.|

###### 2.2.2.5.1. Video Ext Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|playbackmethod|Optional|array of integers|Corresponds to video response Section 5.9 of OpenRTB 2.3. See Play Back Method.|

###### 2.2.2.5.2 Play Back Method
|Value|Name|Description|Used by TripleLift|
|--- |--- |--- |--- |
|1|Auto-Play Sound On|Autoplay with sound|No|
|2|Auto-Play Sound Off|Autoplay without sound|Yes|
|3|Click-to-Play|Play on click|Yes|
|4|Mouse-Over|Play on hover|No|

##### 2.2.2.6. Data Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|type|Required|integer|Type ID of the object. See Data Asset Types.|
|len|Optional|integer|Maximum length of the text in the element's response.|

###### 2.2.2.6.1. Data Asset Types
|Value|Name|Description|Format|Used by TripleLift|
|--- |--- |--- |--- |--- |
|1|sponsored|Advertiser name (rule of thumb, content you want place here "Sponsored By _______").|text|yes|
|2|desc|Caption for the sponsored content unit.|text|yes|
|3|rating|Rating of the product being offered to the user.|number formatted as a string|no|
|4|likes|Number of social ratings or likes.|number formatted as a string|no|
|5|downloads|Number of downloads / installs of the product|number formatted as a string|no|
|6|price|Price of the product|number formatted as a string|no|
|7|saleprice|Sale price that can be used in conjunction with the price to indicate a discounted price|number formatted as a string|no|
|8|phone|Phone number|number formatted as a string|no|
|9|address|Address|text|no|
|10|desc2|Additional descriptive text|text|no|
|11|displayurl|Display URL for the text ad|text|no|
|12|ctatext|CTA description - descriptive text for a call to action|text|no|

### 2.3. Native Ext Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|triplelift|Optional|object|Custom TripleLift object to carry extra TripleLift request parameters|

#### 2.3.1. TripleLift Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|formats|Optional|array of integers|TripleLift will send signals on which format types are supported by the particular placement<ul><li>1 represents image</li><li>2 represents click-to-play video</li><li>3 represents cinemagraph</li><li>8 represents instant play video</li></ul>|

#### 2.3.2. TripleLift OpenRTB Implementation
##### 2.3.2.1. Top Level Objects
|Field|Scope|Notes|
|--- |--- |--- |
|Bid Request Object|Required|Top-level object.|
|Impression Object|Required|Exactly one impression object will be sent in a bid request object. This contains the native object.|
|Native Object|Required for Native integration|The native implementation uses the Native Object for native impressions.|
|Banner Object|Required for Display integration|The TripleLift "Native Over Banner" implementation uses the Banner Object for native impressions.|
|Video Object|Never|Not used|
|Site Object|Optional|Will be used for website inventory|
|App Object|Optional|Will be used for app inventory|
|Content Object|Never|Not used|
|Device Object|Optional|Used for all requests where we have device data|
|User Object|Required|Will be sent with the TripleLift user data|
|Publisher Object|Required|Will always be sent with TripleLift publisher data|
|Producer Object|Never|Not used|
|Geo Object|Never|Used in the device object|
|Data Object|Never|Not used|
|Segment Object|Never|Not used|
|Regulations Object|Never|Not used|
|PMP Object|Optional|Used as a container for direct deals applicable to this impression|
|Deal Object|Optional|Contains deal terms set up between buyer and TripleLift|
|Extensions|Required|Contains the TripleLift Native Extension|

##### 2.3.2.2. Bid Request Object
Note: badv (blocked advertisers) is not currently supported

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|Id of the bid request|
|imp|Required|array of one object|Single impression object|
|site|Optional|object|See Site Object|
|app|Optional|object|See App Object|
|device|Optional|object|See Device Object|
|user|Required|object|See User Object|
|bcat|Optional|array of strings|The array of categories that are blocked by the publisher. These are IAB top-level categories. ex: IAB18|

##### 2.3.2.3. Impression Object
Note: bidfloor is not currently supported. It may be supported in future versions of the spec. 

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|This will always be "1"|
|banner|Required|object|The TripleLift native implementation uses the Native Object for native impressions.|
|tagid|Required|string|TripleLift placement identifier|
|secure|Optional|integer|Flag to indicate if the impression comes from a secure site, where 0 = non-secure, 1 = secure. If omitted, the secure state is either unknown or non-secure.|

##### 2.3.2.4. Site Object
Note: The site object will never be passed in conjunction with the app object.

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|ID of the site on TripleLift. This will be -1 when the site is unavailable.|
|cat|Optional|List of strings|List of IAB content Category codes|
|domain|Optional|string|Site domain (may be masked at the publisher's request)|
|page|recommended|string|URL of the page where the impression will be shown|
|publisher|Optional|object|Publisher object|

##### 2.3.2.5. App Object
Note: The app object will never be passed in conjunction with the site object.

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|Application ID on the exchange|
|name|Optional|string|Name of the application|
|publisher|Optional|object|Publisher object|

##### 2.3.2.6. Publisher Object
Note: cat is currently not supported but may be in future versions

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|Publisher ID on the exchange|
|cat|Optional|List of strings|List of IAB content Category codes|

##### 2.3.2.7. Device Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|ua|Optional|string|Browser user agent string|
|ip|Optional|string|Device IP address when available|
|geo|Optional|object|Derived geography where possible|
|make|Optional|string|Device make ("Apple")|
|model|Optional|string|Device model ("iPhone")|
|os|Optional|string|Device OS ("iOS")|
|devicetype|Optional|int|See Device Type|
|ifa|Optional|string|ID sanctioned for advertiser use in the clear|

###### 2.3.2.7.1. Device Type
Based on the OpenRTB 2.3 spec.

|Value|Description|
|--- |--- |
|1|Mobile/Tablet|
|2|Personal Computer|
|3|Connected TV|
|4|Phone|
|5|Tablet|
|6|Connected Device|
|7|Set Top Box|

##### 2.3.2.8. Geo Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|lat|Optional|float|Latitude of the device|
|lon|Optional|float|Longitude of the device|
|country|Optional|string|Country. In [ISO3](http://unstats.un.org/unsd/tradekb/Knowledgebase/Country-Code)|
|region|Optional|string|Region|
|city|Optional|string|City|

##### 2.3.2.9. User Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|User ID when available|
|buyeruid|Optional|string|The buyer's external ID that has been sync'd with TripleLift, to the extent that it is available. See [Usersync](usersync.md).|

##### 2.3.2.10. PMP Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|private_auction|Optional|integer|This will be either 0 or 1, where 0 = simply a targeting parameter. no preferred auction will take place in the TL exchange, 1 = an indication of a private auction|
|deals|Optional|object array|Array of Deal objects that convey specific terms applied|
|ext|Optional|object|Placeholder for exchange-specific extensions to OpenRTB|

##### 2.3.2.11. Deal Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|Unique identifier for the deal|
|bidfloor|Optional|float|Minimum bid for this impression expressed in CPM|
|bidfloorcur|Optional|string|Currency specified using ISO-4217 alpha codes|
|at|Optional|integer|Override of the overall auction type of the bid request|
|wseat|Optional|string array|Whitelist of buyer seats allowed to bid on this deal. Omission implies no seat restrictions|
|wadomain|Optional|string array|Array of advertiser domains allowed to bid on this deal. Omission implies no seat restrictions|
|ext|Optional|object|Placeholder for exchange-specific extensions to OpenRTB|

### 2.4. Sample Bid Requests
##### 2.3.3.1. Sample website request

```json
{
    "id": "1111",
    "imp": [
        {
            "id": "1",
            "tagid": "2222",
            "secure": 1,
            "native": {
                "ver": 1,
                "request": "{\"ver\":1,\"plcmtcnt\":1,\"layout\":6,\"adunit\":5,\"assets\":[{\"id\":1,\"required\":1,\"title\":{\"len\":25}},{\"id\":2,\"required\":1,\"img\":{\"w\":8,\"h\":7,\"wmin\":6,\"hmin\":6,\"type\":3}},{\"id\":3,\"required\":0,\"img\":{\"wmin\":25,\"hmin\":25,\"type\":2}},{\"id\":4,\"required\":1,\"data\":{\"type\":2,\"len\":75}},{\"id\":5,\"required\":1,\"data\":{\"type\":1,\"len\":25}}]}",
                "ext": {
                    "triplelift": {
                        "formats": [
                            1,
                            2,
                            3
                        ]
                    }
                }
            }
        }
    ],
    "site": {
        "id": "666666",
        "domain": "https://cheezburger.com",
        "publisher": {
            "id": "777777"
        }
    },
    "user": {
        "id": "988776"
    },
    "bcat": [
        "IAB18",
        "IAB2"
    ]
}
```

##### 2.3.3.2. Sample app request, no user ID, no publisher object

```json
{
    "id": "7979d0c78074638bbdf739ffdf285c7e1c74a691",
    "imp": [
        {
            "id": "1",
            "tagid": "94737",
            "native": {
                "ver": 1,
                "request": "{\"ver\":1,\"plcmtcnt\":1,\"layout\":6,\"adunit\":5,\"assets\":[{\"id\":1,\"required\":1,\"title\":{\"len\":25}},{\"id\":2,\"required\":1,\"img\":{\"w\":8,\"h\":7,\"wmin\":6,\"hmin\":6,\"type\":3}},{\"id\":3,\"required\":0,\"img\":{\"wmin\":25,\"hmin\":25,\"type\":2}},{\"id\":4,\"required\":1,\"data\":{\"type\":2,\"len\":75}},{\"id\":5,\"required\":1,\"data\":{\"type\":1,\"len\":25}}]}"
            }
        }
    ],
    "app": {
        "id": "5555555",
        "name": "sample app"
    },
    "device": {
        "make": "Samsung",
        "model": "SCH-I535",
        "os": "Android",
        "ua": "Mozilla/5.0 (Linux; U; Android 4.3; en-us; SCH-I535 Build/JSS15J) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30",
        "ip": "192.168.1.1",
        "geo": {
            "country": "USA",
            "region": "PA"
        }
    },
    "user": {}
}
```

##### 2.3.3.3. Sample website request, with PMP object

```json
{
    "id": "1111",
    "imp": [
        {
            "id": "1",
            "tagid": "2222",
            "pmp": {
                "private_auction": 1,
                "deals": [
                    {
                        "id": "pmp_deal",
                        "bidfloor": 10.50,
                        "wseat": [
                            "166"
                        ]
                    }
                ]
            },
            "secure": 1,
            "native": {
                "ver": 1,
                "request": "{\"ver\":1,\"plcmtcnt\":1,\"layout\":6,\"adunit\":5,\"assets\":[{\"id\":1,\"required\":1,\"title\":{\"len\":25}},{\"id\":2,\"required\":1,\"img\":{\"w\":8,\"h\":7,\"wmin\":6,\"hmin\":6,\"type\":3}},{\"id\":3,\"required\":0,\"img\":{\"wmin\":25,\"hmin\":25,\"type\":2}},{\"id\":4,\"required\":1,\"data\":{\"type\":2,\"len\":75}},{\"id\":5,\"required\":1,\"data\":{\"type\":1,\"len\":25}}]}",
                "ext": {
                    "triplelift": {
                        "formats": [
                            1,
                            2,
                            3
                        ]
                    }
                }
            }
        }
    ],
    "site": {
        "id": "666666",
        "domain": "https://cheezburger.com",
        "publisher": {
            "id": "777777"
        }
    },
    "user": {
        "id": "988776"
    },
    "bcat": [
        "IAB18",
        "IAB2"
    ]
}
```

## 3. Bid Response
The following describes the various attributes that are supported in the TripleLift OpenRTB 2.3 implementation. 

To return the specifics of the native ad, use a JSON-encoded string in the adm field. 

**Note:** This is not a JSON object, it is a string of an object. The spec will describe the JSON structure that determines the string. E.g. 

```json
"adm" : "{\"native\" : { ... }}" 
```

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|native|Required|object|Top level Native object|

### 3.1. Native Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|ver|Required|integer|This must always be 1.|
|assets|Required|array of Assets objects|List of the corresponding Asset objects, matching the specification in the request.|
|link|Required|Link object|Destination link specified by a Link object.|
|imptrackers|Optional|array of strings|Array of impression tracking URLs. This must be images or 204s. Note that these are client side requests. ${AUCTION_PRICE} macro replacement is available.|
|jstracker|Optional|string|This is a valid HTML, Javascript is already wrapped in &lt;script&#47;&gt; tags. It should be executed at impression time where it can be supported. Only whitelist javascript will be executed; otherwise, bid will be rejected (other bids in multibid are still preserved). See [Trackers](trackers.md) for whitelisted domains/vendors.|
|ext|Optional|Ext object|The Ext object contains a viewability pixel. See Native Object Ext Parameters.|

#### 3.1.1. Asset Object
The asset object must contain one of title, img, video or data.

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|integer|Asset Id, corresponding to the request asset id. The bid will be rejected if inappropriate assets or supplied for asset IDs.|
|title|Optional|Title object|Title object for the title asset.|
|img|Optional|Image object|Image object for the image asset(s)|
|video|Optional|Video object|Video object for the video asset|
|data|Optional|Data object|Data object for the data asset(s)|

##### 3.1.1.1. Title Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|text|Required|string|The text associated with the text element.|

##### 3.1.1.2. Image Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|url|Required|string|URL of the image asset.|
|w|Required|integer|width of the image|
|h|Required|integer|height of the image|

##### 3.1.1.3. Data Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|value|Required|string|Data to be displayed|

##### 3.1.1.4. Video Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|vasttag|Required|string|VAST XML String.|
|ext|Optional|object|Contains additional information. This is a custom TL unit that is not part of standard OpenRTB 2.3 Native 1.0 Spec. See Video Ext Parameters.|

###### 3.1.1.4.1. Video Ext Parameters
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|playbackmethod|optional|integer|See Play Back Method. No response will default to Autoplay Sound Off (2).|

##### 3.1.1.5. Link Parameters
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|url|Required|string|Landing page of the ad. This may include auto-redirecting click trackers.|
|clicktrackers|Optional|Array of string|List of third-party click trackers. Note that this is a client initiated request, without macro replacement.|

##### 3.1.1.6. Native Object Ext Parameters
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|viewtracker|Optional|string|Pixel URL to be fired when the ad is viewable. Note that this is a client initiated request, without macro replacement.|
|useadchoices|Optional|boolean|If the useadchoices parameter for your bidder is set to "when_specified", then the adchoicesurl that you specified for your bidder will be used to overlay the adchoices icon. You do not need to include this in the response if useadchoices is either "always" or "never." If you set this to true, but the adchoicesurl has not been defined and no adchoicesoverrideurl has been provided, the bid will lose. If this field is not included in the response, and useadchoices is "when_specified," this will be treated as false, even if adchoicesoverrideurl is provided.|
|adchoicesoverrideurl|Optional|string|This will override the bidder-level adchoicesurl when provided.|

### 3.2. TripleLift OpenRTB Implementation
#### 3.2.1. Top Level Objects
|Field|Scope|Notes|
|--- |--- |--- |
|Bid Response|Required|Top-level object|
|seatbid|Required|Bids are required in bid responses|

#### 3.2.2. Bid Response Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|Id of the bid request|
|seatbid|Required|array of objects|Array of seatbid objects|

#### 3.2.3. Seat Bid Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|bid|Required|array of objects|Array of bid objects|
|seat|Optional|string|ID of the bidder seat on whose behalf this bid is made.|

#### 3.2.4. Bid Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|ID for the bid object chosen by the bidder for tracking and debugging purposes. Useful when multiple bids are submitted for a single impression for a given seat.|
|impid|Required|string|Must be "1"|
|price|Required|float|Bid price in CPM|
|nurl|Optional|string|URL for win notifications. An HTTP POST request will be made to this URL after eligible macros are substituted in. Note: we do not support HTTP GET. Loss notifications are not supported currently. This will be a server to server request.  Note: this URL must be over HTTP. HTTPS is not supported currently.|
|dealid|Optional|string|Unique identifier for the deal|
|crid|Optional|String|Creative ID|
|ext|Optional|object|Bid object extension|

##### 3.2.4.1. Eligible Macros
|Macro|Description|
|--- |--- |
|${AUCTION_ID}|ID of the bid request; from “id” attribute.|
|${AUCTION_BID_ID}|ID of the bid; from “bidid” attribute of the bid response object.|
|${AUCTION_IMP_ID}|ID of the impression just won; from “impid” attribute.|
|${AUCTION_PRICE}|Settlement price using the same currency and units as the bid.|

##### 3.2.4.2. Bid Object Extension
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|agencyName|Optional|string|Name of agency from which bid response is sent|
|agencyId|Optional|integer|Corresponding id of agency from which bid response is sent|

### 3.3. Sample Bid Response
Includes optional seat ID and deal ID. 

```json
{
    "id": "12345",
    "bidid": "54321",
    "seatbid": [
        {
            "seat": "123123123",
            "bid": [
                {
                    "id": "1",
                    "impid": "1",
                    "price": 0.456,
                    "dealid": "sample_deal_id",
                    "nurl": "http://sample_nurl.com/win/123456?price=${AUCTION_PRICE}",
                    "adm": "{\"native\": {\"ver\":1, \"assets\": [{\"id\": 1,\"title\": {\"text\": \"title_text\"}},{\"id\": 2,\"img\": {\"url\": \"http://mainimage.com\", \"w\":300, \"h\":400}},{\"id\": 3,\"img\": {\"url\": \"http://logoimage.com\", \"w\":100, \"h\":100}},{\"id\": 4,\"data\": {\"value\": \"caption text\"}},{\"id\": 5,\"data\": {\"value\": \"brand name\"}}], \"link\" : {\"url\":\"http://destination.com\", \"clicktrackers\":[\"http://clktracker.com/clk\"]}, \"imptrackers\" : [\"http://url.com/123\", \"http://url.com/345\"], \"ext\": {\"viewtracker\":\"http://viewability.com/abc\"} }}"
                },
                {
                    "id": "2",
                    "impid": "1",
                    "price": 0.123,
                    "dealid": "sample_deal_id",
                    "nurl": "http://second_sample_nurl.com/win2/abcd?price=${AUCTION_PRICE}",
                    "adm": "{\"native\": {\"ver\":1, \"assets\": [{\"id\": 1,\"title\": {\"text\": \"second_title_text\"}},{\"id\": 2,\"img\": {\"url\": \"http://secondmainimage.com\", \"w\":302, \"h\":402}},{\"id\": 3,\"img\": {\"url\": \"http://secondlogoimage.com\", \"w\":102, \"h\":102}},{\"id\": 4,\"data\": {\"value\": \"second caption text\"}},{\"id\": 5,\"data\": {\"value\": \"second brand name\"}}], \"link\" : {\"url\":\"http://second-destination.com\", \"clicktrackers\":[\"http://second-clktracker.com/clk\"]}, \"imptrackers\" : [\"http://url.com/123\", \"http://second-nurl.com/345\"], \"ext\": {\"viewtracker\":\"http://second-viewability.com/abc\"} }}"
                }
            ]
        }
    ]
}
```