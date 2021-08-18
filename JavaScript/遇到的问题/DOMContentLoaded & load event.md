## DOMContentLoaded
The DOMContentLoaded event fires when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading. The original target for this event is the Document that has loaded. You can listen for this event on the `window` interface to handle it in the capture or bubbling phases.
## load
A different event, load, should be used only to detect a fully-loaded page.
load is most often used within the `<body> element` to execute a script once a web page has completely loaded all content (including images, script files, CSS files, etc.).
