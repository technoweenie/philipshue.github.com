---
layout: page
title: "Message structure and response"
description: ""
group: navigation_api
---
{% include JB/setup %}

The CLIP API consists of a set of commands that can be called over a REST web service. The API commands fall into one of 4 categories, depending on the HTTP method used:

### Method: GET
Used for: Reading specific data from the bridge.<br />
**Returns**: JSON containing the requested resource.

### Method: PUT
Used for: Modifying existing data on the bridge.<br />
**Returns**: A list containing one item per parameter modified.<br />
**Example**: `[{"success":{"/lights/1/state/hue":254}}]`

### Method: POST
Used for: Adding new data to the bridge.<br />
**Returns**: A list containing one item per resource created.<br />
**Example**: `[{"success":{"id":"/schedules/7"}}]`

### Method: DELETE
Used for: Deleting data from the bridge<br />
**Returns**: A list containing one item per resource deleted.<br />
**Example**: `[{"success":"/groups/1 deleted"}]`

Commands using `PUT` and `POST` methods will normally require a message body to be attached to the request. The message body must be formatted using JavaScript Object Notation (JSON). More details and examples for formatting the message body can be found in the documentation for each command.
 
