
2025-04-10T23:29:57-07:00
Reference:
- https://x.com/jwt0625/status/1877540668735893694
- https://www-sk.icrr.u-tokyo.ac.jp/realtimemonitor/


# Process the gif

2025-04-10T23:35:37-07:00
Almost working?
![[Pasted image 20250410233540.png]]
- damn 4o is good.
Seems like it is missing the top row:
![[Pasted image 20250410233750.png]]

2025-04-10T23:52:35-07:00
Done cleaning up:
![[Pasted image 20250410235239.png]]
- going to test a few more images.
![[Pasted image 20250410235341.png]]
seems to be working fine.

2025-04-10T23:55:54-07:00
Need to further divide it into the rectangle and the two circular disks.
2025-04-11T00:09:37-07:00
Done:
![[Pasted image 20250411000940.png]]
- https://x.com/jwt0625/status/1910590949123907944

# Look for good LED panels

2025-04-11T00:27:17-07:00
Chat's recommendation:

| Geometry       | Component                              | Why                                             |
|----------------|----------------------------------------|--------------------------------------------------|
| **Cylindrical wall** | Flexible WS2812B strips or panels       | Can wrap around cylinders, each LED is addressable |
| **Top & bottom**     | Flat WS2812B matrix (e.g. 8×8 or 16×16) | Easy to mount and match Super-K top/bottom layout |
| **Control**          | ESP32 / Raspberry Pi Pico W            | Enough speed, can stream detector events or pulse simulation |
| **Power**            | USB or 5V 2A–5A wall adapter           | Needed for ~100+ LEDs                             |

https://www.reddit.com/r/led/comments/yo0uu9/whats_the_current_highestdensity/

This is high enough density but too big:
- https://www.aliexpress.us/item/3256806591146679.html
- ![[Pasted image 20250411003915.png]]

2025-04-11T00:40:47-07:00
This looks nice but like a full on display:
![[Pasted image 20250411004055.png]]
- https://www.aliexpress.us/item/3256806692214298.html

This is the best I've seen so far. Does not bend much it seems.
- https://www.aliexpress.us/item/3256802797326412.html
![[Pasted image 20250411004211.png]]

This one has more sold and has different pitch:
![[Pasted image 20250411004520.png]]



