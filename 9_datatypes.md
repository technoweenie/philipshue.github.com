---
layout: page
title: "Datatypes"
description: ""
group: navigation_api
---
{% include JB/setup %}

All data types have a short hand used throughout the API. All uses of the shorthand in the main API documentation should link through to the data types page, with an appropriate anchor, and/or display the data type description on hover.

Below is a list of data types used by the API and their short hand used in the API documentation.

|Short Hand		|**Description**	|
|:------------|:----------------|
|string n..m	|A string in UTF8 encoding where n..m specifies the minimum number of characters, n, and maximum number, m.<br /><br />If n and m are not specified then only certain values are accepted as specified in the description. |
|uint8				|8 bit, unsigned, non-wrapping integer. i.e. an integer in the range of 0 to 255 where integer values outside this range are invalid. |
|uint16				|16 bit, unsigned, non-wrapping integer. i.e. an integer in the range of 0 to 65535 where integer values outside this range are invalid. |
|uint16w			|16 bit, unsigned, wrapping integer. i.e. an integer in the range of 0 to 65535 where integer values outside this range are truncated to 16 bit e.g. 65536 wraps to 0. |
|list n..m of x | A list of items of type x. Where x is another entry in this table. A list is formatted as comma separated values totally enclosed in square brackets e.g. `[1,2,3]`. <br /> n..m specifies the minimum number of entries in list, n, and maximum number, m. |
|bool					|A Boolean value which can take the values *true* or *false* only. |
|object				|An object value is a JSON compliant object. This is of the format of zero or more key value pairs encapsulated in curly braces {}. |
|float n			|A float object is a numeric value expressed in decimal format with n decimal places kept. Further provided decimal data is truncated. E.g 0.9988 |
