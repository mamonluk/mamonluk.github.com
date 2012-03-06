---
layout: post
title: Android Week#4
---

{{ page.title }}
================

<p class="meta">2 Feb 2011 - San Francisco</p>

* Any kind of ssdfsdftrsdfsding you create, should definitely in the string resource file. You should put it in .xml file
* The advantage of using xml file in resource file is you don't have to recompile the data. If you want to change the string you just need to modify the xml file.

{% highlight java %}
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
//notice something different all the xml layout start with android:name, //android:string…etc but this one starts with xmlns:android, 
//this is the only one that has android in the back. 
//All other attributes starts with android:
{% endhighlight %}

