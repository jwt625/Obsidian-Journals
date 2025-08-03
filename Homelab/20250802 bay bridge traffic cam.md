
# Setting up iphone as IP cam

2025-08-02T10:55:42-07:00
Booting up old iphone SE again.

Almost forgot the password, locked myself out for 1 min. Luckily the second try worked.

Installed [droidcam](https://droidcam.app/) and its OBS plugin.
Webpage (http://192.168.12.6:4747/remote) does not work unless activated using the OBS plugin.

![[Pasted image 20250802105739.png]]


2025-08-02T11:01:02-07:00
ok could actually just hit the /video endpoint.
![[Pasted image 20250802110109.png]]

# Identifying and tracking
dude
![[Pasted image 20250802112549.png]]

![[Pasted image 20250802122414.png]]

2025-08-02T13:14:27-07:00
Hmm the digital zoom clearly makes a difference:
![[Pasted image 20250802131435.png]]

2025-08-02T13:36:42-07:00
Motion based tracking:
![[Pasted image 20250802133646.png]]

![[Pasted image 20250802133700.png]]

2025-08-02T14:08:59-07:00
More info on the motion mask:
![[Pasted image 20250802140904.png]]


2025-08-02T14:55:32-07:00
Added object tracking:
![[Pasted image 20250802145537.png]]


2025-08-02T15:30:12-07:00
Got telescope up:
![[Pasted image 20250802153020.png]]

2025-08-02T16:10:42-07:00
I need to get rid of the grey area:
![[Pasted image 20250802161048.png]]

Shit is getting worse, it should be removed:
![[Pasted image 20250802163322.png]]



2025-08-02T16:35:31-07:00
ok seems fixed:
![[Pasted image 20250802163534.png]]
- with:
```python
# Remove shadows if configured (convert grey shadow pixels to black)
bg_config = config.BACKGROUND_SUBTRACTOR_CONFIG
if bg_config["detectShadows"] and bg_config["removeShadows"]:
# Remove shadow pixels (convert 127 to 0, keep 255 as 255)
fg_mask[fg_mask == bg_config["shadowValue"]] = 0
```

2025-08-02T17:41:06-07:00
Ugh this is annoying to deal with:
![[Pasted image 20250802174113.png]]

2025-08-02T18:00:37-07:00
Makes no sense:
![[Pasted image 20250802180041.png]]


2025-08-02T18:49:55-07:00
Trying optical flow:
![[Pasted image 20250802184959.png]]
- kind of working, but not consistent enough.

2025-08-02T20:19:48-07:00
Not as good. Going to focus back on setting up the data stream and grafana dashboard.
Optical flow is also slower, could get max 20 fps.


# Dashboard
2025-08-02T20:20:45-07:00

2025-08-03T01:01:02-07:00
Still struggling to push to prometheus.
And also debugging why my keylogging app is uing 70k+ metrics. Likely because of browser top domain switching.

![[Pasted image 20250803010236.png]]


2025-08-03T02:01:59-07:00
Prometheus running fine locally
- http://localhost:9090/

![[Pasted image 20250803020214.png]]

Still cannot find the cloud equivalent of this. I have clearly seen / used it in [[20250606 desktop key tracking]]. Cannot find it anymore.

Anyways, could see the new metrics on: https://jwt625.grafana.net/a/grafana-metricsdrilldown-app/drilldown

![[Pasted image 20250803020642.png]]


2025-08-03T02:16:35-07:00
Need to give claude/augment an example to make it fix its outdated grafana dashboard json schema.

2025-08-03T02:24:34-07:00
done!!!
https://jwt625.grafana.net/goto/3rf4clwNR?orgId=1

![[Pasted image 20250803022452.png]]








