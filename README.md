# TeaTime Code

This is a exercise repository for the TeaTime example app which is part of Udacity's Advanced Android course. TeaTime is a mock tea ordering app that demonstrates various uses of the Espresso Testing framework (i.e. Views, AdapterViews, Intents, IdlingResources). You can learn more about how to use this repository [here](https://classroom.udacity.com/courses/ud857/lessons/8b2a9d63-0ff5-48ff-90d3-a9855b701dae/concepts/41b82e3c-2797-46e5-8a66-684098ca8cbb)

## Some Important Links:

## Idling Resource

[this](https://github.com/udacity/AdvancedAndroid_TeaTime/blob/TESP.03-Solution-AddOrderSummaryActivityTest/app/src/androidTest/java/com/example/android/teatime/OrderSummaryActivityTest.java) is a very simple implementation of the IdlingResource interface

Remember that if idle is false there are pending operations in the background and any testing operations should be paused. If idle is true all is clear and testing operations can continue.

Implementing the IdlingResource interface is straight forward: it requires completing the 3 required methods. We also created an instance of AtomicBoolean in order to control idleness across multiple threads.

We also declare a private variable called mIdlingResource of type SimpleIdlingResource. Notice that it has an annotation @Nullable which indicates that this variable will be null in production. This is because this setup with IdlingResource is only used for testing, so when the project is run in production, IdlingResource can be null.
