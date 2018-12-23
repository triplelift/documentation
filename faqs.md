# FAQs

## Exchange & Auction Mechanics
### How much time does my bidder have to respond to a TL auction?
We give upstream bidders 100ms to respond to TL before timing the bidder out. Remember, TripleLift prides itself on quality for suppliers and advertisers. Part of the quality initiative is responding to ad calls fast. HTTP keep-alive is strongly recommended. A new connection should not be required for each bid request. Our exchange prefers long lasting connections and will roll over old connections after a certain long period of time.

Each connected bidder instance should be physically located in region you tell us it is. An instance registered to receive traffic from US_EAST should not be located in US_WEST, as the effects of latency will put your bidder at a significant disadvantage.

Bidder instance health is evaluated periodically. Depending on the results of the health check, additional connections may be added. If a bidder instance is determined to be unhealthy (i.e. it rejects most or all connections, or is timing out at a high rate), the exchange instance will start to sample the bids. Once the bidder instance is deemed healthy again, the sampling will be removed. The sampling is done as a precaution to prevent our exchange instances from consuming resources and time on unhealthy bidder instances. Details regarding timeouts and volume are available in the "Reporting" tab of the DSP UI.

## Integration
### How do nURLs / win notifications work?
Per oRTB specs, TripleLift supports server to server HTTP POST win notices. HTTP POST allows bid requests to accommodate greater payloads than HTTP GET and facilitate the use of binary representations. See here: [Notifys](notifys.md)

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

If the integration is via oRTB 2.3, the JS viewability tag should be valid HTML and wrapped in &lt;script&#47;&gt; tags. If the integration is via oRTB 2.2, only the JS viewability url should be sent in the "jstracker" field (no HTML markup).  

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

|Asset|Length|
|--- |--- |
|advertiser name max length|25|
|description / caption max length|200|
|title max length|75|
|logo min width|25|
|logo min height|25|

### How is format eligibility sent in the bid request?
TripleLift will send signals about the eligible formats to the DSP. This differs according to spec so please visit the 2.3 documentation page or 2.2 extension documentation page for more information.