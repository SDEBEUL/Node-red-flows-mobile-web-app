Node-red-flows-mobile-web-app
=============================

A Node-RED (NR) Flow that contains an example mobile web app served from NR.  The mobile app is contained in a NR template and uses web sockets to commmunicate using the HTML ID of the UI widget and a value. To change the app copy node content to a good HTML editor.

Message exchange json {id:"tsw-1", v: 1};
Where ID is the HTML/Dom ID of the widget 
```
<label for="tsw-1">flip switch 1</label>
<select id="tsw-1" data-role="flipswitch" data-state="0" data-req="">
   <option value="0">Off</option>
   <option value="1">On</option>
</select>
```
Toggle flip switch 1 ON from Node-RED
`msg.payload = {id:"tsw-1", v: 1};` 

Process events and actions from mobile UI from WebSocket node
``` 
var obj = JSON.parse(msg.payload);
delete msg.payload;
delete msg._session;

// toggle switch tsw-1	- output 1 
if(obj.id=="tsw-1"){
	msg.id = obj.id; 	
	msg.state = obj.v;	
	return msg;
}
```
