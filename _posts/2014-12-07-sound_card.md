---
title: HDMI is Default ALSA Sound Device
category: linux
layout: back
---

# Default Sound Card

I installed ALSA for a new Arch setup, but the default sound card is set to HDMI with S/PDIF displayed in the middle of the alsamixer screen.

Arch Wiki on ALSA says to define something like
{% highlight bash %}
(in /etc/modprobe.d/alsa-base.conf)
options snd_mia index=0
options snd_hda_intel index=1
{% endhighlight %}

Except that when I cat /proc/asound/modules to get the names, they\'re both snd_hda_intel for both HDMI and speakers/headphone jack.

## Solution!
 [Source](http://unix.stackexchange.com/questions/62683/sound-not-working-when-both-the-sound-card-and-hdmi-use-the-intel-hda-driver-ho)

You can inspect /proc/asound with ls -l to find the proper module ids, ie SB/HDMI. Then you can use the id attribute:

{% highlight bash %}
(in /etc/modprobe.d/alsa-base.conf)
options snd-hda-intel id=SB index=0
options snd-hda-intel id=HDMI index=1
{% endhighlight %}
