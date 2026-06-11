
# Unifi_Intercom_Auto_Picture_Change
Automatic picture change for the Unifi Intercom, depending on the weather forecast. Made in Home Assistant / Node Red.

This is my first post here on Github.

I use this automation for changing the pictures on the Unifi Intercom, depending on the actual weather forecast. This automation is created with some help from AI.

**What I Currently use**
Unifi:
UDM Pro with: 
- Unifi OS version 5.1.15
- Network version 10.4.57
- Access version 4.28.28
- UA G3 Intercom version 1.11.24.0
- UA Hub Gate version 2.5.25.0 

Home Assistant:
- Core 2026.5.0
- Operating System 71.3
- Node Red addon V4.1.9


**Step 1 Unifi**
I did use Chrome for those steps, because Safari did give some difficulty with some steps.
Login into the UDM with your account with a direct connection and not with the unifi.ui.com cloud access.
1.	press in the top bar on the “Intercom Access” icon.
2.	press on the left bar at the bottom at the “People”
3.	press on “Create New” to make an extra login.
4.	Select first the options “Admin” and “Restrict to Local Access Only”, and give it then a Username and Password. (Make sure the password is save and don’t use spaces in the username or password).
5.	Deselect “Use a Predefined Role” then select at “Network, Protect, People” “None” and leave only “Intercom” at “Full Management”.
6.	Select “Save as a Predefined Role” and name it.
7.	Press on “Create”

8.	On the left bar, press on the interface designer icon (3th from the top). You will see the layout of your intercom.
9.	Make sure you have there your alternative pictures. I have here the following pictures, “Sunny, Rainy, Cloudy, Snowy, and Sleepy)
10.	Make sure you have the last picture selected.

<img width="401" height="307" alt="Picture 1" src="https://github.com/user-attachments/assets/61bec96f-9f07-4bb9-8932-b5ea0ae7f093" />

For the next part I did use Chrome.

11.	Press on the function key F12,
12.	In the screen/frame that opened, select on top “Network” 
13.	Now change the picture of the intercom to the first picture.
14.	In the frame on the right, there will appear at the bottom a new line. At me it looks like “<:> settings”
15.	Press on this “<:> settings” and the Payload will appear. If not, then select the payload tap in the screen.
  
<img width="428" height="300" alt="Picture 2" src="https://github.com/user-attachments/assets/87a6b3a7-afc2-4aa5-b4f6-bd32291e91fd" />

16.	Copy the value: “activities_intercom_resource/Intercom………………….JPG” (or .PNG) to Notepad or somewhere else for later use.
17.	Then go to the “Headers” tap
18.	In the headers tap, you will see “Request Headers” and a little below “:path”. Copy the value of path to Notepad. Save it like:
https://IPofUDMP/valueofpath so it will looks something like https://192.168.0.1/proxy/access/api/v2/device/1f3g2daa4c92/settings  (But with your own link)

19.	Select the second picture, at the bottom or the right frame you will see a new “<:> settings” appearing. Select that one, copy the request payload value.
20.	Repeat this last step for the rest of the pictures and save each payload value.

After this we are done in the Unify Intercom settings. Only later the Intercom page can be handy for checking if the pictures are changing. Just remember that after each picture change the page need to be refreshed to see the results!


**Step 2 Home Assistant:**
1.	Go in HA to  Settings  Devices & services  Helpers
2.	Creat a new helper, select in the Creat helper screen “Toggle”
3.	Gif it the name: intercom_sleeping

**Step 3 Node Red**
Import the intercom flow in Node Red as a new flow by pressing on the three stripes on the top right of the Node Red screen and selecting Import.

1.	Weather.forecast_home nodes: Change in those two nodes the server and entity to the right weather.forecast_home entity. I use for the Entity ID for both nodes the standard HA weather forecast. This HA weather forecast has the type of weather “rain, sun, cloud” in the payload and not as a attribute.
2.	Sleep Button node: Change if needed the “sleep button” state node to the server and entity that is created in the helpers screen of HA.
3.	Picture selector node:
 Line 6 : In case you did give the “sleep button” a different name, then change here the “input_boolan.intercom_sleeping” to the correct name.
Line 14 – 19 : Replace here the picture links you did saved in step 1-16 and 1-19 to the right weather states.
4.	Local Login node: Fill in here the User account and Password you did created for the intercom access at step 1-4.
5.	Unifi Login node: Change here in the URL link the IP address to the IP of your UDMP. And be sure that HA/Node-Red is not blocked by the firewall.
6.	Tokens & Dynamic Payload node: No changes
7.	Push to Intercom node:  Place here in the URL that is created at step 1-18

8.	Finished, press on deploy!

After deploying wait a moment for testing. Press the injection node to inject the weather state. Below the picture selector you will see “New picture: …..” In the debug field, you see if good the message: “Success”
When you press the injection node again, you see below the picture selector: “No change, currently: ……”. And when adjusting the sleeping button in HA, you see the state change below the picture selector.
You can see the changes in the Intercom screen in the Unifi intercom screen. You only need to refresh the screen after each change to see the result in the browser. Or you walk to the Intercom ;)
