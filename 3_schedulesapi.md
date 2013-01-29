---
layout: page
title: "3. Schedules API"
description: ""
group: navigation_api
---
{% include JB/setup %}

## 3.1.	Get all schedules

|URL				|`/api/<username>/schedules` 	|
|:----------|:----------------------------|
|Method			|`GET`												|
|Version		|1.0													|
|Permission	|Whitelist										|

###3.1.1.	Description
Gets a list of all schedules that have been added to the bridge.

### 3.1.2.	Response
Returns a list of all schedules in the system. Each group has a name and unique identification number.

If there are no schedules then the bridge will return an empty object, `{}`.

###3.1.3.	Response example
{% highlight json %}
{
  "1":{
  	"name":"Wake up"
  },
  "2":{
  	"name":"Sleep down"
  }
}	
{% endhighlight %}

## 3.2.	Create schedule

|URL				|`/api/<username>/schedules` 	|
|:----------|:----------------------------|
|Method			|`POST`												|
|Version		|1.0													|
|Permission	|Whitelist										|

### 3.2.1.	Description
Allows the user to create new schedules. The bridge can store up to 100 schedules.

### 3.2.2.	Body arguments

|Name					|**Type**  |**Description**|**Required** |
|:------------|:---------|:--------------|:------------|
|name					|string 0..32	|	Name for the new schedule. If a name is not specified then the default name, "schedule", is used.<br /><br />If the name is already taken a space and number will be appended by the bridge, e.g. "schedule 1". |	Optional |
|description	|string 0..64	|Description of the new schedule. If the description is not specified it will be empty.	|Optional |
|command			|object 0..90	|Command to execute when the scheduled event occurs. If the command is not valid then an error of type 7 will be raised.<br />**Tip**: Stripping unnecessary whitespace can help to keep commands within the 90 character limit.|	Required |
|time					|string				|Time when the scheduled event will occur in [ISO 8601:2004](/6_glossaryterms.html) format.<br /><br />The bridge measures time in UTC and only accepts extended format, non-recurring, local time (YYYY-MM-DDThh:mm:ss). <br /><br />Incorrectly formatted dates will raise an error of type 7. If the time is in the past an error 7 will also be raised.	| Required |

### 3.2.3.	Body example
{% highlight json %}
{
	"name": "Wake up",
	"description": "My wake up alarm",
	"command": {
		"address": "/api/0/groups/1/action",
		"method": "PUT",
		"body": {
			"on": true
		}
	},
	"time": "2011-03-30T14:24:40"
}
{% endhighlight %}

### 3.2.4.	Response
Contains a list with a single item that details whether the schedule was added successfully.
 
###3.2.5.	Response example
{% highlight json %}
[{
	"success":{"id": "2"}
}]
{% endhighlight %}
 
## 3.3.	Get schedule attributes

|URL				|`/api/<username>/schedules/<id>` |
|:----------|:--------------------------------|
|Method			|`GET`														|
|Version		|1.0															|
|Permission	|Whitelist												|

### 3.3.1.	Description
Gets all attributes for a schedule.

### 3.3.2.	Response

|Name					|**Type**				|**Description** |
|:------------|:--------------|:---------------|
|name					|string 0..32		|The name of the schedule. | 
|description	|string 0..64		|Description of the schedule. |
|command			|object 0..90		|Command to execute when the scheduled event occurs.	| 
|time					|string					|Time when the scheduled event will occur in [ISO 8601:2004](/6_glossaryterms.html) format. <br /><br />The bridge measures time in UTC and encodes time as a string in extended format, non-recurring, local time (YYYY-MM-DDThh:mm:ss). |

### 3.3.3.	Response example
{% highlight json %}
{
	"name": "Wake up",
	"description": "My wake up alarm",
	"command": {
		"address": "/api/0/groups/1/action",
		"method": "PUT",
		"body": {
			"on": true
		}
	},
	"time": "2011-03-30T14:24:40"
}
{% endhighlight %}
 
## 3.4.	Set schedule attributes

|URL				|`/api/<username>/schedules/<id>`	|
|:----------|:--------------------------------|
|Method			|`PUT`														|
|Version		|1.0															|
|Permission	|Whitelist												|

### 3.4.1.	Description
Allows the user to change attributes of a schedule.

### 3.4.2.	Body arguments

|Name					|**Type**			|**Description**		|**Required** 	|
|:------------|:------------|:------------------|:--------------|
|name					|string 0..32	|Name for the schedule.<br /><br />If the name is already taken a space and number will be appended by the bridge, e.g. "schedule 1".	|Optional |
|description	|string 0..64	|Description of the schedule. |	Optional |
|command			|object 0..90	|Command to execute when the scheduled event occurs. If the command is not valid then an error of type 7 will be raised.<br /><br />**Tip**: Stripping unnecessary whitespace can help to keep commands within the 90 character limit.|	Optional	|
|time					|string				|Time when the scheduled event will occur in [ISO 8601:2004](/6_glossaryterms.html) format.<br /><br />The bridge measures time in UTC and only accepts extended format, non-recurring, local time (YYYY-MM-DDThh:mm:ss). <br /><br />Incorrectly formatted dates will raise an error of type 7. If the time is in the past an error 7 will also be raised. |	Optional	|

### 3.4.3.	Body example
{% highlight json %}
{
	"name": "Wake up"
}
{% endhighlight %}

### 3.4.4.	Response
A response to a successful `PUT` request contains confirmation of the arguments passed in. Note: If the new value is too large to return in the response due to internal memory constraints then a value of "Updated." is returned.
 
### 3.4.5.	Response example
{% highlight json %}
[
	{ "success": {"/schedules/1/name": "Wake up"}}
]
{% endhighlight %}

## 3.5.	Delete schedule

|URL				|`/api/<username>/schedules/<id>`	|
|:----------|:--------------------------------|
|Method			|`DELETE`													|
|Version		|1.0															|
|Permission	|Whitelist												|

### 3.5.1.	Description
Deletes a schedule from the bridge.

### 3.5.2.	Response
The response details whether the schedule was successfully deleted.

### 3.5.3.	Response example
{% highlight json %}
[
	{"success": "/schedules/1 deleted."}
]
{% endhighlight %}

 



 
