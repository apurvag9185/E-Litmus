# Elitmus
Proctoring for Safe Exam

## Steps used

So there’s some basic stuff which is required, it’s just like making a website, with a manifest!

HTML: The building block of all websites, a standard markup language which along with CSS and JAVASCRIPT is used by web developers to create websites, mobile user interfaces, and applications.

CSS: A style sheet language used to set the style for the HTML elements.
JavaScript: Commonly used to create interactive effects within web browsers.
JSON: JavaScript Object Notation, is an open standard format that uses human-readable text to transmit data objects consisting of attribute-value pairs. It is the primary data format used for asynchronous browser/server communication (AJAJ), largely replacing XML (used by AJAX).

A few Preliminaries: Chrome Extensions follow a specific directory structure. That means, the filename is already fixed, they should be organized in a certain way as instructed.

Main Components of Chrome App:

The manifest tells Chrome about your app, what it is, how to launch it, and the extra permissions that it requires.
The background script is used to create the event page responsible for managing the app life cycle.
All code must be included in the Chrome App package. This includes HTML, JS, CSS, and Native Client modules.
All icons and other assets must be included in the package as well.

Directory Structure:

json
<content>.js [ Javascript Files]
<markup>.html [ HTML files ]
png

Here, we are going to make a extension for a online exam to be completed without cheating.
Step 1: Create a new Directory, this is where we will keep all our files.

Step 2: Create a file called Manifest.json

Here’s the Basic Format:

{
"manifest_version": 2,
"name": “EXTENSION NAME",
"description": "DESCRIPTION",
"version": "1.0",
"browser_action": {
"default_icon": “ICON (WITH EXTENSION) ”,
"default_popup": “LAYOUT HTML FILE"
},
"permissions": [
//ANY OTHER PERMISSIONS
]
}

Here is our Manifest.json file:
{
    "manifest_version": 3,
    "name": "Proctoring Paper",
    "description": "Demo",
    "version": "1.0",
    "content_scripts": [{
        "matches": ["<all_urls>"],
        "js": ["content.js"]
    }],
    "permissions": ["tabs", "webRequest", "storage", "activeTab", "webNavigation"]
}

So once’s you’ve got the hang of manifest.json, let’s go ahead.
Step 3: Create a new file called window.html.

It is the HTML that POPS UP when you click the Chrome extension button.

Step 4: Create the javascript file, let’s call it, content.js.
Here is our Manifest.json file:
{
chrome.tabs.getSelected(null,function(tab) {
    if (tab.url.indexOf("google.com") > -1) {
        //Start the test

        chrome.windows.update(chrome.windows.WINDOW_ID_CURRENT, { state: "fullscreen" });


        chrome.tabs.onActivated.addListener(function (activeInfo) {
            chrome.tabs.get(activeInfo.tabId, function (tab) {
                alert("You can't switch between tabs");
            });
        });
    }

    navigator.getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia;
    navigator.getUserMedia({ audio: true, video: true }, function (stream) {
        //Check for the audio, camera and internet stability
        console.log("Audio and video works");
    }, function (err) {
        //Handle error
        alert("Audio or video not working");
    });

    //Full Mode
    
    chrome.tabs.query({}, function (tabs) {
        if (tabs.length > 1) {
            //Limit the number of tabs
            chrome.tabs.remove(tabs[1].id);
        }
    });


    //override the default browser close button
    document.addEventListener('keydown', function (e) {
        if (e.key === 'w' && (e.ctrlKey || e.metaKey)) {
            e.preventDefault();
        }
    });

    chrome.storage.local.set({ 'ip': '', 'requirementsCheck': 'pass' });

});
}


Step 5:
To Load the extension,

Drag and drop the directory where your extension files live onto chrome://extensions in your browser to load it.
If the extension is valid, it’ll be loaded up and active right away!



### Features
- [ ] Extension should work only in selected URLs(test page) during a certain time/trigger.
- [x] The browser should open in full screen mode.
- [x] More than one tab can’t be opened. 
- [x] Users should not be able to close the tab by the normal close button [shortcut keys should not work too]. (User should only be able to close tab by clicking on
      “End Test” Button) 
- [x] Should do requirement check initially when extension is activated:
 
      a. Audio  
      
      b. Camera
      
      c. Internet Stability
- [x] Capture the user related information in local storage.(e.g. IP, requirements check)      
- [x] Can add any additional functionality which is present in the Safe exam browser. Also Any new functionality which is not yet present in the safe exam browser but       can help in preventing cheating in online exams.
      

