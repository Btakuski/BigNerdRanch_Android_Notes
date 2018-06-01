# Big Nerd Ranch Programming - Android 


##### Widgets

* Widgets are the building blocks you use to compose a UI. A widget can show text or graphics, interact with the user, or arrange other widgets on the screen. Buttons, text input controls, and checkboxes are all types of widgets.


* Widgets exist in a hierarchy of `View` objects called the _view hierarchy_


* Every widget is an instance of the `View` class or one of its subclasses


* A `ViewGroup` is a widget that contains and arranges other widgets.


* When a widget is contained by a `ViewGroup`, that widget is said to be a _child_ of the `ViewGroup`  


* Wiring up widgets is a two-step process:
  1. Get references to the inflated `View` objects
  2. Set listeners on those objects to respond to user actions

##### Widget Types

* `LinearLayout`
  * When you want widgets arranged in a single column or row

* `FrameLayout`
  * The simplest `ViewGroup` that does not arrange its children in any particular manner. 
  * Child views are arranged according to their `android:layout_gravity` attribute 

##### XML Elements

* Each element has a set of XML _attributes_. Each _attribute_ is an instruction about how the widget should be configured


* `android:layout_width`
  `android:layout_height`
  * `match_parent` : view will be as big as its parent
  * `wrap_content` : view will be as big as its contents require


* `android:padding`
  * Tells the widget to add the specified amount of space to its contents when determining its size
  * `dp`: density-independent pixels


* `android:orientation`
  * Determines whether their children will appear vertically or horizontally


* `android:text`
  * Tells the widget what text to display
  * _string resources_: A string that lives in a separate XML file called a _strings file_


* `android:id`
  * Generates a resource ID 
  * If there is a `+` sign in the values for `android:id`, this is because you are _creating_ the IDs and only _referencing_ the strings 

##### From Layout XML to View Objects 

* The `onCreate(Bundle)` method is called when an instance of the activity subclass is created. 


* When an activity is created, it needs a UI to manage. To get the activity its UI, you call the following method:
  * `public void setContentView(int LayoutResID)`
  * This method _inflates_ a layout and puts it on screen.
  * When a layout is inflated, each widget in the layout file is instantiated as defined by its attributes 
  * You specify which layout to inflate by passing in the layout's resource ID 

##### Resources and resource IDs

* A `layout` is a _resource_
* A _resource_ is a piece of your application that is not code - things like image files, audio files, and XML files. 
* To access a resource in code, you use its _resourceID_


##### Setting Listeners
* Android applications are typically _event driven_. Event-driven applications start and then wait for an event, such as the user pressing a button. 
* When your application is waiting for a specific event, we say it is "listening for" that event. The object that you create to respond to an event is called a _listener_, and the _listener_ implements a _listener interface_ for that event. 
* This listener is implemented as an _anonymous inner class_
  * It puts the implementation of the listeners' methods right where they are being used
  
##### Making Toasts
* A _toast_ is a short message that informs the user of something but does not require any input or action
  * `Toast.makeText(Context context, int resId, int duration)`

---

## Activity Lifecycle

* Every instance of `Activity` has a lifecycle
* During this lifecycle, an activity transitions between four states: resumed, paused, stopped, and nonexistent. 
  * Nonexistent -> `onCreate()` -> Stopped -> `onStart()` -> Paused -> `onResume()`-> Resumed
  * Resumed -> `onPause()` -> Paused -> `onStop()` -> Stopped -> `onDestroy` -> Nonexistent
* For each transition, there is an `Activity` method that notifies the activity of the change in its state.

| State        | In Memory? | Visible to user? |  In Foreground?  
| ------------- |:-------------:| -----:| ---------:|
| nonexistent  | no | no | no |
| stopped |  yes | no | no |
| paused | yes | yes/partially   |  no |
| resumed | yes |  yes | yes  

* Only one activity across all the apps on the device can be in the resumed state at any given time

___

#### Using Logcat
* Logcat: A log viewer included in the Android SDK tools
* Use "Edit Filter Configuration" in Logcat to filter out messages related to appropriate Activity

#### Device Configuration
* The _device configuration_ is a set of characteristics that describe the current state of an individual device. 
* The characteristics that make up the configuration include:
  * screen orientation
  * screen density
  * screen size
  * keyboard type
  * dock mode
  * language

* When a _runtime configuration change_ occurs, there may be resources that are a better match for the new configuration. So Android destroys the activity, looks for resources that are the best fit for the new configuration, and then rebuilds a new instance of the acitivity with those resources

#### Saving Data Across Rotation
* One way to save data across a runtime configuration change, like rotation, is to override the `Activity` method:
  * `protected void onSaveInstanceState(Bundle outState)`
    * The default implementation of onSaveInstanceState(Bundle) directs all of the activity's views to save their data in the `Bundle` object. 
    * A `Bundle` is a structure that maps string keys to values of certain limited types.
    * When you override `onCreate(Bundle)`, you call `onCreate(Bundle)` on the activity's superclass and pass in the bundle you just received. 
    * In the superclass implementation, the saved state of the views is retrieved and used to re-create the activity's view hierarchy. 
  * You can override `onSaveInstanceState(Bundle)` to save additional data to the bundle and then read that data back in `onCreate(Bundle)`. 
  * The types that you can save to and restore from a `Bundle` are primitive data types and classes that implement the `Serializable` or `Parcelable` interfaces. 
  * It is usually bad practice to put objects of custom types into a `Bundle`, however, because the data might be stale when you get it back out.

___

## The Activity Lifecycle, Revisited
* An activity can be destroyed by the OS if the user navigates away fror a while and Android needs to reclaim memory
* The OS will not reclaim a visible(paused or resumed) activity. Activities are not marked as "killable" until `onStop()` is called and finishes executing.
* How does the data you stash in `onSaveInstanceState(Bundle)` survive the activity's death?
  * When`onSaveInstanceState(Bundle)` is called, the data is saved to the `Bundle` object. That `Bundle` object is then stuffed into your activity's _activity record_ by the OS



