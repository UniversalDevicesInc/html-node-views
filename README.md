# html-node-views
Testing Information and examples for html node views.  This document is intended for developers and testers. HTML Node Views are not production ready.

See https://wiki.universal-devices.com/UD_Mobile_Custom_Tiles for quick start example.

# Requirements:
UD Mobile for iOS version 1.1.47 or greater or Android 1.1.45 or greater.

# Known Issues:
(1) Support for multiple addresse '{"setObservedAddresses": []}' is not fully implemented, currently testing basic functionality for attached nodes.  

(2) Some Favorites and User Preferences settings such as node/favorite color, icon, and Hide Background are not passed to the .html file.

# Server
During testing all HTML files should be hosted on a publicly accessible and secure server.  Production files may be saved on eisy or other location to be determined by UDI.
#### GitPages:
This example was created using Git Pages, follow instructions at https://pages.github.com/ for information starting your server.
Note that gitpages may not update immediately after pushing to your git pages repository. So please wait a few minutes to check if your subdomain is running at https://username.github.io. When changing .html files please inspect your html on a browser using Developer Tools to verify your changes are being served before testing on UD Mobile.

# HTML Favorites Tile File
Download the example file named 'example-tile?height=180&span=3.html' located in this repository (https://github.com/UniversalDevicesInc/html-node-views/blob/main/example-tile%3Fheight%3D180%26span%3D3.html).  Add the example html file to the directory created in the Git Pages instructions, then push the new file to git origin.

```
git add .
```
```
git commit
```
```
git push origin main
```


Any file in this repository will be available at https://username.github.io/filename  however this file name has characters which need to be encoded (more on this below) so use the following URL instead to verify the file is being served.
The character "?" (%3F) must be encoded for this file to be viewed in a web browser.
```
https://username.github.io/example-tile%3Fheight%3D180%26span%3D3.html
```
The URL above should show the following. Tip: "username" must be replaced with your GitHub username, as shown in your GitHub repostory.
![Screenshot 2024-02-25 at 4 05 21 PM](https://github.com/UniversalDevicesInc/html-node-views/assets/14967116/513ce583-ca52-4910-a83d-28fc3e76de75)


# UD Mobile html Node Views
This is a scrollable full screen view similar to html Favorites Tiles below.
Coming Soon!

# UD Mobile html Favorites Tiles
This is a non-scrollable size limited favorites tile.
During testing the html file must be linked manually.  This will likely change before production.
#### Add Node To Favorites:
Go to UD Mobile and add a dimmable light or switch node to Favorites. This can be done from the home tab by clicking the node name (clicking icon may toggle the switch), then pressing the settings icon, then pressing "Add to Favorites".
#### Link file to node
From Favorites Tab enter Edit Mode by clicking the Edit Button (pencil icon) at the top of the screen then click the dimmable node, then click Edit from the popup dialog.  
Change the Favorite Type to "(Beta) Custom Tile".
Enter the following URL in HTML URL. Note that this URL is not URL Encoded:
```
https://username.github.io/example-tile?height=180&span=3.html
```
Favorites Editor Shold look similar to the following:
![88544](https://github.com/UniversalDevicesInc/html-node-views/assets/14967116/5c4fd689-1c87-445a-8eec-fefacbfdc2bd)



Save the favorite.
The favorite tile should now look similar to the following image.

![single_fav_tile](https://github.com/UniversalDevicesInc/html-node-views/assets/14967116/22cf2e8d-1ef8-4715-99cb-7cfc85a6ddba)


Your first Favorites html tile should now be working!

# Favorites Tiles html
#### Favorites Tile File Names:
Favorites html tiles include a query string in the file name so that developers can request the tile height and span "height=180&span=3".  If these values are not present the app will use default values which may cause the tile to be too large/small.  Note that the query string is only required for Favorites html tiles not html Node Views.

# Html JavaScript:
UD Mobile communicates with your html file with JavaScript (<script>).  These functions must be included in your html files to receive or send messages to UD Mobile.

# JavaScript newMessage Function
This function receives messages from UD Mobile. The message parameter "jsonString" is JSON as string which needs to be converted to a JavaScript JSON Object.
```
 function newMessage(jsonString) {
    let json = JSON.parse(jsonString);
    ...
}
```
The JSON object should have a single key with a JSON object as the value.
## newMessage JSON Keys
````
{statusUpdate: {...}}
````
````
{nodeUpdate: {...}}
````
````
{getObservedAddresses: {...}}
````
### {statusUpdate: {...}}
This object contains status <st> updates from UD Mobile.  This can be used to populate your html elements.
```
{"statusUpdate": {"address": "ZB25235_001_1","formatted": "100%","status_name": "On Level", "value" : 100, "prec" : 0, "status_id" : "OL" }}
```

### {nodeUpdate: {...}}
This object contains node data for the node which is linked to this file. This can be used to show the node name (or favorite name) 
```
{"nodeUpdate": {"address": "ZB25235_001_1","name": "ZB 25235.1 On-Off Switch", "enabled":  true}}
```
### {getObservedAddresses: {...}}
This object is a GET request from UD Mobile which is requesting that this file send any additional node addresses which require status observation. The linked node address is included in this object so that developers can request child/sibling nodes if needed.
```
{"getObservedAddresses": {"address": "ZB25235_001_1"}}
```
Upon receipt of this GET request the html file can (optionally) publish additional node addresses for observation, see {"setObservedAddresses": []} below.  There is no need to request the same node address as it is observed automatically.


# JavaScript publishMessage Function
This function sends messages to UD Mobile. The message parameter "jsonString" is JSON as string.  This function holds commands to send messages to both iOS and Android.  There may be a web function in the future.
```
 function publishMessage(jsonString) {
    console.log('publishMessage');
    try {
        <!-- iOS -->
        if (window.webkit != undefined) {
            webkit.messageHandlers.postToUdm.postMessage(jsonString);
        }
        <!-- Android -->
        if (window.androidInterface != undefined) {
            androidInterface.postToUdm(jsonString);
        }
        <!-- TODO Add other Clients such as browser -->
    } catch(err) {
        console.log('error');
    }
}
```

## publishMessage JSON Keys
```
{"setObservedAddresses": []}
```
```
{"sendCommand": {...}}
```
### {"sendCommand": {...}}
This Object is used to send commands to UD Mobile. Note that the Address parameter may be empty if sending to the attached node. If sending to a node which is not attached, then address must be included or command will be sent to the attached node.  Note that UD Mobile will only send commands to observed node addresses, see {"setObservedAddresses": []} JSON Object

The following example shows how to update the attached node with the DON (ON) command to 34%. See the slider in the example-tile for an example.
```
{"sendCommand":{"address":"","cmd":"DON","p":[{"pId":"","value":"34","uom":"51"}]}}
```
The following example shows how to update the attached node with the DON (ON) command to 34% with a Ramp Rate (RR) of 3 seconds. Note that ramp rate may be different values depending on node.
```
{"sendCommand":{"address":"","cmd":"DON","p":[{"pId":"","value":"34","uom":"51"}, {"pId":"RR","value":"3.0","uom":"58"}]}}
```
The following example shows a command without parameters. This sends the DOF (OFF) Command to the attached node.  See the button in example-tile for an example.
```
{"sendCommand": {"address":"", "cmd": "DOF", "p": []}}
```



### {"setObservedAddresses": []}
This Object should contain a list of node addresses which the file would like to observe.  There is no need to send the attached node address.
NOTE: As of writing this document this function is not yet implemented in UD Mobile.
```
{"setObservedAddresses": []}
```
```
{"setObservedAddresses": ["ZB25235_001_2", "ZB25235_001_3"]}
```


# Light and Dark Modes
The 'example-tile?height=180&span=3.html' has the following CSS which will automaitcally change the tile background/text color to match UD Mobile. If this style is not included then the tile may not match colors in UDM.
```
 <style>
    /* To support Light and Dark Modes*/
    :root {
        color-scheme: light dark;
    }
    body {
      /* This is the standard Favorites tile color, this can be removed if no background is desired*/
      background-color: #B0B1B133;
    }
```













