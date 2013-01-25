---
layout: page
title: "How hue works"
description: ""
group: "navigation"
weight: 3
---
{% include JB/setup %}

## Overview

hue is a system of 4 components:

1. 	**Apps** – These are ways to control the lights to make them do smart things.
		You probably have one installed on your smartphone already that you
		control your lights with. Apps you develop could do anything from simply
		changing colors or switching on and off in response to a user input,
		or they might be linked to some sort of sensor or other input, such as
		switching on when a sound is picked up or a tweet is received. They are
		not limited to smartphone apps. It's basically everything that makes use
		of the hue system APIs. It could equally be a website or an [Arduino] (http://www.arduino.cc/) 
		board.
2. **Bridge** – This is used to enable your smart bulbs to communicate with
		each other and the Portal via the internet. The main set of APIs are those
		offered by the bridge. These allow you to control all settings of the lights
		in your system. These APIs require direct access to your bridge so you'll
		only be able to access them when your app and bridge are on the same
		local network.
3. **Portal** – This is a web based control panel which connects your home to
		the internet. It delivers control commands from outside and keeps your
		software in the bridge up-to-date. The portal presents a utility API to help
		you [discover the address of your bridge](/5_portalapi.html#51_discover_local_bridges).
4. **Lights** – This is the output of the system. These smart bulbs contain 3 types of LED specially chosen to produce a range of colors and intensities. Lights
		create a mesh network with each other which enables each light to pass
		on messages to the next extending the range and making everything
		more robust. They are connected to the bridge via an open standards
		protocol called ZigBee Light Link. If you'd like to start a project which
		talks directly to the bulbs that's possible but you'll need to enroll in the
		ZigBee developers program – for more info see [www.zigbee.org] (http://www.zigbee.org).

The hue bridge APIs are your primary tool for controlling your lights. This is a
RESTful interface over HTTP. The purpose of this web service interface is to
give every light in your system a URL in your local network. We also give every
controllable parameter of these lights a local URL. This means that controlling
the lights is achieved by simply sending a new value to this local URL.

Discovering what lights you have in your system and what they are doing is just
as easy as we have a URL for this too. That's the basic idea of a RESTful interface,
for more general information see [Wikipedia](http://en.wikipedia.org/wiki/Representational_state_transfer#RESTful_web_services).

All responses and new values are sent and returned in JSON (JavaScript Object
Notation) with UTF8 encoding so it's easy to generate or parse.