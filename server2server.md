# Server to Server Integration

## Overview
The TripleLift Server to Server integration is compliant with OpenRTB 2.2 Specs. This defines the structure for bid (combination of price, identifier, assets, etc.) request format and bid response format. This documentation will highlight items required by our Server to Server integration. Below you'll see the typical structure of a bid request and bid response object.

## Endpoint
The Server to Server integration is initiated by a bid request from the client's server to this endpoint: "http://tlx.3lift.com/s2s/auction". TL will provide supplier id, which will be appended as query string in requests.

## Bid Request
### Bid Request Structure (Web)
Our server to server integration expects bid requests formatted with the following fields:

<table class="wrapped confluenceTable" data-mce-selected="1"><colgroup><col style="width: 90.0px"><col style="width: 90.0px"><col style="width: 90.0px"><col style="width: 116.0px"><col style="width: 371.0px"></colgroup><tbody><tr><th class="confluenceTh">Parameter</th><th class="confluenceTh" colspan="1">Sub Parameter</th><th colspan="1" class="confluenceTh">Sub Parameter</th><th colspan="1" class="confluenceTh">Scope</th><th class="confluenceTh">Notes</th></tr><tr><td class="confluenceTd">id</td><td class="confluenceTd" colspan="1"><br></td><td class="confluenceTd" colspan="1"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red"><span style="color: rgb(0,0,0)">Required</span></td><td class="confluenceTd">ID of the bid request</td></tr><tr><td class="confluenceTd" rowspan="8">imp</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="highlight-red confluenceTd" data-highlight-colour="red">Required</td><td colspan="1" class="confluenceTd"><span>The imp array will consist of one object</span></td></tr><tr><td class="confluenceTd" colspan="1">id</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="highlight-red confluenceTd" data-highlight-colour="red">Required</td><td class="highlight-red confluenceTd" data-highlight-colour="red">This will always be "1"<p><br></p></td></tr><tr><td class="confluenceTd" rowspan="4">banner</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="highlight-red confluenceTd" data-highlight-colour="red">Required</td><td colspan="1" class="confluenceTd">The TripleLift native implementation uses the Banner Object for native impressions</td></tr><tr><td colspan="1" class="confluenceTd">id</td><td colspan="1" class="confluenceTd">Optional</td><td colspan="1" class="confluenceTd">Client banner ID</td></tr><tr><td colspan="1" class="confluenceTd">w</td><td colspan="1" class="highlight-red confluenceTd" data-highlight-colour="red"><span>Required</span></td><td colspan="1" class="confluenceTd">Banner width</td></tr><tr><td colspan="1" class="confluenceTd"><p>h</p></td><td colspan="1" class="highlight-red confluenceTd" data-highlight-colour="red"><span>Required</span></td><td colspan="1" class="confluenceTd"><p>Banner height</p></td></tr><tr><td colspan="1" class="confluenceTd">tagid</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="highlight-red confluenceTd" data-highlight-colour="red">Optional</td><td colspan="1" class="confluenceTd">This is a slot level identifier.&nbsp;</td></tr><tr><td colspan="1" class="confluenceTd">bidfloor</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd">Optional</td><td colspan="1" class="confluenceTd">Hard floor. TripleLift will not return bids below this number.</td></tr><tr><td class="confluenceTd" rowspan="8">site</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-yellow confluenceTd" colspan="1" data-highlight-colour="yellow"><span>Recommended</span></td><td class="confluenceTd">The URL of the page the ad will be served onto.</td></tr><tr><td class="confluenceTd" colspan="1">id</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Client ID for site</td></tr><tr><td class="confluenceTd" colspan="1">domain</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Domain of site</td></tr><tr><td class="confluenceTd" colspan="1">page</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="highlight-yellow confluenceTd" data-highlight-colour="yellow"><span>Recommended</span></td><td colspan="1" class="confluenceTd">Full URL of site</td></tr><tr><td class="confluenceTd" rowspan="4">publisher</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Publisher of site</td></tr><tr><td class="confluenceTd" colspan="1">id</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Client ID for publisher</td></tr><tr><td colspan="1" class="confluenceTd">name</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Name of publisher</td></tr><tr><td class="confluenceTd" colspan="1">domain</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Publisher Domain</td></tr><tr><td class="confluenceTd" rowspan="13">device</td><td class="confluenceTd" colspan="1">-</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="highlight-yellow confluenceTd" data-highlight-colour="yellow">Recommended</td><td class="confluenceTd" colspan="1">User's device data</td></tr><tr><td class="confluenceTd" colspan="1">ua</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="highlight-yellow confluenceTd" data-highlight-colour="yellow"><span>Recommended</span></td><td colspan="1" class="confluenceTd">Browser user agent string</td></tr><tr><td class="confluenceTd" colspan="1">ip</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="highlight-yellow confluenceTd" data-highlight-colour="yellow"><span>Recommended</span></td><td colspan="1" class="confluenceTd">Device IP address when available</td></tr><tr><td class="confluenceTd" rowspan="6">geo</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd"><br></td></tr><tr><td class="confluenceTd" colspan="1">country</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Country. In&nbsp;<a class="external-link" href="http://unstats.un.org/unsd/tradekb/Knowledgebase/Country-Code" rel="nofollow">ISO3</a></td></tr><tr><td class="confluenceTd" colspan="1">region</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Region</td></tr><tr><td colspan="1" class="confluenceTd">metro</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Designated Metropolitan Area (DMA)</td></tr><tr><td colspan="1" class="confluenceTd">city</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">City</td></tr><tr><td colspan="1" class="confluenceTd">zip</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Zip Code</td></tr><tr><td colspan="1" class="confluenceTd">language</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="highlight-yellow confluenceTd" data-highlight-colour="yellow"><span>Recommended</span></td><td colspan="1" class="confluenceTd">User Language</td></tr><tr><td class="confluenceTd" colspan="1">make</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Device make ("Apple")</td></tr><tr><td class="confluenceTd" colspan="1">model</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Device Model ("iPhone")</td></tr><tr><td class="confluenceTd" colspan="1">os</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Operating System ("iOS 9")</td></tr><tr><td class="confluenceTd" rowspan="5">user</td><td colspan="1" class="confluenceTd">-</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red">Required</td><td class="confluenceTd" colspan="1">Information on user</td></tr><tr><td class="confluenceTd" colspan="1">id</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Client ID for user</td></tr><tr><td class="confluenceTd" colspan="1">buyeruid</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="highlight-red confluenceTd" data-highlight-colour="red">Required</td><td colspan="1" class="confluenceTd">TripleLift user ID (See <a href="#ServertoServerIntegration-usersync" data-anchor="usersync" data-linked-resource-default-alias="usersync" data-base-url="https://triplelift.atlassian.net/wiki" class="confluence-link">User Syncing</a>)</td></tr><tr><td class="confluenceTd" rowspan="2">geo</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Geo location of user</td></tr><tr><td class="confluenceTd" colspan="1">country</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Country of user</td></tr></tbody></table>

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

<table class="wrapped confluenceTable" resolved=""><colgroup><col style="width: 90.0px;"><col style="width: 90.0px;"><col style="width: 90.0px;"><col style="width: 116.0px;"><col style="width: 371.0px;"></colgroup><tbody><tr><th class="confluenceTh">Parameter</th><th colspan="1" class="confluenceTh">Sub Parameter</th><th colspan="1" class="confluenceTh">Sub Parameter</th><th colspan="1" class="confluenceTh">Scope</th><th class="confluenceTh">Notes</th></tr><tr><td class="confluenceTd">id</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red"><span style="color: rgb(0,0,0);">Required</span></td><td class="confluenceTd">ID of the bid request</td></tr><tr><td rowspan="7" class="confluenceTd">imp</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red">Required</td><td colspan="1" class="confluenceTd"><span>The imp array will consist of one object</span></td></tr><tr><td colspan="1" class="confluenceTd">id</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red">Required</td><td class="confluenceTd">This will always be "1"<p><br></p></td></tr><tr><td rowspan="4" class="confluenceTd">banner</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red">Required</td><td colspan="1" class="confluenceTd">The TripleLift native implementation uses the Banner Object for native impressions</td></tr><tr><td colspan="1" class="confluenceTd">id</td><td colspan="1" class="confluenceTd">Optional</td><td colspan="1" class="confluenceTd">Client banner ID</td></tr><tr><td colspan="1" class="confluenceTd">w</td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red"><span>Required</span></td><td colspan="1" class="confluenceTd">Banner width</td></tr><tr><td colspan="1" class="confluenceTd"><p>h</p></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red"><span>Required</span></td><td colspan="1" class="confluenceTd"><p>Banner height</p></td></tr><tr><td colspan="1" class="confluenceTd">bidfloor</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd">Optional</td><td colspan="1" class="confluenceTd">Hard floor. TripleLift will not return bids below this number.</td></tr><tr><td rowspan="13" class="confluenceTd">app</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-yellow confluenceTd" colspan="1" data-highlight-colour="yellow"><span>Recommended</span></td><td class="confluenceTd">The URL of the page the ad will be served onto.</td></tr><tr><td colspan="1" class="confluenceTd">id</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Client ID for site</td></tr><tr><td colspan="1" class="confluenceTd">name</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-yellow confluenceTd" colspan="1" data-highlight-colour="yellow">Recommended</td><td colspan="1" class="confluenceTd">Application name (may be masked at publisher’s request).</td></tr><tr><td colspan="1" class="confluenceTd">domain</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Domain of the application (e.g., “<a href="http://mygame.foo.com" class="external-link" rel="nofollow">mygame.foo.com</a>”).</td></tr><tr><td colspan="1" class="confluenceTd">cat</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Array of IAB content categories for the overall application.</td></tr><tr><td colspan="1" class="confluenceTd">bundle</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red"><span>Required</span></td><td colspan="1" class="confluenceTd">Application bundle or package name (e.g., com.foo.mygame). This is intended to be a unique ID across multiple exchanges.</td></tr><tr><td colspan="1" class="confluenceTd">storeurl&nbsp;</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">i.e. "<a href="https://itunes.apple.com/app/id50234631398" class="external-link" rel="nofollow">https://itunes.apple.com/app/id50234631398</a>"</td></tr><tr><td colspan="1" class="confluenceTd">ext</td><td colspan="1" class="confluenceTd">iTunesId</td><td colspan="1" class="confluenceTd">Optional</td><td colspan="1" class="confluenceTd">i.e. "iTunesId": "50423631398"</td></tr><tr><td colspan="1" class="confluenceTd">keywords</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd">Optional</td><td colspan="1" class="confluenceTd">List of keywords describing this app in a comma separated string.</td></tr><tr><td rowspan="4" class="confluenceTd">publisher</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Publisher of site</td></tr><tr><td colspan="1" class="confluenceTd">id</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Client ID for publisher</td></tr><tr><td colspan="1" class="confluenceTd">name</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Name of publisher</td></tr><tr><td colspan="1" class="confluenceTd">domain</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Publisher’s highest level domain name, for example “<a href="http://foopub.com" class="external-link" rel="nofollow">foopub.com</a>”.</td></tr><tr><td rowspan="14" class="confluenceTd">device</td><td colspan="1" class="confluenceTd">-</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-yellow confluenceTd" colspan="1" data-highlight-colour="yellow">Recommended</td><td colspan="1" class="confluenceTd">User's device data</td></tr><tr><td colspan="1" class="confluenceTd">ifa</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red"><span style="color: rgb(0,0,0);">Required</span></td><td colspan="1" class="confluenceTd">Native identifier for advertisers; an opaque ID assigned by the device or browser for use as an advertising identifier.</td></tr><tr><td colspan="1" class="confluenceTd">ua</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-yellow confluenceTd" colspan="1" data-highlight-colour="yellow"><span>Recommended</span></td><td colspan="1" class="confluenceTd">Browser user agent string</td></tr><tr><td colspan="1" class="confluenceTd">ip</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-yellow confluenceTd" colspan="1" data-highlight-colour="yellow"><span>Recommended</span></td><td colspan="1" class="confluenceTd">Device IP address when available</td></tr><tr><td rowspan="6" class="confluenceTd">geo</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd"><br></td></tr><tr><td colspan="1" class="confluenceTd">country</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Country. In&nbsp;<a class="external-link" href="http://unstats.un.org/unsd/tradekb/Knowledgebase/Country-Code" rel="nofollow">ISO3</a></td></tr><tr><td colspan="1" class="confluenceTd">region</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Region</td></tr><tr><td colspan="1" class="confluenceTd">metro</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Designated Metropolitan Area (DMA)</td></tr><tr><td colspan="1" class="confluenceTd">city</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">City</td></tr><tr><td colspan="1" class="confluenceTd">zip</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Zip Code</td></tr><tr><td colspan="1" class="confluenceTd">language</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-yellow confluenceTd" colspan="1" data-highlight-colour="yellow"><span>Recommended</span></td><td colspan="1" class="confluenceTd">User Language</td></tr><tr><td colspan="1" class="confluenceTd">make</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Device make ("Apple")</td></tr><tr><td colspan="1" class="confluenceTd">model</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Device Model ("iPhone")</td></tr><tr><td colspan="1" class="confluenceTd">os</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Operating System ("iOS 9")</td></tr><tr><td rowspan="4" class="confluenceTd">user</td><td colspan="1" class="confluenceTd">-</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red">Required</td><td colspan="1" class="confluenceTd">Information on user</td></tr><tr><td colspan="1" class="confluenceTd">id</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Client ID for user</td></tr><tr><td rowspan="2" class="confluenceTd">geo</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Geo location of user</td></tr><tr><td colspan="1" class="confluenceTd">country</td><td colspan="1" class="confluenceTd"><span>Optional</span></td><td colspan="1" class="confluenceTd">Country of user</td></tr></tbody></table>

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

<table class="wrapped confluenceTable" resolved=""><colgroup><col><col><col><col><col></colgroup><tbody><tr><th class="confluenceTh">Parameter</th><th colspan="1" class="confluenceTh">Sub Parameter</th><th colspan="1" class="confluenceTh">Sub Parameter</th><th colspan="1" class="confluenceTh">Scope</th><th class="confluenceTh">Notes</th></tr><tr><td class="confluenceTd">id</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red"><span style="color: rgb(0,0,0);">Required</span></td><td class="confluenceTd">ID of the bid request</td></tr><tr><td rowspan="6" class="confluenceTd">seatbid</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red">Required</td><td colspan="1" class="confluenceTd">Array of seatbid objects</td></tr><tr><td rowspan="5" class="confluenceTd">bid</td><td colspan="1" class="confluenceTd"><br></td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red">Required</td><td class="confluenceTd">Array of bid objects</td></tr><tr><td colspan="1" class="confluenceTd">id</td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red">Required</td><td colspan="1" class="confluenceTd">ID for the bid object chosen by the bidder for tracking and debugging purposes. Useful when multiple bids are submitted for a single impression for a given seat.&nbsp;</td></tr><tr><td colspan="1" class="confluenceTd">impid</td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red"><span>Required</span></td><td colspan="1" class="confluenceTd"><span>This will always be "1"</span></td></tr><tr><td colspan="1" class="confluenceTd">price</td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red"><span>Required</span></td><td colspan="1" class="confluenceTd">Bid price in CPM</td></tr><tr><td colspan="1" class="confluenceTd">adm</td><td class="highlight-red confluenceTd" colspan="1" data-highlight-colour="red"><span>Required</span></td><td colspan="1" class="confluenceTd">Ad Markup. This can just be placed on the page where the ad should render.</td></tr><tr><td class="confluenceTd">cur</td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd"><br></td><td colspan="1" class="confluenceTd">Optional</td><td colspan="1" class="confluenceTd">Currency of bid response</td></tr></tbody></table>

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
