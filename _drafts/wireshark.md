---
layout: post
excerpt: Usage and Commands for better capturing of traffic using wireshark
category: linux-tools
title: Wireshark Usage
comments: false
google_adsense: false
---
* Enable packet capture for non-root users
{% highlight bash %}
$ sudo dpkg-reconfigure wireshark-common
$ sudo usermod -a -G wireshark $USER
$ sudo adduser $USER wireshark
{% endhighlight %}
Re-login for these changes to take effect.
