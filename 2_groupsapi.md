---
layout: page
title: 2. Groups API
description: ""
group: navigation_api
weight: 2
---

{% include JB/setup %}

## 2.1.	Get all groups
|URL				|`/api/<username>/groups` |
|:----------|:------------------------|
|Method			|`GET`										|
|Version		|1.0											|
|Permission	|Whitelist								|

### 2.1.1.	Description
Gets a list of all groups that have been added to the bridge. A group is a list of lights that can be created, [modified](#24_set_group_attributes) and deleted by a user. The maximum numbers of groups is 16. N.B. For the first bridge firmware release, bridge software version 01003542 only, a limited number of these APIs are supported in the firmware so only control of groups/0 is supported. 

### 2.1.2.	Response
Returns a list of all groups in the system, each group has a name and unique identification number.

If there are no groups then the bridge will return an empty object, `{}`.

### 2.1.3.	Response example
{% highlight json %}
{
	"1": {
		"name": "Group 1"
	},
	"2": {
		"name": "VRC 2"
	}
}
{% endhighlight %}

### 2.1.4.	Notes
Group 0 is a special group containing all lights in the system, and is not returned by the 'get all groups' command.

 
## 2.2.	Create group

This method is not supported in the 1.0 version of the API.  It is scheduled for release in the next version of the API. 


## 2.3.	Get group attributes

|URL				|`/api/<username>/groups/<id>`	|
|:----------|:------------------------------|
|Method			|`GET`													|
|Version		|1.0														|
|Permission	|Whitelist											|

### 2.3.1.	Description
Gets the name, light membership and last command for a given group.

### 2.3.2.	Response

|Name		|**Type**						|**Description**	|
|:------|:------------------|:----------------|
|action	|object							|The last command that was sent to the whole group.<br /><br />Note this is not necessarily the current state of the group. |
|lights	|array of light IDs	|	The IDs of the lights that are in the group. |
|name		|string 0, 32				|A unique, editable name given to the group. |

### 2.3.3.	Response 
{% highlight json %}
{
	"action": {
		"on": true,
		"hue": 0,
		"effect": "none",
		"bri": 100,
		"sat": 100,
		"ct": 500,
		"xy": [0.5, 0.5]
	},
	"lights": [
		"1",
		"2"
	],
	"name": "bedroom",
	"scenes": {
	}
}
{% endhighlight %}

### 2.3.4.	Notes

Scenes are not used in the version of the API 


## 2.4.	Set group attributes

|URL				|`/api/<username>/groups/<id>` 	|
|:----------|:------------------------------|
|Method			|`PUT`													|
|Version		|1.0														|
|Permission	|Whitelist											|

### 2.4.1.	Description
Allows the user to modify the name and light membership of a group.
 
### 2.4.2.	Body arguments
|Name		|**Type**						|**Description**|**Required**	|
|:------|:------------------|:--------------|:------------|
|name		|string 0..32				|The new name for the group. If the name is already taken a space and number will be appended by the bridge e.g. "Custom Group 1".| Optional |
|lights	|array of light IDs	|The IDs of the lights that should be in the group. This resource must contain an array of at least one element.<br /><br />If an invalid light ID is given, error 7 will be returned and the group not created. | Optional |

### 2.4.3.	Body example
{% highlight json %}
{"name":"Bedroom","lights":["1"]}
{% endhighlight %}

### 2.4.4.	Response
A response to a successful `PUT` request contains confirmation of the arguments passed in. **Note**: If the new value is too large to return in the response due to internal memory constraints then a value of "Updated." is returned. 


### 2.4.5.	Response example
{% highlight json %}
[
	{"success":{"/groups/1/lights":["1"]}},
	{"success":{"/groups/1/name":"Bedroom"}}
]
{% endhighlight %}

### 2.4.6.	Notes
{% highlight json %}
{
	"error": {
		"type": 302,
		"address": "/groups/<id>/lights",
		"description": " device, <id>, could not be added to group. Device's group table is full."
	}
}
{% endhighlight %}

This will be returned if the device cannot accept any new groups in its internal table. Maximum is 16. 

	
 

## 2.5.	Set group state
|URL				|`/api/<username>/groups/<id>/action` |
|:----------|:------------------------------------|
|Method			|`PUT`																|
|Version		|1.0																	|
|Permission	|Whitelist														|

### 2.5.1.	Description
Modifies the state of all lights in a group.

User created groups will have an ID of 1 or higher; however a special group with an ID of 0 also exists containing all the lamps known by the bridge. 


### 2.5.2.	Body arguments

|Name			|**Type**							|**Description**	|**Required** 	|
|:--------|:--------------------|:----------------|:--|
|on				|bool									|On/Off state of the light. On=true, Off=false |Optional	|
|bri			|uint8 								|The brightness value to set the light to.<br /><br />Brightness is a scale from 0 (the minimum the light is capable of) to 255 (the maximum). Note: a brightness of 0 is not off.<br /><br />e.g.  "brightness": 60 will set the light to a specific brightness. |Optional |
|hue			|uint16w							|The hue value to set light to.<br /><br />The hue value is a wrapping value between 0 and 65535. Both 0 and 65535 are red, 25500 is green and 46920 is blue.<br /><br />e.g. "hue": 50000 will set the light to a specific hue. |Optional |
|sat			|uint8 								|Saturation of the light. 255 is the most saturated (colored) and 0 is the least saturated (white). |Optional	 |
|xy				|list 2..2 of float 4	|The x and y coordinates of a color in [CIE color space](/coreconcepts.html#color_gets_more_complicated).<br /><br />The first entry is the x coordinate and the second entry is the y coordinate. Both x and y must be between 0 and 1.<br />If the specified coordinates are not in the [CIE color space](/coreconcepts.html#color_gets_more_complicated), the closest color to the coordinates will be chosen. |Optional |
|ct				|uint16 							|[The Mired Color temperature](http://en.wikipedia.org/wiki/Mired) of the light. 2012 connected lights are capable of 153 (6500K) to 500 (2000K). |Optional	 |
|alert		|string								|[The alert effect](/coreconcepts.html#some_extra_fun_stuff), which is a temporary change to the bulb’s state, and has one of the following values:<br/>**"none"** – The light is not performing an alert effect.<br />**"select"** – The light is performing one [breathe cycle](/6_glossaryterms.html).<br />**"lselect"** – The light is performing breathe cycles for 30 seconds or until an `"alert": "none"` command is received.	|Optional |
|effect		|string								|The dynamic effect of the light, currently "none" and "colorloop" are supported. Other values will generate an error of type 7.<br /><br />Setting the effect to colorloop will cycle through all hues using the current brightness and saturation settings. |Optional |
|transitiontime | uint16				|The duration of the transition from the light's current state to the new state. This is given as a multiple of 100ms and defaults to 4 (400ms). For example, setting transistiontime:10 will make the transition last 1 second. |	Optional |

### 2.5.3.	Body example
{% highlight json %}
{
	"on": true,
	"hue": 2000,
	"effect": "colorloop"
}
{% endhighlight %}

### 2.5.4.	Response
A response to a successful `PUT` request contains confirmation of the arguments passed in. Note: If the new value is too large to return in the response due to internal memory constraints then a value of "Updated." is returned.
 
### 2.5.5.	Response example
The response details the success of sending each state parameter to the group. Note that the success is not reported for each light bulb and a "success" for the group does not guarantee that the lights actually changed as they may be unreachable or not capable of the requested change.

{% highlight json %}
[
	{"success":{"/groups/1/action/on": true}},
	{"success":{"/groups/1/action/effect":"colorloop"}},
	{"success":{"/groups/1/action/hue":6000}}
]
{% endhighlight %}

### 2.5.6.	Notes
A light cannot have its hue, saturation, brightness, effect, ct or xy modified when it is turned off. Doing so will return error 201.

There are 3 methods available to set the color of the light – hue and saturation (hs), xy or color temperature (ct). If multiple methods are used then a priority is used: xy > ct > hs. All included parameters will be updated but the 'colormode' will be set using the priority system.


## 2.6.	Delete group
This method is not supported in the 1.0 version of the API.  It is scheduled for release in the next version of the API.