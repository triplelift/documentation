# Win Notifications

## Request Details
Win notifications are sent via HTTP POST to the nurl endpoint specified in the bid response. You must ensure that your server is able to accept POSTs. If needed, we can support HTTP GET (get in touch).

All win notifications are sent server-to-server. Win notifications are not sent via the client. You should use impression pixels to track the delivery of impressions. 

## Macro Replacement
The nurl may use any of the eligible macros below. These macros will be replaced with the appropriate data before the request is sent. 

### Eligible Macros
|Macro|Description|
|--- |--- |
|${AUCTION_ID}|ID of the bid request; from “id” attribute.|
|${AUCTION_BID_ID} |ID of the bid; from “id” attribute of the bid object.|
|${AUCTION_IMP_ID} |ID of the impression just won; from “impid” attribute.|
|${AUCTION_PRICE} |Settlement price using the same currency and units as the bid.|