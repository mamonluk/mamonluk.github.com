---
layout: post
title: Android Week#7 Mar1, 2012
---

{{ page.title }}
================

<p class="meta">01 Mar 2011 - San Francisco</p>

* making a toast
* private void mkToast()
* application context object - in android OS, it controls all of the activities!. This is where
if you are only dealing with 1 activity, it doesnt matter if you create application context object
* if you use the keyword "this" it may cause memory leak
* if you are dealing with multiple activity, hence you need to create application context object

{% highlight java %}
private void mkToast(String text)
{
Context con = getApplicationContext(); 
//this is a factory method, belongs to activity class
//when you assign a factory method, you would think the object assign to it is a interface, i.e. Context con;
//factory method means its gonna decide what object to create at runtime

Toast t = Toast.makeText(con, text, Toast.LENGTH_SHORT) 
//arguments are the Context, desired message, duration
//if you don't want to use the predefined length, just enter a length in milliseconds
//this is a factory method ,
t.show;
}
{% endhighlight %}

//toast show at the bottom of your phone

* you can set the layout for toast

{% highlight java %}
	t.setGravity(int G, int x, int y)      
	//(0,0)------>
		   |	
		   |
		   |
		
	t.setGravity(Graivity.TOP | Gravity.LEFT, 0, 0);
*Methods for taost
	=t.cancel()
	=t.getDuration();
	=t.getGravity();
	=int getHorizontalMargin();
{% endhighlight %}
* Constants
 1. Gravity.LEFT
 2. Gravity.RIGHT
 3. Gravity.TOP
 4. Gravity.BOTTOM
 5. Gravity.CENTER (this is vertical AND horizontal)
	
</br>

* Intent - is a mechanism that allows us to hook two or more activities
		-using intent to use alarm, event, call another activity, using intent to communicate background processes
		-daemon are background processes
		-two kinds of intent, internal and external
		-there is an intent that let us work with map
		-browse the internet using intent, call the phone, etc
		
* Each activity has its own layout file

{% highlight java %}
public class Util extends Activity
{
public void callWebBrowser()  //public void callWebBrowser(Activity activity) 
							  //then you don't need to extend Activity
{
	startActivity(new Intent(Intent.ACTION_VIEW, uri.parse("http://www.google.com")));		
	//if you didn't extend it then you need to create an object of activity
	
}
public void callWebSearch()
{
 startActivity(new Intent(Intent.ACTION_WEB_SEARCH, uri.parse("http://www.google.com")));
}
public void call()
{
	startActivity(new Intent(Intent.ACTION_CALL, uri.parse("tel:415-392-7088")));
}

public void showMap(double lat, double lon)
{
	startActivit(new Intent(Intent.ACTION_VIEW))
	uri.parse("geo:" + lat + "," + lon)
}
public void dial()
{
	startActivity(new Intent(Intent.ACTION_DIAL));
}
}
{% endhighlight %}

* Remember to add this to Androidmanifest file for CALLING PHONE `<uses-permission android:name="android.permission.CALL_PHONE" />

<br/>
* debugging activity through Log

{% highlight java %}
public void onCreate(Bundle b)
{
	super.onCreate(b);
	setContentView(R.layout.some_view)
	Intent i = this.getIntent();
	if(i==null)
	{
		Log.d("mytest", "no intent invoked me");
	}
}
{
{% endhighlight %}


<br/>
* Sample Code creating Activity

{% highlight java %}
package cs211d.demo;
import android.app.Activity;
import andriod.os.Bundle;
import android.os.Content;
import android.content.Intent;
import android.widget.Button;   //when using toast need to import android.widget.Toast;

public class Screen1 extends Activity
{
	public void onCreate(Bundle b)
	{
		super.onCreate(b);
		setContentView(R.layout.main);
		Button b= (Button)findViewById(R.id.btnClick);
	
	b.setOnClickListener(new View.OnClickLIstener()
	{
		public void onClick(View v)
		{
			Context con = getApplicationContext();
			Intent i=new Intent(con, screen2.class); 
			//Intent i=new Intent(this); 
			//this is not recommended when using multiple activity, this would create memory leak
			
			startActivity(i);	
		}
	});
}
{% endhighlight %}

<br/>
###side note
<LinearLayout ....>
	<TextView 
		---
		---
	/>
	<TableLayout....>
		---
		---
	</TableLayout>
</LinearLayout>

* TableLayout by default is put side by side
* LinearLayout by default is vertical 
*  We can put nested layout to one another,,,,


* /res/main.xml

{% highlight java %}
<?xml ------?>
<LinearLayout ......
	android:orientation="vertical"
	android:layout_width="fill_parent"
	android:layout_height="fill_parent"
	>
<TextView 
	android: layout_width="fill_parent"
	android:layout_height="wrap_content"
	android:text="you are in 1st screen"
	/>
<Button
	android:id="@id/btnClick"
	android:layout_width="wrap_content"
	android:layout_height="wrape_content"
	android:text="open new screen"
	/>
</LinearLayout>
{% endhighlight %}

* /src/screen2.java

{% highlight java %}
public class Screen2 extends Activity
{
	public void onCreate(Bundle b)
	{
		super.onCreate(b);
		setContentView(R.layout.main);
		Button b= (Button)findViewById(R.id.btnClick2);
	
	b.setOnClickListener(new View.OnClickLIstener()
	{
		public void onClick(View v)
		{
			setResult(RESULT_OK);
			finish();
		}
	});
}
{% endhighlight %}

<br/>
###/AndroidManifext.xml
`<activity class=".Screen2" android:label="Screen2">`
`</activity>`










