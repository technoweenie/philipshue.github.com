---
layout: page
title: 1. Lights API
description: ""
group: navigation_api
weight: 1
---
{% include JB/setup %}

## 1.1.	Get all lights

|URL				|`/api/<username>/lights`	|
|:----------|:------------------------|
|Method			|`GET`										|
|Version		|1.0											|
|Permission	|Whitelist								|

### 1.1.1.	Description
Gets a list of all lights that have been discovered by the bridge.
 
### 1.1.2.	Response
Returns a list of all lights in the system, each light has a name and unique identification number.

If there are no lights in the system then the bridge will return an empty object, {}.

### 1.1.3.	Response example
{% highlight json %}
{
	"1": {
		"name": "Bedroom"
	},
	"2": {
		"name": "Kitchen"
	}
}
{% endhighlight %}

## 1.2.	Get new lights

|URL				|`/api/<username>/lights/new`	|
|:----------|:----------------------------|
|Method			|`GET`												|
|Version		|1.0													|
|Permission	|Whitelist										|

### 1.2.1.	Description
Gets a list of lights that were discovered the last time a [search for new lights](#13_search_for_new_lights) was performed. The list of new lights is always deleted when a new search is started.

### 1.2.2.	Response

|Name			|**Type**		|**Description** |
|:--------|:------|:------------------|
|Lastscan	|string	|Returns "active" if a scan is currently on-going, "none" if a scan has not been performed since the bridge was powered on, or else the date and time that the last scan was completed in ISO 8601:2004 format (YYYY-MM-DDThh:mm:ss). |
 
### 1.2.3.	Response example
{% highlight json %}
{
	"7": {"name": "Hue Lamp 7"},
	"8": {"name": "Hue Lamp 8"},
	"lastscan": "2012-10-29T12:00:00"
}
{% endhighlight %}

## 1.3.	Search for new lights

|URL				|	`/api/<username>/lights` 	|
|:----------|:--------------------------|
|Method			|`POST`										 	|
|Version		|1.0												|
|Permission	|Whitelist									|

### 1.3.1.	Description
Starts a search for new lights.

The bridge will search for 1 minute and will add a maximum of 15 new lights. To add further lights, the command needs to be sent again after the search has completed. If a search is already active, it will be aborted and a new search will start.

When the search has finished, new lights will be available using the [get new lights](#12_get_new_lights) command. In addition, the new lights will now be available by calling [get all lights](#11_get_all_lights) or by calling [get group attributes](/2_groupsapi.html#23_get_group_attributes) on group 0. Group 0 is a special group that cannot be deleted and will always contain all lights known by the bridge.

### 1.3.2.	Response
Contains a list with a single item that details whether the search started successfully.
 
### 1.3.3.	Response example
{% highlight json %}
[ { "success": { "/lights": "Searching for new devices" } } ]
{% endhighlight %}

## 1.4.	Get light attributes and state

|URL				|	`/api/<username>/lights/<id>`	|
|:----------|:------------------------------|
|Method			|`GET`													|
|Version		|1.0														|
|Permission	|Whitelist											|

### 1.4.1.	Description
Gets the attributes and state of a given light.

### 1.4.2.	Response

|Name				|**Type**			|**Description**																														|
|:----------|:------------|:--------------------------------------------------------------------------|
|State			|state object	|Details the state of the light, see the state table below for more details.|
|Type				|string				|A fixed name describing the type of light e.g. "Extended color light".			|
|name				|string 0, 32	|A unique, editable name given to the light.				                        |
|modelid		|string 6, 6	|The hardware model of the light.																						|
|swversion	|string 8, 8	|An identifier for the software version running on the light.								|
|Pointsymbol|object				|This parameter is reserved for future functionality.												|

The state object contains the following fields

|Name				|**Type**							|**Description** |
|:----------|:--------------------|:---------------|
|On					|bool									|On/Off state of the light. On=true, Off=false |
|Bri				|uint8								|Brightness of the light. This is a scale from the minimum brightness the light is capable of, 0, to the maximum capable brightness, 255. Note a brightness of 0 is not off. |
|Hue				|uint16w							|Hue of the light. This is a wrapping value between 0 and 65535. Both 0 and 65535 are red, 25500 is green and 46920 is blue. |
|sat				|uint8 								|Saturation of the light. 255 is the most saturated (colored) and 0 is the least saturated (white). |
|xy					|list 2..2 of float 4	|The x and y coordinates of a color in [CIE color space](/coreconcepts.html#color_gets_more_complicated).<br /><br />The first entry is the x coordinate and the second entry is the y coordinate. Both x and y are between 0 and 1. |
|ct					|uint16								|[The Mired Color temperature](http://en.wikipedia.org/wiki/Mired) of the light. 2012 connected lights are capable of 153 (6500K) to 500 (2000K).
|alert			|string								|[The alert effect](/coreconcepts.html#some_extra_fun_stuff), which is a temporary change to the bulb’s state. This can take one of the following values:<br />**"none"** – The light is not performing an alert effect.<br />**"select"** – The light is performing one [breathe cycle](/6_glossaryterms.html).<br />**"lselect"** – The light is performing breathe cycles for 30 seconds or until an `"alert": "none"` command is received. <br /><br /> Note that in version 1.0 this contains the last alert sent to the light and not its current state. This will be changed to contain the current state in an upcoming patch.|
|effect			|string								|The dynamic effect of the light, can either be "none" or "colorloop". <br /><br />If set to colorloop, the light will cycle through all hues using the current brightness and saturation settings. |
|colormode	|string 2, 2					|Indicates the color mode in which the light is working, this is the last command type it received. Values are "hs" for Hue and Saturation, "xy" for XY and "ct" for Color Temperature. This parameter is only present when the light supports at least one of the values.|
|reachable	|bool									|Indicates if a light can be reached by the bridge. Currently always returns true, functionality will be added in a future patch.|

### 1.4.3.	Response example
{% highlight json %}
{
	"state": {
		"hue": 50000,
		"on": true,
		"effect": "none",
		"alert": "none",
		"bri": 200,
		"sat": 200,
		"ct": 500,
		"xy": [0.5, 0.5],
		"reachable": true,
		"colormode": "hs"
	},
	"type": "Living Colors",
	"name": "LC 1",
	"modelid": "LC0015",
	"swversion": "1.0.3", 	
	"pointsymbol": {
		"1": "none",
		"2": "none",
		"3": "none",
		"4": "none",
		"5": "none",
		"6": "none",
		"7": "none",
		"8": "none"
	}
}
{% endhighlight %}

### 1.4.4.	Notes
Note the usage of the colormode parameter: There are 3 ways of setting the light color: xy, color temperature (ct) or hue and saturation (hs). A light may contain different settings for xy, ct and hs, but only the mode indicated by the colormode parameter will be certain to give the active light color.


## 1.5.	Set light attributes (rename)
|URL				|`/api/<username>/lights/<id>`|
|:----------|:----------------------------|
|Method			|`PUT`												|
|Version		|1.0													|
|Permission	|Whitelist										|

### 1.5.1.	Description
Used to rename lights. A light can have its name changed when in any state, including when it is unreachable or off.

###1.5.2.	Body arguments
|Name	|**Type**					|**Description**	|**Required** |
|:----|:----------------|:----------------|:------------|
|Name	|string 0, 32			|The new name for the light. If the name is already taken a space and number will be appended by the bridge e.g. "Bedroom Light 1".	| Required |

### 1.5.3.	Body 
{% highlight json %}
{"name":"Bedroom Light"}
{% endhighlight %}

### 1.5.4.	Response
A response to a successful `PUT` request contains confirmation of the arguments passed in. Note: If the new value is too large to return in the response due to internal memory constraints then a value of "Updated." is returned. 

### 1.5.5.	Response example
{% highlight json %}
[{"success":{"/lights/1/name":"Bedroom Light"}}]
{% endhighlight %}
	
## 1.6.	Set light state
|URL				|`/api/<username>/lights/<id>/state`|
|:----------|:----------------------------------|
|Method			|`PUT`															|
|Version		|1.0																|
|Permission	|Whitelist													|

### 1.6.1.	Description
Allows the user to turn the light on and off, modify the hue and effects.
	
### 1.6.2.	Body arguments
|Name				|**Type**							|**Description**	| 	|
|:----------|:--------------------|:----------------|:--|
|on					|bool									|On/Off state of the light. On=true, Off=false|	Optional |
|bri				|uint8								|The brightness value to set the light to.<br /><br />Brightness is a scale from 0 (the minimum the light is capable of) to 255 (the maximum). **Note**: a brightness of 0 is not off.<br /><br />e.g. "brightness": 60 will set the light to a specific brightness | Optional |
|hue				|uint16w 							|The hue value to set light to.<br /><br />The hue value is a wrapping value between 0 and 65535. Both 0 and 65535 are red, 25500 is green and 46920 is blue.<br /><br />e.g. "hue": 50000 will set the light to a specific hue. |Optional |
|sat				|uint8								|Saturation of the light. 255 is the most saturated (colored) and 0 is the least saturated (white).|	Optional |
|xy					|list 2..2 of float 4 |The x and y coordinates of a color in [CIE color space](/coreconcepts.html#color_gets_more_complicated).<br /><br />The first entry is the x coordinate and the second entry is the y coordinate. Both x and y must be between 0 and 1.<br />If the specified coordinates are not in the [CIE color space](/coreconcepts.html#color_gets_more_complicated), the closest color to the coordinates will be chosen.|	Optional |
|ct					|uint16 							|[The Mired Color temperature](http://en.wikipedia.org/wiki/Mired) of the light. 2012 connected lights are capable of 153 (6500K) to 500 (2000K).|	Optional |
|alert			|string								|[The alert effect](/coreconcepts.html#some_extra_fun_stuff), is a temporary change to the bulb’s state, and has one of the following values:<br />**"none"** – The light is not performing an alert effect.<br />**"select"** – The light is performing one [breathe cycle](/6_glossaryterms.html).<br />**"lselect"** – The light is performing breathe cycles for 30 seconds or until an `"alert": "none"` command is received.|	Optional |
|effect			|string								|The dynamic effect of the light. Currently "none" and "colorloop" are supported. Other values will generate an error of type 7.<br /><br />Setting the effect to colorloop will cycle through all hues using the current brightness and saturation settings.| Optional	|
|transitiontime	|uint16						|The duration of the transition from the light’s current state to the new state. This is given as a multiple of 100ms and defaults to 4 (400ms). For example, setting `transistiontime:10` will make the transition last 1 second.|	Optional |

### 1.6.3.	Body example
{% highlight json %}
{
	"hue": 50000,
	"on": true,
	"bri": 200
}
{% endhighlight %}
	
### 1.6.4.	Response
A response to a successful PUT request contains confirmation of the arguments passed in. Note: If the new value is too large to return in the response due to internal memory constraints then a value of "Updated." is returned.
 
### 1.6.5.	Response example
{% highlight json %}
[
	{"success":{"/lights/1/state/bri":200}},
	{"success":{"/lights/1/state/on":true}},
	{"success":{"/lights/1/state/hue":50000}}
]
{% endhighlight %}

### 1.6.6.	Notes
A light cannot have its hue, saturation, brightness, effect, ct or xy modified when it is turned off. Doing so will return error 201.

There are 3 methods available to set the color of the light – hue and saturation (hs), xy or color temperature (ct). If multiple methods are used then a priority is used: xy > ct > hs. All included parameters will be updated but the ‘colormode’ will be set using the priority system.


 

