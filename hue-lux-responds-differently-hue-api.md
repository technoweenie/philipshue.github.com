---
layout: page
title: "hue lux responds differently in the hue API"
description: ""
group: "navigation"
weight: 6
---
{% include JB/setup %}

<p class="p1">With hue Lux, Philips introduce lamps and hue systems which can only be controlled by brightness and not by color or color temperature</p>
<p class="p1">Consequences for apps:</p>
<p class="p2"><span class="s1">&nbsp;</span>A bridge may contain no lights supporting color or color temperature. Applications focusing on the use of color-</p>
<ul>
<li class="p3">might not make sense to the user, ie recommend to add colored lights to improve experience</li>
<li class="p3"><span style="line-height: 1.42857143;">may not function correctly</span></li>
</ul>
<p class="p2">A bridge may contain lights supporting only brightness and on/off or only on/off<br>
(type:�Dimmable light�)</p>
<ul>
<li class="p3">The light resource provided by the bridge (hue API or SDK) will not contain color attributes</li>
<li class="p3">Sending color or color temperature to such a light will result in a failure</li>
<li class="p3">Commands sending color in combination with on/off or brightness will result both
<ul>
<li class="p3">in change in on/off resp brightness</li>
<li class="p3">in returning an error for color resp color temperature</li>
</ul>
</li>
</ul> 
<p class="p3">&nbsp;</p>
<p class="p3">Light state returned by lux lights in the API as JSON is different, the color parameters do not apply to lux:</p>
<p class="p1">Hue:</p>
<pre><span class="s2">"1"</span><span class="p">:</span>  <span class="p">{</span>
    <span class="nt">"state"</span><span class="p">:</span> <span class="p">{</span>
        <span class="nt">"on"</span><span class="p">:</span> <span class="kc">true</span><span class="p">,</span>
        <span class="nt">"bri"</span><span class="p">:</span> <span class="mi">252</span><span class="p">,</span>
        <span class="nt">"hue"</span><span class="p">:</span> <span class="mi">8788</span><span class="p">,</span>
        <span class="nt">"sat"</span><span class="p">:</span> <span class="mi">252</span><span class="p">,</span>
        <span class="nt">"xy"</span><span class="p">:</span> <span class="p">[</span>
            <span class="mf">0.5821</span><span class="p">,</span>
            <span class="mf">0.3892</span>
        <span class="p">],</span>
        <span class="nt">"ct"</span><span class="p">:</span> <span class="mi">500</span><span class="p">,</span>
        <span class="nt">"alert"</span><span class="p">:</span> <span class="s2">"none"</span><span class="p">,</span>
        <span class="nt">"effect"</span><span class="p">:</span> <span class="s2">"none"</span><span class="p">,</span>
        <span class="nt">"colormode"</span><span class="p">:</span> <span class="s2">"xy"</span><span class="p">,</span>
        <span class="nt">"reachable"</span><span class="p">:</span> <span class="kc">true</span>
    <span class="p">},</span>
    <span class="nt">"type"</span><span class="p">:</span> <span class="s2">"Extendedcolorlight"</span><span class="p">,</span>
    <span class="nt">"name"</span><span class="p">:</span> <span class="s2">"Extendedcolorlight1"</span><span class="p">,</span>
    <span class="nt">"modelid"</span><span class="p">:</span> <span class="s2">"LCT001"</span><span class="p">,</span>
    <span class="nt">"swversion"</span><span class="p">:</span> <span class="s2">"66010820"</span><span class="p">,</span>
    
<span class="p">}</span><span class="p">,</span>
</pre><p>Lux:</p>
<pre><span class="s2">"1"</span><span class="p">,</span><span class="p">{</span>
    <span class="nt">"state"</span><span class="p">:</span> <span class="p">{</span>
        <span class="nt">"on"</span><span class="p">:</span> <span class="kc">true</span><span class="p">,</span>
        <span class="nt">"bri"</span><span class="p">:</span> <span class="mi">254</span><span class="p">,</span>
        <span class="nt">"alert"</span><span class="p">:</span> <span class="s2">"none"</span><span class="p">,</span>
        <span class="nt">"effect"</span><span class="p">:</span> <span class="s2">"none"</span><span class="p">,</span>
        <span class="nt">"reachable"</span><span class="p">:</span> <span class="kc">true</span>
    <span class="p">},</span>
    <span class="nt">"type"</span><span class="p">:</span> <span class="s2">"Dimmable light"</span><span class="p">,</span>
    <span class="nt">"name"</span><span class="p">:</span> <span class="s2">"Lux Lamp 1"</span><span class="p">,</span>
    <span class="nt">"modelid"</span><span class="p">:</span> <span class="s2">"LWB004"</span><span class="p">,</span>
    <span class="nt">"swversion"</span><span class="p">:</span> <span class="s2">"66012040"</span><span class="p">,</span> 
<span class="p">}</span><span class="p">,</span>
</pre><p>&nbsp;</p>
