# FAQs

## Auction
### How much time does my bidder have to respond to a TL auction?

We give upstream bidders 100ms to respond to TL before timing the bidder out. Remember, TripleLift prides itself on quality for suppliers and advertisers. Part of the quality initiative is responding to ad calls fast. 

## Integration
### How do nURLs / win notifications work?

Per oRTB specs, TripleLift supports server to server HTTP POST win notices. HTTP POST allows bid requests to accommodate greater payloads than HTTP GET and facilitate the use of binary representations. If needed, we can support HTTP GET (get in touch).

### How do usersyncs work?

See here: [Usersync](usersync.md)

### How does the secure flag work?

The secure flag (“secure”: 1) will only be sent in the bid request if the request is for a page that is secure. The secure flag will not be sent in a bid request if the page is not secure.

If the secure flag is sent in the bid request, bid responses must contain secure urls for anything that may render client side, e.g. impression trackers, jstrackers, click trackers, view trackers, and image and logo url (if CSR/retargeter). 

### Can you support gzip?

Yes.

### Are there any nuances to TripleLift's oRTB implementation?

Yes. The "ver" field is passed as an integer vs. a string.

### Is adomain required?

Yes.

### How is Viewability supported?

Viewability can be tracked either by passing an image tracker via the "viewtracker" field or a viewability tag via the "jstrackers" field.

1. The "viewtracker" field should only be used to pass image pixels that will be fired when TripleLift determines the ad is in view.
2. The "jstracker" field should be used to track viewability with JS tags from approved vendors.

If the integration is via oRTB 2.3, the JS viewability tag should be valid HTML and wrapped in <script> tags. If the integration is via oRTB 2.2, only the JS viewability url should be sent in the "jstracker" field (no HTML markup).  

Approved list of venders & domains in the Javascript Trackers section.

### Are deals supported? What types? How are they integrated into bid requests and bid responses?

TripleLift supports both PMP and targeting deals. For an integration to support deals, the "pmp" object in the bid request must be processed and the "dealid" field must be present in the bid response.

## Assets
### Does TripleLift enforce image restrictions?
Yes, TripleLift will enforce image size and ratio restrictions.

### How does TripleLift enforce image ratio restrictions?
TripleLift will enforce a ratio restriction on the images to ensure that the image can be presented in the most flattering way to consumers. TL computer vision works better in when the image submitted is large and closer in ratio to the target image.

Let's talk mechanics. Every TripleLift placement has an image associated with it. This target image location has dimensions. These dimensions are passed to you in the bid request. Your response must fit within a certain range to qualify for winning the impressions – particularly, your image must not be outside 0.5x to 2.0x the ratio of the target image.

Here's an example. Say the target image location has dimensions (width x height) of 750x500. The image ratio (width/height) is 1.5 (750/500 = 3/2 = 1.5). To be eligible, you must submit an image that is inside of 0.5x or 2.0x this ratio. Thus, your image ratio must fall within [.75, 3].

### What are the limits to the native assets?
TripleLift will send in the bid request limits for certain native components.

<table class="wrapped confluenceTable tablesorter tablesorter-default" role="grid" resolved=""><colgroup><col><col></colgroup><thead><tr role="row" class="tablesorter-headerRow"><th class="confluenceTh tablesorter-header sortableHeader tablesorter-headerUnSorted" data-column="0" tabindex="0" scope="col" role="columnheader" aria-disabled="false" unselectable="on" aria-sort="none" aria-label="Asset: No sort applied, activate to apply an ascending sort" style="user-select: none;"><div class="tablesorter-header-inner">Asset</div></th><th class="confluenceTh tablesorter-header sortableHeader tablesorter-headerUnSorted" data-column="1" tabindex="0" scope="col" role="columnheader" aria-disabled="false" unselectable="on" aria-sort="none" aria-label="Length: No sort applied, activate to apply an ascending sort" style="user-select: none;"><div class="tablesorter-header-inner">Length</div></th></tr></thead><tbody aria-live="polite" aria-relevant="all"><tr role="row"><td class="confluenceTd">advertiser name max length</td><td class="confluenceTd">25</td></tr><tr role="row"><td class="confluenceTd">description / caption max length</td><td class="confluenceTd">200</td></tr><tr role="row"><td colspan="1" class="confluenceTd">title max length</td><td colspan="1" class="confluenceTd">75</td></tr><tr role="row"><td class="confluenceTd">logo min width</td><td class="confluenceTd">25</td></tr><tr role="row"><td colspan="1" class="confluenceTd">logo min height</td><td colspan="1" class="confluenceTd">25</td></tr></tbody></table>

### How is format eligibility sent in the bid request?
TripleLift will send signals about the eligible formats to the DSP. This differs according to spec so please visit the 2.3 documentation page or 2.2 extension documentation page for more information.

## Javascript Trackers
### Which javascript trackers are allowed?

#### Viewability
##### IAS
pixel.adsafeprotected.com

integralads.com

##### MOAT
moat.com

moatads.com

js.moatads.com

z.moatads.com

##### DoubleVerify 
doubleverify.com

cdn.doubleverify.com

##### ComScore 
comscore.com

sb.voicefive.com

sb.scorecardresearch.com

##### WhiteOps
s.dessaly.com

s.acexedge.com

#### OBA/AdChoices
ghostery.com

truste.com

choices.truste.com 

c.betrad.com

cdn.go.affec.tv
