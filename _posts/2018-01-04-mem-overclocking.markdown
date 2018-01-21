---
layout: post
title:  "First rig mem overclocking"
date:   2018-01-04 19:15:00 +0200
categories: crypto mining
---

## Single RX480

Claymore v10.2 hash rate : 24.3 MH/s

{% highlight bash %}
 2100 MHz : 24.1 MH/s
 2200 MHz : 23.9 MH/s
{% endhighlight %}

=> back to 2000 MHz.
Attempts with memory frequency increase using /sys/class/drm/card0/device/pp_mclk_od does ot change hashrate.

Applying ubermix3.1 timings (PolarisBiosEditor 1.6.6 one click timing patch):

{% highlight bash %}
GPU  MEM  Temp Hashrate Power
1114 2000  59°C 29MH/s  158W
{% endhighlight %}

## Playing with BIOS default frequencies

Using SRDPolarisBiosEditor, one can set default frequencies for GPU and memory.

Modifying memory default frequency in the bios is effective contrary to writing to pp_mclk_od.

* Staring at GPU0 mem 2000MHz and GPU1 mem 1750 MHz

`GPU0 28.913 Mh/s, GPU1 25.424 Mh/s`

* GPU0 mem 2100MHz GPU1 mem 1850 MHz

{% highlight bash %}
ETH: GPU0 20.002 Mh/s, GPU1 27.116 Mh/s
GPU #0 got incorrect share. If you see this warning often, make sure you did not overclock it too much!
{% endhighlight %}

=> reduce GPU0 freq, increase GPU1 freq

* GPU0 2050Mhz mem GPU1 mem 1950Mhz

`ETH: GPU0 29.935 Mh/s, GPU1 28.685 Mh/s`

=> keep GPU0 freq, increase GPU1 freq

* GPU0 mem 2050Mhz GPU1 mem 2000Mhz
