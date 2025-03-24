
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



# 20250322, camera over wifi

2025-03-22T00:52:27-07:00
Suffered a while to revive memory of using arduino IDE.
Testing a new script that o3-mini-high wrote:

```
  

// Replace with your network credentials

  

#include "config.h"

#define CAMERA_MODEL_XIAO_ESP32S3

#include "esp_camera.h"

#include <WiFi.h>

#include <WebServer.h>

  

// Replace these with your network credentials

// const char* ssid = "YOUR_SSID";

// const char* password = "YOUR_PASSWORD";

  

// Create a web server on port 80

WebServer server(80);

  

// Include your camera pins definition (usually provided in camera_pins.h)

#include "camera_pins.h"

  

// HTTP handler to capture and send the JPEG image

void handleCapture() {

camera_fb_t * fb = esp_camera_fb_get();

if (!fb) {

Serial.println("Camera capture failed");

server.send(500, "text/plain", "Camera capture failed");

return;

}

// Set the response headers and send the JPEG image

server.sendHeader("Content-Type", "image/jpeg");

server.sendHeader("Content-Length", String(fb->len));

server.send(200, "image/jpeg", (const char*)fb->buf);

  

// Return the frame buffer back to the driver

esp_camera_fb_return(fb);

}

  

void setup() {

Serial.begin(115200);

delay(1000);

  

// --- Camera configuration ---

camera_config_t config;

config.ledc_channel = LEDC_CHANNEL_0;

config.ledc_timer = LEDC_TIMER_0;

// These pin assignments come from the camera_pins.h for XIAO ESP32S3 Sense

config.pin_d0 = Y2_GPIO_NUM;

config.pin_d1 = Y3_GPIO_NUM;

config.pin_d2 = Y4_GPIO_NUM;

config.pin_d3 = Y5_GPIO_NUM;

config.pin_d4 = Y6_GPIO_NUM;

config.pin_d5 = Y7_GPIO_NUM;

config.pin_d6 = Y8_GPIO_NUM;

config.pin_d7 = Y9_GPIO_NUM;

config.pin_xclk = XCLK_GPIO_NUM;

config.pin_pclk = PCLK_GPIO_NUM;

config.pin_vsync = VSYNC_GPIO_NUM;

config.pin_href = HREF_GPIO_NUM;

config.pin_sscb_sda = SIOD_GPIO_NUM;

config.pin_sscb_scl = SIOC_GPIO_NUM;

config.pin_pwdn = PWDN_GPIO_NUM;

config.pin_reset = RESET_GPIO_NUM;

  

config.xclk_freq_hz = 20000000;

config.pixel_format = PIXFORMAT_JPEG;

// Choose an appropriate frame size based on your module’s PSRAM availability.

// For modules with PSRAM (like the XIAO ESP32S3 Sense), you can use higher resolutions.

config.frame_size = FRAMESIZE_SVGA;

config.jpeg_quality = 12; // 0-63, lower means better quality

config.fb_count = 1; // Use one frame buffer

  

// Initialize the camera

esp_err_t err = esp_camera_init(&config);

if(err != ESP_OK) {

Serial.printf("Camera init failed with error 0x%x", err);

return;

}

Serial.println("Camera initialized");

  

// --- Connect to WiFi ---

WiFi.mode(WIFI_STA);

WiFi.begin(ssid, password);

Serial.print("Connecting to WiFi");

while(WiFi.status() != WL_CONNECTED) {

Serial.print(".");

delay(500);

}

Serial.println();

Serial.print("Connected! IP Address: ");

Serial.println(WiFi.localIP());

  

// --- Set up the web server ---

server.on("/capture", HTTP_GET, handleCapture);

server.begin();

Serial.println("HTTP server started");

}

  

void loop() {

server.handleClient();

delay(1);

}
```

I have to turn on Tools > PSRAM.

2025-03-22T00:54:23-07:00
hell yeah:
-   Camera initialized
- Connecting to WiFi........
- Connected! IP Address: 192.168.12.135
- HTTP server started


2025-03-22T01:06:04-07:00
Getting `ERR_RESPONSE_HEADERS_MULTIPLE_CONTENT_LENGTH` error.

2025-03-22T01:07:26-07:00
Holy fuck I got an image:
![[Pasted image 20250322010732.png]]

## Debug message from 03-mini-high:
- The error means that two different Content-Length headers are being sent. This happens because the WebServer library’s `send()` function automatically adds a Content-Length header based on the (empty) body you provided, while you’re also setting it manually.
- One common workaround is to bypass the built-in `send()` call for the response and instead send the HTTP status line and headers manually over the client, then stream the binary JPEG data.
### Explanation

- **Manual Header Sending:**  
    We first get the underlying WiFiClient using `server.client()` and then print the full HTTP response header manually. This way, we have full control over what headers are sent and avoid duplicate Content-Length headers.
    
- **No call to `server.send()` with a body:**  
    We do not call `server.send(200, "image/jpeg", "")` because that call would automatically set headers (including a Content-Length header for an empty body) and cause duplication.
    
- **Sending Binary Data:**  
    After the headers (followed by a blank line), we send the image data with `client.write(fb->buf, fb->len);`.



## Here is the infinite monitor:
![[Pasted image 20250322010829.png]]
ok I'm going to sleep.

2025-03-22T08:48:25-07:00
One picture is ~ 1 MB.
My NAS is 8 TB 8 4. It could last 1 year with one picture per second:
![[Pasted image 20250322085036.png]]

So 10 sec per picture would be fine for 10 years!

# Better camera
2025-03-22T19:26:34-07:00
The images suck. Asked for better cameras:
Below is a comparison table with a few options:

| **Option**    | **Sensor** | **Resolution (MP)** | **Max Image Resolution** | **Approx. Price** | **Notes**                                                                                                                                                  |
| ------------- | ---------- | ------------------- | ------------------------ | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 (Default)   | OV2640     | ~2                  | Up to UXGA (1600×1200)   | ~$8–$10           | The default sensor; moderate image quality and widely supported.                                                                                           |
| 2 (Upgrade)   | OV5640     | ~5                  | 2592×1944                | ~$12–$15          | Offers higher resolution and better image quality; compatible with the ESP32 camera interface.                                                             |
| 3 (Advanced)* | OV4689     | ~8                  | Up to 3840×2160 (4K)     | ~$20–$30          | A significant upgrade in resolution. _Typically available in MIPI CSI form factor, so using it may require extra adapter hardware and custom driver work._ |

How to improve the default camera:
Here’s a quick comparison table:

|**Approach**|**Pros**|**Cons**|
|---|---|---|
|**Tuning Sensor Settings**|No hardware changes; can be adjusted via firmware|May be limited by sensor’s inherent performance|
|**Supplemental IR Lighting**|Improves brightness without visible light|Requires additional components and proper placement|
|**Upgrading the Sensor (e.g. OV5640)**|Better overall image quality and low-light performance|May require adjustments in wiring, pin configuration, or code|


# New camera
2025-03-23T09:31:18-07:00
Tried changing settings. No big diff.
Thinking about OV5640.
- ordered two:
![[Pasted image 20250323212001.png]]

