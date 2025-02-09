
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

2025-02-05T23:25:13-08:00
Continued with connecting to wifi, and it worked.


## How do I check the version of the ESP32?
2025-02-07T23:16:53-08:00

Asked Mistral ai's Le Chat. It is fast af.
```
void setup() {
  Serial.begin(115200);
  delay(1000);
  Serial.println("ESP32 Version: " + String(ESP.getChipModel()));
  Serial.println("SDK Version: " + String(ESP.getSdkVersion()));
}

void loop() {
  // Nothing to do here
}

```

Got:
```Sketch uses 303164 bytes (9%) of program storage space. Maximum is 3342336 bytes.
Global variables use 20316 bytes (6%) of dynamic memory, leaving 307364 bytes for local variables. Maximum is 327680 bytes.
esptool.py v4.8.1
Serial port /dev/cu.usbmodem31201
Connecting...
Chip is ESP32-S3 (QFN56) (revision v0.2)
Features: WiFi, BLE, Embedded PSRAM 8MB (AP_3v3)
Crystal is 40MHz
MAC: 24:58:7c:e2:a8:04
```
- However these are always there I guess lol whenever I recompile

Going to continue the examples.

## Loudness detection
2025-02-07T23:19:06-08:00

https://wiki.seeedstudio.com/xiao_esp32s3_sense_mic/#detection-of-sound-loudness

Error: `Compilation error: I2S.h: No such file or directory`

Fixed by using the second version:
```
#include <ESP_I2S.h>

I2SClass I2S;

  

void setup() {

// Open serial communications and wait for port to open:

// A baud rate of 115200 is used instead of 9600 for a faster data rate

// on non-native USB ports

Serial.begin(115200);

while (!Serial) {

; // wait for serial port to connect. Needed for native USB port only

}

  

// setup 42 PDM clock and 41 PDM data pins

I2S.setPinsPdmRx(42, 41);

  

// start I2S at 16 kHz with 16-bits per sample

if (!I2S.begin(I2S_MODE_PDM_RX, 16000, I2S_DATA_BIT_WIDTH_16BIT, I2S_SLOT_MODE_MONO)) {

Serial.println("Failed to initialize I2S!");

while (1); // do nothing

}

}

  

void loop() {

// read a sample

int sample = I2S.read();

  

if (sample && sample != -1 && sample != 1) {

Serial.println(sample);

}

}
```

Port info:
![[Pasted image 20250207233213.png]]

Yes got the python code to read the serial port:

```
  

import serial

import time

  

# Configure the serial port (replace 'COM3' with your serial port)

ser = serial.Serial('/dev/cu.usbmodem31201', 9600, timeout=1)

  

# Give some time for the serial connection to initialize

time.sleep(2)

  

try:

while True:

if ser.in_waiting > 0:

line = ser.readline().decode('utf-8').strip()

print(line)

except KeyboardInterrupt:

print("Serial communication stopped.")

finally:

ser.close()
```





