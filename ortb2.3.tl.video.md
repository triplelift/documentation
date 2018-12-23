# TripleLift OpenRTB 2.3 Video Integration Specs

## 0. Overview
As is standard for ad exchanges, the winning bid will pay the second price. The protocol is based on the OpenRTB 2.3 specification. 

Bidders may submit up to 10 bids total (not per seatbid) in the entire bid response. This means that the only the first 10 bids in lexical parse order across seatbids will be considered. Additional bids will not be processed. Only the highest eligible bid response for a particular bidder will be used (meaning a bidder's losing bids will not be counted for price reduction). 

### 0.1 Basic Auction on the TripleLift Exchange
1. TripleLift receives an ad request from a partner.
2. TripleLift sends a bid request via HTTP POST to all partner bidders http endpoints. Each bidder responds within 100ms. 
3. TripleLift holds a second price auction, subject to the publishers price floors and brand constraints.
4. TripleLift creates an ad through the assets specified in the winning bid, and sends a notification to the bidder's notify endpoint. See [Notifys](notifys.md).
5. The ad is rendered.

## 1. General
### 1.1. Technical
The TripleLift RTB exchange is implemented via HTTP (not HTTPS). Connections should be established with HTTP Keep-Alive.

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

### 2.1. TripleLift OpenRTB Implementation
#### 2.1.1. Top Level Objects
|Field|Scope|Notes|
|--- |--- |--- |
|Bid Request Object|Required|Top-level object.|
|Impression Object|Required|Exactly one impression object will be sent in a bid request object.|
|Native Object|Never|Not used|
|Banner Object|Never|Not used|
|Video Object|Required|Used in the video object|
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
|Extensions|Optional|Used for Extension Objects|

#### 2.1.2. Bid Request Object
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

#### 2.1.3. Impression Object
Note: bidfloor is not currently supported. It may be supported in future versions of the spec. 
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|This will always be "1"|
|video|Required|object|The TripleLift video implementation uses the Video Object for video impressions.|
|tagid|Required|string|TripleLift placement identifier|
|secure|Optional|integer|Flag to indicate if the impression comes from a secure site, where 0 = non-secure, 1 = secure. If omitted, the secure state is either unknown or non-secure.|

#### 2.1.4. Video Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|mimes|Required|array of strings|Content MIME types supported. Can be empty. Flash is not supported currently.|
|minduration|Required|integer|Minimum video ad length|
|maxduration|Required|integer|Maximum video ad length|
|protocols|Required|array of integers|Supported video bid response protocols. TripleLift supports values 1 through 6. See Protocols.|
|playbackmethod|Required|array of integers|Corresponds to video response. See Play Back Method.|
|linearity|Required|integer|Indicates if the impression must be linear, non-linear, etc. TripleLift supports value 2, non-linear/overlay.|
|api|Optional|array of integers|List of supported API frameworks for this impression. If an API is not explicitly listed, it is assumed not to be supported. See API Frameworks.|

#### 2.1.4.1. Protocols
|Value|Description|Used by TripleLift|
|--- |--- |--- |
|1|VAST 1.0|Yes|
|2|VAST 2.0|Yes|
|3|VAST 3.0|Yes|
|4|VAST 1.0 Wrapper|Yes|
|5|VAST 2.0 Wrapper|Yes|
|6|VAST 3.0 Wrapper|Yes|

#### 2.1.4.2. Play Back Method
|Value|Name|Description|Used by TripleLift|
|--- |--- |--- |--- |
|1|Auto-Play Sound On|Autoplay with sound|No|
|2|Auto-Play Sound Off|Autoplay without sound|Yes|
|3|Click-to-Play|Play on click|Yes|
|4|Mouse-Over|Play on hover|No|

#### 2.1.4.3. API Frameworks
|Value|Description|Used by TripleLift|
|--- |--- |--- |
|1|VPAID 1.0|No|
|2|VPAID 2.0|Yes|
|3|MRAID-1|No|
|4|ORMMA|No|
|5|MRAID-2|No|

#### 2.1.5. Site Object
Note: The site object will never be passed in conjunction with the app object.
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|ID of the site on TripleLift. This will be -1 when the site is unavailable.|
|cat|Optional|List of strings|List of IAB content Category codes|
|domain|Optional|string|Site domain (may be masked at the publisher's request)|
|page|recommended|string|URL of the page where the impression will be shown|
|publisher|Optional|object|Publisher object|

#### 2.1.6. App Object
Note: The app object will never be passed in conjunction with the site object.

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|Application ID on the exchange|
|name|Optional|string  Name of the application|
|publisher|Optional|object|Publisher object|

#### 2.1.7. Publisher Object
Note: cat is currently not supported but may be in future versions

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|Publisher ID on the exchange|
|cat|Optional|List of strings|List of IAB content Category codes|

#### 2.1.8. Device Object
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

#### 2.1.8.1. Device Type
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

#### 2.1.9. Geo Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|lat|Optional|float|Latitude of the device|
|lon|Optional|float|Longitude of the device|
|country|Optional|string|Country. In [ISO3](http://unstats.un.org/unsd/tradekb/Knowledgebase/Country-Code)|
|region|Optional|string|Region|
|city|Optional|string|City|

#### 2.1.10. User Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|User ID when available|
|buyeruid|Optional|string|The buyer's external ID that has been sync'd with TripleLift, to the extent that it is available. See [Usersync](usersync.md).|

#### 2.1.11. Pmp Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|private_auction|Optional|integer|This will be either 0 or 1, where 0 = simply a targeting parameter. no preferred auction will take place in the TL exchange, 1 = an indication of a private auction|
|deals|Optional|object array|Array of Deal objects that convey specific terms applied|
|ext|Optional|object|Placeholder for exchange-specific extensions to OpenRTB|

#### 2.1.12. Deal Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|Unique identifier for the deal|
|bidfloor|Optional|float|Minimum bid for this impression expressed in CPM|
|bidfloorcur|Optional|string|Currency specified using ISO-4217 alpha codes|
|at|Optional|integer|Override of the overall auction type of the bid request|
|wseat|Optional|string array|Whitelist of buyer seats allowed to bid on this deal. Omission implies no seat restrictions|
|wadomain|Optional|string array|Array of advertiser domains allowed to bid on this deal. Omission implies no seat restrictions|
|ext|Optional|object|Placeholder for exchange-specific extensions to OpenRTB|

### 2.2. Sample Bid Requests

#### 2.2.1. Sample video request
```json
{
    "id": "1111",
    "imp": [{
        "id": "1",
        "tagid": "2222",
        "secure": 1,
        "video": {
            "mimes": ["video/mp4", "video/webm", "video/mpeg"],
            "minduration": 1,
            "maxduration": 900,
            "protocols": [1, 2, 3, 4, 5, 6],
            "playbackmethod": [3],
            "linearity": 2,
            "api": [2]
        }  
    }],
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
    "bcat": ["IAB18", "IAB2"]
}
```

#### 2.2.2. Sample video request, with PMP object
```json
{
    "id": "1111",
    "imp": [{
        "id": "1",
        "tagid": "2222",
        "pmp": {
            "private_auction" : 1,
            "deals" : [{"id":"pmp_deal", "bidfloor": 10.50, "wseat":["166"]}]
        },
        "secure": 1,
        "video": {
            "mimes": ["video/mp4", "video/webm", "video/mpeg"],
            "minduration": 1,
            "maxduration": 900,
            "protocols": [1, 2, 3, 4, 5, 6],
            "playbackmethod": [3],
            "linearity": 2,
            "api": [2]
        }
    }],
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
    "bcat": ["IAB18", "IAB2"]
}
```

## 3. Bid Response
The following describes the various attributes that are supported in the TripleLift OpenRTB 2.3 implementation. 

### 3.1. TripleLift OpenRTB Implementation
#### 3.1.1. Top Level Objects
|Field|Scope|Notes|
|--- |--- |--- |
|Bid Response|Required|Top-level object|
|seatbid|Required|Bids are required in bid responses|

#### 3.1.2. Bid Response Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|Id of the bid request|
|seatbid|Required|array of objects|Array of seatbid objects|

#### 3.1.3. Seat Bid Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|bid|Required|array of objects|Array of bid objects|
|seat|Optional|string|ID of the bidder seat on whose behalf this bid is made|

#### 3.1.4. Bid Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|ID for the bid object chosen by the bidder for tracking and debugging purposes. Useful when multiple bids are submitted for a single impression for a given seat. |
|impid|Required|string|Must be "1"|
|price|Required|float|Bid price in CPM|
|nurl|Optional|string|URL for win notifications. An HTTP POST request will be made to this URL after eligible macros are substituted in. Loss notifications are not supported currently. Note that this will be a server to server request.|
|dealid|Optional|string|Unique identifier for the deal|
|adomain|Required|string array|Advertiser domain|
|adm|Required|string|Ad markup containing encoded VAST/VPAID XML tag. Supported versions: VPAID 1.0, VPAID 2.0, VAST 3.0, VAST 4.0|
|crid|Required|string|Creative ID|
|ext|Optional|object|Bid object extension|

##### 3.1.4.1. Eligible Macros
|Macro|Description|
|--- |--- |
|${AUCTION_ID}|ID of the bid request; from “id” attribute.|
|${AUCTION_BID_ID}|ID of the bid; from “id” attribute of the bid object.|
|${AUCTION_IMP_ID}|ID of the impression just won; from “impid” attribute.|
|${AUCTION_PRICE}|Settlement price using the same currency and units as the bid.|

#### 3.1.5. Bid Object Extension Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|agencyName|Optional|string|Name of agency from which bid response is sent|
|agencyId|Optional|integer|Corresponding id of agency from which bid response is sent|

### 3.2. Sample Bid Response
```json
{
    "id": "12345",
    "seatbid": [
        {
            "seat": "123123123",
            "bid": [
                {
                    "id": "1",
                    "impid": "1",
                    "price": 0.456,
                    "nurl": "http://sample_nurl.com/win/123456?price=${AUCTION_PRICE}",
                    "adomain": ["http://www.sampleadvertiser.com"],
                    "adm": "%3C%3Fxml%20version%3D%221.0%22%20encoding%3D%22utf-8%22%3F%3E%0A%3CVAST%20version%3D%222.0%22%3E%0A%20%20%20%20%3CAd%20id%3D%2212345%22%3E%0A%20%20%20%20%20%20%20%20%3CInLine%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%3CAdSystem%20version%3D%221.0%22%3ESpotXchange%3C%2FAdSystem%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CAdTitle%3E%3C!%5BCDATA%5BSample%20VAST%5D%5D%3E%3C%2FAdTitle%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CImpression%3Ehttp%3A%2F%2Fsample.com%3C%2FImpression%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CDescription%3E%3C!%5BCDATA%5BA%20sample%20VAST%20feed%5D%5D%3E%3C%2FDescription%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CCreatives%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CCreative%20sequence%3D%221%22%20id%3D%221%22%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CLinear%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CDuration%3E00%3A00%3A30%3C%2FDuration%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CTrackingEvents%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FTrackingEvents%3E%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CVideoClicks%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CClickThrough%3E%3C!%5BCDATA%5Bhttp%3A%2F%2Fsample.com%2Fopenrtbtest%5D%5D%3E%3C%2FClickThrough%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FVideoClicks%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CMediaFiles%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3CMediaFile%20delivery%3D%22progressive%22%20bitrate%3D%22256%22%20width%3D%22640%22%20height%3D%22480%22%20type%3D%22video%2Fmp4%22%3E%3C!%5BCDATA%5Bhttp%3A%2F%2Fsample.com%2Fvideo.mp4%5D%5D%3E%3C%2FMediaFile%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FMediaFiles%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FLinear%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FCreative%3E%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3C%2FCreatives%3E%0A%20%20%20%20%20%20%20%20%3C%2FInLine%3E%0A%20%20%20%20%3C%2FAd%3E%0A%3C%2FVAST%3E",
                    "crid": "qwerty1234"
                }
            ]
        }
    ]
}
```