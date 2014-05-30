---
layout: post
title: SVG与JS交互实例之画板
date: 2014-05-30 07:08
author: admin
comments: true
categories: [SVG]
tags: [SVG,JS,draw]
---
 
SVG与JS交互，可以创建更加丰富的应用，以下为一个SVG画板，通过鼠标的点击事件，模拟出画笔。

<?xml version="1.0" encoding="utf-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11-flat-20030114.dtd">
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"
zoomAndPan="disable" onload="getSVGDoc(evt)">
<title>SVG简单画板实例</title>
<desc>SVG简单画板实例</desc>
<defs>
<script type="text/javascript">
<![CDATA[
var svgdoc,svgroot,newline,posx,posy,posmin=50,drawing;
var linecol="#000",linewidth="1px",check=false;
var svgns="http://www.w3.org/2000/svg";
function getSVGDoc(load_evt)
{
svgdoc=load_evt.target.ownerDocument;
svgroot=svgdoc.rootElement;
drawing=svgdoc.getElementById("drawing");
svgroot.addEventListener("mousedown",MDown,false);
svgroot.addEventListener("mousemove",MMove,false);
svgroot.addEventListener("mouseup",MUp,false);
}
function MDown(mousedown_event)
{
Coords(mousedown_event);
newline=svgdoc.createElementNS(svgns,"line");
newline.setAttribute("x1",posx);
newline.setAttribute("x2",posx);
newline.setAttribute("y1",posy);
newline.setAttribute("y2",posy);
newline.setAttribute("stroke",linecol);
newline.setAttribute("stroke-width",linewidth);
check=true;
}
function MMove(mousemove_event)
{
if(check)
{
Coords(mousemove_event);
newline.setAttribute("x2",posx);
newline.setAttribute("y2",posy);
drawing.appendChild(newline);
}
}
function MUp()
{
check=false;
}
function Coords(mouse_event)
{
posx=mouse_event.clientX;
posy=mouse_event.clientY;
if(posx<posmin)posx=posmin;
if(posy<posmin)posy=posmin;
if(posx>640+posmin)posx=640+posmin;
if(posy>480+posmin)posy=480+posmin;
}
function SetLineColor(mousedown_event)
{
linecol=mousedown_event.target.style.getPropertyValue("fill");
svgdoc.getElementById("aktline").style.setProperty("stroke",linecol,"");
}
function SetLineWidth(mousedown_event)
{
linewidth=mousedown_event.target.style.getPropertyValue("stroke-width");
svgdoc.getElementById("aktline").style.setProperty("stroke-width",linewidth,"");
}
]]>
</script>
</defs>
<text x="30" y="30" style="fill: #000; font-size: 24px">
SVG简易画板，更多实例请访问<tspan font-weight="bold" fill="red" >www.waylau.com</tspan>
</text>
<rect x="50" y="50" width="550" height="400" rx="5" ry="5"
style="fill: #FFF; stroke: #000"/>
<g onmousedown="SetLineColor(evt)">
<rect x="30" y="60" width="10" height="10"
style="fill: #FF0; stroke: #000"/>
<rect x="30" y="80" width="10" height="10"
style="fill: #F00; stroke: #000"/>
<rect x="30" y="100" width="10" height="10"
style="fill: #090; stroke: #000"/>
<rect x="30" y="120" width="10" height="10"
style="fill: #00C; stroke: #000"/>
<rect x="30" y="140" width="10" height="10"
style="fill: #000; stroke: #000"/>
</g>
<g onmousedown="SetLineWidth(evt)">
<line x1="30" y1="180" x2="40" y2="180"
style="stroke: #000; stroke-width: 1px"/>
<line x1="30" y1="200" x2="40" y2="200"
style="stroke: #000; stroke-width: 2px"/>
<line x1="30" y1="220" x2="40" y2="220"
style="stroke: #000; stroke-width: 3px"/>
<line x1="30" y1="240" x2="40" y2="240"
style="stroke: #000; stroke-width: 4px"/>
<line x1="30" y1="260" x2="40" y2="260"
style="stroke: #000; stroke-width: 5px"/>
</g>
<line id="aktline" x1="18" y1="58" x2="18" y2="262"
style="stroke: #000; stroke-width: 1px"/>
<g id="drawing"></g>
</svg>