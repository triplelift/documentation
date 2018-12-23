# Direct Integrations

## Overview
The environment for DSPs consists of three discrete components - a real-time-bidding protocol, an API for automating certain functions, and a UI for setting up key components of your bidder, such as instances, bid requests filters, etc. 

## Testing your integration
We have a full sandbox environment that can simulate both real-time and API calls. To test your integration, ask your TripleLift contact for credentials for the DSP UI. Your credentials will be different between sandbox and production.

### Testing with the Sandbox UI
The sandbox DSP UI is available at http://sand-bidder.triplelift.net. Create an instance in the "Instances" tab, change the budget to be something non-zero in the "Profile" tab, then click on the "Testing" tab. 

## Setting up your usersync
TripleLift can store the mappings of your user IDs, which will be sent in the bid response. You may also store the mappings. The following are the scenarios for initiating and storing user ID mappings:

|Usersync initiator|Usersync stored by|Details|Implementation|
|--- |--- |--- |--- |
|TripleLift|TripleLift|TripleLift initiates the pixel call on an impression with one of our partner sites, the request will hit your usersync endpoint. If you know the user, you redirect back to us with your user ID. If you don't know the user, you simply return the pixel.|Using the production DSP UI, you would add a pixel endpoint that points to an endpoint on your system. This redirects to the xuid endpoint.|
|TripleLift|Partner|TripleLift initiates the pixel call on an impression with one of our partner sites, the request hits the TripleLift /getuid endpoint. This will redirect to your designated endpoint with the $UID field populated by our server.|Using the production DSP UI, you would add a pixel endpoint that points to the TripleLift /getuid endpoint, with the redirect populated to hit your endpoint and store the replaced $UID field as the TripleLift ID.|
|Partner|TripleLift|You either 1) drop a pixel that points directly to the TripleLift endpoint with your user ID populated, as required in the /xuid call, or 2) make a call to your server and redirect to the /xuid call with the ID populated.|This is setup and managed entirely by you.|
|Partner|Partner|You drop a pixel that points directly to the TripleLift /getuid endpoint, with a redirect pointing to your endpoint.|This is setup and managed entirely by you.|

For more details on the endpoints, please see [Usersync](usersync.md).

## Setting up your production bidder
When you are ready to go live in production, please work with your account manager to validate that your integration is complete. Your account manager will then provide production credentials for our DSP UI. You can login to the DSP UI at http://bidder.triplelift.com. **Note:** We strongly suggest you do any new production integrations (i.e. turning on a bidder for the first time) with low eligible bid percent and during the business hours US Eastern Time (UTC-4/5).

### Instances
Bidder instances are a way for TripleLift's datacenter to route to the appropriate endpoints. For each datacenter region (See Datacenters), you should create one or more instances that are physically close to that region. For example, if you have datacenters in Virginia and North Carolina, you could associate both with the TripleLift US East region by creating two instances - one for each datacenter.

You will only get traffic for the TripleLift datacenters to which you have associated instances. If you only want North American traffic, for example, only use US East and US West, and you will automatically not get any other traffic. You must set up at least one bidder instance to receive any traffic.

#### Datacenters
|Region|Location|AWS Region|
|--- |--- |--- |
|US East|Northern Virginia|us-east-1|
|US West|Northern California|us-west-1|
|Europe|Frankfurt|eu-central-1|
|Asia South|Singapore|ap-southeast-1|

### Profile
The profile is used to control several attributes about your bidder. Most important for getting started are the elements listed below:

Budget - the budget parameter is a fail-safe spend cap that you can use to ensure that your bidder doesn't accidentally have a period of run-away spend. The budget is not a guarantee that your bidder won't spend more than that amount - and it is not a contractual maximum - but TripleLift will throttle requests to your bidder as it approaches your budget, eventually sending none as the budget is exceeded. 

Bid Filtering - you may choose certain aspects about which requests you will see using bid filtering. For example, if you only want to see users for which you have a mapping stored in the TripleLift system, you would set non-mapped users to 0. 

## Creative Approval
When a bid is submitted, all the assets are validated. New assets must go through our creative approval process, which consists of the aspects described below. The bid automatically enters the assets into the approval process (if they're not already approved). There is no fee for creative approval, and it will generally complete within business 24 hours. 

Creative Approval status is available in the "Misc" tab of the DSP UI.

1. **Image Approval:** Each image must be validated both by an automated system and a human. Humans will block low-quality or profane images from the platform, and may reject those that are otherwise unfit for advertising. 
2. **Logo Approval:** Logos are not required in the bid, but when they are provided they must be approved. Generally, the same standards used for image approval will be used for logo approval. 
3. **Brand Approval:** The brand specified in the bid response is used for category-level and brand-level allow-lists and block-lists. Therefore, we must categorize every new brand that comes into the system. If you bid with a brand (or a phrasing of a brand) that we haven't seen, a human will automatically categorize and approve the brand. 

## Reporting
Spend and performance reports are available in the "Reporting" tab of the DSP UI. You may also get a nearly-real-time view of a few key stats about your bidder using the "General Stats" tab within the Reporting page.