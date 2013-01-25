---
layout: page
title: "Error messages"
description: ""
group: "navigation_api"
---
{% include JB/setup %}


Where error messages are used in the API documentation, it is recommended that a link to the error messages page with a suitable anchor tag be provided.

If an API call fails to execute an error message is returned. This will take the following form:
{% highlight json %}
	{
		"error": {
			"type": <ID> ,
			"address": </resource/parameteraddress>,
			"description": <description>
		}
	}
{% endhighlight %}

One error message per failed action will be returned using a priority system. The priority system uses the error number, with a lower number meaning a higher priority. In the case of a `PUT` command, an error may be given for each parameter on which a change was attempted.

The following tables list all error codes that are used by the bridge.

Key:

|`<resource>`		|Resource being acted on e.g. /lights/1 |
|:--------------|:--------------------------------------|
|`<method_name>`|HTTP Method e.g. `PUT/POST/DELETE/GET`		|
|`<parameter>`	|URI of the parameter being modified e.g. `/lights/1/name`	|
|`<value>`			|Value the parameter is being set to e.g. 128 |


### Generic Errors

|ID		|**Description**		|**Details **|
|:----|:------------------|:-----------|
|1		|unauthorized user	|This will be returned if an invalid username is used in the request, or if the username does not have the rights to modify the resource. |
|2		|body contains invalid JSON	| This will be returned if the body of the message contains invalid JSON. |
|3		|resource, `<resource>`, not available	| This will be returned if the addressed resource does not exist. E.g. the user specifies a light ID that does not exist. |
|4		|method, `<method_name>`, not available for resource, `<resource>`	| This will be returned if the method (`GET/POST/PUT/DELETE`) used is not supported by the URL e.g. `DELETE` is not supported on the `/config` resource |
|5		|missing parameters in body	| Will be returned if required parameters are not present in the message body. The presence of invalid parameters should not trigger this error as long as all required parameters are present. |
|6		|parameter, `<parameter>`, not available	| This will be returned if a parameter sent in the message body does not exist. This error is specific to `PUT` commands; invalid parameters in other commands are simply ignored. |
|7		|invalid value, `<value>`, for parameter, `<parameter>` |	This will be returned if the value set for a parameter is of the incorrect format or is out of range. |
|8		|parameter, `<parameter>`, is not modifiable | This will be returned if an attempt to modify a read only parameter is made. |
|901	|Internal error, `<error code>` |	This will be returned if there is an internal error in the processing of the command. This indicates an error in the bridge, not in the message being sent. 


### Command Specific Errors

|ID		|**Description**		|**Details** |
|:----|:------------------|:-----------|
|101	|link button not pressed | The link button has not been pressed in the last 30 seconds. |
|201	|parameter, `<parameter>`, is not modifiable. Device is set to off. |	This will be returned if a user attempts to modify a parameter that cannot be modified due to the current state of the device. This will most commonly be returned if the hue/sat/bri/effect/xy/ct parameters are modified while the on parameter is false. | 
|301	|group could not be created. Group table is full. |	The bridge can store a maximum of 16 groups. This error will be returned when trying to add a new group if the limit has been reached. | 
|302	|device, `<id>`, could not be added to group. Device's group table is full. | The lamp can store a maximum of 16 groups. This error will be returned when trying to add a new group if the limit has been reached. |


 
