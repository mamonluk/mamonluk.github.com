---
layout: post
title: Android Week#11 Apr12, 2012
---

{{ page.title }}
================

<p class="meta">12 Apr 2012 - San Francisco</p>

###Side Note
Scanner class has nice method called "skip" which allows you to skip over 0 or more lines to avoid lines with the supplied regex

runtime class lets you "push" automatically.

-----
###SD Card
sd cards were originally designed for handling music,movies,animation,photos etc.
there is a utility is the sdk calld: mksdcard

this is for manually creating the sdcard.

first create a directory and put the sd card into it.
       cd /android              (this is where you sdk is)
       mkdir sdcard
       mksdcard 512M /android/sdcard/sdcard.img

( ^ size (no less than 8 mb! can be Ko r M, no (G!)), path (include .img                           extension!),

`emulator -avd em22 -sdcard /android/sdcard/sdcard.img`

* to push into it:
`adb push /tmp/myfile /mnt/sdcard/myfile`
(android version less than 2.2 there was no /mnt/ )

* what is inside the sdcard?
dcim    (subdirectory: digitial camera image directory)
standard directory. all manufacturers of cameras for devices, should follow this. 
This directory is in the root of your sdcard!
it has one or more subdirectories, and their names should follow certain rules:
it should be 8 characters long, 123ABCDE (first 3 characters should be numeric, followed by 5 uppercase letters)
some manufacturers have created a directory called Camera (this is bad!) in the emulator there is a directory instead dcim called 100ANDRO. There is no method in java telling us where camera pictures are. :(
              
* google has some remedies, but they are not 100% elegent.
 - use this method when working with SDCard:
<% highlight java %>
 File Environment.getExternalStorageDirectory()
- directories for certain data, use these constants:
 DIRECTORY_ALARMS (audio alarm files go here)
 DIRECTORY_DCIM (all camera pictures go here)
 DIRECTORY_DOWNLOADS (items user has dled)
 DIRECTORY_MOVIES (movie files)
 DIRECTORY_MUSIC (all music files)
 DIRECTORY_NOTIFICATIONS (audio files for notifcations)
 DIRECTORY_PICTURES (all image files, inside DCIM)
 DIRECTORY_PODCASTS (all radioshows etc)
 DIRECTORY_RINGTONES (ring tones for the phone)
<% endhighlight %>
 example:

`File f = new File(Environment.DIRECTORY_MUSIC);` - will give you complete path
 
* these do not exisit on emulator! only actual device! on emulator                                      use these directories:
 mnt/sdcard/Alarms
 mnt/sdcard/DCIM
 mnt/sdcard/Download (not plural!)
 mnt/sdcard/Movies
 mnt/sdcard/Music
 mnt/sdcard/Notifications
 mnt/sdcard/Pictures
 mnt/sdcard/Podcasts
 mnt/sdcard/Ringtones

* Now to locate the directories in your program:
`File f = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_MOVIES);`

* when adding a file to the sdcard (v1.6 and up) very likely youll get error message saying you cannot write into sdcard. you need to add a permission to the AndroidManifest.xml permission : add to the last part of the xml file:
`<user-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />`

* there are many permissions like this, that you will need to use in the future. abbas adds all of them (7-8 of them) into the AndroidManifest file, whether he uses them or not. just put them in there to be safe. one after the other.

<user-permission android:name="android.permission.INTERNET" />

<user-permission android:name="android.permission.READ_CONTACTS"/>

<user-permission android:name="android.permission.WRITE_CONTACTS"/>

<user-permission android:name="android.permission.READ_CALENDAR"/>

<user-permission android:name="android.permission.WRITE_CALENDAR"/>

<user-permission android:name="android.permission.RECORD_AUDIO"/>

<user-permission android:name="android.permission.BATTERY_STATS"/>

<user-permission android:name="android.permission.BLUETOOTH"/>

* the best place to put these in the AndroidManifest.xml file is under the closing application tag
</application>
<put permissions here>
	
###SQLite 
* Has two different mode name `shell mode` and `command line`
* SQlite commands start with `.` i.e. `.help` `.tables` `.separator` `.exit`
* How to start SQLite3 `sqlite3`
* Lets create a database `sqlite3 mydb` this will go to interactive
`sqlite> create table test(id integer primary key auto increment, name text);`
`sqlite> insert into test (name) values ('Alex');`
`sqlite> insert into test (name) values ('John');`
* make sure you add auto increment if you want the primary key to be automatically incremented

* or you can do this too
`sqlite> insert into test (name) values ('Alex'),('John');`

###Configuration settings of SQLite
* `.mode column`
* `.headers on`    //put the headers on, the name of each column
* `.show`   //gives you internal settings of sqlite
* `.prompt sqlite>>` //will change the default prompt to sqlite>>

* Example SQL Query
* `select * from test;`
* `select count(*) from test;`
* last_insert_rowid()` - remember do not use this to get the number of rows in your table, cause the primary key will be random unless you do auto increment

* How to create csv file(csv file will put output with , as separator)
`.output file.csv`
`.separator ,`
`select * from test;`
`.output stdout`
//the example output will be `1,Alex, 2,John,3,David`

`.output file.csv`
`.mode csv`
`select * from test;`
`.output stdout`

* How to transfer the table
`create table test2(id integer primary key, name test);`
`.import file.csv test2`
`sqlite3 mydb .dump > test.sql`




