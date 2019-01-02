# Wrapper Integration

## 1. Prebid.js 0.34.1
(compatible with any wrapper pre 1.0)
### Prebid wrapper setup
For each ad unit you would like to send to our header bidder, include the following object in the bids array:

```json
{
    "bidder": "triplelift",
    "params": {
        "inventoryCode": "[INVENTORY_CODE]",
        "floor": [FLOOR]
    }
}
```

Where `[INVENTORY_CODE]` is replaced with the inventory code of the TripleLift placement that will serve in that ad slot. Including a floor price is optional. For example, for a mid_article unit, the object in the bids array might be be:

```json
{
    "bidder": "triplelift",
    "params": {
        "inventoryCode": "bgr_mid_article",
        "floor": 1.0
    }
}
```
Your full list of placements can be found in our console.

### DFP creative setup
For each TripleLift line item, please set up the following creative to serve:

```html
<script>
    top.window.pbjs.renderAd(document, '%%PATTERN:hb_adid%%');
</script>
```

## 2. Prebid.js 1.0+
(compatible with any wrapper post 1.0) 
### Prebid wrapper setup
For each ad unit you would like to send to our header bidder, include the following object in the bids array:

```json
{
    "bidder": "triplelift",
    "params": {
        "inventoryCode": "[INVENTORY_CODE]",
        "floor": [FLOOR]
    }
}
```

Where `[INVENTORY_CODE]` is replaced with the inventory code of the TripleLift placement that will serve in that ad slot. Including the ad unit's floor price is optional. For example, for a mid_article unit, the object in the bids array might be be:

```json
{
    "bidder": "triplelift",
    "params": {
        "inventoryCode": "bgr_mid_article",
        "floor": 1.0
    }
}
```

Your full list of placements can be found in our console.

**Note:** If you plan to utilize SRA (single request architecture) with Prebid 1.+ please make sure this method -- `googletag.pubads().enableSingleRequest()` -- is being called in your tag. It should look something like this:

```html
<script>
    googletag.cmd.push(function() {
        googletag.defineSlot('/12345678/adunit_name', [300, 600], 
            'div-gpt-ad-1234567890-1').addService(googletag.pubads());
        googletag.pubads().enableSingleRequest();
        googletag.enableServices();
    });
</script>
```

### DFP creative setup
(Please make sure your key/value pairs are set up properly http://prebid.org/dev-docs/bidders/triplelift.html)

For each TripleLift line item, please set up the following creative to serve:

```html
<script>
    top.window.pbjs.renderAd(document, '%%PATTERN:hb_adid%%');
</script>
```

## 3. Custom Wrapper
(TripleLift API spec)
### Bid Request

- **Method:** GET
- **Endpoint:** tlx.3lift.com/header/auction
- **Content type:** URL encoded

The query string parameters to append are:

|Parameter|Scope|Notes|
|--- |--- |--- |
|callback|Required|The function that will wrap the jsonp response from the endpoint. For example, in prebid our function that handles the response from our API is pbjs.TLCB so we set this parameter as callback=pbjs.TLCB.|
|inv_code|Required|The inventory code for the placement (e.g., publishername_article_business).|
|referrer|Required|The URL of the page the ad will be served onto.|
|size|Optional|The dimensions of the ad unit we are serving into. We will return the value you pass us.|
|callback_id|Optional|For internal wrapper tracking purposes. We will return the value you pass us.|

```javascript
$.ajax({
    url: "http://tlx.3lift.com/header/auction",
    type: "GET",
    crossDomain: true,
    xhrFields: {
        withCredentials: true
    },
    data: {
        "callback": "triplelift_callback",
        "callback_id": 6650,
        "inv_code": "pubname_main_feed",
        "referrer": "http://www.pubname.com/page"
    },
    dataType: "jsonp"
});
```

### Bid Response
#### With bid returned
When we have a bid for the impression the response will return the following parameters:

|Field|Notes|
|--- |--- |
|cpm|CPM of the bid returned.|
|width|Width of the size parameter requested.|
|height|Height of the size parameter requested.|
|ad|Script element to be appended to the DOM in the location that our ad should load in the event we win. This ad markup can be written to an iframe or appended directly to the top window.|
|callback_id|The callback_id sent in the initial bid request.|

#### No bid or demand disabled
When there is no bid or the tag is not live we will return the following parameters:

|Field|Notes|
|--- |--- |
|status|"no bid", "demand_disabled" (tag is not live)|
|callback_id|The callback_id sent in the initial bid request.|

To receive a bid response for a demand disabled placement, append `&tripleliftTest=true` to the `referrer` URL.

## 4. Standalone User Sync
In a standard Header Bidding setup, TripleLift’s tag is never dropped on the page which limits our ability run a user sync call. This means that we are unable to pass audience data onto our 3rd party programmatic partners and in turn limit the ability for buyers to find their targeted audience segments on your site.  

By adding the Standalone User Sync tag, we’d be able to collect that audience data and pass it along to our 3rd party partners to gain higher match rates and drive more value for buyers. More value, higher bids!  

The implementation is very straightforward and can be facilitated in two ways:

An iframe:  

`<iframe src="//ib.3lift.com/sync" style="display:none" width="0" height="0"></iframe>`

A javascript tag:  

`<script src="//ib.3lift.com/sync.js"></script>`

These should sit somewhere in the html `<body>`.