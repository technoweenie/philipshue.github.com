---
layout: page
title: 4. Configuration API
description: ""
group: navigation_api
weight: 4
published: true
---

{% include JB/setup %}

## 4.1.	Create user

|URL				|`/api`	|
|:----------|:------|
|Method			|`POST`	|
|Version		|1.0		|
|Permission	|All		|

### 4.1.1.	Description
Creates a new user. The link button on the bridge must be pressed and this command executed within 30 seconds.

Once a new user has been created, the user key is added to a 'whitelist', allowing access to API commands that require a whitelisted user. At present, all other API commands require a whitelisted user.

We ask that published apps use the name of their app as the devicetype. 

### 4.1.2.	Body arguments

|Name				|**Type**				|**Description**	|**Required ** |
|:----------|:--------------|:----------------|:-------------|
|devicetype	|string 0..40		|Description of the type of device associated with this username.<br /><br />This field must contain the name of your app.	|Required |
|username		|string 10..40	|A username. If this is not provided, a random key will be generated and returned in the response. It is recommended that a unique identifier for the device be used as the username|	Optional |

### 4.1.3.	Body example
{% highlight json %}
{"devicetype": "iPhone", "username": "1234567890"}
{% endhighlight %}
	
### 4.1.4.	Response
Contains a list with a single item that details whether the user was added successfully along with the username parameter. 

Note: If the requested username already exists then the response will report a success.

### 4.1.5.	Response example
{% highlight json %}
[{"success":{"username": "1234567890"}}]
{% endhighlight %}

### 4.1.6.	Notes
The link button on the bridge must have been recently pressed for the command to execute successfully. If the link button has not been pressed a 101 error will be returned.
 

## 4.2.	Get configuration

|URL				|`/api/<username>/config`	|
|:----------|:------------------------|
|Method			|`GET`										|
|Version		|1.0											|
|Permission	|Whitelist								|

### 4.2.1.	Description
Returns list of all configuration elements in the bridge. Note all times are stored in UTC.

### 4.2.2.	Response

|Name						|**Type**			|**Description**	|
|:--------------|:------------|:----------------|
|proxyport			|uint16				|Port of the proxy being used by the bridge. If set to 0 then a proxy is not being used. |
|utc						|string				|Current time stored on the bridge.	|
|name						|string 4..16	|Name of the bridge. This is also its uPnP name, so will reflect the actual uPnP name after any conflicts have been resolved. |
|swupdate				|object				|Contains information related to software updates. | 
|whitelist			|object				|An array of whitelisted user IDs.	|
|swversion			|string				|Software version of the bridge. |
|proxyaddress		|string 0..40	|IP Address of the proxy server being used. A value of "none" indicates no proxy. |
|mac						|string				|MAC address of the bridge. |
|linkbutton			|bool					|Indicates whether the link button has been pressed within the last 30 seconds. |
|ipaddress			|string				|IP address of the bridge. |
|netmask				|string				|Network mask of the bridge. | 
|gateway				|string				|Gateway IP address of the bridge. |
|dhcp						|bool					|Whether the IP address of the bridge is obtained with DHCP. |
|portalservices	|bool				|This indicates whether the bridge is registered to synchronize data with a portal account. |

### 4.2.3.	Response example
{% highlight json %}
{
	"proxyport": 0,
	"utc": "2012-10-29T12:00:00",
	"name": "Smartbridge 1",
	"swupdate": {
		"updatestate":1,
 		"url": "www.meethue.com/patchnotes/1453",
 		"text": "This is a software update",
 		"notify": false
 	},
	"whitelist": {
		"1234567890": {
			"last use date": "2010-10-17T01:23:20",
			"create date": "2010-10-17T01:23:20",
			"name": "iPhone Web 1"
		}
	},
	"swversion": "01003542",
	"proxyaddress": "none",
	"mac": "00:17:88:00:00:00",		
	"linkbutton": false,
	"ipaddress": "192.168.1.100",
	"netmask": "255.255.0.0",
	"gateway": "192.168.0.1",
	"dhcp": false
}
{% endhighlight %}

## 4.3.	Modify configuration

|URL				|`/api/<username>/config`	|
|:----------|:------------------------|
|Method			|`PUT`										|
|Version		|1.0											|
|Permission	|Whitelist								|

### 4.3.1.	Description
Allows the user to set some configuration values.

### 4.3.2.	Body arguments

|Name					|**Type**				|**Description**		|**Required**	|
|:------------|:--------------|:------------------|:------------|
|proxyport		|uint16					|Port of the proxy being used by the bridge. If set to 0 then a proxy is not being used.	|Optional	|
|name					|string 4..16		|Name of the bridge. This is also its uPnP name, so will reflect the actual uPnP name after any conflicts have been resolved.	|	Optional	|
|swupdate			|object					|Contains information related to software updates. |	Optional	|	
|proxyaddress	|string 0..40		|IP Address of the proxy server being used. A value of "none" indicates no proxy. |	Optional	|	
|linkbutton		|bool						|Indicates whether the link button has been pressed within the last 30 seconds.	|Optional	|
|ipaddress		|string					|IP address of the bridge.	|	Optional	|
|netmask			|string					|Network mask of the bridge.	| 	Optional	|
|gateway			|string					|Gateway IP address of the bridge.	|	Optional	|
|dhcp					|bool						|Whether the IP address of the bridge is obtained with DHCP.	|	Optional	|
|portalservices	|	bool				|This indicates whether the bridge is registered to synchronize data with a portal account.	| Optional |

### 4.3.3.	Body example
{% highlight json %}
{"proxyport":100}
{% endhighlight %}
	
### 4.3.4.	Response
A response to a successful `PUT` request contains confirmation of the arguments passed in. Note: If the new value is too large to return in the response due to internal memory constraints then a value of "Updated." is returned.
 
### 4.3.5.	Response example
{% highlight json %}
[
	{"success": {"/config/proxyport":100}}
]
{% endhighlight %}
 

## 4.4.	Delete user from whitelist

|URL				|`/api/<username>/config/whitelist/<username2>`	|
|:----------|:----------------------------------------------|
|Method			|`DELETE`																				|
|Version		|1.0																						|
|Permission	|Whitelist																			|

### 4.4.1.	Description
Deletes the specified user, `<username2>`, from the whitelist.

### 4.4.2.	Response
The response details whether the user was successfully removed from the whitelist.

### 4.4.3.	Response example
{% highlight json %}
[{
	"success": "/config/whitelist/1234567890 deleted."
}]
{% endhighlight %}


## 4.5.	Get full state (datastore)

|URL				|`/api/<username>`	|
|:----------|:------------------|
|Method			|`GET`							|
|Version		|1.0								|
|Permission	|Whitelist					|

### 4.5.1.	Description
This command is used to fetch the entire datastore from the device, including settings and state information for lights, groups, schedules and configuration. It should only be used sparingly as it is resource intensive for the bridge, but is supplied e.g. for synchronization purposes.

### 4.5.2.	Response

|Name				|**Type**		|**Description**	|
|:----------|:----------|:----------------|
|lights			|object			|A collection of all lights and their attributes.	|
|groups			|object			|A collection of all groups and their attributes.	|
|config			|object			|All configuration settings.	|
|schedules	|object			|A collection of all schedules and their attributes. |

### 4.5.3.	Response example
{% highlight json %}
{
	"lights": {
		"1": {
			"state": {
				"on": false,
				"bri": 0,
				"hue": 0,
				"sat": 0,
				"xy": [0.0000, 0.0000],
				"ct": 0,
				"alert": "none",
				"effect": "none",
				"colormode": "hs",
				"reachable": true
			},
			"type": "Extended color light",
			"name": "Hue Lamp 1",
			"modelid": "LCT001",
			"swversion": "65003148",
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
		},
		"2": {
			"state": {
				"on": true,
				"bri": 254,
				"hue": 33536,
				"sat": 144,
				"xy": [0.3460, 0.3568],
				"ct": 201,
				"alert": "none",
				"effect": "none",
				"colormode": "hs",
				"reachable": true
			},
			"type": "Extended color light",
			"name": "Hue Lamp 2",
			"modelid": "LCT001",
			"swversion": "65003148",
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
	},
	"groups": {
		"1": {
			"action": {
				"on": true,
				"bri": 254,
				"hue": 33536,
				"sat": 144,
				"xy": [0.3460, 0.3568],
				"ct": 201,
				"effect": "none",
				"colormode": "xy"
			},
			"lights": ["1", "2"],
			"name": "Group 1"
		}
	},
	"config": {
		"name": "Philips hue",
		"mac": "00:00:88:00:bb:ee",
		"dhcp": true,
		"ipaddress": "192.168.1.74",
		"netmask": "255.255.255.0",
		"gateway": "192.168.1.254",
		"proxyaddress": "",
		"proxyport": 0,
		"UTC": "2012-10-29T12:00:00",
		"whitelist": {
			"newdeveloper": {
				"last use date": "2012-10-29T12:00:00",
				"create date": "2012-10-29T12:00:00",
				"name": "test user"
			}
		},
		"swversion": "01003372",
		"swupdate": {
			"updatestate": 0,
			"url": "",
			"text": "",
			"notify": false
		},
		"linkbutton": false,
		"portalservices": false
	},
	"schedules": {
		"1": {
			"name": "schedule",
			"description": "",
			"command": {
				"address": "/api/<username>/groups/0/action",
				"body": {
					"on": true
				},
				"method": "PUT"
			},
			"time": "2012-10-29T12:00:00"
		}
	}
}
{% endhighlight %}