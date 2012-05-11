---
layout: post
title: Android Week#8 Mar8, 2012
---

{{ page.title }}
================

<p class="meta">08 Mar 2012 - San Francisco</p>

###Intents Components
* Action,Data, Extra data, class name
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

* if you want to test if program is running on emulator or phone

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

* Running a script to check if emulator is running

{% highlight java %}
a=$(adb devices | grep emulator)
if[ -z "$a" ]; then
	echo "no emulator up & running"
	exit 1
fi
b=$(echo $a | grep "offline")
if [ -n "$b" ];
	echo "no emulator running"
	exit 2
fi
{% endhighlight %}

###Creating Extra data
{% highlight java %}
Intent in = new Intent(getApplicationContext(), HelpActivity.class);
in.putExtra("com.cs211d.chippy.LEVEL", 23);
startActivity(in);
//			key             value
//putExtra(String name, boolean value)
					    int 
					    double
					    String
					    float
					    int []
{% endhighlight %}

* You need to add this in AndroidManifest.xml

{% highlight java %}
<activity android:name=".gameActivity"></activity>
{% endhighlight %}

* Retrieving data from the other acticity
{% highlight java %}
Intent in = getIntent();
int helpLevel = in.getIntExtra("com.cs211d.chippy.Level", 1);
								//the 1 is the default value
{% endhighlight %}

* How to know which Activity sent the data
`this.getIntent.getData` -return the uri of activity who sent the data i.e. screen1.class returns null if data is empty

* Another way of sending data to an intent

{% highlight java %}
Bundle b= new Bundle()
b.putString("Name", "John");  //b.putString("Name", "82");
in.putExtra(b);
//in is object of Intent
{% endhighlight %} 

* Transferring large amount of data to an activity

{% highlight java %}
int nums[]={3,9,5,17};
Intent in = new Intent(this, B.class);
in.putExtra("numbers", nums);
startActivity(in);
{% endhighlight %}
* Receiving the large data from an activity
{% highlight java %}
Bundle b = getIntent().getExtra();
int nums[]=b.getIntArray("numbers");  //remem there is no default values
{% endhighlight %}

* pp 123 in book alot of examples about transferring data

<br/>
* Transferring array list to an activity
`in.putIntegerArrayListExtra("allAges", ArrayList<Integer> al)`
`in.putStringArrayListExtra("allNames", ArrayList<String> al)`
* getting the data
`Bundle b=getIntent().getExtra();`
`b.getStringArrayListExtra("allnames"); //or b.getIntegerArrayListExtra("all Ages")`

###Getting the current android version
`Android.Build.VERSION.SDK`
`Android.os.Build.VERSION.SDK.INT`
`Android.os.Build.VERSION_CODES.FROYO //DONUT, CUPCAKE`



