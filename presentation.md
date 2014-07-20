#[fit]Multipeer 
#[fit]Networking
#[fit]---------------------------
#[fit]Rob Stearn
![fit, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/pavlo.JPG)

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

^So let's get an idea of what multipeer networking means now

---
#[fit]Multipeer networking

##[fit]sends data *from one device to another*

^sends data *from one device to another*

---
#[fit]Multipeer networking

##[fit]sends data *from one device to many*

##[fit]requires *no server infrastructure*

^ requires *no server infrastructure*

---
#[fit]Multipeer networking

##[fit]sends data *from one device to many*

##[fit]requires *no server infrastructure*

##[fit]sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

^ NOTE: does not require Bluetooth LE
P2P Wifi does not require you all be on a network together.
P2P Wifi Requires: Lightning on iOS, 2012 Mac.

---
#[fit]Multipeer networking

##[fit]sends data *from one device to many*

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

##[fit]sends data *from one device to many*

##[fit]requires *no server infrastructure*

##[fit]sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

##[fit]*bridges across interfaces*

##[fit]can send data as *Message, File or Stream*

^ we'll look at in the Code section

---
#[fit]Multipeer networking

##[fit]sends data *from one device to many*

##[fit]requires *no server infrastructure*

##[fit]sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

##[fit]*bridges across interfaces*

##[fit]can send data as *Message, File or Stream*

##[fit]can ensure *order and delivery of data*

^ more on that later

---
#[fit]Multipeer networking

##[fit]sends data *from one device to many*

##[fit]requires *no server infrastructure*

##[fit]sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

##[fit]*bridges across interfaces*

##[fit]can send data as *Message, File or Stream*

##[fit]can ensure *order and delivery of data*

##[fit]API requires iOS7+

^but only for this API, the underlying tech is in at least 5
And networking is hardly new, right?
So what are we really gaining here?
It's about Discovery...

---
#[fit]Discovery
![fit, filtered](https://cdn2.iconfinder.com/data/icons/picons-essentials/71/map-512.png)

^the real win is in simplifying the process of discovery.
Finding peers and initiating a connection to them
Discovery is underpinned by Bonjour.
What is Bonjour?

---
#[fit]Discovery

##[fit]Zero-configuration networking over IP

^Apples submission for the standard

---
#[fit]Discovery

##[fit]Zero-configuration networking over IP
##[fit]Every peer acts as a DNS for itself

^the mDNS responder daemon 

---
#[fit]Discovery

##[fit]Zero-configuration networking over IP
##[fit]Every peer acts as a DNS for itself
##[fit]Uses URLs with with the .local domain

---
#[fit]Discovery

##[fit]Zero-configuration networking over IP
##[fit]Every peer acts as a DNS for itself
##[fit]Uses URLs with with the .local domain
##[fit]Uses multicast DNS to discover services

^Each peer responds with it's name, port and service list

---
#[fit]Discovery

##[fit]Zero-configuration networking over IP
##[fit]Every peer acts as a DNS for itself
##[fit]Uses URLs with with the .local domain
##[fit]Uses multicast DNS to discover services
##[fit]Peers self-assign an IP in the ```169.254.xxx.xxx``` range

^I heard an urban myth that this is Microsofts IP subnet but can't confirm/deny it.

---
#Discovery APIs
![fit, 100%, original](https://dl.dropboxusercontent.com/u/5034400/MPP/APIStack.png)

^sits at the top of a stack of tech.
We'll mention when you can dip down into NSNetService.
DNS-SD is a C-Based API you can use to make Bonjour clients for your 'Green Friends'

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
One device will then create an Advertiser, to broadcast it's service.
 
---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/sender.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/recieve.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/plain.png)

^ This is picked by a Browser object on the other device and displayed as an available peer.

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/sender.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/send.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/receiver.png)

^Tapping on the displayed name sends an invite back to the Advertiser to join Sessions.

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/plain.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bidirectional.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/plain.png)

^ When the invitation request is accepted a bi-directional communication session is open to send data.
The session object on each end stores a list of the connected peers.
That's not the whole story though, and this is the cool bit.

---
#[fit]Session, Browser and Advertiser

![60%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/plain.png)
![30%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bidirectional.png)
![60%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/plain.png)
![30%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bidirectional.png)
![60%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/plain.png)

^ If one of those devices is already communicating with a peer
then they all can see each other in a mesh
And as I mentioned before, it bridges across network interfaces.
NOTE: as son as you connect to a peer AUTO CONNECTS their connected peers.

---
#[fit]Session, Browser and Advertiser

![35%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bluetooth.png)
![20%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bidirectional.png)
![35%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/both.png)
![20%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bidirectional.png)
![35%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/wifi.png)
![20%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bidirectional.png)
![35%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bluetooth.png)
![20%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/bidirectional.png)
![35%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/both.png)

^ There is a limit though

---
#[fit]kMCSessionMaximumNumberOfPeers

^setable property of the session
the problem is what is that limit?

---
#[fit]kMCSessionMaximumNumberOfPeers

##[fit]Bluetooth: 8

^if bluetooth is involved then it's 8
as this is the max point-point connections
allowed by the CoreBluetooth API

---
#[fit]kMCSessionMaximumNumberOfPeers

##[fit]Bluetooth: 8
##[fit]Wi-fi: 8?

^since the property doesn't distinguish interfaces 
then it's the same for wifi. 
There's uncertainty what this means for WiFi only mesh networks.
Young API, still improving.
Look for this to change. Esp. OS X

---

#[fit]Session
#[fit]Browser
#[fit]Advertiser
#[fit]Sending data

^now let's get deeper into these.
There's really 2 levels of API...
UI-driven and programmatic.

---
#Session / MCPeerID
## ```MCPeerID *peerID = [[MCPeerID alloc] initWithDisplayName:@"cocoadelica"];```

^ You start of creating the Session by making an MCPeerID to represent the local user.
You pass it a string for the name to display in Browsers.
MCPeerID is a key part of the process and we'll see it a lot.
It's used in both levels of API

---
#Session / Initialize
### ```self.session = [[MCSession alloc] initWithPeer:peerID securityIdentity:nil* encryptionPreference: MCEncryptionRequired**];```

^ now we can create the session itself, an instance of MCSession
Likewise also used in both API levels

---
#Session / Initialize
### ```self.session = [[MCSession alloc] initWithPeer:peerID securityIdentity:nil* encryptionPreference: MCEncryptionRequired**];```

####* optional Array with ```SecIdentityRef``` and ```SecCertificateRef``` items 

^ couple of things to point out, the security identity is an optional NSArray.
Provides authorisation 
It would contain a SecIdentityRef struct which contains a certificate/key pair which idents the local user.
Session gets delegate callback when it receives a certifcate chain


---
#Session / Initialize
### ```self.session = [[MCSession alloc] initWithPeer:peerID securityIdentity:nil* encryptionPreference: MCEncryptionRequired**];```

####* optional Array with ```SecIdentityRef``` and ```SecCertificateRef``` items 
####** choose from ```MCEncryptionRequired```, ```MCEncryptionOptional``` or ```MCEncryptionNone```

^ the third parameter specifies Encryption type. We'll go into why this can be important in the Gotcha's
but in short it decides when and how to allow connection depending on the receivers encryption choice.

---
#Session / Delegate

####[fit] ```didChangeState:```
####[fit] ```didReceiveCertificate:```
####[fit] ```didReceiveData:```
####[fit] ```didStartReceivingResourceWithName:```
####[fit] ```didFinishReceivingResourceWithName:```
####[fit] ```didReceiveStream:```


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
###[fit]Use a 1-15 character string for service type
###[fit]Check IANA domain naming conventions

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
enough to implement a block user function, dismiss and either continue into the session or cancel
discoveryInfo is additional data about the peer, more shortly.
But we're not here for Easy right?

---
#[fit]Browser / Details
##"Hard Mode"
###Build a custom UI
###Use ```MCNearbyServiceBrowser``` for data and callbacks

^ So we can select hard mode. 
Now we're into the more programatic API
Not so hard. Make a custom UI for presenting peers.
Then use an MCNearbyServiceBrowser for the controller logic (explain)

---
#[fit]Browser / 'Hard Mode'
###[fit]```MCNearbyServiceBrowserDelegate```
###didNotStartBrowsingForPeers:
###foundPeer:withDiscoveryInfo:
###lostPeer:
	
^Up to you to handle what happens when a peer is 
found/lost, and any errors in starting the discovery process.
note discoveryInfo again.

---
#[fit]Browser / Details
##"Expert Mode"
###Build a custom UI for browsing
###*Subclass* ```MCNearbyServiceBrowser``` for data and callbacks, implement with either NSNetServiceBrowser or the C Bonjour API.

^ when you subclass, you can implement the underlying service either with NSNetServiceBrowser or 
with the C Bonjour API.
re-inventing the wheel to do this for Bonjour browsing.
allow to browse for non=Bonjour services.

---

#[fit]Advertiser / Initialize
###```[[MCAdvertiserAssistant alloc] initWithServiceType:service discoveryInfo:discoveryDict session:mySession];```

---
#[fit]Advertiser / Initialize
###```[[MCAdvertiserAssistant alloc] initWithServiceType:service discoveryInfo:discoveryDict session:mySession];```

###```discoveryInfo``` is your opportunity to add additional info and context about your user.

^could be a real name, metadata etc...

---
#[fit]Advertiser / discoveryInfo
###NSDictionary
###Keys and Values must be of type NSString
###Max size per key/value pair of 256 bytes

^Warning: adding data to this can increase the discovery times.
don't go Base64 encoding images

---
#[fit]Advertiser / Details
##"Hard Mode"
###Build a custom UI for invitations
###Use ```MCNearbyServiceAdvertiser``` advertising & callbacks

---
#[fit]Advertiser / 'Hard Mode'
###[fit]```MCNearbyServiceAdvertiserDelegate```
###```advertiser: didNotStartAdvertisingPeer: ```
###```advertiser: didReceiveInvitationFromPeer: withContext: invitationHandler:```

---
#[fit]Sending data
![fit, 130%, filtered](https://www.mandiant.com/blog/wp-content/ammo/Binary-Data_Pic-3.png)
##Messages
##Resources
##Streams

^Apple provide 3 APIs for sending data to peers

---
#[fit]Messages
###Data with known bounds, serialized into ```NSData``` objects, sent atomically
###```sendData: toPeers: withMode: error:```
###```session: didReceiveData: fromPeer:```

^examples include Text messages, Bezier paths anything small and serializable
note the mode option...

---
###[fit]Messages can be sent as either:
###```MCSessionSendDataReliable``` which guarantees delivery and order

^handles retransmission if missed, dropped or corrupted and ensures delivery order
adds overhead to communication use if data integrity is important like text messaging

---
###[fit]Messages can be sent as either:
###```MCSessionSendDataReliable``` which guarantees delivery and order
###```MCSessionSendDataUnreliable``` which does not

^Unreliable mode where performance is more important.

---
###[fit]Messages can be sent as either:
###```MCSessionSendDataReliable``` which guarantees delivery and order
###```MCSessionSendDataUnreliable``` which does not
###Analagous to TCP/UDP

^If this sounds familiar then thats because it indicates the underlying implementation

---
#[fit]Resources
###Text files, File URLs or Web URLs
###```sendResourceAtURL: withName: toPeer: withCompletionHandler:```
### Callbacks to start and finish transfer, uses ```NSProgress```

^send files and the content of URL resources this way
use the completion handler to handle failures
the start method passes an NSProgress object that you
can use to indicate UI transfer progress.

---
#[fit]Streams
###Unbounded data, uses NSStreams
###```startStreamWithName: toPeer: error:``` returns an ```NSOutputStream```
###```session: didReceiveStream: withName: fromPeer:``` gives an ```NSInputStream```

^use for audio, video streaming
Be aware that streams need handling in your code.

---
#[fit]Streams
###For both input and output streams:
###> Add stream to a run loop
###> Open the stream
###> Set a delegate and respond to ```stream: handleEvent:``` callback


^For you to handle the Streams.
You must: Add to run loop, open the stream, respond to delegate methods

---
#[fit]Underneath
##SRV Records
##TXT Records
![fit, 185%, filtered](http://www.insider-london.co.uk/wp-content/uploads/2012/04/London-underground-walking_tours.jpg)

^when you start up an advertiser you're really adding a record to you
local mDNS instance that looks like this.

---
#[fit]Underneath
#[fit]```my_service._http._tcp.local. 120 IN SRV 0 0 515 mynameisprince.local```

^how does this map to the information we use in the multipeer framework?

---
#[fit]Underneath
#[fit]```my_service._http._tcp.local. 120 IN SRV 0 0 515 mynameisprince.local```
###[fit]```my_service``` is the Service Name.

^this is name you defined as the Service Type in the MCSession
it's not what Bonjour considers the Service Type...

---
#[fit]Underneath
#[fit]```my_service._http._tcp.local. 120 IN SRV 0 0 515 mynameisprince.local```
###[fit]```my_service``` is the Service Name.
###[fit]```_http.``` is the actual Service Type.

^...but this is. It's defined for you by the advertiser.
Forms the scheme of the Bonjour URL.
TBH I don't know the exact one that the advertiser uses.

---
#[fit]Underneath
#[fit]```my_service._http._tcp.local. 120 IN SRV 0 0 515 mynameisprince.local```
###[fit]```my_service``` is the Service Name.
###[fit]```_http.``` is the actual Service Type.
###[fit]```515``` is the connection port number.

^the port number that the service is available over. 
Assigned dynamically by the framework.
Because every peer is it's own mDNS instance, these don't have to match
between peers.

---
#[fit]Underneath
#[fit]```my_service._http._tcp.local. 120 IN SRV 0 0 515 mynameisprince.local```
###[fit]```my_service``` is the Service Name.
###[fit]```_http.``` is the actual Service Type.
###[fit]```515``` is the connection port number.
###[fit]```nynameisprince``` is the displayname from ```MCPeerID```

^this is the displayName property you assigned 
to the users MCPeerID object. It forms the host of the Bonjour URL.
Requests to the users device for services will cause mDNS daemon 
to respond with this service record

---
#[fit]Underneath
#[fit]```my_printer._printer._tcp.local. TXT papersize=A4, jamWhenUrgent=YES```

^remember the discoveryInfo?
it becomes a Bonjour TXT record for the service

---
#[fit]Underneath
#[fit]```my_printer._printer._tcp.local. TXT papersize=A4, jamWhenUrgent=YES```

###[fit]key value pairs
###[fit]256 byte limit
###[fit]More data, more time to transmit

^the limitations on it are what restricts size of discoveryInfo

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
#[fit]Demo
![fit, 220%, filtered](http://www.history.com/news/wp-content/uploads/2012/05/hindenburg-wide.jpg)

---
#[fit]LocalTalk
##[fit]on the app store
##[fit]update in progress for iOS 8
##[fit]OS X version to follow
##[fit]free!
![right, fit](https://dl.dropboxusercontent.com/u/5034400/MPP/splash low.png)

---
#[fit]Future
![fit, 150%, filtered](http://thumbs.dreamstime.com/z/tunnel-futuristic-18180424.jpg)

---
##[fit]Multipeer for Mac

---
##[fit]Multipeer for Mac
##[fit]Common API with iOS

^uses NSViewController instead of UI

---
##[fit]Multipeer for Mac
##[fit]Common API with iOS
##[fit]But! Supports background operation

---
##[fit]Multipeer for Mac
##[fit]Common API with iOS
##[fit]But! Supports background operation
##[fit]However! Not Bluetooth

^I honestly don't know why
AdHoc Wifi is an alternative and
it does add Ethernet as an interface.
Which should mean it comes to AirDrop too.

---
##[fit]Multipeer for Mac
##[fit]Common API with iOS
##[fit]But! Supports background operation
##[fit]However! Not Bluetooth
##[fit]AirDrop rebuilt to use the new API

---
##[fit]Multipeer for Mac
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
#[fit]Questions?
![fit, 300%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/Show-of-hands.jpg)

---
#[fit] Rob Stearn
#[fit] @cocoadelica
#[fit] www.cocoadelica.co.uk
#[fit] robstearn@me.com

---
[WWDC 2013 Session 708](http://devstreaming.apple.com/videos/wwdc/2013/708xbx3x7xusbzidl0j3acxest/708/708-HD.mov?dl=1)
[WWDC 2014 Session 709](http://devstreaming.apple.com/videos/wwdc/2014/709xx1q8hdvo14x/709/709_hd_cross_platform_nearby_networking.mov?dl=1)
[IANA service type list](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xml)
[Bonjour Overview](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/NetServices/Introduction.html)
[Multipeer Networking Reference](https://developer.apple.com/library/ios/documentation/MultipeerConnectivity/Reference/MultipeerConnectivityFramework/Introduction/Introduction.html)
[NSNetServices & CFNetServices Programming Guide](https://developer.apple.com/library/mac/documentation/networking/conceptual/nsnetserviceprogguide/Introduction.html)
[MultipeerGroupChat sample code](https://developer.apple.com/library/ios/samplecode/MultipeerGroupChat/Introduction/Intro.html)
[Forums](https://devforums.apple.com/community/ios/core/coreosgen)



