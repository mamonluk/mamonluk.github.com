---
layout: post
title: Android Week#9 Mar15, 2012
---

{{ page.title }}
================

<p class="meta">15 Mar 2012 - San Francisco</p>

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
* `SharedPreferences.Editor ed = sp.edit();`

  `ed.putString("myCar", "Honda")`
  `ed.putInt("myAge", 82);`
  //diff variation i.e. putDouble, putLong
  `ed.commit();`  //ed.apply(); this is faster in terms of speed, this is only available only after version 2.7


