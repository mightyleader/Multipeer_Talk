#[fit]Multipeer 
#[fit]Networking
#[fit]---------------------------
#[fit]Rob Stearn

---
![fit, 270% filtered](http://cdn.macrumors.com/article/2011/03/23/095149-federighi_lion.jpg)
##[fit]History
##[fit]Overview
##[fit]User Interface
##[fit]Code
##[fit]Pitfalls
##[fit]Future

---
#[fit]History
![fit, 170%, filtered](http://lowendmac.com/wp-content/uploads/mac-plus-and-laserwriter.jpg)

^ there's a history of peer-to-peer networking in Mac OS. 
Anybody here used a Mac since System 7? 
For those who haven't, you never experienced... the Chooser!

---
![fit](http://computers.mcbx.netne.net/apple/general/chooser.jpg)

---
![fit, 230%, filtered](http://4.bp.blogspot.com/-nVc-nbWq7_s/To2LKMf1TiI/AAAAAAAAI7Y/hSrKS6BeWeI/s640/Matrix_024Pyxurz.jpg)
#[fit]RumorMonger

###http://c2.com/cgi/wiki?RumorMonger

---
#[fit]Overview
![fit, 175%, filtered](http://www.overviewthemovie.com/wp-content/uploads/2012/12/MegaEarth01.jpg)

^Wireless
Session
Advertiser
Browser

---
#[fit]Multipeer networking... 

allows you to send data *from one device directly to another*


---
#[fit]Multipeer networking... 

allows you to send data *from one device directly to another*

requires *no server infrastructure*


---
#[fit]Multipeer networking... 

allows you to send data *from one device directly to another*

requires *no server infrastructure*

Data can be transmitted over *Bluetooth LE*, *Ad-Hoc* WiFi or *Infrastructure* WiFi

---
#[fit]Multipeer networking... 

allows you to send data *from one device directly to another*

requires *no server infrastructure*

can be transmitted over *Bluetooth LE*, *Ad-Hoc* WiFi or *Infrastructure* WiFi

*bridges across interfaces*

---
#[fit]Consider 3 iOS devices...
 
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/wifi.png)

---
#[fit]Consider 3 iOS devices...

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/wifi.png)


![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/both.png)


---
#[fit]Consider 3 iOS devices...

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/wifi.png)


![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/both.png)


![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/bluetooth.png)

1 connects to 2 via Bluetooth

3 connects to 2 via WiFi

1, 2 and 3 can all see each other

no direct connection between 1 & 3 required

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/plain.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/plain.png)

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/sender.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/recieve.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/plain.png)

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/sender.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/send.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/receiver.png)

---
#[fit]Session, Browser and Advertiser

![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/plain.png)
![40%, filtered](https://dl.dropboxusercontent.com/u/5034400/bidirectional.png)
![75%, filtered](https://dl.dropboxusercontent.com/u/5034400/plain.png)

---
#[fit]Code
![fit, 100%, filtered](http://images4.alphacoders.com/270/27094.jpg)

---
Session

---
Advertiser

---
Browser

---
How it works

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
###```MCBrowserViewController``` based on ```UITableView```

---
#[fit]2 levels of API
#Standard
###```MCBrowserViewController``` based on ```UITableView```
###```MCAdvertiserAssistant``` based on ```UIAlertView```

---
#[fit]2 levels of API
#Custom

---
#[fit]2 levels of API
#Custom

---
#[fit]Pitfalls
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
### created with Deckset
#### http://www.decksetapp.com
### icons made with Sketch
#### http://bohemiancoding.com/sketch/

