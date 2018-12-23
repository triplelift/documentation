# TripleLift OpenRTB 2.2 Integration Specs

## 0. Overview
TripleLift Native RTB is the largest exchange for DSPs to bid on native advertising impressions. As is standard for ad exchanges, the winning bid will pay the second price. The protocol is based on an extension of the OpenRTB 2.2 specification. 

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
x-openrtb-version: 2.2
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
The following describes the various attributes that are supported in the TripleLift OpenRTB 2.2 implementation.

Please note that values marked "Required" will always be sent. "Optional" means they may or may not be sent.

### 2.1. TripleLift Extension
Note: The TripleLift extension is contained in the ext field as specified below and is referenced in the banner object.

```json
"ext": "{\"triplelift\" : { ... }}"
```

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|triplelift|Required|object|TripleLift Request Object.|
|formats|Optional|array of integers|TripleLift will send signals on which format types are supported by the particular placement<ul><li>1 represents image</li><li>2 represents click-to-play video</li><li>3 represents cinemagraph</li><li>8 represents instant play video</li></ul>| 

### 2.2. TripleLift Object
#### 2.2.1. TripleLift Native Parameters
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|imgw|Required|integer|Width of the image in the placement|
|imgh|Required|integer|Height of the image in the placement|
|wmin|Optional|integer|Minimum width of the impression in pixels. If included, it indicates that a range of sizes is allowed with this minimum width and "imgw" is taken as recommended. If not included, then "imgw" should be considered an exact requirement.|
|hmin|Optional|integer|Minimum height of the impression in pixels. If included, it indicates that a range of sizes is allowed with this minimum width and "imgh" is taken as recommended. If not included, then "imgh" should be considered an exact requirement.|
|headinglen|Optional|integer|Length of the heading field|
|captionlen|Optional|integer|Length of the caption field|
|subheadinglen|Optional|integer|DEPRECATED|
|video|Optional|object|See Video Object; if present then placement eligible for video. Note that this does not mean it is eligible for video only – all video eligible placements are image eligible as well.|

##### 2.2.1.1. Video Object

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|mimes|Required|array of strings|Content MIME types supported. Can be empty. Flash is not supported currently.|
|minduration|Required|integer|Minimum video ad length|
|maxduration|Required|integer|Maximum video ad length|
|protocols|Required|array of integers|Corresponds to video response Section 5.8 of OpenRTB 2.2. TripleLift supports values 1 to 6.|
|ext|optional|object|TL custom video options. See Video Ext Object.|

###### 2.2.1.1.1. Video Ext Object

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|playbackmethod|optional|array of integers|Corresponds to video response Section 5.9 of OpenRTB 2.2. See Play Back Method.|

###### 2.2.1.1.2. Play Back Method

|Value|Name|Description|Used by TripleLift|
|--- |--- |--- |--- |
|1|Auto-Play Sound On|Autoplay with sound|No|
|2|Auto-Play Sound Off|Autoplay without sound|Yes|
|3|Click-to-Play|Play on click|Yes|
|4|Mouse-Over|Play on hover|No|

#### 2.2.2. TripleLift OpenRTB Implementation
##### 2.2.2.1. Top Level Objects
|Field|Scope|Notes|
|--- |--- |--- |
|Bid Request Object|Required|Top-level object.|
|Impression Object |Required|Exactly one impression object will be sent in a bid request object.|
|Banner Object|Required|The TripleLift native implementation uses the Banner Object for native impressions.|
|Video Object|Never|Not used|
|Site Object |Optional|Will be used for website inventory|
|App Object |Optional|Will be used for app inventory|
|Content Object |Never|Not used|
|Device Object|Optional|Used for all requests where we have device data.|
|User Object |Required|Will be sent with the TripleLift user data|
|Publisher Object|Required|Will always be sent with TripleLift publisher data|
|Producer Object|Never|Not used|
|Geo Object|Never|Used in the device object.|
|Data Object|Never|Not used|
|Segment Object|Never|Not used|
|Regulations Object|Never|Not used|
|PMP Object|Optional|Used as a container for direct deals applicable to this impression|
|Direct Deals Object|Optional|Contains deal terms set up between buyer and TripleLift|
|Extensions|Required|Contains the TripleLift Native Extension| 

##### 2.2.2.2. Bid Request Object
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

##### 2.2.2.3. Impression Object
Note: bidfloor is not currently supported. It may be supported in future versions of the spec. 

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|This will always be "1"|
|banner|Required|object|The TripleLift native implementation uses the Banner Object for native impressions.|
|tagid|Required|string|TripleLift placement identifier|

##### 2.2.2.4. Banner Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|w|Required|integer|This will always be 0|
|h|Required|integer|This will always be 0|
|battr|Required|array of integers|Includes the list of attributes that are banned. If video is blocked, it will include a "7", per the OpenRTB specs. This is currently the only attribute that will be banned. Thus it will be either an empty an array or an array with just a 7.|
|ext|Required|object|See TripleLift Native Extension|

##### 2.2.2.5. Site Object
Note: The site object will never be passed in conjunction with the app object.

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|ID of the site on TripleLift. This will be -1 when the site is unavailable.|
|cat|Optional|List of strings|List of IAB content Category codes|
|domain|Optional|string|Site domain (may be masked at the publisher's request)|
|page|recommended|string|URL of the page where the impression will be shown|
|publisher|Optional|object|Publisher object|

##### 2.2.2.6. App Object
Note: The app object will never be passed in conjunction with the site object.

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|Application ID on the exchange|
|name|Optional|string|Name of the application|
|publisher|Optional|object|Publisher object|\ 

##### 2.2.2.7. Publisher Object
Note: cat is currently not supported but may be in future versions

|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|Publisher ID on the exchange|
|cat|Optional|List of strings|List of IAB content Category codes|

##### 2.2.2.8. Device Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|ua|Optional|string|Browser user agent string|
|ip|Optional|string|Device IP address when available|
|geo|Optional|object|Derived geography where possible|
|make|Optional|string|Device make ("Apple")|
|model|Optional|string|Device model ("iPhone")|
|os|Optional|string|Device OS ("iOS")|
|devicetype|Optional|int|See Device Type|

##### 2.2.2.8.1. Device Type
Based on the OpenRTB 2.2 spec. TripleLift uses the "From 2.2" values. 

|Field|Description|Notes|
|--- |--- |--- |
|1|Mobile/Tablet|From 2.0|
|2|Personal Computer|From 2.0|
|3|Connected TV|From 2.0|
|4|Phone|From 2.2|
|5|Tablet|From 2.2|
|6|Connected Device|From 2.2|
|7|Set Top Box|From 2.2|

##### 2.2.2.9. Geo Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|lat|Optional|float|Latitude of the device|
|lon|Optional|float|Longitude of the device|
|country|Optional|string|Country. In [ISO3](http://unstats.un.org/unsd/tradekb/Knowledgebase/Country-Code)|
|region|Optional|string|Region|
|city|Optional|string|City|

##### 2.2.2.10. User Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Optional|string|User ID when available|
|buyeruid|Optional|string|The buyer's external ID that has been sync'd with TripleLift, to the extent that it is available. See [Usersync](usersync.md).|

##### 2.2.2.11. Pmp Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|private_auction|Optional|integer|This will be either 0 or 1, where 0 = simply a targeting parameter. no preferred auction will take place in the TL exchange, 1 = an indication of a private auction|
|deals|Optional|object array|Array of Deal objects that convey specific terms applied|
|ext|Optional|object|Placeholder for exchange-specific extensions to OpenRTB|

##### 2.2.2.12. Deal Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|Unique identifier for the deal|
|bidfloor|Optional|float|Minimum bid for this impression expressed in CPM|
|bidfloorcur|Optional|string|Currency specified using ISO-4217 alpha codes|
|at|Optional|integer|Override of the overall auction type of the bid request|
|wseat|Optional|string array|Whitelist of buyer seats allowed to bid on this deal. Omission implies no seat restrictions|
|wadomain|Optional|string array|Array of advertiser domains allowed to bid on this deal. Omission implies no seat restrictions|
|ext|Optional|object|Placeholder for exchange-specific extensions to OpenRTB|

### 2.3. Sample Bid Requests
#### 2.3.1. Sample website request

```json
{
    "id": "1111",
    "imp": [{
        "id": "1",
        "tagid": "2222",
        "banner": {
            "w": 0,
            "h": 0,
            "battr" : [7],
            "ext" : {
                "triplelift" : {
                    "imgw" : 300,
                    "imgh" : 300,
                    "headinglen" : 25,
                    "captionlen" : 150,
                    "formats": [1, 2, 3]
                }
            }
        }
    }],
    "site": {
        "id": "666666",
        "domain": "http://cheezburger.com",
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

#### 2.3.2. Sample app request, no user ID, no publisher object

```json
{
    "id": "7979d0c78074638bbdf739ffdf285c7e1c74a691",
    "imp": [{
        "id": "1",
        "tagid": "94737",
        "banner": {
            "w": 0,
            "h": 0,
            "battr" : [7],
            "ext" : {
                "triplelift" : {
                    "imgw" : 300,
                    "imgh" : 300,
                    "headinglen" : 25,
                    "captionlen" : 150,
                    "formats": [1]
                }
            }
        }
    }],
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
    "user": {
    }
}
```

#### 2.3.3. Sample video request

```json
{
    "id": "9576509704600808170",
    "at": 2,
    "imp": [{
        "id": "1",
        "tagid": "990",
        "banner": {
            "w": 0,
            "h": 0,
            "ext": {
                "triplelift": {
                    "imgw": 370,
                    "imgh": 278,
                    "headinglen": 75,
                    "captionlen": 200,
                    "video": {
                        "mimes": [],
                        "minduration": 1,
                        "maxduration": 900,
                        "protocols": [1, 2, 3, 4, 5, 6],
                        "ext": {
                            "playbackmethod": [3, 500]
                        },
                    "formats": [1, 2, 3]
                    }
                }
            }
        }
    }],
    "site": {
        "id": "32",
        "domain": "makersalley.com",
        "cat": ["IAB8", "IAB10"],
        "page": "http://www.makersalley.com/page",
        "publisher": {
            "id": "186",
            "cat": ["IAB8", "IAB10"]
        }
    },
    "device": {
        "ua": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36",
        "ip": "127.0.0.1",
        "devicetype": 2,
        "make": "apple",
        "os": "mac os x",
        "language": "en"
    },
    "user": {
        "id": "2615902767870488485"
    },
    "test": 1,
    "bcat": []
}
```

#### 2.3.4. Sample website request, with PMP object

```json
{
    "id": "1111",
    "imp": [{
        "id": "1",
        "tagid": "2222",
        "banner": {
            "w": 0,
            "h": 0,
            "battr": [7],
            "ext": {
                "triplelift": {
                    "imgw": 300,
                    "imgh": 300,
                    "headinglen": 25,
                    "captionlen": 150,
                    "formats": [1, 2, 3]
                }
            }
        },
        "pmp": {
            "private_auction": 1,
            "deals": [{
                "id": "tlx-privatedeal",
                "bidfloor": 5.0,
                "wseat": ["123"]
            }]
        }
    }],
    "site": {
        "id": "666666",
        "domain": "http://cheezburger.com",
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
The following describes the various attributes that are supported in the TripleLift OpenRTB 2.2 implementation.

### 3.1. TripleLift Extension
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|heading|Required|string|String to be used for the heading in the native placement|
|caption|Required|string|String to be used for the caption field in the native placement|
|impTrackers|Optional|array of strings|Array of impression trackers to be delivered with the impression. Note that these are client side requests, without macro replacement. The impTrackers field is meant to be URLs that can be made sources of 1x1 images and appended to the DOM. We do not accept js or scripts in this field|
|jsTrackers|Optional|array of strings|Javascript URLs that will be executed at impression time where it can be supported. Only whitelisted javascript will be executed; otherwise, bid will be rejected (other bids in multibid are still preserved). See [Trackers](trackers.md) for whitelisted domains/vendors.|
|logoUrl|Optional|string|URL of the logo to be overlaid on the image|
|imgUrl|Required|string|Image to render in the native unit. Submit a .gif to access cinemagraph units.|
|imgWidth|Optional|int|Image width (Strongly recommended for retargeters)|
|imgHeight|Optional|int|Image height (Strongly recommended for retargeters)|
|clickUrl|Required|string|Clickthrough URL - this should include all the click trackers that are necessary|
|advName|Required|string|Name of the advertiser - to be used as "Sponsored by advName"|
|viewablePixel|Optional|string|If measuring viewability on the delivered impression, this pixel will be delivered when the impression is in view. Pixels to measure when the impression has first served should be included in the impTrackers array. Note that this is a client side request, without macro replacement. This will become the source of an image tag that is appended to publishers page / fired off over HTTP GET for mobile apps.|
|video|Optional|object|If a video interaction is desired, include this object with all the necessary parameters. If you include a video object and any requirements elements of the video object are not provided, the bid will be rejected. See Video Object.|
|useAdchoices|Optional|boolean|If useAdchoices is set to 'always' in the bidder profile, it will use the Ad Choices url in the bid response in adchoicesOverrideUrl. If no url is provided in the bid request, it will default to the url provided in the profile. If no url is provided in the bid response or in the profile, the bid will not win. If useAdchoices is set to 'when_specified', it will use the url provided in the bid response. If useAdchoices is set to 'never', it will never use Ad Choices, even if one is sent in the bid response.|
|adchoicesOverrideUrl|Optional|string|This will override the bidder-level adchoicesUrl when provided.|
|subheading|Optional|string|DEPRECATED|

#### 3.1.1. Video Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|vasttag|Required|string|VAST XML String.|
|ext|Optional|object|Contains additional information. This is a custom TL unit that is not part of standard OpenRTB2.3 Native 1.0 Spec. See Video Ext Parameters.|

#### 3.1.2. Video Ext Parameters
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|playbackmethod|optional|integer|See Play Back Method. No response will default to Autoplay Sound Off (2).|

### 3.2. TripleLift OpenRTB Implementation
#### 3.2.1. Top Level Objects
|Field|Scope|Notes|
|--- |--- |--- |
|Bid Response|Required|Top-level object|
|seatbid|Required|Bids are required in bid responses|
|ext|Required|Contains the TripleLift response extension|

#### 3.2.2. Bid Response Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|Id of the bid request|
|seatbid|Required|array of objects|Array of seatbid objects|

#### 3.2.3. Seat Bid Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|bid|Required|array of objects|Array of bid objects|
|seat|Optional|string|ID of the bidder seat on whose behalf this bid is made|

#### 3.2.4. Bid Object
|Field|Scope|Type|Notes|
|--- |--- |--- |--- |
|id|Required|string|ID for the bid object chosen by the bidder for tracking and debugging purposes. Useful when multiple bids are submitted for a single impression for a given seat. |
|impid|Required|string|Must be "1"|
|price|Required|float|Bid price in CPM|
|nurl|Optional|string|URL for win notifications. An HTTP POST request will be made to this URL after eligible macros are substituted in. Loss notifications are not supported currently. Note that this will be a server to server request. Note: this URL must be over HTTP. HTTPS is not supported currently.|
|dealid|Optional|string|Unique identifier for the deal|
|crid|Optinal|String|Creative ID|
|ext|Required|object|TripleLift Response Extension|

##### 3.2.4.1. Eligible Macros
|Macro|Description|
|--- |--- |
|${AUCTION_ID}|ID of the bid request; from “id” attribute.|
|${AUCTION_BID_ID}|ID of the bid; from “bidid” attribute of the bid response object.|
|${AUCTION_IMP_ID}|ID of the impression just won; from “impid” attribute.|
|${AUCTION_PRICE}|Settlement price using the same currency and units as the bid.|

### 3.3. Sample Bid Responses
#### 3.3.1. Sample image response
Includes optional deal ID.

```json
{
    "id": "12345",
    "bidid": "54321",
    "seatbid": [
        {
            "bid": [
                {
                    "id": "1",
                    "impid": "1",
                    "price": 0.456,
                    "dealid": "sample_deal_id",
                    "nurl": "http://sample_nurl.com/win/123456?price=${AUCTION_PRICE}",
                    "ext": {
                        "heading": "This is the heading",
                        "caption": "The caption",
                        "impTrackers": ["http://tracker1.com/123456", "http://tracker2.com/87"],
                        "imgUrl": "http://the_image.com/123.jpg",
                        "clickUrl": "http://clicktracker.com/123/http://landingpage.com/product?id=123",
                        "advName": "Ecommerce company",
                        "useAdchoices": true,
                        "adchoicesOverrideUrl":"http://myadchoicesoverrideurl.com/"
                    }
                },
                {
                    "id": "2",
                    "impid": "1",
                    "price": 0.789,
                    "dealid": "sample_deal_id",
                    "nurl": "http://second-sample_nurl.com/abc?price=${AUCTION_PRICE}",
                    "ext": {
                        "heading": "This is the second heading",
                        "caption": "The second caption",
                        "impTrackers": ["http://second-tracker1.com/789", "http://second-tracker2.com/123"],
                        "imgUrl": "http://the_second_image.com/456.jpg",
                        "clickUrl": "http://second-clicktracker.com/abcabcabc/http://second-landingpage.com/product?id=456456",
                        "advName": "Second ecommerce company",
                        "useAdchoices": true,
                        "adchoicesOverrideUrl":"http://myadchoicesoverrideurl.com/"
                    }
                }
            ]
        }
    ]
}
```

#### 3.3.2. Sample video response

```json
{
  "id": "",
  "seatbid": [
    {
      "bid": [
        {
          "id": "0",
          "impid": "1",
          "price": 1.00,
          "nurl": "http://exchange-tester.triplelift.net:8077/notify?id=${AUCTION_ID}&amp;price=${AUCTION_PRICE}&amp;abid=${AUCTION_BID_ID}&amp;aimpid=${AUCTION_IMP_ID}&amp;type=Bid22Vast",
          "ext": {
            "heading": "VAST! is a test creative",
            "caption": "This is a description for a test creative",
            "impTrackers": [
              "https://test.com/p/3/p/199/?p=215099&amp;d=WyCr8AAAAUrgHnRk&amp;rnd=888569982818959561&amp;ec=triplelift_rtb_imp&amp;id=1111&amp;key=1234"
            ],
            "imgUrl": "http://advertiser.com/image.png",
            "clickUrl": "http://clickthru.com/",
            "advName": "Advertiser",
            "video": {
              "vasttag": "<VAST version=\"2.0\">\n <Ad id=\"videoAd_wrapper\">\n <Wrapper>\n <AdSystem>AdServer</AdSystem>\n <VASTAdTagURI>\n\t\t <![CDATA[https://bs.serving-sys.com/BurstingPipe/adServer.bs?cn=is&amp;c=23&amp;pl=VAST&amp;pli=16276769&amp;PluID=0&amp;pos=1683&amp;ord=[timestamp]&amp;cim=1]]>\n \n\t\t</VASTAdTagURI>\n <Impression>\n <![CDATA[https://reports.adspdbl.com/analytics.gif?ran=876&amp;type=event&amp;state=mousepress&amp;audience=&amp;campaign=CLT-test-usa-001-cp&amp;spartanUnitId=&amp;general=TLFT&amp;click_title=Vast%20ad%20impression%20collective&amp;resolution=557x318&amp;keyword_data=/&amp;domain=&amp;path=]]>\n </Impression>\n<Creatives>\n\t\t<Creative>\n\t\t\t<Linear>\n\t <TrackingEvents>\n\t <Tracking event=\"start\">\n <![CDATA[https://reports.adspdbl.com/analytics.gif?ran=876&amp;type=event&amp;state=mousepress&amp;audience=&amp;campaign=CLT-test-usa-001-cp&amp;spartanUnitId=&amp;general=TLFT&amp;click_title=Vast%20ad%20start%20collective&amp;resolution=557x318&amp;keyword_data=/&amp;domain=&amp;path=]]&gt;\n\t\t&lt;/Tracking&gt;\n\t\t<Tracking event=\"firstQuartile\">\n <![CDATA[https://reports.adspdbl.com/analytics.gif?ran=254&amp;type=event&amp;state=mousepress&amp;audience=&amp;campaign=CLT-test-usa-001-cp&amp;spartanUnitId=&amp;general=TLFT&amp;click_title=Vast%20ad%20first%20quartile%20collective&amp;resolution=557x318&amp;keyword_data=/&amp;domain=&amp;path=]]&gt;\n\t\t&lt;/Tracking&gt;\n\t\t<Tracking event=\"midpoint\">\n <![CDATA[https://reports.adspdbl.com/analytics.gif?ran=853&amp;type=event&amp;state=mousepress&amp;audience=&amp;campaign=CLT-test-usa-001-cp&amp;spartanUnitId=&amp;general=TLFT&amp;click_title=Vast%20ad%20halfway%20collective&amp;resolution=557x318&amp;keyword_data=/&amp;domain=&amp;path=]]&gt;\n\t\t&lt;/Tracking&gt;\n\t\t<Tracking event=\"thirdQuartile\">\n <![CDATA[https://reports.adspdbl.com/analytics.gif?ran=928&amp;type=event&amp;state=mousepress&amp;audience=&amp;campaign=CLT-test-usa-001-cp&amp;spartanUnitId=&amp;general=TLFT&amp;click_title=Vast%20ad%20third%20quartile%20collective&amp;resolution=557x318&amp;keyword_data=/&amp;domain=&amp;path=]]&gt;\n\t\t&lt;/Tracking&gt;\n\t\t<Tracking event=\"complete\">\n <![CDATA[https://reports.adspdbl.com/analytics.gif?ran=517&amp;type=event&amp;state=mousepress&amp;audience=&amp;campaign=CLT-test-usa-001-cp&amp;spartanUnitId=&amp;general=TLFT&amp;click_title=Vast%20ad%20complete%20collective&amp;resolution=557x318&amp;keyword_data=/&amp;domain=&amp;path=]]&gt;\n\t\t&lt;/Tracking&gt;\n<Tracking event=\"pause\">\n <![CDATA[https://reports.adspdbl.com/analytics.gif?ran=517&amp;type=event&amp;state=mousepress&amp;audience=&amp;campaign=CLT-test-usa-001-cp&amp;spartanUnitId=&amp;general=TLFT&amp;click_title=Vast%20ad%20paused%20collective&amp;resolution=557x318&amp;keyword_data=/&amp;domain=&amp;path=]]&gt;\n\t\t&lt;/Tracking&gt;\n<Tracking event=\"mute\">\n <![CDATA[https://reports.adspdbl.com/analytics.gif?ran=517&amp;type=event&amp;state=mousepress&amp;audience=&amp;campaign=CLT-test-usa-001-cp&amp;spartanUnitId=&amp;general=TLFT&amp;click_title=Vast%20ad%20mute%20collective&amp;resolution=557x318&amp;keyword_data=/&amp;domain=&amp;path=]]&gt;\n\t\t&lt;/Tracking&gt;\n<Tracking event=\"fullscreen\">\n <![CDATA[https://reports.adspdbl.com/analytics.gif?ran=517&amp;type=event&amp;state=mousepress&amp;audience=&amp;campaign=CLT-test-usa-001-cp&amp;spartanUnitId=&amp;general=TLFT&amp;click_title=Vast%20ad%20fullscreen%20collective&amp;resolution=557x318&amp;keyword_data=/&amp;domain=&amp;path=]]&gt;\n\t\t&lt;/Tracking>\n\t </TrackingEvents>\n \t <VideoClicks>\n <ClickTracking><![CDATA[https://reports.adspdbl.com/analytics.gif?ran=853&amp;type=event&amp;state=mousepress&amp;audience=&amp;campaign=CLT-test-usa-001-cp&amp;spartanUnitId=&amp;general=TLFT&amp;click_title=Vast%20ad%20click%20collective&amp;resolution=557x318&amp;keyword_data=/&amp;domain=&amp;path=]]&gt;&lt;/ClickTracking>\n </VideoClicks>\t\n</Linear>\n\t\t</Creative>\n</Creatives>\n </Wrapper>\n\n </Ad>\n</VAST>",
              "ext": {"playbackmethod":3}
            }
          }
        }
      ]
    }
  ],
  "cur": "USD"
}
```