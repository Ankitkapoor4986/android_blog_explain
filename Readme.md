Android Main Building Blocks

Android  system was made with the goal  to make an operating system which runs on many many different devices.

An android app consist of xml and java where an xml is mostly like a helper to set the layout,and store the strings in saperate xml file but the main xml in Android is menifest.xml which holds the permissions which are needed by App and declaration information of main building blocks

In Android Java gets compiled into .class files which are parsed and converted into .dex (Dalvik Executable Code) which gets combined with xml and forms .apk file.This .apk file runs into Dalvik
Virtual Machine.

Note: Google Introduced Dalvik because of 2 reasons
1.DVM is more efficient in terms of saving the battery
2.There will be no cost of licencing the DVM which was there when we hve to use JVM.


Following is the basic structure of Android App

https://raw.githubusercontent.com/Ankitkapoor4986/android_blog_explain/master/workspace.png


The image is explained below

src->main->java->There is a source code.

Src->main->res->Contains all xml resources.

Src->main->res->layout->The xmls in this folder is used to make the layout of the screen .

Src->main->res->menu->Folder contains the optional menu of Android Activity.

Src->main->res->mipmap*->Contains the icon of the App.

Src->main->res->values->string.xml->Contains the String Constants of the application.

Src->main->res->values->dimens.xml->Contains the dimention for the activity

Src->main->res->AndroidMenifest.xml->This xml holds the permissions and list of all the main building blocks


Now coming to the  main building blocks of Android system

Activity ->Can be reffered to as a screen in the operating system.

Service ->This is used to run the background task .They mostly run in main thread however there are special type of services which run in different thread.

BroadCast Receivers ->These are the receivers which receive on different events i.e they can act like a callback to different events e.g system starting up

Application Objects ->This is the main object which holds the application level values .

Content Providers ->These are the building blocks which are used to share the data between the Apps e.g list of songs gets shared between the VLC player and Android default music player.This is possible because of Content Providers only

All the main building blocks have some specified methods which are run by the Android system and  all the building blocks have to be listed in AndroidManifest.xml


Internally android system uses Observer pattern

Some more concepts in Android is explained below

Intent->Intent are objects which acts as a glue between different components.Through intent we can pass the control from one compoment to another

Now here is the detailed discription of the main building blocks


Activity->

This is conceded as a screen in Android world which have some specified methods which are run by the android system.

Main methods of an activity are

void onCreate(Bundle savedInstanceState)

This method is called once in the lifecycle of an activity which is the initialization method.

In it's arguements Bundle is passed .This thing is used when we rotate the screen and layout is changed from landscape to potrait or from potrait to landscape.


void onResume()

onResume is called  whenever the Activity is resumed after initialization.

The main difference between onCreate and onResume is  that onCreate is used for initialization and  onResume is called whenever Activity is resumed where resuming is done after initialization or after going to some other activity and then coming back.

e.g suppose we  initialize one  Activity then we initialize another now if we call the same activity again then onResume will be called

another example is

We call one activity then press the home key and again we call that activity then onResume will be called.

void onStart()

This is called just before onResume.For doing anything which needs to be done before onResume and not in onCreate.

void onStop()

This method is called whenever a we are moving form the current activity.Whether it is after pressing the home key or we move from one activity to another via a App itself.

void onPause()

This is called just before the onStop method.

onDestroy()

This method is used to free the memory .
This is called when we press the back button and doing so close the App.

https://raw.githubusercontent.com/Ankitkapoor4986/android_blog_explain/master/activity_lifecycle.png


Following is the example of a  simple activity

https://raw.githubusercontent.com/Ankitkapoor4986/android_blog_explain/master/activity_code.png

https://raw.githubusercontent.com/Ankitkapoor4986/android_blog_explain/master/activity_code_cont.png

The above code is explained as below

24 setContentView(R.layout.new_activity_layout);
The above line is the main line which sets the layout of the Activity and link it to the activity basically this line sets the contents from xml to code

25 textView = (EditText) findViewById(R.id.edit_text_id);
The above line is used to get any view component's reference in the code  


26 ButtonClickListener<MainActivity> buttonClickListenerBackButton =
        new ButtonClickListener<>(textView, this, MainActivity.class);
28 findViewById(new_activity_back_button).setOnClickListener (buttonClickListenerBackButton);
 From line 26 to 28 we are initalizing a button onClick listener and setting it

 41 String text = getIntent().getStringExtra(Constants.TEXT_VAL);

 In above line getIntent() method gets the intent which calls the Activity.Through getStringExtra we get the value which is passed through intent
