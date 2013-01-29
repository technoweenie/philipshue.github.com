---
layout: page
title: 5. Portal API
description: ""
group: navigation_api
weight: 5
---
{% include JB/setup %}

This part of the hue developer program is still under development.  

Currently available methods:

## 5.1	Discover local bridges

|URL				|[www.meethue.com/api/nupnp](http://www.meethue.com/api/nupnp)|
|:----------|:------------------------------------------------------------|
|Method			|`GET`			|
|Version		|1.0				|
|Permission	|Open				|

### 5.1.1	Description
This will enable you to discover the local IP your bridge has been assigned on your network.

### 5.1.2	Response
Returns a list of all bridges on the local network and their internal IP addresses.

If there are no bridges on your external IP then the system will return an empty list, `[]`. No bridges will return an empty list `[]` instead of empty object `{}`.

### 5.1.3	Response example

{% highlight json %}
[
	{
		"id":"001788fffe0923cb",
		"internalipaddress":"192.168.1.37",
		"macaddress":"00:17:88:09:23:cb"
	}
]
{% endhighlight %}
