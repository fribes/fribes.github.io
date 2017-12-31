---
layout: post
title:  "First Rig Bill"
date:   2017-12-26 19:15:00 +0200
categories: crypto mining
---

# Starting point

While looking for a mining GPU, I found a refurbished RX480 8GB

`F-ASUS STRIX-RX480-O8G-GAMING                      224.96€`

Trying it on my desktop computer led to installing to much crap in my opinion, and was tricky to avoid breaking existing configuration which use a nvidia card. Time to go shopping !

# Aim

Assemble a small rig capable to host 3 gfx cards. Buy locally, only equipment that is in stock to get the rig running the same day with a single board.

# Costs

{% highlight bash %}
LDLC US-750G 750W power supply 80+ Gold            101.38€
Intel Pentium G4560 3.5GHz LGA1151 Box              77.40€
MSI B250M PRO-VD                                    64.53€
Crucial CT4G4DFS824A (4Go DDR4 2400 PC19200)        49.99€

Motherboard total                                  293.30€

{% endhighlight %}

For mass storage, I re use a USB key (Sandisk extreme 16Go, 17.81€ in oct. 2015)	

# Rationale

## Power supply

Based on a 3 RX480 rig, the expected consumption is 3x150W plus motherboard. Say 500W.
Power supply efficienfy is best around 50% use, and drops at 90%. 750W is a tradeoff for efficiency, morevover, the 650W was not available.

## Motherboard

The cheapest providing 3 pci-e slots.
MSI B250M PRO-VD provides 1 PCI-E 16x slot and 2 PCI-E 1x slots.
It's enough to start with a single gfx board, and expand afterwards thanks to risers.

## CPU

Cheapest, low consumption, as the computing effort is done by the GPUs.

## Memory

Cheapest compatible with motherboard : DDR4 mandatory, 4GB is the smallest amount availble.

# Assembly

![First rig](/assets/images/rig_v0_1_0.jpg)
