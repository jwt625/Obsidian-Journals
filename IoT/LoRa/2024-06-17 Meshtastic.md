
# Flash firmware
20240617, 23:22: created this doc.
Previous:
- I got two HELTEC LoRa 32 WiFi BLE board from Amazon
	- WiFi_LoRa_32_V3
- need a case, and a holder for the battery, and an antenna

The hardware is supported: https://meshtastic.org/docs/hardware/devices/heltec/

23:43 Installing USB to UART driver:
- https://meshtastic.org/docs/getting-started/serial-drivers/esp32/
- https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers
- https://meshtastic.org/docs/getting-started/serial-drivers/test-serial-driver-installation/
Before:
![[Pasted image 20240617234536.png]]
Installed this one from https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers?tab=downloads:
![[Pasted image 20240617234733.png]]
After installation:
![[Pasted image 20240617234754.png]]
23:48 Now flashing firmware:
![[Pasted image 20240617234822.png]]
```
esptool.js
Serial port WebSerial VendorID 0x10c4 ProductID 0xea60
Connecting....
Detecting chip type... ESP32-S3
Chip is ESP32-S3
Features: Wi-Fi,BLE
Crystal is 40MHz
MAC: 34:b7:da:56:aa:b0
Uploading stub...
Running stub...
Stub running...
Erasing flash (this may take a while)...
Chip erase completed successfully in 2.723s
Not changing the image
Compressed 1852592 bytes to 1140676...
Writing at 0x0... (1%)
```

23:55 Flashing done. Connected to iphone app. Going to flash the second one.

2024-06-18, 00:06 Done. So easy to setup.



# Picking antenna
2024-06-18 09:49

See tests by the community: https://github.com/meshtastic/antenna-reports?tab=readme-ov-file
I'm going for Molex 206764-0050:
- https://www.digikey.com/en/products/detail/molex/2067640100/9520958

Damn shipping is 7 bucks. What else do I need from Digikey?

# Case
2024-06-18 22:34
Found this one looking pretty attractive: https://www.thingiverse.com/thing:6625731
- found it through public search on onshape lol:
![[Pasted image 20240618224245.png]]

Found his youtube channel and homepage:
- https://www.youtube.com/@slabua/videos
- https://www.slblabs.com/

No post about the meshtastic project.
Trying to identify the boards:
![[Pasted image 20240618223733.png]]

How tf are they this chip:
- Blue board: GPS module, $9: https://www.amazon.com/HiLetgo-GY-NEO6MV2-Controller-Ceramic-Antenna/dp/B01D1D0F5M
- Purple: BME280 temperature, humidity, pressure sensor: https://www.amazon.com/Sumklin-Temperature-Precision-Barometric-GY-BME280/dp/B0CHWKVJLH/
- Battery: https://www.amazon.com/3000mAh-974058-Rechargeable-Replacement-Electronic/dp/B09YQ393N2
- the thing on the top looks like the GPS antenna

Pbb need to read and learn how the HELTEC LoRa board work:
https://heltec.org/project/wifi-lora-32-v3/

# Learning HELTEC LoRa board
23:22 Installed Arduino IDE:
![[Pasted image 20240618232250.png]]

Installing Heltec ESP32 library following:
- https://github.com/HelTecAutomation/Heltec_ESP32?tab=readme-ov-file#how-to-install-this-library

Maybe I should start with more straightforward tutorials like this one:
- https://randomnerdtutorials.com/esp32-bme280-arduino-ide-pressure-temperature-humidity/
- got it from asking perplexity
23:28 I am going to order the BME280 and the GPS modules from Amazon. Pbb also the battery.
How do I charge the battery?
- need a lithium polymer battery charger
23:37 Ordered. Going to sleep.

# Printing the case
20240622, 10:38
![[Pasted image 20240622103855.png]]
![[Pasted image 20240622104104.png]]
10:43 Connected to printer over wifi and send the work. Pretty convenient.




# ESP32 with BME280 Sensor

## Twitter sensor recommendations
20240622, 23:09
https://x.com/0zAND1z/status/1804472999241826688
```
NEO6Mv2 is used frequently in India. Here’s an urban weather station POC I built for

[@BLRxZo](https://x.com/BLRxZo)

<3 With some (luck) calibration, you can manage to keep it indoors too!
```
`DHT22, MQ9, SGP-30, and DSM501A!`

Actually there are people showing how to do it with meshtastic / Heltec V3 board:
- https://www.youtube.com/watch?v=VXRbyFbLYg4&ab_channel=JD
- https://www.youtube.com/watch?v=9gV4yKQVtuw&ab_channel=TechMinds
![[Pasted image 20240624231659.png]]

2024-06-24T22:29:57-07:00 Trying out the web client:
- https://meshtastic.org/docs/software/web-client/

2024-06-24T23:00:26-07:00 Texting works between my phone and my desktop
![[Pasted image 20240624230043.png]]
Soldered pins and trying to get BME280 to work.



# Reference
https://bayme.sh/
https://meshtastic.org/
https://www.thingiverse.com/thing:6625731