# html-node-views
Testing Information and examples for html node views.  This document is intended for developers and testers. HTML Node Views are not production ready.

# Server
During testing all HTML files should be hosted on a publicly accessible and secure server.  Production files may be saved in eisy or other location to be determined by UDI.
#### GitPages:
This example was created using Git Pages, follow instructions a https://pages.github.com/ for information staring your server.
Note that gitpages may not update immediately after pushing to your git pages repository. So please wait a few minutes to check if your subdomain is running at https://username.github.io. When changing .html files please inspect your html on a browser using Developer Tools to verify your changes are being served before testing on UD Mobile.

#File
Copy the example-tile?height=180&span=3.html then add to the repository created above.  Push the new file to origin.

```
git add .
git push origin main
```
Any file in this repository will be available at https://username.github.io/filename  however this file name has characters which need to be encoded (more on this below) so use the following URL instead to verify the file is being served
The characters "?" (%3F), "=" (%3D), and "&" (%26) must be encoded for this file to be viewed
```
https://username.github.io/example-tile%3Fheight%3D180%26span%3D3.html
```
The url above should show the following.
![Screenshot 2023-12-04 at 8 29 12 PM](https://github.com/UniversalDevicesInc/html-node-views/assets/14967116/c02d076e-c475-486e-a93f-6dfc1898c8fd)

# UD Mobile html node view
This is a scrollable full screen view.
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
Save the favorite.
The favorite tile should now look similar to the following image.

![single_fav_tile](https://github.com/UniversalDevicesInc/html-node-views/assets/14967116/b2839db0-b871-4a87-8eca-c6a508c6170d)













