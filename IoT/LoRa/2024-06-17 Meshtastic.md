
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




# Reference
https://bayme.sh/
https://meshtastic.org/
