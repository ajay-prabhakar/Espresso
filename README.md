# TeaTime Code

This is a exercise repository for the TeaTime example app which is part of Udacity's Advanced Android course. TeaTime is a mock tea ordering app that demonstrates various uses of the Espresso Testing framework (i.e. Views, AdapterViews, Intents, IdlingResources). You can learn more about how to use this repository [here](https://classroom.udacity.com/courses/ud857/lessons/8b2a9d63-0ff5-48ff-90d3-a9855b701dae/concepts/41b82e3c-2797-46e5-8a66-684098ca8cbb)

**Android has mainly two types of tests**

##### Unit Tests

Testing every method/function (or unit) of your code for e.g: given a function, calling it with param x should return y. These tests are run on JVM locally without the need of an emulator or Device.

##### Instrumentation Testing

Instrumentation tests are used for testing Android Frameworks such as UI,SharedPreferences and So on.Since they are for Android Framework they are run on a device or an emulator.
Instrumentation tests use a separate apk for the purpose of testing. Thus, every time a test case is run, Android Studio will first install the target apk on which the tests are conducted. After that, Android Studio will install the test apk which contains only test related code.

**What is Espresso?**

Espresso is an instrumentation Testing framework made available by Google for the ease of UI Testing.

##### Setting Up
Go to your app/build.gradle

1.Add dependencies
```
androidTestCompile ‘com.android.support.test.espresso:espresso-core:3.0.1’
androidTestCompile ‘com.android.support.test:runner:1.0.1’
```
2.Add to the same build.gradle file the following line in

```
android.defaultConfig{
testInstrumentationRunner “android.support.test.runner.AndroidJUnitRunner”
}
```

This sets up the Android Instrumentation Runner in our app.

**AndroidJUnitRunner** is the instrumentation runner. This is essentially the entry point into running your entire suite of tests. It controls the test environment, the test apk, and launches all of the tests defined in your test package

And annotate it with `@RunWith(AndroidJUnit4::class)`

The instrumentation runner will process each test class and inspect its annotations. It will determine which class runner is set with @RunWith, initialize it, and use it to run the tests in that class. In Android’s case, the AndroidJUnitRunner explicitly checks if the AndroidJUnit4 class runner is set to allow passing configuration parameters to it.

**Important things to note is that**
The activity will be launched using the `@Rule` before test code begins

By default the rule will be initialised and the activity will be launched(onCreate, onStart, onResume) before running every `@Before` method

Activity will be Destroyed(onPause, onStop, onDestroy) after running the `@After` method which in turn is called after every `@Test` Method

The activity’s launch can be postponed by setting the launchActivity to false in the constructor of ActivityTestRule ,in that case you will have to manually launch the activity before the tests

### Example Code
In the example below we do the testing of a login screen where we search the login and password edit text and enter the values and after that we test the two scenarios Login success and Login Failure

```
import android.support.test.rule.ActivityTestRule
import android.support.test.runner.AndroidJUnit4
import org.junit.runner.RunWith
import android.support.test.espresso.Espresso;
import android.support.test.espresso.action.ViewActions
import android.support.test.espresso.assertion.ViewAssertions.matches
import android.support.test.espresso.matcher.ViewMatchers.*

@RunWith(AndroidJUnit4::class)
class MainActivityInstrumentationTest {

    @Rule
    @JvmField
    public val rule  = ActivityTestRule(MainActivity::class.java)

    private val username_tobe_typed="Chromicle"
    private val correct_password ="password"
    private val wrong_password = "passme123"

    @Test
    fun login_success(){
        Log.e("@Test","Performing login success test")
        Espresso.onView((withId(R.id.user_name)))
                .perform(ViewActions.typeText(username_tobe_typed))

        Espresso.onView(withId(R.id.password))
                .perform(ViewActions.typeText(correct_password))

        Espresso.onView(withId(R.id.login_button))
                .perform(ViewActions.click())

        Espresso.onView(withId(R.id.login_result))
                .check(matches(withText(R.string.login_success)))
    }

    @Test
    fun login_failure(){
        Log.e("@Test","Performing login failure test")
        Espresso.onView((withId(R.id.user_name)))
                .perform(ViewActions.typeText(username_tobe_typed))

        Espresso.onView(withId(R.id.password))
                .perform(ViewActions.typeText(wrong_password))

        Espresso.onView(withId(R.id.login_button))
                .perform(ViewActions.click())

        Espresso.onView(withId(R.id.login_result))
                .check(matches(withText(R.string.login_failed)))
    }
}
```

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

