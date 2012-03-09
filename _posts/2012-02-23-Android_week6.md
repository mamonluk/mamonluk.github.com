---
layout: post
title: Android Week#6 Feb23, 2012
---

{{ page.title }}
================

<p class="meta">23 Feb 2012 - San Francisco</p>

* Radio Button - inherits from a class called compound button  ->(inherits) TextView
* `getCheckRadioButtonId()` - returns the radio button that is push 
	* return -1 when nothing is check
	* usually use for error checking

<br/>
###res/layout/radioButton.xml
{% highlight java %}
<?xml ....?>
<RadioGroup xmlns: android="...."
				   android:orientation="vertical"
				   android:layout_width="fill_parent"
				   android:layout_height="wrap_content"
					>
	<RadioButton android:id="@+id/radio1"
				 android:layout_width="wrap_content"
				 android:layout_height="warp_conetnt"
				 android:text="music1" />
	<RadioButton
				---
				--- />
</RadioGroup>
{% endhighlight %}

<br/>
###sample program
{% highlight java %}
public class DemoRadioButton extends Activity implements RadioGroup.OnCheckedChangeListener
{
	RadioGroup rg1, rg2, rg3;
	public void onCreate(Bundle b)
	{
		rg1=(RadioGroup)findViewById(R.id.radio1);
		rg1=(RadioGroup)findViewById(R.id.radio2);
		rg3=(RadioGroup)findViewById(R.id.radio3);
		rg1.setOnCheckedChangeListener(this);
		rg2.setOnCheckedChangeListener(this);
		rg3.setonCheckedChangeListener(this);
	}
	@Override
	public void onCheckChanged(RadioGroup rg, int checkedId)
	{
		switch(checkedId)
		{
			case R.id.radio1: .......;
			case R.id.radio2: .......;
			case R.id.radio3: .......;
		}
	}
}
{% endhighlight %}

<br/>
* `if(getCheckRadioButtonId==-1) rg1.toggle();`  - if you want radio button to be check by default

###/res/layout/checkbox.xml
{% highlight java %}
<?xml ....?>
<CheckBox xmlns:android="....."
				android:id="@+id/check"
				android:layout_width="wrap_content"
				android:layout_height="wrap_content"
				android:text="This checkbox is unchecked"
				android:textColor="red"
				/>
{% endhighlight %}

###sample code

{% highlight java %}
import android.app.Activity;
import andriod.os.BUndle;
import android.os.widget.CheckBox
public class DemoCheckBox extends Activity implements CompoundButton.OnCheckedChangeListener
{
	CheckBox cv;

		@Override
		public void onCreate(Bundle b)
		{
			super.onCreate(b);
			cb=(CheckBox)findViewById(R.id.check);
			cb=setOnCheckedCHangeLIstener(this);
			//or cb.setOnChecekdChangeLIsterner(new DemoCheckBox());
			
		}
		public void onCheckedChange(CompundButton cb, boolean isChecked)
		{
			if(isChecked)
			   cb.setText("this checkbox is: checked");
			else
			   cb.setText("this checkbox is unchecked"); 
		}
}
{% endhighlight %}

* Methods for CheckBox
	1. boolean isChecked()
	2. void setChecked(boolean)
	3. void toggle();

* Next week Intent, Bundle 
	* Bundle is use for transferring data between activity













