Overview
======================
This project is based on OpenTok's [Basic-Video-Chat-Kotlin](https://github.com/opentok/opentok-android-sdk-samples/tree/main/Basic-Video-Chat-Kotlin) Android application.

Changes Made
======================
* Added a connect/disconnect button to join or leave an OpenTok session
* Added a button to switch a camera source
* Added a toggle button to turn the publisher's audio and video on or off

How to Launch the App
======================
Open the project folder in Android Studio. Set a session credentials in the config file and run the app. For more details, please visit [Basic-Video-Chat-Java](https://github.com/opentok/opentok-android-sdk-samples/tree/main/Basic-Video-Chat-Java)

App Screenshots
======================
<img width="366" alt="Screenshot 2024-08-20 at 3 37 13 PM" src="https://github.com/user-attachments/assets/24b06b2c-f032-46e3-b00d-8e4efa9a2f9e">

Added Code and Layout Details
======================  
## **Connect button functionality**  
The requestPermissions method, which was originally called in the onCreate method, has been commented out. Instead, it is now called within the connect function. This change ensures that permissions are requested when the user clicks the "Connect" button, rather than during the activity's creation.
```
// in MainActivity.java
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        publisherViewContainer = findViewById(R.id.publisher_container)
        subscriberViewContainer = findViewById(R.id.subscriber_container)
//        requestPermissions()
    }

    fun connect(view: View) {
        Log.i(TAG, "CONNECT CLICKED")
        requestPermissions()
    }
```
## **Disconnect button functionality**  
Stops the publishing and subscribing of video streams, removes their views from the UI, clears references to the publisher and subscriber, and finally disconnects the session.
```
// in MainActivity.java
    fun disconnect(view: View) {
        Log.i(TAG, "DISCONNECT CLICKED")
        publisher?.let { pub ->
            session?.unpublish(pub)
            publisherViewContainer.removeView(pub.view)
            publisher = null
        }
        subscriber?.let { sub ->
            session?.unsubscribe(sub)
            subscriberViewContainer.removeView(sub.view)
            subscriber = null
        }
        session?.disconnect()
    }
```
## **Cycle camera button functionality**  
Switch the camera.
```
// in MainActivity.java
    fun cycleCamera(view: View) {
        Log.i(TAG, "CYCLE CAMERA CLICKED")
        publisher?.cycleCamera()
    }
```
## **Publisher's audio toggle button functionality**  
Turn the publisher’s audio on or off with a button click. Change the button text to reflect the current state.
```
// in MainActivity.java
    fun pubAudioToggle(view: View) {
        Log.i(TAG, "PUBLISHER AUDIO TOGGLE CLICKED")
        val pubAudioToggle: Button = findViewById(R.id.pubAudioToggle)
        publisher?.let {
            if (it.publishAudio) {
                it.publishAudio = false
                pubAudioToggle.text = "PUBLISHER AUDIO OFF"
            } else {
                it.publishAudio = true
                pubAudioToggle.text = "PUBLISHER AUDIO ON"
            }
        }
    }
```
## **Publisher's video toggle button functionality**  
Toggling the publisher's video publishing state on and off, updating the button text to reflect the current state.
```
// in MainActivity.java
    fun pubVideoToggle(view: View) {
        Log.i(TAG, "PUBLISHER VIDEO TOGGLE CLICKED")
        val pubVideoToggle: Button = findViewById(R.id.pubVideoToggle)
        publisher?.let {
            if (it.publishVideo) {
                it.publishVideo = false
                pubVideoToggle.text = "PUBLISHER VIDEO OFF"
            } else {
                it.publishVideo = true
                pubVideoToggle.text = "PUBLISHER VIDEO ON"
            }
        }
    }
```
## **Button layout**  
Defines five buttons in the layout with an 8dp margin and specific constraints for positioning.
```
// in activity_main.xml
    <Button
        android:id="@+id/connect"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="8dp"
        android:onClick="connect"
        android:text="@string/connect"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/disconnect"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="8dp"
        android:onClick="disconnect"
        android:text="@string/disconnect"
        app:layout_constraintStart_toEndOf="@+id/connect"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/cycleCamera"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="8dp"
        android:onClick="cycleCamera"
        android:text="@string/cycleCamera"
        app:layout_constraintStart_toEndOf="@+id/disconnect"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/pubAudioToggle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="8dp"
        android:onClick="pubAudioToggle"
        android:text="@string/pubAudioToggle"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/connect" />

    <Button
        android:id="@+id/pubVideoToggle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="8dp"
        android:onClick="pubVideoToggle"
        android:text="@string/pubVideoToggle"
        app:layout_constraintStart_toEndOf="@+id/pubAudioToggle"
        app:layout_constraintTop_toBottomOf="@+id/disconnect" />
```
