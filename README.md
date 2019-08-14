# TeaTime Code

This is a exercise repository for the TeaTime example app which is part of Udacity's Advanced Android course. TeaTime is a mock tea ordering app that demonstrates various uses of the Espresso Testing framework (i.e. Views, AdapterViews, Intents, IdlingResources). You can learn more about how to use this repository [here](https://classroom.udacity.com/courses/ud857/lessons/8b2a9d63-0ff5-48ff-90d3-a9855b701dae/concepts/41b82e3c-2797-46e5-8a66-684098ca8cbb)

## Some Important Links:

## Idling Resource

[this](https://github.com/udacity/AdvancedAndroid_TeaTime/blob/TESP.03-Solution-AddOrderSummaryActivityTest/app/src/androidTest/java/com/example/android/teatime/OrderSummaryActivityTest.java) is a very simple implementation of the IdlingResource interface

Remember that if idle is false there are pending operations in the background and any testing operations should be paused. If idle is true all is clear and testing operations can continue.

Implementing the IdlingResource interface is straight forward: it requires completing the 3 required methods. We also created an instance of AtomicBoolean in order to control idleness across multiple threads.

We also declare a private variable called mIdlingResource of type SimpleIdlingResource. Notice that it has an annotation @Nullable which indicates that this variable will be null in production. This is because this setup with IdlingResource is only used for testing, so when the project is run in production, IdlingResource can be null.

#### Summary

When the changeTextBt is clicked, onClick() in MainActivity triggers MessageDelayer.processMessage().

processMessage() sets the IdlingResource to false, then creates a Handler which contains a Runnable that will be run after a pre-determined time delay, DELAY_MILLIS. The Runnable that will be executed after the delay:

1) Returns the String entered by the user via a callback to the calling activity (e.g. MainActivity)

2) Sets the IdlingResource to true

[class](https://classroom.udacity.com/courses/ud855/lessons/f0084cc7-2cbc-4b8e-8644-375e8c927167/concepts/1449718b-df48-4789-a152-4e52f0093006)

<img width="437" alt="screen-shot-2017-03-09-at-2 27 40-pm" src="https://user-images.githubusercontent.com/48018942/63033351-1c159a00-bed5-11e9-9a4d-e9bfb457b314.png">

Implement the IdlingResource interface (SimpleIdlingResource.java)

Create a callback interface (MessageDelayer.java) where the actual asynchronous task will occur

Set the state of IdlingResource to false when the task is running, and then back to true when the task is done

Have the delayer notify the activity that the process is complete via a callback (MainActivity.onDone)

