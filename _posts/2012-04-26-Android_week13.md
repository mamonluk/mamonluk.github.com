---
layout: post
title: Android Week#13 Apr12, 2012
---

{{ page.title }}
================

<p class="meta">26 Apr 2012 - San Francisco</p>

###MultiThreading in Android:
normally programs have one process. threads allow you to have multiple processes.
processing is done by calling

fork()
p1 = fork();

* creating a process by forking is very costly with resources! creating several of these, could kill your system, 
(on a phone).on your cellphone, one process can be divided into smaller pieces. these pieces are called threads. this way, we have one process with multiple threads, and it is much more efficient.

* Also called multithreading.

* working with threads is very tricky! when you create several threads, and they are supposed to all access a single file, or a single data structure, problems can occur!we need to find a mechanism to control these threads. for instance, once thread writes into a data structure, and the other threads dont access that data until the first thread is done.

* many methods that are available in java and android are NOT safe for multithreading. they are called thread unsafe methods. if you use them, you can corrupt things.

* the majority of the variables we use in java... none of these are made for handling threads efficiently or correctly! if you use a regular variable to handle threads, you have problems!

* after version 5 of java, they made something called "atomic" variables.
{% highlight java %}
atomicboolean
atomicinteger
atomicshort
etc.
{% endhighlight %}

* we also have atomic datastructures, like atomicarray etc.
* atomic are primitive types still!
* 90% of collections are thread safe with atomic.

* When you are running your application, all of our activities are in one thread: main application thread.
suppose you are using the setText() method.. whenever you use it, the text on the screen is updated. this is not simple! the data you enter goes into a data structure called message queue. it will wait here, and when the time comes, your data will be taken from the message cue, and THEN will be placed on the screen. the reason for this, is that if there are several threads, and all are trying to display something, they will conflict with each other. so based on priority, the data in the message queue is displayed.

* all of the activies that involve UI elements, by design, must be done in the "main application thread". this simply means, you cannot cannot some other threads to do this job. so if you have 5 threads, you cant have each of them displaying various things... all of these UI events must be handled in the main application thread. this can be a big problem. lets say you want to show the progress bar. (there are 2 types of progress bars:

horizontal bar that gets filled up. (this is called determinant, means you know how long)
circular that turns (this is called indeterminant, meaning you dont know how long)

so if your activity is measurable, then use determinant otherwise use indeterminant.

only use these, for when jobs take a long time.

if you are working with a time consuming job(longer than half a second), you CANNOT put that in main application thread. if you do, you will end up with ANR (application not responding) error. you will see this often! when you see ANR, android has automatically killed your process!

as a result, no matter what it is, your time consuming jobs must be done in a seperate thread. maybe you are sorting lots of data? you need to show a status bar to show the user that something is happening in hte background.

so how do we have the threads talk to each other? what if you are sorting in one thread, and display UI stuff from that sorted data? they need to share this data, as they cant be in the same thread! use a mechanism called Handler. Handler lets communication take palce between your thread and the main application thread. Handler acts as a bridge.

majority of onSomething methods (onCreate etc), by design must run in the main application thread.

how to make determinant bar:
(there are many ways to create progress bar GUI... easiest way is to use xml. in main.xml you can make progressbar tag. immediately the default will show up (indetermanint). if you just put it in there like this, it will spin forever! default is medium size, but you can make it large or small. etc

there is the method: setMax(int n) that sets max value for determinant bar. 0-100 for example. default starting point is 0, but you can change that too: setProgress(int n) to set the starting number. at any time you could run a method getProgress() which says how much progress we have covered!

there are 2 ways to specifiy if you want determinant or indeterminant... xml and in java.

in java, there is a method: setIndeterminant(boolean b)

###Sample Program creating progress bar 
* in your main.xml
{% java highlight %}
<!-- in main.xml -->
<!--default is indeterminant if not specified! -->
<LinearLayout>
<ProgressBar android:id="@+id/progressbar"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
/>
</LinearLayout>
{% endhighlight %}

* in your .java file 

{% highlight java %}
import android.os.Handler;
import android.os.widget.progressBar;

public class MainActivity extends Activity
{
	private static int progress;
	private ProgressBar pb;
	private int progressStatus=0;
	private Handler h = new Handler();
	
	@Override
	public void onCreate(Bundle b)
	{
		super.onCreate(b);
		setContentView(R.layout.main);
		progress = 0;
		pb = (ProgressBar) FindViewById(R.id.progressbar);
		
		pb.setMax(200);			//this is for making progress bar indeterminant
		new Thread(new Runnable()
		  public void run()
	   	  {
			while(progressStatus < 200)
			{
				progressStatus=doSomeWork();
			}
			
			//hide the proress bar now
			h.post(new Runnable ()
			{
			   public void run()
				{
					pb.setVisibility(8)  //0=visibility, 4=invisible, 8=gone
				}
			});
		
			//Different version, so that progress bar will become determinant
			//h.post(new Runnable()
			//{
			//	public void run()
			//	{
			//		pb.setProgress(progressStatus);
			//	}
			//});
			
		  }		
	    	private int doSomeWork()
			{ sleep(50);  return(++progress); }
		}).start();
		
		public void sleep(int msec)
		{
			try{
				Thread.slee(msec);
			}
			catch(Throwable e){ }
		}
	}	
}
{% endhighlight %}

* In you main.xml
//this will make the progress bar determinant 
style="?android:attr/progressBarStyleHorizontal"    
style="?android:attr/progressBarStyleLarge"
style="?android:attr/progressBarStyleSmall"

###Using Handler
{% highlight java %}
import android.app.Activity;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.widget.progressBar;
import java.util.concurrent.atomic.AtomicBoolean;

public class DemoHandler extends Activity
{
	progressBar pb;
	//define the hanlder to receive the message from the message pool
	Handler h = new Handler(){
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
		pb=(ProgressBar)findViewById(R.id.progressbar);
	}
	
	public void onStart()
	{
		super.onStart();
		pb.setProgress(0);
		
		Thread t = new Thread(new Runnable()
		{
			public void run()
			{
				for(i=0; i<20 && isRunning.get(); i++)
				{
					sleep(1000);
					h.sendMessage(h.obtainMessag());
				}		
			}
		});
	isRunning.set(true);
	t.start();
	}
	
	public void onStop()
	{
		super.onStop();
		isRunning.set(false);
	}	 
}
{% endhighlight %}
















