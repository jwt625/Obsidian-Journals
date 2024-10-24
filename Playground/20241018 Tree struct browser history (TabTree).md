`GitHub\PlayGround\20241017_tree_history`

# Initial version
2024-10-20T11:11:34-07:00
Worked on tracking browsing using chrome plugin yesterday.

Trying to visualize the tree right now.

Following claude:
Using https://d3js.org/getting-started

To use this updated version:

1. Save the HTML content to a file (e.g., `tree-visualization.html`).
2. Create a file named `tabTree.json` in the same directory as your HTML file.
3. Copy your JSON data into `tabTree.json`. Make sure it's valid JSON.
4. Due to browser security restrictions, you'll need to serve these files from a web server rather than opening the HTML file directly. You can use a simple local server for development purposes. Here are a couple of options:
    - If you have Python installed:
        - For Python 3.x: Run `python -m http.server` in the directory containing your files.
        - For Python 2.x: Run `python -m SimpleHTTPServer` in the directory containing your files.
    - If you have Node.js installed:
        - Install `http-server` globally: `npm install -g http-server`
        - Run `http-server` in the directory containing your files.
5. Open a web browser and navigate to `http://localhost:8000` (or whatever port your server is using).
6. Click on `tree-visualization.html` to view your visualization.

This approach separates your data from your visualization code, making it easier to update the data independently. It also allows for potentially larger datasets without bloating your HTML file.

2024-10-20T11:51:45-07:00
Very nice:
![[Pasted image 20241020115147.png]]

## TODO
Tracker side:
- get thumbnail
- [x] add button to start and stop tracking
	- maybe also a timer for duration of the tracking
- [x] integrate the viewer directly into the plugin
- [x] track also the top five occurrence words?
Viewer side:
- display more details when hovering?
- dynamic move the nodes when hovering and displaying more details?

# Migrating to Claude projects

2024-10-22T22:12:29-07:00

Going to feed it the existing project files.

It recommended the following order for the new features:
Suggested Implementation Order:
1. [x] Start/Stop Tracking (fundamental control feature)
2. [x] Word Frequency Analysis (data enrichment, no UI changes needed)
3. [x] Integrated Viewer (foundation for other viewer features)
4. Thumbnails (more complex, requires storage handling)
5. Enhanced Hover Details (builds on previous features)
6. Dynamic Node Movement (polish feature)
7. Duration Timer (optional enhancement)

As well as additional features:
1. Search/Filter functionality based on:
    - Word frequency data
    - Time ranges
    - Domain patterns
2. Export options (different formats)
3. Session management (save/load different tracking sessions)
4. Tab grouping visualization
5. Statistics dashboard

2024-10-22T22:58:29-07:00
Implementing start/stop tracking, as well as word count.
2024-10-22T23:28:20-07:00
Tested and committed. Going to integrate the viewer tomorrow.

2024-10-23T08:01:05-07:00
- add different icons for different status
- better logging

# Viewer integration

2024-10-23T21:10:34-07:00
Asked Claude to plan out the viewer integration process.
It generated a few overview and process / flow charts:

```
extension/
├── popup/
│   ├── popup.html    (simple controls & mini preview)
│   └── popup.js      (basic interactions)
├── viewer/
│   ├── viewer.html   (full visualization page)
│   ├── viewer.js     (main visualization logic)
│   └── components/   (modular visualization components)
│       ├── tree.js   (D3.js tree rendering)
│       ├── controls.js (layout/view controls)
│       └── details.js (node details/hover panel)
├── background.js
└── manifest.json
```

Component comm flow:
```
Background Script <--> Viewer Page
         ↑
         |
      Popup UI
```

Data flow:

```
[Background]     [Storage]        [Viewer]
  Tree Data  -->  Chrome  -->  Initial Load
     |            Storage         |
  Updates  ---------------------->|  (via message passing)
     |                           |
  Changes  <----------------------|  (if we add interactive features)
```


Should try to debug the tracker errors before moving on to integrating viewer.

background.js:166 Uncaught (in promise) TypeError: Cannot read properties of undefined (reading 'create')
    at initializeEventListeners (background.js:166:17)
    at background.js:187:5

2024-10-23T21:49:41-07:00 debug of missing functions and permission of alarms resolved.

2024-10-23T23:27:05-07:00
Ok a lot of suffering to rewrite the background.js and improve the viewer.
- plugin popup looks much nicer:
![[Pasted image 20241023234228.png]]
The toggle animation is glitched for some of the leaf nodes.
- 2024-10-23T23:40:06-07:00: mostly fixed. 


TODO:
- fix the plugin popup failed to initialize when the viewer tab is open
