## Open Connect 
- Content Delivery 
- Additional Offerings: 
	- Ads, Cloud Games, Live
- Infrastructure for Officers and Creative Spaces
	- Wifi 
## CDN
Content Delivery Network 
distributed network of servers around the globe 
![[Screenshot 2023-07-27 at 1.14.00 PM.png]]
Distribute more traffic 
### why build own CDN 
1. Cost 
2. build CDN custom to the Netflix service 
3. Strategic ISP providers 
	1. Internet Service Providers (ISP)
4. Enhanced visibility 

![[Screenshot 2023-07-27 at 1.21.17 PM.png]]


## OCA 
Open Connect Appliance 

need *2TB* to serve 30% of the content in Netflix 
![[Screenshot 2023-07-27 at 1.40.53 PM.png]]

#### What makes up a title
![[Screenshot 2023-07-27 at 1.43.52 PM.png]]

## Streaming 
playback manifest: details needed to play the content back
![[Screenshot 2023-07-27 at 1.53.50 PM.png]]

## Measure Quality of Experience 
QoE: Quality of Experience
Play delay: how long does it take to get the first content back 
Rebuffer (2 types)
- network rebuffer
	- can be improved by having own CDN 
	- solution: reduce bitrate (e.g. 1080p -> 480p) when almost hit network rebuffer
- media rebuffer

