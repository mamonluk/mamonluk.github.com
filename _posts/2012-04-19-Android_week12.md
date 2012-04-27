---
layout: post
title: Android Week#12 Apr12, 2012
---

{{ page.title }}
================

<p class="meta">19 Apr 2012 - San Francisco</p>

###Homework
Modify the State Capitals game to:

Create a database, Populate a table with the state and capital information
use a select statement to determine whether the answer was correct or not
Due by the end of the semester, though there may be additional extra credit homework available.

* Using SQLite – Setup and creating a Database

{% highlight java %}
package whatever.com

import android.content.*;
import android.database.*;
import android.database.sqlite.*;

public class Demo extends SQLiteOpenHelper
{
  public static final String DBNAME = "mydb";
  static final String NAME = "name", AGE = "age";

  public Demo(Context c)
  {
    super(c, DBNAME, null, 1);	// 3rd arg is object of SQLite factory, 4th is version # of database
  }

  public void onCreate(SQLiteDatabase db)
  {
    String cmd = "create table info ( _id int primary key auto increment, " +
    "name text, age int );";
    db.execSQL(cmd);		// execSQL() is good for SQL commands that do not return a value 

  }

  @Override
  public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
  {
    db.execSQL("drop table if exists info");
    onCreate(db);	// re-creates the table.
  }

  SQLiteDatabase db;
  db = openOrCreateDatabase("mydb", SQLiteDatabase.CREATE_IF_NECESSARY, null);
  db.setVersion(1);		// since the version number in the create statement, above, was null
  int dbVersion = db.getVersion();

  // when making changes to the database, need to lock it so that other threads cannot make
  // changes at the same time.
  db.setLockEnabled(true);
}
{% endhighlight %}

###Inserting Data
The insert() method returns the record number, and –1 if the insert failed.  This can be compared to 0 to determine if the insert succeeded.  InsertOrThrow can be used instead to throw an Exception on fail.
{% highlight java %}
ContentValues cv = new ContentValues();
cv.put(NAME, "John");
cv.put(AGE, 19);
db.insert("info", null, cv);	// arg1 is name of table, arg2 is useless

cv.put(NAME, "Abbas");
cv.put(AGE, 82);
db.insert("info", null, cv); 
{% endhighlight %}

###Getting Data
A cursor is needed to handle the return values of a multiple-line select statement.

{% highlight java %}
String query = "select name, age from info;";
Cursor c = db.rowQuery();
while ( ! c.moveToNext() )
{
  String name = c.getString(0);
  int age = c.getInt(1);

  ….
}

c.close();	// make sure to close the Cursor to avoid creating a memory leak
{% endhighlight %}

###A more efficient way to insert data with many fields
insert() is overloaded to accept another way of inserting data:

`db.insert("info", name, "John");`
`db.insert("info", age, 32);`

It does still return the record id.

###Deleting Records
Google recommends using Integer rather than int to let Java autobox in case it changes in the future.
{% highlight java %}
public void deleteRecord(SQLiteDatabase db, Integer recID)
{
  db.delete("emp", "id = ?", new String[]{ recID.toString()});
}
{% endhighlight %}

###Doing this in an Activity
Since the class can't extend both Activity and SQLiteOpenHelper, a different method is necessary.
{% highlight java %}
public class Demo1 extends Activity
{
  SQLiteDatabase myDB;

  public void onCreate(Bundle b)
  {
    …

    myDB = openOrCreateDatabase("my_sqlite_database.db",
      SQLiteDatabase.CREATE_IF_NECESSARY,
      null);

    myDB.setVersion(1);
    myDB.setLockingEnabled(true);

  }
}
{% endhighlight %}

###The mode for opening a database can be:
  
* CREATE_IF_NECESSARY
* OPEN_READONLY
* OPEN_READWRITE

long getMaximumSize() will return the max size of a database.

SQLiteDatabase.openDatabase(String db, null, int mode)
{
  
}







