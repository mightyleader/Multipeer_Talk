#[fit]Multipeer 
#[fit]Networking
#[fit]---------------------------
#[fit]Rob Stearn

^I'm Rob Stearn
iOS developer at Kaldor
Really excited to talk about multipeer networking
Apple made it so easy to use, but powerful enough to customise
I'd really like to see us make a lot more use of it

---
![fit, 270% filtered](http://cdn.macrumors.com/article/2011/03/23/095149-federighi_lion.jpg)
##[fit]History
##[fit]Overview
##[fit]Code
##[fit]Underneath
##[fit]!
##[fit]Future

^ so we're going to go through a little history, then run through an overview of the technology, get into some code and explore the different levels of API you can use. I'll outline a couple of gotchas and we'll finish with a look ahead.

---
#[fit]History
![fit, 170%, filtered](http://lowendmac.com/wp-content/uploads/mac-plus-and-laserwriter.jpg)

^ there's a history of peer-to-peer networking in Mac OS. 
Anybody here used a Mac since System 7? 
For those who haven't, you never experienced... the Chooser!

---
![fit](http://computers.mcbx.netne.net/apple/general/chooser.jpg)

^ click an item, see the local resources, pick a server or printer 
no IP addresses just names
worked within a LAN
lot of network traffic, slow
but it just worked

---
#[fit]Overview
![fit, 175%, filtered](http://www.overviewthemovie.com/wp-content/uploads/2012/12/MegaEarth01.jpg)

^Wireless
Session
Advertiser
Browser

---
#[fit]Multipeer networking

##[fit]sends data *from one device to another*

^sends data *from one device to another*

---
#[fit]Multipeer networking

##[fit]sends data *from one device to another*

##[fit]requires *no server infrastructure*

^ requires *no server infrastructure*

---
#[fit]Multipeer networking

##[fit]sends data *from one device to another*

##[fit]requires *no server infrastructure*

##[fit]sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

^ NOTE: does not require Bluetooth LE
Ad Hoc or P2P Wifi does not require you all be on a network together.
Required: Lightning on iOS, 2012 Mac.

---
#[fit]Multipeer networking

##[fit]sends data *from one device to another*

##[fit]requires *no server infrastructure*

##[fit]sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

##[fit]*bridges across interfaces*

^let's dive into that a bit

---
#[fit]Consider 3 iOS devices...

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/wifi.png)

^ here a device with wifi turned on, though not connected to a specific network

---
#[fit]Consider 3 iOS devices...

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/wifi.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/both.png)

^ here's a second one that has wifi on, also not on a network, and also bluetooth active


---
#[fit]Consider 3 iOS devices...

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/wifi.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/both.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bluetooth.png)

^ and here's a third that just got bluetooth on. Not connected just on.

---
#[fit]Consider 3 iOS devices...

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/wifi.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/both.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bluetooth.png)

1 connects to 2 via Bluetooth

3 connects to 2 via WiFi

1, 2 and 3 can all see each other

no direct connection between 1 & 3 required

^ all the devices connect to each other in a mesh


---
#[fit]Multipeer networking

##[fit]sends data *from one device to another*

##[fit]requires *no server infrastructure*

##[fit]sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

##[fit]*bridges across interfaces*

##[fit]can send data as *blob, URL or stream*

^ we'll look at in the Code section

---
#[fit]Multipeer networking

##[fit]sends data *from one device to another*

##[fit]requires *no server infrastructure*

##[fit]sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

##[fit]*bridges across interfaces*

##[fit]can send data as *blob, URL or stream*

##[fit]can ensure *order and delivery of data*

^ more on that later

---
#[fit]Multipeer networking

##[fit]sends data *from one device to another*

##[fit]requires *no server infrastructure*

##[fit]sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

##[fit]*bridges across interfaces*

##[fit]can send data as *blob, URL or stream*

##[fit]can ensure *order and delivery of data*

##[fit]API requires iOS7+

^but only for MCxxxx, the underlying tech is doable in at least 6
but networking is hardly new, right?
what are we really gaining here?

---
#[fit]Discovery
![fit, filtered](https://cdn2.iconfinder.com/data/icons/picons-essentials/71/map-512.png)

^the real win is in simplifying the process of discovery.
Finding and initiating a connection with optional encryption and authentication.
Discovery is underpinned by Bonjour.
What is Bonjour?

---
Bonjour is Apple’s proposal for zero-configuration networking over IP
Uses URLs with with the .local domain
Uses multicast DNS to discover services
Each peer responds with available services for it's own host name to map to address/port
Each peer self-assigns an IP in the ```169.254.xxx.xxx``` range and pings the network to check it's not in use. 

^I heard an urban myth that this is Microsofts IP subnet but can't confirm of deny it.

---
#[fit]Code
![fit, 100%, filtered](http://images4.alphacoders.com/270/27094.jpg)

^ so thats the theory, how does this work in practice?
Well first let's look at the objects involved...

---
#[fit]3 Objects:
##[fit]Session
##[fit]Browser
##[fit]Advertiser

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/plain.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/plain.png)

^ the multipeer API uses 3 objects, in most cases each device will run all three.
Firstly both devices will instantiate a session object that stays in memory for the life of the communication.

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/sender.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/recieve.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/plain.png)

One device will then create an Advertiser, to broadcast it's service.

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/sender.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/send.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/receiver.png)

This is picked by a Browser object on the other device and displayed as an available peer.
Tapping on the displayed name sends an invite back to the Advertiser to join Sessions.

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/plain.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bidirectional.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/plain.png)

^ When the invitation request is accepted a bi-directional communication session is open to send data.

---
#[fit]Session
#[fit]Browser
#[fit]Advertiser
#[fit]Transmission types
#[fit]Transmission modes

^now let's get deeper into these.
There's really 2 levels of API...
UI-driven and programmatic.

---
#Session / MCPeerID
## ```MCPeerID *peerID = [[MCPeerID alloc] initWithDisplayName:@"cocoadelica"];```

^ You start of creating the Session by making an MCPeerID to represent the local user.
You pass it a string for the name to display in Browsers.
MCPeerID is a key part of the process and we'll see it a lot.

---
#Session / Initialize
### ```self.session = [[MCSession alloc] initWithPeer:peerID securityIdentity:nil* encryptionPreference: MCEncryptionRequired**];```

^ now we can create the session itself, an instance of MCSession

---
#Session / Initialize
### ```self.session = [[MCSession alloc] initWithPeer:peerID securityIdentity:nil* encryptionPreference: MCEncryptionRequired**];```

####* optional Array with ```SecIdentityRef``` and ```SecCertificateRef``` items 

^ couple of things to point out, the security identity is an optional NSArray.
It would contain a SecIdentityRef struct which contains a certificate/key pair which idents the local user.


---
#Session / Initialize
### ```self.session = [[MCSession alloc] initWithPeer:peerID securityIdentity:nil* encryptionPreference: MCEncryptionRequired**];```

####* optional Array with ```SecIdentityRef``` and ```SecCertificateRef``` items 
####** choose from ```MCEncryptionRequired```, ```MCEncryptionOptional``` or ```MCEncryptionNone```

^ the third parameter specifies Encryption type. We'll go into why this can be important in the Gotcha's
but in short it decides when and how to allow connection depending on the receivers encryption choice.

---
#Session / Delegate
### ```self.session.delegate = self;```

## Delegate callbacks for the session let you handle data transmission events and state changes.

^The delegate gets callbacks about session status, data transfer and connection events. 

---
#Session / Details
##Foreground operation only 

^So some things to note about MCSession.
Foreground only on iOS

---
#Session / Details
##Foreground operation only 
##Disconnects on breakpoints

^tricky to debug sometimes as stopping at a breakpoint kills the session

---
#Session / Details
##Foreground operation only 
##Disconnects on breakpoints
##Good candidate for a singleton*

^ Yes I know - Singletons considered harmful -
The advice I was given in the labs at DubDub was to use a Singleton
for the controller

---
#[fit]Browser / Initialize
###```MCBrowserViewController *bvc = [[MCBrowserViewController alloc] initWithServiceType:(NSString *)serviceType session:(MCSession *)session];```

^ OK, to browse we have a few options depending on your requirements and bravery.
MCBrowserViewController is the vanilla option.

---
#[fit]Browser / Initialize
###```MCBrowserViewController *bvc = [[MCBrowserViewController alloc] initWithServiceType:(NSString *)serviceType session:(MCSession *)session];```
###[fit]use a reverse DNS notation for service type

^ init with a serviceType in reverse DNS form and your session object.
So what do you get for this?...

---
#[fit]Browser
#[fit]"Easy mode"
![220%,right](https://dl.dropboxusercontent.com/u/5034400/MPP/mcbrowservc.png)

^ you get a canned view controller you can display to browse for peers on your service.

---
#[fit]Browser
#[fit]"Easy mode"
##[fit]boxed solution
![220%,right](https://dl.dropboxusercontent.com/u/5034400/MPP/mcbrowservc.png)

---
#[fit]Browser
#[fit]"Easy mode"
##[fit]boxed solution
##[fit]minimal styling
![220%,right](https://dl.dropboxusercontent.com/u/5034400/MPP/mcbrowservc.png)

^tinting etc... 

---
#[fit]Browser
#[fit]"Easy mode"
##[fit]boxed solution
##[fit]minimal styling
##[fit]awesome!
![220%,right](https://dl.dropboxusercontent.com/u/5034400/MPP/mcbrowservc.png)

---
#[fit]Browser
#[fit]"Easy mode"
##[fit]boxed solution
##[fit]minimal styling
##[fit]awesome!
##[fit]a bit dull
![220%,right](https://dl.dropboxusercontent.com/u/5034400/MPP/mcbrowservc.png)

---
#[fit]Browser / Delegate
###[fit]```MCBrowserViewControllerDelegate```
###browserViewController:shouldPresentNearbyPeer:withDiscoveryInfo:
###browserViewControllerDidFinish:
###browserViewControllerWasCancelled:

^ theres some callbacks when you get when you become the delegate
enough to implement a block user function maybe and know when the user is done with the session
to dismiss and either continue into the session or cancel

---
#[fit]Browser / Details
##"Hard Mode"
###Build a custom UI
###Use ```MCNearbyServiceBrowser``` for data and callbacks

^ But we're not here for Easy right?
So we can select hard mode. 
Not so hard really. Make a custom UI for presenting peers.
Then use an MCNearbyServiceBrowser for the controller logic (explain)
But wait... that's not hardcore enough for you, right?
What if you want more control of lower level implementation?

---
#[fit]Browser / Details
##"Expert Mode"
###Build a custom UI
###*Subclass* ```MCNearbyServiceBrowser``` for data and callbacks

^ when you subclass, you can implement the underlying service either with NSNetServiceBrowser or 
with the C Bonjour API. 

---
#[fit]Browser / Details
example of NSNetServiceBrowser or C Bonjour code here

---
#[fit]Advertiser / Initialize
###```[[MCAdvertiserAssistant alloc] initWithServiceType:service discoveryInfo:discoveryDict session:mySession];```

---
#[fit]Advertiser / Initialize
###```[[MCAdvertiserAssistant alloc] initWithServiceType:service discoveryInfo:discoveryDict session:mySession];```

###```discoveryInfo``` is your opportunity to add additional info and context about your user.

^could be a real name, avatar etc... but keep it as small as possible
Size can affect discovery times.
I'll explain why later...

---
#[fit]Advertiser
expert

---
#[fit]Advertiser
code example

---
#[fit]Transmission Types
##Data
##Resource 
##Stream

---
#[fit]Transmission modes
##Reliable
##Unreliable

---
#[fit]Underneath
![fit, 185%, filtered](http://www.insider-london.co.uk/wp-content/uploads/2012/04/London-underground-walking_tours.jpg)

---
#[fit]Underneath
#[fit]```my_service._http._tcp.local. 120 IN SRV 0 0 515 mynameisprince.local```

---
#[fit]Underneath
#[fit]```my_service._http._tcp.local. 120 IN SRV 0 0 515 mynameisprince.local```
###[fit]```my_service``` is the Service Name.

---
#[fit]Underneath
#[fit]```my_service._http._tcp.local. 120 IN SRV 0 0 515 mynameisprince.local```
###[fit]```my_service``` is the Service Name.
###[fit]```_http.``` is the actual Service Type.

---
#[fit]Underneath
#[fit]```my_service._http._tcp.local. 120 IN SRV 0 0 515 mynameisprince.local```
###[fit]```my_service``` is the Service Name.
###[fit]```_http.``` is the actual Service Type.
###[fit]```515``` is the connection port number.

---
#[fit]Underneath
#[fit]```my_service._http._tcp.local. 120 IN SRV 0 0 515 mynameisprince.local```
###[fit]```my_service``` is the Service Name.
###[fit]```_http.``` is the actual Service Type.
###[fit]```515``` is the connection port number.
###[fit]```nynameisprince``` is the displayname from ```MCPeerID```

---
#[fit]!
![fit, 150% filtered](http://i.stack.imgur.com/nI9zx.jpg)
 
---
#Duplicates in the browser
![right](https://dl.dropboxusercontent.com/u/5034400/MPP/IMG_6028.PNG)

^if you exit and re-enter a session while a peer is browsing then
you'll find that you can (will) appear twice in their browser.

---
#Duplicates in the browser
###Issue: ```MCPeerID``` created each time ```MCSession``` is instantiated
![right](https://dl.dropboxusercontent.com/u/5034400/MPP/IMG_6028.PNG)

^this happens because if a new PeerID is used when creating the session.

---
#Duplicates in the browser
###Issue: ```MCPeerID``` created each time ```MCSession``` is instantiated
###Solution: Each peer should serialize its own ```MCPeerID``` with ```NSKeyedArchiver```, save it to disk and reuse for each new session.
![right](https://dl.dropboxusercontent.com/u/5034400/MPP/IMG_6028.PNG)

^thankfully MCPeerID is just a wrapper round a Very Big Number and is NSSecureCoding compliant.

---
#Invitation response times
![right](https://dl.dropboxusercontent.com/u/5034400/MPP/IMG_6029.PNG)

---
#Invitation response times
###Issue: Once an invitation is accepted there's a delay before connecting
![right](https://dl.dropboxusercontent.com/u/5034400/MPP/IMG_6029.PNG)

---
#Invitation response times
###Issue: Once an invitation is accepted there's a delay before connecting
###Solution: Encryption is on by default which adds overhead to initial connections. Consider using ```MCEncryptionNone```
![right](https://dl.dropboxusercontent.com/u/5034400/MPP/IMG_6029.PNG)

---
#Comparing MCPeerIDs
![right](https://dl.dropboxusercontent.com/u/5034400/MPP/apple-a7.png)

^in the WWDC14 video  recommends checking your peerID against incoming peerID
in the shouldPresentNearbyPeer browser delegate method.

---
#Comparing MCPeerIDs
###Issue: MCPeerID ```hash``` method returns it's unique ID number, as an ```NSUInteger```.
![right](https://dl.dropboxusercontent.com/u/5034400/MPP/apple-a7.png)

^anyone see the problem? 
A 32-bit client might receive a 64-bit peerID. Unique?

---
#Comparing MCPeerIDs
###Issue: MCPeerID ```hash``` method returns it's unique ID number, as an ```NSUInteger```.
###Solution: Truncate the returned ```hash``` to 32-bit for comparison.
![right](https://dl.dropboxusercontent.com/u/5034400/MPP/apple-a7.png)

^Engineering believe that the 32-bit portion is unique

---
#[fit]Future
![fit, 150%, filtered](http://thumbs.dreamstime.com/z/tunnel-futuristic-18180424.jpg)

---
##[fit]Multipeer for OS X in Yosemite

---
##[fit]Multipeer for OS X in Yosemite
##[fit]Common API with iOS

^uses NSViewController instead of UI

---
##[fit]Multipeer for OS X in Yosemite
##[fit]Common API with iOS
##[fit]But! Supports background operation

---
##[fit]Multipeer for OS X in Yosemite
##[fit]Common API with iOS
##[fit]But! Supports background operation
##[fit]However! Not Bluetooth

^I honestly don't know why
AdHoc Wifi is an alternative and
it does add Ethernet as an interface.
Which should mean it comes to AirDrop too.

---
##[fit]Multipeer for OS X in Yosemite
##[fit]Common API with iOS
##[fit]But! Supports background operation
##[fit]However! Not Bluetooth
##[fit]AirDrop rebuilt to use the new API

---
##[fit]Multipeer for OS X in Yosemite
##[fit]Common API with iOS
##[fit]But! Supports background operation
##[fit]However! Not Bluetooth
##[fit]AirDrop rebuilt to use the new API
##[fit]OS X > iOS AirDrop. *Finally*.

---
#[fit]End
![fit, 60%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/cocoa.png)
### created with Deckset
### http://www.decksetapp.com
### icons made with Sketch
### http://bohemiancoding.com/sketch/

---
#[fit]LocalTalk
##[fit]on the app store
##[fit]update in progress for iOS 8
##[fit]OS X version to follow
##[fit]free!
![right, fit, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/LTSplash.png)

---
#[fit]Questions?

---
#[fit] Rob Stearn
#[fit] @cocoadelica
#[fit] www.cocoadelica.co.uk
#[fit] robstearn@me.com

---
Resources
WWDC2013 video
WWDC2014 video
IANA service type list
Bonjour Overview PG
Multipeer Networking Framework Reference
NSNetServices and CFNetServices Programming Guide 
MultipperGroupChat sample code
Forums



