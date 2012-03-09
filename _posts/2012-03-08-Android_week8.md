---
layout: post
title: Android Week#8
---

{{ page.title }}
================

<p class="meta">08 Mar 2012 - San Francisco</p>

###Intents Components
* Action, Data, Extra data, class name
	* class name can have two different approach
		* call your class within another class
		* call your class with another package
		
{% highlight java %}
public static void call(Activity a, String poneNo)
{
	Intent i = new Intent(Intent.ACTION_CALL);
	i.setData(Uri.parse("tel:" + phoneNo.));
	a.startActivity(i);
}

Intent i = new Intent();
i.setComponent(new ComponentName("com.android.contacts", "com.android.contacts.DialContactsEntryActivity"));
								  //this is the package   //full qualified class name
//setComponent - is a method that allows us to set what package to use

startActivity(i);	
{% endhighlight %}

###side note
* if you want to test if emulator is working or not 

{% highlight java %}
if(System.Environment.DeviceType != DeviceType.Emulator);
{
	Log.e(TAG, "Emulator is not running");
}
{% endhighlight %}

* if oyu want to test if running of emulator or phone

{% highlight java %}
if("google_sdk".equals(Buid.PRODUCT))
{
	String android_id = Secure.getString(getContentResolver(), Secure.ANDROID_ID);
	if(android_id==null)
	{
		emulator...
	}
	else
	{
		device
	}
}
{% endhighlight %}

* another way of testing if it's running on the phone

`if("google_sdk".equals(Build.PRODUCT)) {   ------  }`

* another way of testing if it's running on the phone

{% highlight java %}
public boolean isEmulator()
{
	return Build.MANUFACTURER.equalsIgnoreCase("unknown");  //
}
{% endhighlight %}
