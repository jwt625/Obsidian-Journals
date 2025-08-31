
What I got:
![[Pasted image 20250103220845.png]]

![[Pasted image 20250103220931.png]]
![[Pasted image 20250103221003.png]]




2025-01-03T22:02:59-08:00
Done setting up the new wifi7 router and the synology NAS.
- Used mostly default settings for both of them
- SHR for the NAS

Wifi router not connected to the T-mobile fixed wireless yet. Too far.

Had to change the macos network service order to let it use wifi instead of ethernet:
![[Pasted image 20250103220423.png]]

Backed up photos from DAS to NAS

2025-01-03T22:04:34-08:00

Testing speed between mac mini and the NAS:
![[Pasted image 20250103220458.png]]

GbE feels it sucks after I used thunderbolt (1 GB/s, 10x faster). And it was not even thunderbolt 5 yet because I got a crappy M.2 enclosure.

Ok good for now. Going to continue backing up from DAS to NAS, and now I could access photos etc. from mac mini yay.
- will use DAS as backup after copying shit to NAS



2025-08-18T21:44:56-07:00
Fuck, I forgot the password for the NAS.

2025-08-18T21:47:02-07:00
ok found it lol.
- I should save this into a yaml file

Checking because I got a cheap external storage from Bestbuy:
![[Pasted image 20250818215049.png]]

Amazing shit. How does it have larger storage capacity than my whole NAS. Maybe I should have gotten larger HDD.

![[Pasted image 20250818215150.png]]

I don't remember how much I have in my DAS...

## for checking the NAS addr
Go to router: http://192.168.0.1/webpages/index.html#/
