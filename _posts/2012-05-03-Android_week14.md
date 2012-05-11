---
layout: post
title: Android Week#14 May03, 2012
---

{{ page.title }}
================

<p class="meta">03 May 2012 - San Francisco</p>


* pp 407 in textbook
sendMessage() - put msg into que
sendMessageAtFrontOfQueue() - put mes in front of que
sendMessageAtTime(msec) - put msg into queue @certain time
sendMessageDelayed(msec) - puts msg into queue after m msec

###Alerntaive to emulator
Droid@Screen
ANDROID_HOME
ANDROID_SDK_HOME

droidAtScreen.nnn.jar
droidAtScreen-1.0.1.jar

  - modify antfile (comment out line calling emulator replace w this line)
`alias DS=java -jar droidAtScreen-1.0.1.jar`

###
obtainMessage() - create object of a message and put it in message queue, this is empty message

obtainMessage(int what) - 
i.e. 1=spring, 2=summer or enum season
se{SPRING, SUMMER,FALL,WINTER}

obtainMessage(se.getValue(SUMMER));

obtain(int what, Object obj) - pass an object
i.e. Document d = new Document(); //object must be parcelable
obtainMessage(1, d);    //class Document extends GoodDoc implements Parcelable


obtainMessage(int what, int arg1, int arg2) - 
obtainMessage(int what, int arg1, int arg2, Object o)
obtainMessage(Object ob)





###How to get the message from the message pool
package my.demo.ProgressBar;

public class DemoHandler extends Activity
{
		progressBar pb;
		
		//we are overloading the mehod handleMessage
		Handler h = new Handler
		{
			@Override
			public void handleMessage(Message msg)
			{
				pb.incrementProgressBy(5);
			}
		};
		
		AtomicBoolean isRunning = new AtomicBoolean(false);
		
		@Override
		public void onCreate(Bundle b)
		{
			super.onCreate(b);
			setContentView(R.layout.main)
			pb = (ProgressBar)findViewById(R.id.progressbar);
		}
		
		public void onStart()
		{
			super.onStart();
			pb.setProgress(0);
			
			Thread t = new Thread(new RUnnable()
				public void run()
				{
					for(int i=0; i<20 && isRunning.get(); i++)
					{
						sleep(1000);
						h.sendMessage(h.obtainMessage());
					}
				});
				isRunning.set(true);
				t.start();
		}
		
		//remember when for example you receive an emergency call
		//then onStop() method will be called
		public void onStop()
		{
			super.onStop();
			isRunning.set(false);
		}
}

###Services are running in the background
 * A Services is not dependent on an Activity, A service can still run even after the activity is dead
 * For Example a music that is playing music in the background
 * Services is like daemon in linux
 * generate program that generates random number, then call startService(), you also have a method call stopService()
 * Service can run forever

package my.demo.Service;
import andriod.app.SErvices
import android.content.Intent;
import nadroid.os.IBinder;
import android.widget.Toast;

import java.net.*;

public class MyService extends Service
{
	@Override 
	public IBinder onBind(Intent arg0){ return null; }
	
	@Override
	public int onStartCommand(Intent i, int flages, int startId)
	{
		Toast.makeText(this, "Service Started", Toast.Length_LONG).show();
		return(START_STICKY) //this means to run forever
		//START_NO_STICKY - 
		//START_REDELIVER_INTENT - use when onStart command is invoked
	}
	
	@Override
	public void onDestroy()
	{
		super.onDestroy();
		Toast.makeText(this, "Service Destroid", Toast.LENGTH_LONG).show();
	}
	
	
}

###in your AndroidManifest.xml
<service android:name =".MyService">
	<intent-filter>
		<action android:name="my.demo.Servce" />
	</intent-filter>
</service>


###sample program using Services
package my.demo.Service;
import andriod.app.Services
import android.content.Intent;
import nadroid.os.IBinder;
import android.widget.Toast;

public class MainActivity extends Activity
{
	@Override
	public void onCreate(Bundle b)
	{
		super.onCreate(b);
		setContentView(R.layout.main);
		Button btnStart = (Button)findViewById(R.id.btnStartService);
		
		btnStart.setOnClickListener(new View.onClickListener()
		{
			public void onClick(View v)
			{
				startService(new Intent(getBaseContext(), "MyService.class"));
				
				//new Intent("my.demo.SErvice") //another way of calling service, the param is the name of package. You only use this when you have 1 service in your package, if you have two service you need to use the other method
				
			}
		});
		
		Button btnStop = (Button)findViewById(R.id.btnStopService);
		btnStop.setOnClickListener(new View.onClickListener()
		{
			public void onClick(View v)
			{
				stopService(new Intent(getBaseContext(), "MyService.class"))
			}
		});
	}
}

* In your main.xml
<Button android:id ="@+id/btnStartService"
	android:layout_width="fill_parent"
	android:layout_height="wrap_content"
	android:text="Start Service" />

<Button android:+"@id/btnStopServie"
	---
	---
/>
 










