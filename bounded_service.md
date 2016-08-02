Bounded Service
------

Another major type of service in BoundedService which is used get the data back to the Activity

Following are the important methods of this type of service

**void onBind()**

The method returns the Ibinder interface .Called when the Activity calls the bindService method.

**void onUnbind()**

This method is called when we call unbindService

**void onCreate()**

Method called when service is first created.

**void onDestroy()**

Method called when the service is destroyed.

Following are the some more important interface/classes whose implementation/extended  instances we would need in order to work with the bounded service

1. ***ServiceConnection***

This interface have methods which are the callback methods of onBind and onUnbind methods of service.

Following are the important methods 

**void onServiceConnected**

This method is called as a callback  method of onBind



**void onServiceDisconnected**

This method is called as a callback method of onUnbind 

2. ***Ibinder***

This act as a glue between the onBind of Service and onServiceConnect where it is passed as a parameter in onService connect and is returned by onBind

Following is the example of BoundedService and it's coresponding activity

```java
public class BoundedService extends Service {

    private static final String TAG = "BoundedService";
    public static final String FROM_BOUNDED_SERVICE_METHOD = "From Bounded Service method";

    private final IBinder boundedServiceBinder = new BoundedServiceBinder(this);

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        Log.d(TAG, "OnBind called");
        return boundedServiceBinder;
    }

    @Override
    public void onCreate() {
        Log.d(TAG, "onCreate ");
        super.onCreate();
    }

    @Override
    public void onRebind(Intent intent) {
        Log.d(TAG, "onRebind ");
        super.onRebind(intent);
    }

    @Override
    public boolean onUnbind(Intent intent) {
        Log.d(TAG, "onUnbind ");
        return super.onUnbind(intent);
    }

    @Override
    public void onDestroy() {
        Log.d(TAG, "onDestroy");
        super.onDestroy();
    }

    public String workToDo() {
        Log.d(TAG, "**************************");
        Log.d(TAG, "workToDo ");
        Log.d(TAG, "we need to do this work");
        Log.d(TAG, "***************************");
        return FROM_BOUNDED_SERVICE_METHOD;

    }


}
```

Following is the code of BoundedServiceBinder

```java

package com.example.com.myapplication.binders;

import android.os.Binder;

import com.example.com.myapplication.service.BoundedService;


public class BoundedServiceBinder extends Binder {
    private final BoundedService service;

    public BoundedServiceBinder(BoundedService boundedService) {
        this.service = boundedService;
    }

    public BoundedService getService() {
        return service;
    }
}
```
Follwoing is the code of BoundedActivity

``` java
public class BoundedActivity extends AppCompatActivity implements ServiceConnection {

    private static String TAG = "BoundedActivity";
    private boolean serviceConnected = false;
    private BoundedServiceBinder boundedServiceBinder;
    private BoundedService boundedService;
    private TextView textFromService;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Log.d(TAG, "onCreate called");
        setContentView(R.layout.bounded_activiy_layout);
        Button backButton = (Button) findViewById(R.id.bounded_activity_back_button);
        ButtonClickListener<ThirdActivity> backButtonListener = new ButtonClickListener<>(null, this, ThirdActivity.class);
        backButton.setOnClickListener(backButtonListener);
        Button fwdButton = (Button) findViewById(R.id.fwd_button_bounded_activity);
        ButtonClickListener<DBActivity> forwardButtonListener = new ButtonClickListener<>(null, this, DBActivity.class);
        fwdButton.setOnClickListener(forwardButtonListener);
        textFromService = (TextView) findViewById(R.id.bounded_activity_from_bounded_service);
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.bounded_service_menu, menu);
        return true;
    }


    @Override
    protected void onResume() {
        super.onResume();
        Log.d(TAG, "OnResume called");
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        Intent bindServiceIntent = new Intent(this, BoundedService.class);
        switch (item.getItemId()) {
            case R.id.bind_service:

                bindService(bindServiceIntent, this, Context.BIND_AUTO_CREATE);

                if (serviceConnected) {
                    textFromService.setText(boundedService.workToDo());
                }
                return true;
            case R.id.unbind_service:

                unbindService(this);
                return true;
        }
        return false;
    }


    @Override
    public void onServiceConnected(ComponentName name, IBinder serviceBinder) {
        Log.d(TAG, "onServiceConnected *************");
        serviceConnected = true;
        boundedServiceBinder = (BoundedServiceBinder) serviceBinder;
        boundedService = boundedServiceBinder.getService();
    }

    @Override
    public void onServiceDisconnected(ComponentName name) {
        serviceConnected = false;

    }


}
```

Actually the code flows like this

1. Activity calls bindService(Intent intent) which goes to onBind(Intent intent) of BoundedService
2. The callback method of onBind is onServiceConnect which comes after implementing the ServiceConnection interface.