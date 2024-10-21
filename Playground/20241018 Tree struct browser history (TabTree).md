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
- add button to start and stop tracking
	- maybe also a timer for duration of the tracking
- integrate the viewer directly into the plugin
Viewer side:
- 