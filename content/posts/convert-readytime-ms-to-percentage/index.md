---
title: "Script to convert VM readytime from milliseconds to percentage"
date: 2021-10-27T15:36:59+02:00
draft: false
hideReadMore: true
comments: false
tags:
    - VMWare
    - vCenter
    - Powershell
    - readytime
    - millisecond to percentage
---

Created a small script to quickly convert the CPU Ready time view of a VM shown in milliseconds to percentage per vCPU

<!--more-->

Been doing some work on monitoring some virtual machine cpu behavior cross-checking tools such as esxtop, vROPS and vCenter.  
Thought it might be handy to have a quick script that can calculate readytimes per vCPU using vCenter milliseconds as input.
![vCenter Monitor Readytime in ms](img/vcenter-readytime-ms-00.png)
<small>_screenshot of vcenter monitoring readytime in milliseconds_</small>

![script in action](img/vcenter-readytime-ms-script.png)
<small>_script in action_</small>

The script can be found [here](https://gist.github.com/virtualistic/bc9118e3d142718fb5e48f3cf9c4f3aa)
