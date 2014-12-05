Node-RED Flow for mobile-web-app
=================================

**Updated to support browser INIT request which triggers onload/refresh/reconnect.**

A Node-RED (NR) Flow that contains an example mobile web app served from NR.  The mobile app is contained in a NR template and uses web sockets to commmunicate using the HTML/DOM ID of the UI widget and a value. To change the app copy node content to a good HTML editor. N.B. The app assumes your are using HTTPS so the web socket URI needs to chnaged to just ws if your using unsecure HTTP (line 20).

**To access this UI http://ip add of Node-RED:1880/mobiui**

Message exchange json `{id: ,v: }`
Where id is the HTML/DOM id of the widget 
```HTML
<label for="tsw-1">flip switch 1</label>
<select  id="tsw-1"  data-role="flipswitch">
   <option value="0">Off</option>
   <option value="1">On</option>
</select>
```
Example to toggle flip switch 1 On from Node-RED send following to WebSocket output node
```javascript
msg.payload = {id:"tsw-1", v: 1};
``` 

Process events and actions from mobile UI from WebSocket input node
```javascript 
var obj = JSON.parse(msg.payload);
delete msg.payload;
delete msg._session;

// toggle switch tsw-1	- output 1 
if(obj.id=="tsw-1"){
	msg.payload = {"id" : obj.id, "v": obj.v};	
	return msg;
}
```

HTML widget naming for updating from Node-RED 3 charaters folloed by hypen 
```javascript
flip-switch: 	tsw-nnn
slider: 	sld-nnn
value:		val-nnn
```
Sheduler 			
=================================
I've added a scheduler to the UI which the user can set 21 diffrent events each with a start time, end time and values.
End times can be ignored to create one off events eg. one with just a start time.

**To access this UI http://ip add of Node-RED:1880/mobiuiShed**

```javascript
Create from Node-RED root a folder called data
Copy schedule.json to data folder
Copy & paste mobi_scheduler_flow.json to a Node-RED workspace
```
**If you already have the mobi-flow installed. Either delete the flow completely or merge the new flow to ensure there is only 1 Web socket in & out node.  If old ones are left the app won't work!** 

When a user commits changes from the browser they are saved in **schedule.json**. 
The schedule is reset at just after midnight on a day transition. Events that span two days are **Not** reset then, but when the end event is triggered.

If an scheduled event has triggered and it's time is edited for a later period in the day it will **Not** run until the next day. However this will probably change to allow re-triggering of events.


![alt tag](http://industrialinternet.co.uk/wp-content/uploads/2013/03/schedule2-145x300.png)
