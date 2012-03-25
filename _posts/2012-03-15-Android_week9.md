---
layout: post
title: Android Week#9 Mar15, 2012
---

{{ page.title }}
================

<p class="meta">15 Mar 2012 - San Francisco</p>

* hw for next week
A simple program. No layout defined ... design your own
A game that test your knowledge of states capitals
if city is given, name the state, if state is given, name the city


* `System.setProperty("Font", "Helvetica")`
* `String System.getProperty("Font", "SansSerif")  //SansSerif is the default`

###Google has made another preference which is called shared preferences
* `SharedPreferences sp = getSharedPreferences("myfile", 0);` 
// data file will be saved in this file the parameter 0 or MODE_PRIVATE means, the file is only accessible between the activities in your project.
* SharedPreference sp = getPreferences(MODE_PRIVATE); 
//SharedPreference is an interface and its using factory method  
//the disadvantage of using this is doesnt save it to a file but instead memory

* `String myFont = sp.getString("Font", "xxx")` 
	* //first param is the name of preference to be return, second param is the value to return if preference does not exists
	* //multiple variation getInteger, getDouble, getLong, etc

<br/>
###Populating data in the sharedPreference file
 `SharedPreferences.Editor ed = sp.edit();`
  `ed.putString("myCar", "Honda")`
  `ed.putInt("myAge", 82);`
  //diff variation i.e. putDouble, putLong
  `ed.commit();`  
  //ed.apply(); this is faster in terms of speed, this is only available only after version 2.7

* `ed.clear` //remove everything from the sharedPreference
* `ed.remove(String key);` //remove specific key in sharedPreference

###Methods for SharedPreferences
* `boolean sp.contain(String key);`
* `Map<String, ?> getAll()`

###Different types of storage
* Internal Storage - disadvantage of using this is it is save in memory so you cannot transfer it
* External Sotrage SD card -
* sqlite 
* Network Connection (Cloud)

###Saving data to to Internal Storage
{% highlight java %}
String str="this line will be saved internally";
FileOutputStream fos = openFileOutput("myFile", Context.MODE_PRIVATE);
													 //.MODE_APPEND
													 //.MODE_WORLD_WRITABLE
fos.write(str.getBytes());
fos.close();
* Make sure to import java.io.*; and import android.content.SharedPreferences;
* Entire i/o classes in java is included in android.
{% endhighlight %}
{% highlight java %}
String str="this line will be saved internally";
FileOutputStream fos = openFileOutput("myFile", Context.MODE_PRIVATE);
													 //.MODE_APPEND
													 //.MODE_WORLD_WRITABLE
PrintWriter pw = new PrintWriter(fos);
fos.close();
{% endhighlight %}


###Side Note
* There are alot of classes in java for i/o but you only need to use two classes `PrintWriter` and `Scanner` 
	*`PrintWriter` - this is for output, can be used for println, print, printf
	*`Scanner` - this is for input
	

###Retrieving data from Internal Storage
{% highlight java %}
FileInputStream fis = openFileInput("myFile")
Scanner sc=new Scanner(fis);
while(sc.hasNext())
{
	String line = sc.nextLine();
	----
	----
}
fis.close();
{% endhighlight %}


###Checking if SD card is available or not
boolean storageAvailable = false;
boolean storageWritable = false;

String state = Environment.getExternalStorage.state();
if(Environment.MEDIA_MOUNTED.equals(state))
{
	storageAvailable = StorageWritable =true;
}
else if (Environment.MEDIA_MOUNTED_READ_ONLY.equals(state))
{
	storageAvailable=true;
	storageWritable=false;
}
else
	storageAvailable=storageWritable=false;
	
###Saving data to External Storage SD Card
* MODE_WORLD_READABLE - make sure this is the param
* You need to make sure if SD card is available or not 

{% highlight java%}
String str="This line will be saved in the SD Card"
try{
	File sdCard = Environment.getExternalStorage.Directory();
	File dir = new File(sdCard, getAbsolutePath() + "/MyFile");
	dir.mkdirs();   //going inside the directory
	File f = new File(dir, "textFile.txt");
	PrintWriter pw = new PrintWriter(f);
	pw.println(str);
	pw.flush();
	pw.close();
}catch (IOException e) {  .....  }

###Load data from SD Card
{% highlight java %}
File sdCard = Environment.getExternalStorageDirectory();
File dir = new File(sdCard.getAbsolutPath() + "/MyFile");
File f = new File(dir, "textFile.txt");  //you cannot use scanner directly here bec. Scanner assumes you are going to parse it
Scanner sc = new Scanner(f);
while(sc.hasNext())
{
	String line = sc.nextLine();  
	----
	----
}
{% endhighlight %}




	




