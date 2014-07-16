Node-RED Flow for mobile-web-app
=================================

A Node-RED (NR) Flow that contains an example mobile web app served from NR.  The mobile app is contained in a NR template and uses web sockets to commmunicate using the HTML/DOM ID of the UI widget and a value. To change the app copy node content to a good HTML editor. N.B. The app assumes your are using HTTPS so the web socket URI needs to chnaged to just ws if your using unsecure HTTP (line 20).

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
	msg.id = obj.id; 	
	msg.state = obj.v;	
	return msg;
}
```

HTML widget naming for updating from Node-RED 3 charaters folloed by hypen 
```javascript
flip-switch: 	tsw-nnn
slider: 	sld-nnn
value:		val-nnn
```
			
