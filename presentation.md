#[fit]Multipeer 
#[fit]Networking
#[fit]---------------------------
#[fit]Rob Stearn

---
![fit, 270% filtered](http://cdn.macrumors.com/article/2011/03/23/095149-federighi_lion.jpg)
##[fit]History
##[fit]Overview
##[fit]Code
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

---
#[fit]Overview
![fit, 175%, filtered](http://www.overviewthemovie.com/wp-content/uploads/2012/12/MegaEarth01.jpg)

^Wireless
Session
Advertiser
Browser

---
#[fit]Multipeer networking... 

sends data *from one device to another*

^sends data *from one device to another*

---
#[fit]Multipeer networking... 

sends data *from one device to another*

requires *no server infrastructure*

^ requires *no server infrastructure*

---
#[fit]Multipeer networking... 

sends data *from one device to another*

requires *no server infrastructure*

sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

^ sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

---
#[fit]Multipeer networking... 

sends data *from one device to another*

requires *no server infrastructure*

sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

*bridges across interfaces*

^ *bridges across interfaces*

---
#[fit]Multipeer networking... 

sends data *from one device to another*

requires *no server infrastructure*

sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

*bridges across interfaces*

can send data as *blob, resource or stream*

^ can send data as *blob, resource or stream* which we'll look at in the Code section

---
#[fit]Multipeer networking... 

sends data *from one device to another*

requires *no server infrastructure*

sends over *Bluetooth, *Ad-Hoc or Infrastructure WiFi*

*bridges across interfaces*

can send data as *blob, URL or stream*

can ensure *order and delivery of data*

^ can ensure *order and delivery of data* more on that later

---
#[fit]Consider 3 iOS devices...
 
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/wifi.png)

^ here a device with wifi turned on, though not connected to a specific network

---
#[fit]Consider 3 iOS devices...

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/wifi.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/both.png)

^ here's a second one that has wifi on, also not on a network, and also bluetooth active


---
#[fit]Consider 3 iOS devices...

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/wifi.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/both.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/bluetooth.png)

^ and here's a third that just got bluetooth on. Not active just around.

---
#[fit]Consider 3 iOS devices...

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/wifi.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/both.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/bluetooth.png)

1 connects to 2 via Bluetooth

3 connects to 2 via WiFi

1, 2 and 3 can all see each other

no direct connection between 1 & 3 required

^ all the devices connect to each other in a mesh

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/plain.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/plain.png)

^ the multipeer API uses 3 objects, in most cases each device will run all three.
Firstly both devices will instantiate a session object that stays in memory for the life of the communication.

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/sender.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/recieve.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/plain.png)

One device will then create an Advertiser, to broadcast it's service.

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/sender.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/send.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/receiver.png)

This is picked by a Browser object on the other device and displayed as an available peer.
Tapping on the displayed name sends an invite back to the Advertiser to join Sessions.

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/plain.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/bidirectional.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/plain.png)

^ When the invitation request is accepted a bi-directional communication session is open to send data.

---
#[fit]Code
![fit, 100%, filtered](http://images4.alphacoders.com/270/27094.jpg)

^ so thats the theory, how does this work in practice?
Well the Session, Browser and Advertiser objects map directly Obj-C classes...

---
#[fit]Session
#[fit]Browser
#[fit]Advertiser
#[fit]Transmission types
#[fit]Transmission modes

---
#Session / MCPeerID
## ```MCPeerID *peerID = [[MCPeerID alloc] initWithDisplayName:@"cocoadelica"];```

---
#Session / MCPeerID
## ```MCPeerID *peerID = [[MCPeerID alloc] initWithDisplayName:@"cocoadelica"];```

## The Peer ID represents the user 

---
#Session / Initialize
### ```self.session = [[MCSession alloc] initWithPeer:peerID securityIdentity:nil* encryptionPreference: MCEncryptionRequired**];```

---
#Session / Initialize
### ```self.session = [[MCSession alloc] initWithPeer:peerID securityIdentity:nil* encryptionPreference: MCEncryptionRequired**];```

####* optional Array with ```SecIdentityRef``` and ```SecCertificateRef``` items 

---
#Session / Initialize
### ```self.session = [[MCSession alloc] initWithPeer:peerID securityIdentity:nil* encryptionPreference: MCEncryptionRequired**];```

####* optional Array with ```SecIdentityRef``` and ```SecCertificateRef``` items 
####** choose from ```MCEncryptionRequired```, ```MCEncryptionOptional``` or ```MCEncryptionNone```

---
#Session / Delegate
### ```self.session.delegate = self;```

## Delegate callbacks for the session let you handle data transmission events.

^more on the these callbacks soon...

---
#Session / Details
##Foreground operation only 


---
#Session / Details
##Foreground operation only 
##Disconnects on breakpoints


---
#Session / Details
##Foreground operation only 
##Disconnects on breakpoints
##Good candidate for a Singleton*

---
#[fit]Browser
###```MCBrowserViewController *bvc = [[MCBrowserViewController alloc] initWithServiceType:(NSString *)serviceType session:(MCSession *)session];```

---
#[fit]Browser
###```MCBrowserViewController *bvc = [[MCBrowserViewController alloc] initWithServiceType:(NSString *)serviceType session:(MCSession *)session];```
###[fit]use a reverse DNS notation for service type

---
#[fit]Browser
#[fit]"Easy mode"
![220%,right](https://dl.dropboxusercontent.com/u/5034400/MPP/mcbrowservc.png)

---
#[fit]Browser
#[fit]"Easy mode"
##[fit]handles invites
![220%,right](https://dl.dropboxusercontent.com/u/5034400/MPP/mcbrowservc.png)

---
#[fit]Browser
#[fit]"Easy mode"
##[fit]handles invites
##[fit]boxed solution
![220%,right](https://dl.dropboxusercontent.com/u/5034400/MPP/mcbrowservc.png)

---
#[fit]Browser
#[fit]"Easy mode"
##[fit]handles invites
##[fit]boxed solution
##[fit]minimal styling
![220%,right](https://dl.dropboxusercontent.com/u/5034400/MPP/mcbrowservc.png)

---
#[fit]Browser
#[fit]"Easy mode"
##[fit]handles invites
##[fit]boxed solution
##[fit]minimal styling
##[fit]awesome!
![220%,right](https://dl.dropboxusercontent.com/u/5034400/MPP/mcbrowservc.png)

---
#[fit]Browser
#[fit]"Easy mode"
##[fit]handles invites
##[fit]boxed solution
##[fit]minimal styling
##[fit]awesome!
##[fit]a bit dull
![220%,right](https://dl.dropboxusercontent.com/u/5034400/MPP/mcbrowservc.png)

---
#[fit]Browser
###```MCBrowserViewControllerDelegate```
###browserViewController:shouldPresentNearbyPeer:withDiscoveryInfo:
###browserViewControllerDidFinish:
###browserViewControllerWasCancelled:

---
#[fit]Browser
##"Hard Mode"
###Build a custom UI
###Use ```MCNearbyServiceBrowser``` for data and callbacks

---
#[fit]Browser
##"Expert Mode"
###Build a custom UI
###*Subclass* ```MCNearbyServiceBrowser``` for data and callbacks

---
Advertiser

---
Sending data
Types of data

---
Sending data
Transmission modes

---
#[fit]User 
#[fit]Interface
![fit, 500% filtered](http://www.noupe.com/wp-content/uploads/2012/02/wireframingkits61.jpg)

---
#[fit]2 levels of API
#Standard & Custom

---
#[fit]2 levels of API
#Standard

---
#[fit]2 levels of API
#Standard
###```MCBrowserViewController``` based on ```UITableView```

---
#[fit]2 levels of API
#Standard
###```MCBrowserViewController``` based on ```UITableView```
###```MCAdvertiserAssistant``` based on ```UIAlertView```

---
#[fit]2 levels of API
##Custom

---
#[fit]2 levels of API
##Custom
###```MCNearbyServiceBrowser``` handles finding nearby users and sends callbacks to ```MCNearbyServiceBrowserDelegate```

---
#2 levels of API
##Custom
###```MCNearbyServiceBrowser``` finds nearby users and sends callbacks to ```MCNearbyServiceBrowserDelegate```
###```MCNearbyServiceAdvertiser``` advertises your service and handles invitations by callbacks to ```MCNearbyServiceAdvertiserDelegate``` 

---
#[fit]!
![fit, 150% filtered](http://i.stack.imgur.com/nI9zx.jpg)

---
#Duplicates in the browser
![right](https://dl.dropboxusercontent.com/u/5034400/IMG_6028.PNG)

---
#Duplicates in the browser
###Issue: ```MCPeerID``` created each time ```MCSession``` is instantiated
![right](https://dl.dropboxusercontent.com/u/5034400/IMG_6028.PNG)

---
#Duplicates in the browser
###Issue: ```MCPeerID``` created each time ```MCSession``` is instantiated
###Solution: Each peer should serialize its own ```MCPeerID``` with ```NSKeyedArchiver```, save it to disk and reuse for each new session.
![right](https://dl.dropboxusercontent.com/u/5034400/IMG_6028.PNG)

---
#Invitation response times
![right](https://dl.dropboxusercontent.com/u/5034400/IMG_6029.PNG)

---
#Invitation response times
###Issue: Once an invitation is accepted there's a delay before connecting
![right](https://dl.dropboxusercontent.com/u/5034400/IMG_6029.PNG)

---
#Invitation response times
###Issue: Once an invitation is accepted there's a delay before connecting
###Solution: Encryption is on by default which adds overhead to initial connections. Consider using ```MCEncryptionNone```
![right](https://dl.dropboxusercontent.com/u/5034400/IMG_6029.PNG)

---
#[fit]Future
![fit, 150%, filtered](http://thumbs.dreamstime.com/z/tunnel-futuristic-18180424.jpg)

---
#[fit]LocalTalk
###on the app store
###v0.8
###v1.0 in progress for iOS 8
###OS X version to follow
![right, fit, filtered](https://dl.dropboxusercontent.com/u/5034400/LTSplash.png)

---

#[fit] Rob Stearn
#[fit] @cocoadelica
#[fit] www.cocoadelica.co.uk
#[fit] robstearn@me.com

---
#[fit]End
![fit, 60%, filtered](https://dl.dropboxusercontent.com/u/5034400/MPP/cocoa.png)
### created with Deckset
#### http://www.decksetapp.com
### icons made with Sketch
#### http://bohemiancoding.com/sketch/

