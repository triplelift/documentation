# Server to Server Integration

## Overview
The TripleLift Server to Server integration is compliant with OpenRTB 2.2 Specs. This defines the structure for bid (combination of price, identifier, assets, etc.) request format and bid response format. This documentation will highlight items required by our Server to Server integration. Below you'll see the typical structure of a bid request and bid response object.

## Endpoint
The Server to Server integration is initiated by a bid request from the client's server to this endpoint: "http://tlx.3lift.com/s2s/auction". TL will provide supplier id, which will be appended as query string in requests.

## Bid Request
### Bid Request Structure (Web)
Our server to server integration expects bid requests formatted with the following fields:

<table><colgroup><col><col><col><col><col></colgroup><tbody><tr><th>Parameter</th><th>Sub Parameter</th><th>Sub Parameter</th><th>Scope</th><th>Notes</th></tr><tr><td>id</td><td></td><td></td><td>Required</td><td>ID of the bid request</td></tr><tr><td rowspan="8">imp</td><td></td><td></td><td>Required</td><td>The imp array will consist of one object</td></tr><tr><td>id</td><td></td><td>Required</td><td>This will always be "1"</td></tr><tr><td rowspan="4">banner</td><td></td><td>Required</td><td>The TripleLift native implementation uses the Banner Object for native impressions</td></tr><tr><td>id</td><td>Optional</td><td>Client banner ID</td></tr><tr><td>w</td><td>Required</td><td>Banner width</td></tr><tr><td>h</td><td>Required</td><td>Banner height</td></tr><tr><td>tagid</td><td></td><td>Optional</td><td>This is a slot level identifier.</td></tr><tr><td>bidfloor</td><td></td><td>Optional</td><td>Hard floor. TripleLift will not return bids below this number.</td></tr><tr><td rowspan="8">site</td><td></td><td></td><td>Recommended</td><td>The URL of the page the ad will be served onto.</td></tr><tr><td>id</td><td></td><td>Optional</td><td>Client ID for site</td></tr><tr><td>domain</td><td></td><td>Optional</td><td>Domain of site</td></tr><tr><td>page</td><td></td><td>Recommended</td><td>Full URL of site</td></tr><tr><td rowspan="4">publisher</td><td></td><td>Optional</td><td>Publisher of site</td></tr><tr><td>id</td><td>Optional</td><td>Client ID for publisher</td></tr><tr><td>name</td><td>Optional</td><td>Name of publisher</td></tr><tr><td>domain</td><td>Optional</td><td>Publisher Domain</td></tr><tr><td rowspan="13">device</td><td>-</td><td></td><td>Recommended</td><td>User's device data</td></tr><tr><td>ua</td><td></td><td>Recommended</td><td>Browser user agent string</td></tr><tr><td>ip</td><td></td><td>Recommended</td><td>Device IP address when available</td></tr><tr><td rowspan="6">geo</td><td></td><td>Optional</td><td></td></tr><tr><td>country</td><td>Optional</td><td>Country. In <a href="http://unstats.un.org/unsd/tradekb/Knowledgebase/Country-Code" rel="nofollow">ISO3</a></td></tr><tr><td>region</td><td>Optional</td><td>Region</td></tr><tr><td>metro</td><td>Optional</td><td>Designated Metropolitan Area (DMA)</td></tr><tr><td>city</td><td>Optional</td><td>City</td></tr><tr><td>zip</td><td>Optional</td><td>Zip Code</td></tr><tr><td>language</td><td></td><td>Recommended</td><td>User Language</td></tr><tr><td>make</td><td></td><td>Optional</td><td>Device make ("Apple")</td></tr><tr><td>model</td><td></td><td>Optional</td><td>Device Model ("iPhone")</td></tr><tr><td>os</td><td></td><td>Optional</td><td>Operating System ("iOS 9")</td></tr><tr><td rowspan="5">user</td><td>-</td><td></td><td>Required</td><td>Information on user</td></tr><tr><td>id</td><td></td><td>Optional</td><td>Client ID for user</td></tr><tr><td>buyeruid</td><td></td><td>Required</td><td>TripleLift user ID</td></tr><tr><td rowspan="2">geo</td><td></td><td>Optional</td><td>Geo location of user</td></tr><tr><td>country</td><td>Optional</td><td>Country of user</td></tr></tbody></table>

### Bid Request Sample (Web)
```json
{
  "id": "SyKwQTKcTR0Uaq60I1DXoA",
  "site": {
    "id": "11f3cc632fa62d843fa080a76af1df44c02e41bf",
    "name": "website.co.uk",
    "domain": "website.co.uk",
    "ref": "https://t.co/izY76pbBbP",
    "keywords": "apse={\"shouldSampleLatency\":false,\"chunkRequests\":false}",
    "publisher": {
      "id": "376"
    },
    "page": "https://www.website.co.uk/tvshowbiz/article-6276669/Will-Worlds-Hottest-Grandma-OLDEST-Maxim-cover-girl-history.html"
  },
  "device": {
    "ua": "Mozilla/5.0 (iPhone; CPU iPhone OS 12_0 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/12.0 Mobile/15E148 Safari/604.1",
    "geo": {
      "country": "AR",
      "city": "Buenos Aires",
      "zip": "1871"
    },
    "ip": "190.137.17.208"
  },
  "ext": {
    "hb": 1
  },
  "imp": [
    {
      "id": "1wKqk3HPX1aO2xxTrsD3eA",
      "banner": {
        "w": 900,
        "h": 250,
        "pos": 0
      },
      "instl": 0,
      "tagid": "leader_bottom",
      "secure": 1
    }
  ],
  "at": 1,
  "cur": [
    "USD"
  ],
  "tmax": 300
}
```

### Bid Request Structure (App)
Our server to server integration expects bid requests formatted with the following fields:

<table><colgroup><col><col><col><col><col></colgroup><tbody><tr><th>Parameter</th><th>Sub Parameter</th><th>Sub Parameter</th><th>Scope</th><th>Notes</th></tr><tr><td>id</td><td></td><td></td><td>Required</td><td>ID of the bid request</td></tr><tr><td rowspan="7">imp</td><td></td><td></td><td>Required</td><td>The imp array will consist of one object</td></tr><tr><td>id</td><td></td><td>Required</td><td>This will always be "1"</td></tr><tr><td rowspan="4">banner</td><td></td><td>Required</td><td>The TripleLift native implementation uses the Banner Object for native impressions</td></tr><tr><td>id</td><td>Optional</td><td>Client banner ID</td></tr><tr><td>w</td><td>Required</td><td>Banner width</td></tr><tr><td>h</td><td>Required</td><td>Banner height</td></tr><tr><td>bidfloor</td><td></td><td>Optional</td><td>Hard floor. TripleLift will not return bids below this number.</td></tr><tr><td rowspan="13">app</td><td></td><td></td><td>Recommended</td><td>The URL of the page the ad will be served onto.</td></tr><tr><td>id</td><td></td><td>Optional</td><td>Client ID for site</td></tr><tr><td>name</td><td></td><td>Recommended</td><td>Application name (may be masked at publisher’s request).</td></tr><tr><td>domain</td><td></td><td>Optional</td><td>Domain of the application (e.g., “mygame.foo.com”).</td></tr><tr><td>cat</td><td></td><td>Optional</td><td>Array of IAB content categories for the overall application.</td></tr><tr><td>bundle</td><td></td><td>Required</td><td>Application bundle or package name (e.g., com.foo.mygame). This is intended to be a unique ID across multiple exchanges.</td></tr><tr><td>storeurl</td><td></td><td>Optional</td><td>i.e. "https://itunes.apple.com/app/id50234631398"</td></tr><tr><td>ext</td><td>iTunesId</td><td>Optional</td><td>i.e. "iTunesId": "50423631398"</td></tr><tr><td>keywords</td><td></td><td>Optional</td><td>List of keywords describing this app in a comma separated string.</td></tr><tr><td rowspan="4">publisher</td><td></td><td>Optional</td><td>Publisher of site</td></tr><tr><td>id</td><td>Optional</td><td>Client ID for publisher</td></tr><tr><td>name</td><td>Optional</td><td>Name of publisher</td></tr><tr><td>domain</td><td>Optional</td><td>Publisher’s highest level domain name, for example “foopub.com”.</td></tr><tr><td rowspan="14">device</td><td>-</td><td></td><td>Recommended</td><td>User's device data</td></tr><tr><td>ifa</td><td></td><td>Required</td><td>Native identifier for advertisers; an opaque ID assigned by the device or browser for use as an advertising identifier.</td></tr><tr><td>ua</td><td></td><td>Recommended</td><td>Browser user agent string</td></tr><tr><td>ip</td><td></td><td>Recommended</td><td>Device IP address when available</td></tr><tr><td rowspan="6">geo</td><td></td><td>Optional</td><td></td></tr><tr><td>country</td><td>Optional</td><td>Country. In <a href="http://unstats.un.org/unsd/tradekb/Knowledgebase/Country-Code" rel="nofollow">ISO3</a></td></tr><tr><td>region</td><td>Optional</td><td>Region</td></tr><tr><td>metro</td><td>Optional</td><td>Designated Metropolitan Area (DMA)</td></tr><tr><td>city</td><td>Optional</td><td>City</td></tr><tr><td>zip</td><td>Optional</td><td>Zip Code</td></tr><tr><td>language</td><td></td><td>Recommended</td><td>User Language</td></tr><tr><td>make</td><td></td><td>Optional</td><td>Device make ("Apple")</td></tr><tr><td>model</td><td></td><td>Optional</td><td>Device Model ("iPhone")</td></tr><tr><td>os</td><td></td><td>Optional</td><td>Operating System ("iOS 9")</td></tr><tr><td rowspan="4">user</td><td>-</td><td></td><td>Required</td><td>Information on user</td></tr><tr><td>id</td><td></td><td>Optional</td><td>Client ID for user</td></tr><tr><td rowspan="2">geo</td><td></td><td>Optional</td><td>Geo location of user</td></tr><tr><td>country</td><td>Optional</td><td>Country of user</td></tr></tbody></table>

### Bid Request Sample (App)
```json
{
  "id": ".7-amQa.APqP.TIkiL7VsQ",
  "app": {
    "id": "f78a9a4efd9446118c21b44a4a7b360a",
    "name": "Binghamton Daily - iOS - mDTB",
    "keywords": "TEST",
    "publisher": {
      "id": "4656"
    },
    "ext": {
      "iTunesId": "50423631398"
    },
    "bundle": "504631398",
    "storeurl": "https://itunes.apple.com/app/id50234631398"
  },
  "device": {
    "h": 736,
    "w": 414,
    "lmt": 0,
    "make": "Apple",
    "model": "iPhone10,5",
    "os": "iOS",
    "osv": "12.0.1",
    "ua": "Mozilla/5.0 (iPhone; CPU iPhone OS 12_0_1 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/16A404",
    "ifa": "5A4CA1AF-5694-4B34-9871-3DBF544EA839",
    "geo": {
      "country": "US",
      "city": "Binghamton",
      "zip": "21236"
    },
    "ext": {
      "idfa": "5A4CA1AF-5694-4B34-9871-3DBF544EA839",
      "idfasha1": "6ea507fdf6df11c5918cffa1549e068b58a18bc3"
    },
    "connectiontype": 6,
    "ip": "107.77.202.202"
  },
  "ext": {
    "hb": 1
  },
  "imp": [
    {
      "id": ".7-amQa.APqP.TIkiL7VsQ",
      "banner": {
        "w": 300,
        "h": 250,
        "pos": 0
      },
      "instl": 0,
      "tagid": "b99fb24e-1b92-4c97-8c39-198f944b66f9",
      "secure": 1
    }
  ],
  "at": 1,
  "cur": [
    "USD"
  ],
  "tmax": 400
}
```

## Bid Response
### Bid Response (Web and App)
Our response will contain the following fields:

<table><colgroup><col><col><col><col><col></colgroup><tbody><tr><th>Parameter</th><th>Sub Parameter</th><th>Sub Parameter</th><th>Scope</th><th>Notes</th></tr><tr><td>id</td><td></td><td></td><td>Required</td><td>ID of the bid request</td></tr><tr><td rowspan="6">seatbid</td><td></td><td></td><td>Required</td><td>Array of seatbid objects</td></tr><tr><td rowspan="5">bid</td><td></td><td>Required</td><td>Array of bid objects</td></tr><tr><td>id</td><td>Required</td><td>ID for the bid object chosen by the bidder for tracking and debugging purposes. Useful when multiple bids are submitted for a single impression for a given seat.</td></tr><tr><td>impid</td><td>Required</td><td>This will always be "1"</td></tr><tr><td>price</td><td>Required</td><td>Bid price in CPM</td></tr><tr><td>adm</td><td>Required</td><td>Ad Markup. This can just be placed on the page where the ad should render.</td></tr><tr><td>cur</td><td></td><td></td><td>Optional</td><td>Currency of bid response</td></tr></tbody></table>

### Bid Response Sample (Web and App)
```json
{ 
   "id":"14_132",
   "seatbid":[ 
      { 
         "bid":[ 
            { 
               "id":"182826794766055180",
               "impid":"1",
               "price":0.779999,
               "adm":"<script>document.createElement('IMG').src=\"https://tlx.3lift.com/s2s/notify?px=1&amp;aid=182826794766055180&amp;n=CAERAAAAAAAAAAAaRWh0dHA6Ly9lYzItNTItNTctNDAtNjAuZXUtY2VudHJhbC0xLmNvbXB1dGUuYW1hem9uYXdzLmNvbTo4MDc3L25vdGlmeSCLvz4oz%2F8uMN%2FNLzgAQABIAVAAYLDjLWgCcK40ei0IzhcQjJsBGLQOILASKL0TML%2FsFDiljgFCC1RydXRoRmluZGVySAFYJGABcAGAAeSSA4gBAJABAPgBAIACAIgCAZACAJgCAA%3D%3D\";window.tl_auction_response_388859=\"dynamicCreativeRender({\\\"settings\\\":{\\\"advertiser_name\\\":\\\"TruthFinder\\\",\\\"type\\\":\\\"image\\\",\\\"additional_data\\\":{\\\"bmid\\\":\\\"3022\\\",\\\"ts\\\":\\\"1474300882\\\",\\\"clid\\\":\\\"18213\\\",\\\"brid\\\":\\\"19852\\\",\\\"tid\\\":\\\"341567\\\",\\\"aid\\\":\\\"182826794766055180\\\",\\\"advid\\\":\\\"1844\\\",\\\"liid\\\":\\\"2493\\\",\\\"ioid\\\":\\\"2352\\\"},\\\"format_id\\\":1,\\\"render_options_bm\\\":0},\\\"assets\\\":[{\\\"asset_id\\\":51556,\\\"image_id\\\":3349230,\\\"image_url\\\":\\\"\\/\\/images.3lift.com\\/3349230.jpg\\\",\\\"image_width\\\":1024,\\\"image_height\\\":536,\\\"heading\\\":\\\"Type&nbsp;In&nbsp;Your&nbsp;Name(or&nbsp;anyone\\'s)&nbsp;-&nbsp;This&nbsp;is&nbsp;Addicting\\\",\\\"caption\\\":\\\"Just&nbsp;type&nbsp;in&nbsp;a&nbsp;Name&nbsp;and&nbsp;select&nbsp;a&nbsp;State..&nbsp;Then&nbsp;brace&nbsp;yourself&nbsp;for&nbsp;what&nbsp;you&nbsp;might&nbsp;find&nbsp;on&nbsp;you&nbsp;or&nbsp;anyone&nbsp;else!\\\",\\\"clickthrough_url\\\":\\\"http:\\/\\/tracking.truthfinder.com\\/?a=353&amp;oc=27&amp;c=303&amp;dip=&amp;s1=Add&amp;s2=Shadow&amp;s3=Desktop\\\",\\\"img_server_params\\\":\\\"v=9\\\"}]});\";&lt;/script&gt;&lt;script&nbsp;src=\"//ib.3lift.com/ttj?inv_code=androidcentral_all_rightrail_btf\"&nbsp;data-auction-response-id=\"388859\"&gt;&lt;/script>"
            }
         ]
      }
   ],
   "cur":"USD"
}
```
