---
layout: post
title: Android Week#3 Feb2, 2012
---

{{ page.title }}
================

<p class="meta">2 Feb 2012 - San Francisco</p>

* Any kind of string you create, should definitely in the string resource file. You should put it in .xml file
* The advantage of using xml file in resource file is you don't have to recompile the data. If you want to change the string you just need to modify the xml file.

{% highlight java %}
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
//notice something different all the xml layout start with android:name, //android:string…etc but this one starts with xmlns:android, 
//this is the only one that has android in the back. 
//All other attributes starts with android:
{% endhighlight %}

#For colors.xml
located in res/values/ - you need to create this file manually, filename doesn't need to be colors.xml

* Read chapter 3 pp64 (Android pro) this is the most important chapter in the textbook
* all your resource file needs to have <?xml version="1" encoding="utf=8">

Inside the color.xml
{% highlight java %}
<resources> 
	<color name="red">#f00</color>
	<color name="blue">#0000ff</color>
	<color name="green">#f0f0</color>
</resources>
{% endhighlight %}

Inside your main.xml
{% highlight java %}
android:textColor="@color/red"  //this will automatically look for variable color name red
andoird:text="helloWorld"
{% endhighlight %}

Inside R.java
{% highlight java %}
package com.cs211d;
	public final class R
	{
		public static final class attr
		{}
		public static final class color
		{
			public static final int green=0x7f030001;
			public static final int red=0x7f040001;
		}
		public static final class layout
		{
			public static final int main=0x7f001;
		}
		public static final class string
		{
			public static final int line1=0x8...;
			public static final int line2=0x9…;
			public static final int line3=0xA…;
		}
{% endhighlight %}

#In AndroidManifest.xml
located in the root of a project, this file contain the complete detail of each project i.e. contain the activities, you can modify this file so that you will be in **debug mode** , also this is modifiable

#how to debug your program
In android debug has 2 context/meaning

* loading into your device and debuging it
* using logcat

#log class
made for logging information for our program, need to import in your code `import android.util.Log`. Also it comes with static method

* int Log.e(...) //for error
* int Log.w(...) //for warnings
* int Log.i(...) //for information message
* int Log.d(...) //for debug message
* int Log.v(...) //verbose message
* int wtf 		 //what a terrible function

Sample Code
<code>
	try 
	{
		String tag="In my lost hw";
	} catch(Exception e) { Log.e (tag, "my error ...."); }
</code>

* Log.v(tag, message)
* Log.d(tag, message)
* Log.d(tag, message, e)
* Log.e(tag, message)
* Log.e(tag, message, e)
* Log.i(tag, message)
* Log.i(tag, message, e)
* Log.w(tag, message)
* Log.w(tag, message, e)
* Log.wtf(tag, message)
* Log.wtf(tag, message, e)

</br>
#Accessing logcat in command line
type in `adb logcat` located in platform-tools folder

* Abbase provided file called `lcat` located in his home directory
	* will filterized your logcat
	* @use: lcat "hw#1"
	* @use: lcat -e   //empty logcat
	* `adb logcat -f my_oldlogcat` //saves the current logcat
	* `adb logcat -d` - dumps the current message then stop
	* `adb logcat MyActibity:I MyApp:D *:S`  //show information, show debug message, don't show reast of the file supress it