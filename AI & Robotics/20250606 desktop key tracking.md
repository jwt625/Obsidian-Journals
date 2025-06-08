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


