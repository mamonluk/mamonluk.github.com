---
layout: post
title: Android Week#10 Apr05, 2012
---

{{ page.title }}
================

<p class="meta">05 Apr 2012 - San Francisco</p>

###Answer for Midterm:
Question 3: How would you insert file in your sd card 

Ans: `adb push /john/cat.jpg /sdcard/cat.jpg`

Question 4: Check if running on phone or emulator
Ans: 
{% highlight java %}
public String CellphoneOrEmulator()
{
	return("google_sdk.equals(Build.PRODUCT)? "CELLPHONE" : "EMULATOR");
}
{% endhighlight %}

Question 5: How do you know how long the toast appear in seconds

Ans:
{% highlight java %}
Toast t = Toast.makeText(getApplicationContext() "whats ", Toast.LENGTH_SHORT);
int duration=t.getDuration();
{% endhighlight %}

Question 6: What are the intents components

Ans: action, data, extra data, class name

Question 7: create method isEmualtorUp()

Ans:
{% highlight java %}
public String isEmualtorUp()
{
return(Build.MANUFACTURE.equalsIgnoreCase("unknown"));
}
{% endhighlight %}

Question 8: make a method "whoCallsMe" which means what activity called me
Ans:
{% highlight java %}
public String whoCallsMe()
{
	return(this.getIntent().getData());
}
{% endhighlight %}

Question 9: Make method to return version of android device

Ans:
{% highlight java %}
public double getVersion()
{
	return(Android.os.Build.VERSION.SDK.INT);
}
{% endhighlight %}

Question 10: How would you avoid receiving wrong phone number

Ans:
`<EditText> android:inputType="Phone"` or `et.setInputType(InputType.TYPE_CLASS_PHONE);` 
or `et.setInputType(InputType.TYPE_CLASS_TEXT)`

###Side Note
How come when given an apk file then you tried to install to other phone it doesnt work.
Ans: All application have public key and date limitation. Inside your .apk file there is limitation or expiration data. When you post your application, you need to encrypt it. put public key(google recommend that you put the min limitation for 25 years)
* creating a hash key - to keep your source code secure. If someone tried to change source code then the hash key will be change 

* public key is a key that you publicize but private key should be kept and can be use to decrypt your code.
* given a two big prime numbers x=p1 * p2 , note that when x is given then p1 and p2 is hard to find, it takes around 500 years to figure it out. p1 and p2 will be the private key, x will be the public key.
* Notes regarding signing your application: The android system requires all install application be digitally signed with certificate whose private key held be application developer

* android sdk automatically signs your application. Debug key
* System checks the expiration of the application date only at install time
*we use keytool and jarsigner to generate key and sign the application. Finally we are using zipaline to optimize application packages. If you didn't do the following steps except zipaline then google app market will not accept your application. 
* keytool is made by sun and comes with java application
* if you are using linux, do not use its own keytool software. User keytool made by java instead. 
* In the root of your application, you will see a file called keystore file in
`~/.android/debug.keystore` this file contains the key.
* `keytool -list debug.keystore` to view your priate key, remember you cannot cat this file bec. this is binary, the keystore is valid for 1 year after creation

###How to make a key
* How to make your own keystore (makesure you are inside ~/.android)
`keytool -genkey -v -Keystore release.Keystore -alias release -keyalg RSA -validity 500000 -keysize 2048`

* Then a bunch of question will be asked to you
	1. first question "Enter keystore password" when someone wants to install your application, this question will be asked again.
	2. "What is your first and last name", you need to provide your real name because when you post it to google market it will ask for verification
	3. "What is the name of your organizational unit?"
	4. "what is the name of you organization?"
	5. "what is the name of your city of locality?"
	6. "what is the name of your state or province"
	7. "what is the two_letter country code for this unit?"

* Now we need to sign our application so we achieve this by doing the ff(inside ~/.android):
`ant release`

* (inside your bin directory) type the ff:
`jarsigner -keystore ~/.android/release.keystore` then what would happen is the file would change from "helloAndroid-unsigned.apk" to "helloAndroid-signed.apk"

* `zipalign -v 4 name_of_your_app` - this is not required by google market but this will encrypt and delete all garbage files in your app. Optimizes your app.	

* How to look up for your private key
`keytool -list release.keystore` - this will show you the private key usually the last line

###Life Cycle of Activity - OnCreate(),onStart(),OnResume(), etc
* onCreate() is one of the method in the activity that we override. 
	1. onCreate(Bundle b) - whenever activity is created this method is called
	2. onStart() - as soon as your activity is showed or visible this method is called
	3. onReusme() -when your acitivity starts interacting w/ user this method will be called
	4. onPause() - whenever your acitivty for any reason is puase, this method will be called
	5. onStop() - when activity is no longer visible to the user onStop() will be called
	6. onDestry() - just before activity is being killed
	7. onRestart() - 
	
* Activity Cycle


* Demo Code

{% highlight java %}
package com.demo.Activities

public class MainActivity extends Activity
{ 
	String tag="Events";
	@Override
	public void onCreate(Bundle b)
	{
		super.onCreate(b);
		setContentView(R.Layout.main);
		Log.d(tag, "in onCreate");
	}
	public void onStart()
	{
		super.onStart();
		---
		Log.d(tag, "in onStart");
	}
	public void onRestart()
	{
		super.onRestart();
		---
		Log.d(tag, "in onRestart");
	}
	public void onResume()
	{
		---
		---
		Log.d(tag, "in onResume");
	}
	public void onPause()
	{
		---
		---
		Log.d(tag, "in onPause");
	}
	public void onStop()
	{
		---
		---
		Log.d(tag, "in onCStop");
	}
	public void onDestroy()
	{
		---
		---
		Log.d(tag, "in onDestroy");
	}
}
{% endhighlight %}

* Run the demo and check logcat and you will see this
	* When you start your app you will see this onCreate(), onStart(), onResume();
	* When you push back button on your devicey you will see this in onPause(), onStop(), onDestroy().
	* When you click Home button and hold for couple of seconds onCreate(), onStart(), onResume().
	* When you press phone button onPause(), onStop()
	* then press back button onRestart(), onStart(), onResume().
 
<a href="http://imgur.com/iLKaN"><img src="http://i.imgur.com/iLKaN.jpg" alt="" title="Hosted by imgur.com" /></a>

