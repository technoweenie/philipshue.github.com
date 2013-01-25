---
layout: page
title: "Core concepts"
description: ""
group: "navigation"
weight: 5
---
{% include JB/setup %}

The hue system is built around the idea of everything in your system having a unique URL served by the bridge.  Interacting with these URLs lets you modify them or find out their current state as explained above.

#### Hue web addresses
A hue resource web address will always start with the following.

`http://<bridge IP address>/api`

This is the RESTful root and is how your app or controller talks to the hue bridge interface.

#### The hue username
In most of the commands (the exception is creating a username) you’ll also include a username after this:

`http://<bridge IP address>/api/<username>`

This username determines which resources you have access to. If you provide a username that isn't known to the bridge then most resources won’t be available to you. Using one that is authorized, as shown in the [getting started](/3_gettingstarted.html) section, will allow you to interact with pretty much everything interesting.

Each new instance of an app should use a unique username which you generate using the [Create New User command](/4_configurationapi.html#41_create_user).

#### Lights/groups and more
There are different kinds of resources to interact with and we've clustered them together with similar objects.  The four available currently are:
*	`/lights` resource which contains all the light resources
*	`/groups` resource which contains all the groups
*	`/config` resource which contains all the configuration items
*	`/schedules` which contains all the schedules

We'll be adding more as we add features to the system

You can query resources available in your bridge by doing a `GET` on its local URL. For example the following returns all configuration items in your bridge.

|Address	|`http://<bridge ip address>/api/newdeveloper/config`	|
|:--------|:----------------------------------------------------|
|Body			|																											|	
|Method		|`GET`																								|


#### Changing a setting or resource
The principle for changing a setting, attribute or other 'resource' on the device is to send a `PUT` request to the URL of its parent. The desired new value is attached to the request in the 'message body' in JSON format.

For example to change the name of the bridge (`/config/name`) we address the parent of this setting (`/config`) and send the new name with the request in the message body.

|Address	|`http://<bridge ip address>/api/newdeveloper/config`	|
|:--------|:----------------------------------------------------|
|Body			|`{"name":"Developer Bridge"}`												|
|Method		|`PUT`																								| 

If you're doing something that isn't allowed, maybe setting a value out of range or typo in the resource name, then you'll get an error message letting you know what’s wrong. For much more information see the [error section](/8_errormessages.html).

Sometimes, when you do a `GET`, the JSON response will have attributes that contain an object for a value – in other words a resource has a set of child resources.

For example:

|Address	|`http://<bridge ip address>/api/newdeveloper/lights/1`	|
|:--------|:------------------------------------------------------|
|Method		|`GET`																									|

gives a JSON response with an attribute called "state". The value of state is an object which in turn contains a whole bunch of other attributes! To modify these we would need to address `/lights/1/state`.


#### Representations
The hue system is built on the principle of representations. Each object in the hue system has a corresponding resource in the hue interface (a representation of the real object). If you interact directly with these representations and afterwards the necessary Zigbee commands are sent out to lights to effect the change.

This lets us do things fast without worrying about the wireless messages bouncing between the lights, but it can mean that things are temporarily inconsistent. By this we mean the state of the lights and the state of their representation are not the same for a short period. It corrects itself after a while, but for example if you set all your lights to red and then power cycle a light so that it goes to white, it could take a few minutes for the bridge to update its representation.

The same thing is true for our portal with a representation there, which may be temporarily inconsistent with the bridge representation. This all lets us do things fast and deal with lots of concurrent users.


#### Controlling light
There are many properties that can be controlled with hue – more than you might expect. All of these properties are in the `/state` resource of a light. Let’s go through them and explain.


#### The easy ones
**"on"** – This is the easiest example. A light can be on or off. So setting this resource to true turns a light on to its last setting. Setting the resource to false turns the light off.

**"bri** – This is the brightness of a light from its minimum brightness 0 to its maximum brightness 255 (note minimum brightness is not off). This range has been calibrated so there a perceptually similar steps in brightness over the range. You can set the “bri” resource to a specific value e.g. the following commands sets the lights to 42/255 of their maximum brightness.

|Address	|`http://<bridge ip address>/api/newdeveloper/lights/1/state`	|
|:--------|:------------------------------------------------------------|
|Body			|`{"bri":42}`																									|
|Method		|`PUT`																												|	


#### Color gets more complicated
The color point of light has lots of ways of being quantified. The diagram below shows a plot of what is known as the CIE color space – a green triangle outlines the colors which hue can address.

![CIE color space](/assets/themes/twitter/img/cie-colorspace.png)

All points on this plot have unique xy coordinates that can be used when setting the color of a hue bulb. If an xy value outside of the green triangle is chosen, it will produce the closest color it can make. To control lights with xy use the "xy" resource which takes the value of an array of two values between 0 and 1 e.g. `"xy":[0.675,0.322]` is <span class="text-red">red</span>.

We can also choose to address the color point of light in a different way, using colors on the black curved line in the center of the diagram. This is the line that follows white colors from a warm white to a cold white. hue supports color temperatures from 2000K (warm) to 6500K  (cold) with great white light. To set the light to a white value you need to interact with the "ct" (color temperature) resource, which takes values in a scale called "reciprocal megakelvin" or "mirek". Using this scale, the warmest color 2000K is 500 mirek (`"ct":500`) and the coldest color 6500K is 153 mirek (`"ct":153`). As with xy, the light will go to the closest value it can produce if the specified color temperature is outside of the achievable range.
 
The final way to choose the color of your lights is with hue and saturation. These values will depend on the color capabilities of the bulb you are using, but since all hue bulbs are the same that’s not a problem. The hue property at maximum saturation basically goes around all points on the edge of the green triangle (all the most saturated colors).

Reducing the saturation takes this hue and moves it in a straight line towards the white point. So `"sat":255` always gives the most saturated colors and reducing it to `"sat":200` makes them less intense and  more white.

Below are some common hue values and the corresponding xy values.

<table class="colored-table">
<thead>
	<tr>
	<th>Hue</th>
	<th>Final-x</th>
	<th>Final-y</th>
	</tr>
</thead>
<tbody>
<tr class="row-red">
<td>0</td><td>6.7500E-01</td><td>3.2200E-01</td>
</tr>
<tr class="row-yellow">
<td>12750</td><td>	5.4200E-01</td><td>	4.2000E-01</td>
</tr>
<tr class="row-lightgreen">
<td>25500</td><td>	4.0900E-01</td><td>	5.1800E-01</td>
</tr>
<tr class="row-green">
<td>36210</td><td>	2.8800E-01</td><td>	2.7900E-01</td>
</tr>
<tr class="row-blue">
<td>46920</td><td>	1.6700E-01</td><td>	4.0000E-02</td>
</tr>
<tr class="row-magenta">
<td>56100</td><td>	4.2100E-01</td><td>	1.8100E-01</td>
</tr>
<tr class="row-red">
<td>65280</td><td>	6.7500E-01</td><td>	3.2200E-01</td>
</tr>
</tbody>
</table>

As with bri you can choose to set "hue" and "sat" to an absolute value.

So this command sets a light to blue:

|Address	|`http://<bridge ip address>/api/newdeveloper/lights/1/state`	|
|:--------|:------------------------------------------------------------|
|Body			|`{"hue":46920}`																							|
|Method		|`PUT`																												|


#### Some extra fun stuff
We have some extra fun resources supported by the bridge you can play with.

**"effect"** – this resource is for dynamic effects and currently only supports two values, but we'll add more in the future. `{"effect":"colorloop"}` puts the lights in a color looping mode until it is stopped by sending `{"effect":"none"}`.

**"alert"** – this resource is for temporary effects and more may come in the future. At the moment to make the light do a blink in its current color send `{"alert":"select"}`


#### Groups
You can also choose to control all your lights at once. For this you need to address the `/groups/0` resource (this is a special group that always contains all lights). For groups we have the "action" resource (similar to the state resource for lights but containing the last values sent to the group rather than a state).

You can modify resources in the "action" resource in the same way as the state resource. So the following turns all lights off:

|Address	|`http://<bridge ip address>/api/newdeveloper/groups/0/action`	|
|:--------|:--------------------------------------------------------------|
|Body			|`{"on":false}`																									|
|Method		|`PUT`																													|

And this sets them all to full brightness red.

|Address|`http://<bridge ip address>/api/newdeveloper/groups/0/action`	|
|:--------|:------------------------------------------------------------|
|Body		|`{"on":true,"bri":255,"sat":255,"hue":0}`											|
|Method	|`PUT`																													|



#### Limitations
With these commands you can control any number of your lights in any way you want, just address the correct light resource and send it the command you want.

We have some limitations to bear in mind:
* We can't send commands to the lights too fast. If you stick to around 10 commands per second to the `/lights` resource as maximum you should be fine. For `/groups` commands you should keep to a maximum of 1 per second.
*	If you try and control multiple conflicting parameters at once e.g. `{"ct":250,"xy":[0.5,0.5]}` the lights can only physically do one, for this the following rule applies: xy beats ct beats hue, sat – Simple.
*	All your lights have to be in range. If you can control them from the hue app then you’re fine to play with them via the developer interface. Remember all lights act as a signal repeater so if you want more range put a light in between.
