---
layout: post
title:  "First rig power save"
date:   2018-01-02 19:15:00 +0200
categories: crypto mining
---

Claymore v10.2 hash rate : 24.3 MH/s

GPU clock, Power (plug), Temp

7: 1310Mhz 194W  64°C

4: 1184Mhz 160W  59°C

3: 1114Mhz 148W  56°C

`echo manual | sudo tee /sys/class/drm/card0/device/power_dpm_force_performance_level > /dev/null`

`echo 3 | sudo tee /sys/class/drm/card0/device/pp_dpm_sclk > /dev/null`

At 910Mhz (53°C) hashrate starts droping (24.0MH/s)