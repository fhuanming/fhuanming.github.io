---
title: Redshift 安装与使用
date: 2018-03-04 18:00:00
tags: Redshift Ubuntu
---

因为一些已知的[原因](https://forum.justgetflux.com/topic/2373/sorry-we-only-support-8-and-10-bit-displays-right-now)，导致了我的Ubuntu无法使用f.lux. 于是我开始使用[Redshift](http://jonls.dk/redshift/). 以下是简略的安装以及使用方法。

<!-- more -->

安装
{% highlight shell %}
$ apt-get install redshift
{% endhighlight %}

建立配置文件
{% highlight shell %}
$ vim .config/redshift.conf
{% endhighlight %}

Sample Config File
{% highlight config %}
; Global settings for redshift
[redshift]
; Set the day and night screen temperatures
temp-day=5000
temp-night=3500

; Enable/Disable a smooth transition between day and night
; 0 will cause a direct change from day to night screen temperature.
; 1 will gradually increase or decrease the screen temperature.
transition=1

; Set the screen brightness. Default is 1.0.
;brightness=0.9
; It is also possible to use different settings for day and night
; since version 1.8.
;brightness-day=0.7
;brightness-night=0.4
; Set the screen gamma (for all colors, or each color channel
; individually)
gamma=0.8
;gamma=0.8:0.7:0.8
; This can also be set individually for day and night since
; version 1.10.
;gamma-day=0.8:0.7:0.8
;gamma-night=0.6

; Set the location-provider: 'geoclue', 'geoclue2', 'manual'
; type 'redshift -l list' to see possible values.
; The location provider settings are in a different section.
location-provider=manual

; Set the adjustment-method: 'randr', 'vidmode'
; type 'redshift -m list' to see all possible values.
; 'randr' is the preferred method, 'vidmode' is an older API.
; but works in some cases when 'randr' does not.
; The adjustment method settings are in a different section.
adjustment-method=randr

; Configuration of the location-provider:
; type 'redshift -l PROVIDER:help' to see the settings.
; ex: 'redshift -l manual:help'
; Keep in mind that longitudes west of Greenwich (e.g. the Americas)
; are negative numbers.
[manual]
lat=12.3456789 ; You need to change these two line to your location.
lon=-12.3456789

; Configuration of the adjustment-method
; type 'redshift -m METHOD:help' to see the settings.
; ex: 'redshift -m randr:help'
; In this example, randr is configured to adjust screen 1.
; Note that the numbering starts from 0, so this is actually the
; second screen. If this option is not specified, Redshift will try
; to adjust _all_ screens.
[randr]
;screen=1
{% endhighlight %}

运行，看配置文件是否满足你的需求
{% highlight shell %}
$ redshift
{% endhighlight %}

加入自启动
$ See official document [here](https://help.ubuntu.com/stable/ubuntu-help/startup-applications.html)
