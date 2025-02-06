
# Blink it
2025-01-08T23:28:13-08:00
Manual:
- https://wiki.seeedstudio.com/xiao_esp32s3_getting_started/
Also:
- https://wiki.seeedstudio.com/xiao_esp32s3_camera_usage/

2025-01-08T23:40:21-08:00
Seems like I need a small heat sink. Of course..
Installed Arduino IDE.

Connected and installing ESP32 board managers:
![[Pasted image 20250108234253.png]]


2025-01-08T23:58:17-08:00
Trying to blink the yellow LED.
Ok ran into a bunch of issues such as `LED_BUILTIN` not defined. A few back and forth and found that I had to select the right board under tool -> board. And then it runs:
![[Pasted image 20250108235905.png]]

![[Pasted image 20250109000007.png]]
Got a thunderbolt 5 for this, feels so wrong lol.


2025-01-09T00:18:59-08:00
Now following the camera guide:
- https://wiki.seeedstudio.com/xiao_esp32s3_camera_usage/
See also
- https://github.com/limengdu/SeeedStudio-XIAO-ESP32S3-Sense-camera/tree/main/take_photos
Opening serial monitor in arduino IDE is `Cmd + Shift + M`.
- see Card Mount Failed. Makes sense.

# Wifi
2025-02-05T22:46:49-08:00
Fuck it's already been a month.

Following
- https://wiki.seeedstudio.com/xiao_esp32s3_wifi_usage/#scan-nearby-wifi-networks
- `File > Examples > WiFi > WiFiScan
Seems to run fine?
![[Pasted image 20250205230831.png]]
2025-02-05T23:14:58-08:00
Oh ok I need to open the Serial monitor:
![[Pasted image 20250205231510.png]]



