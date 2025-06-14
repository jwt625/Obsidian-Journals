See
- https://github.com/jwt625/PlayGround/tree/main/20250606_keyLogging

2025-06-06T21:57:30-07:00
Continuing
![[Pasted image 20250606215733.png]]

Maybe worth a separate repo?

2025-06-06T22:09:38-07:00
Going to implement proper event logging.

2025-06-06T22:50:06-07:00
Made the buckets finer. Still need to update the grafana.

![[Pasted image 20250606225102.png]]



2025-06-07T00:09:23-07:00
Testing adding keystroke every second.

2025-06-07T00:52:06-07:00
added clicks.
![[Pasted image 20250607005259.png]]

2025-06-07T09:25:28-07:00
Changed layout as well as added proper max session duration metric
![[Pasted image 20250607092541.png]]


2025-06-07T12:02:16-07:00
This query is art, I have no freaking idea what or why the hell it looks like this:
```SQL
histogram_quantile(1.0, sum by (le, app) (increase(app_session_duration_seconds_bucket[$__range])) * on(app) group_left() (sum by (app) (increase(app_session_duration_seconds_count[$__range])) > 0))
```


2025-06-07T15:53:08-07:00
![[Pasted image 20250607155309.png]]

This actually look quite regular, esp. the pulses into obsidian.

2025-06-08T21:01:48-07:00
Filtering out the loginwindow:
```SQL
histogram_quantile(1.0,

sum by (le, app) (

increase(app_session_duration_seconds_bucket{app!~"loginwindow|finder|dock|.*window.*"}[$__range])

)

* on(app) group_left()

(

sum by (app) (increase(app_session_duration_seconds_count{app!~"loginwindow|finder|dock|.*window.*"}[$__range])) > 10

)

)
```


Should work on the chrome extension, look at this:
![[Pasted image 20250608210226.png]]

2025-06-08T22:41:43-07:00

Perfect! I've successfully implemented the requested Chrome tab
  session duration tracking and resolved the circular import issue.
  Here's what has been completed:

  ✅ Domain-Aware Metrics Implementation Complete

  Key Changes Made:

  1. Fixed Circular Import Issue: Removed metrics/helpers.go and moved
  getDomainForApp() helper function directly into keyboard and trackpad
  packages
  2. Added Chrome Tab Session Metrics: Enhanced metrics with new
  Chrome-specific tracking:
    - chrome_tab_session_duration_seconds - Histogram of Chrome tab
  focus sessions by domain
    - chrome_tab_total_time_seconds - Total time spent on each Chrome
  domain
    - current_chrome_tab_session_seconds - Real-time gauge of current
  tab session
  3. Enhanced Chrome Monitoring:
    - Session tracking with start/end times for each domain
    - Automatic session recording when switching between domains
    - Real-time gauge updates during active sessions
  4. Updated All Tracking Packages: All metrics now include optional
  domain labels:
    - keystrokes_total{key_type="letter", app="google_chrome", 
  domain="github_com"}
    - mouse_clicks_total{button_type="left", app="google_chrome", 
  domain="youtube_com"}
    - keystroke_interval_activity{app="google_chrome", 
  key_type="letter", domain="stackoverflow_com"}

  Ready for Testing:
  The system now tracks comprehensive Chrome tab session durations
  similar to app session tracking, providing fine-grained visibility
  into time spent on different domains. All data flows through
  Prometheus for complete observability.


2025-06-08T23:05:25-07:00
Chrome extension does not have write access to arb. locations, but it oes have download folder?



2025-06-08T23:13:35-07:00
It has decided to do POST to a http endpoint of the extension from the go server to get the into
 Simplified HTTP-Based Chrome Integration Complete!

  I've completely eliminated the complex file-based and native messaging
   approach and implemented a much simpler direct HTTP communication:

  🌐 How It Works Now:

  1. Chrome Extension: Sends domain updates directly via fetch() to
  http://localhost:8080/chrome-update
  2. Go Application: Receives HTTP POST requests and immediately updates
   Chrome domain
  3. Zero File I/O: No temp files, no Downloads folder, no native
  messaging host

  📡 HTTP Communication:

  Chrome Extension sends:
  fetch('http://localhost:8080/chrome-update', {
    method: 'POST',
    body: JSON.stringify({
      type: 'current_domain',
      domain: 'youtube_com',
      url: 'https://youtube.com/...'
    })
  })

  Go Application responds:
  {"status": "ok", "domain": "youtube_com"}

  🚀 To Test:

  4. Reload the Chrome extension (to get updated permissions and code)
  5. Start the tracker: ./start.sh
  6. Navigate between websites in Chrome
  7. Check Go logs - should immediately show "🌐 Chrome domain via HTTP:
   youtube_com"
  8. Check extension debug popup - should show "working (HTTP)" status

  This approach is much simpler and real-time - no polling, no file
  watching, just direct HTTP calls when domains change. The domains
  should now appear immediately in your Go logs and flow through to
  Prometheus metrics!

2025-06-08T23:15:37-07:00
Ok it is working:
![[Pasted image 20250608231540.png]]

2025-06-08T23:34:20-07:00
![[Pasted image 20250608233421.png]]
![[Pasted image 20250608233501.png]]

