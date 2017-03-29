---
layout: project_view
title:  "Advance Log Filter"
permalink: /project/advance-log-filter/
categories:
- Project
img: advance-log-filter/thumbnail.png
thumb: generic.png
carousel:
- advance-log-filter/carousel/single01.png
---
**Description**

Chrome Extension – Advanced Log Filter was written to add simple filtering
options to the Chrome Devtool’s console. It works based off console.log() calls
that begin with a ‘tag name’ inside of two brackets, such as
console.log(‘[UI_SYSTEM] Something went wrong!’);

---

**Installation**

Navigate to chrome://extensions Expand the developer dropdown menu and click
“Load Unpacked Extension” Navigate to local folder Assuming there are no errors,
the extension should load into your browser.

---

**Usage**
```javascript
console.log('[MYTAG] Message');
```

---

**Known Issues**

Currently we’re having to use a whitelist Chrome Extension ID to allow us to
inject JS into the devtools window. Thus, this extension is not compatible with
Discover DevTools Companion.
