# FirebaseChatApp
This is an example project for a simple Firebase/Android app.

The project based on this article: https://code.tutsplus.com/tutorials/how-to-create-an-android-chat-app-using-firebase--cms-27397

![alt text](https://github.com/ghataa/FirebaseChatApp/blob/master/final.png "ScreenShot")

## 1. Start a new, empty Android Studio project with an empty Activity

Application name: FirebaseChatApp

Company domain: pte.example.firebasechatapp

Package name: pte.example.firebasechatapp

Select Phone and Tablet with target API 21 (Lollipop)

## 2. Define layout for MainActivity (activity_main.xml)

2.1

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin">

    <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentEnd="true"
        android:clickable="true"
        android:tint="@android:color/white"
        app:fabSize="mini" />

    <android.support.design.widget.TextInputLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentStart="true"
        android:layout_toLeftOf="@id/fab">

        <EditText
            android:id="@+id/input"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:hint="@string/input_text_hint" />
    </android.support.design.widget.TextInputLayout>

    <ListView
        android:id="@+id/list_of_messages"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_above="@id/fab"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        android:layout_marginBottom="16dp"
        android:divider="@android:color/transparent"
        android:dividerHeight="16dp" />

</RelativeLayout>
```

2.2 Also define the following dimens (in res/values/dimen.xml)

```xml
<dimen name="activity_vertical_margin">16dp</dimen>
<dimen name="activity_horizontal_margin">16dp</dimen>
```

2.3 And string resource (res/values/strings.xml)
```xml
<string name="input_text_hint">Input</string>
```

2.4 Finally add this gradle dependency (to app/build.gradle file) and sync project

```groovy
dependencies {
    implementation 'com.android.support:design:27.1.1'
}
```

2.5 We also need an icon to the send button. Download it from the following url and add it to the project as vector asset

https://material.io/icons/

Now you can use it for the FloatingActionButton

```xml
android:src="@drawable/ic_send_white_24px"
```

Now build your project and check the app!

## 3. Create model class and layout for a chat message

3.1 Add a new package with name of "model" and create a class for firebase chat message

```java
// Firebase model
public class ChatMessage {

    private String messageText;
    private String messageUser;
    private long messageTime;

    public ChatMessage(String messageText, String messageUser) {
        this.messageText = messageText;
        this.messageUser = messageUser;

        // Initialize to current time
        messageTime = new Date().getTime();
    }

    public ChatMessage() {
        // default constructor for Firebase
    }

    public String getMessageText() {
        return messageText;
    }

    public void setMessageText(String messageText) {
        this.messageText = messageText;
    }

    public String getMessageUser() {
        return messageUser;
    }

    public void setMessageUser(String messageUser) {
        this.messageUser = messageUser;
    }

    public long getMessageTime() {
        return messageTime;
    }

    public void setMessageTime(long messageTime) {
        this.messageTime = messageTime;
    }
}
```
3.2 Create a layout for the message (message.xml)

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/message_user"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"
        android:textSize="@dimen/text_small"
        android:textStyle="normal|bold" />

    <TextView
        android:id="@+id/message_time"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignBottom="@+id/message_user"
        android:layout_alignParentEnd="true"
        android:textSize="@dimen/text_small" />

    <TextView
        android:id="@+id/message_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentStart="true"
        android:layout_below="@+id/message_user"
        android:layout_marginTop="5dp"
        android:textAppearance="@style/TextAppearance.AppCompat.Body1"
        android:textSize="@dimen/text_normal" />

</RelativeLayout>
```
3.3 Also add the predefined text sizes to the dimen.xml file

```xml
<dimen name="text_normal">18sp</dimen>
<dimen name="text_small">14sp</dimen>
```

## 4. Add backend functionality

4.1 Add Firebase google-services.json file to your project (app/)

4.2 Use google-services gradle plugin for Firebase, so add this line to your project-level build.gradle (/build.gradle)

```groovy
buildscript {
  dependencies {
    // Add this line
    classpath 'com.google.gms:google-services:3.2.0'
  }
}
```

4.3 Also modify your app-level build.gradle (/app/build.gradle), finally sync your project

```groovy
dependencies {
  // Add this line
  implementation 'com.google.firebase:firebase-core:12.0.1'
}
...
// Add to the bottom of the file
apply plugin: 'com.google.gms.google-services'
```

## 5. Handle user authentication

5.1 We'll use the pre-created sign in screen by firebase, so add this dependency to your app/build.gradle

```groovy
implementation 'com.firebaseui:firebase-ui:3.3.0'
```

5.2 Add the following snippet to the onCreate() method of MainActivity

```java
if (FirebaseAuth.getInstance().getCurrentUser() == null) {
    // Start sign in/sign up activity
    startActivityForResult(
            AuthUI.getInstance()
                    .createSignInIntentBuilder()
                    .build(),
            SIGN_IN_REQUEST_CODE);
} else {
    // User is already signed in. Therefore, display a welcome Toast
    Toast.makeText(this,
            getString(R.string.welcome_toast,
                    FirebaseAuth.getInstance().getCurrentUser().getDisplayName()),
            Toast.LENGTH_LONG).show();

    displayChatMessages();
}
```

5.3 Define a unique integer value for starting the pre-created sign-in screen

```java
private static final int SIGN_IN_REQUEST_CODE = 1212;
```

5.4 Also add the toast message to string resources

```xml
<string name="welcome_toast">Welcome %s</string>
```

5.5 And define the displayChatMessages() method (empty for now)

```java
private void displayChatMessages() {
    
}
```

5.6 Finally handle the end of sing-in process in the onActivityResult() method and also add the toast messages to string resources

```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);

    if (requestCode == SIGN_IN_REQUEST_CODE) {
        if (resultCode == RESULT_OK) {
            Toast.makeText(this,
                    R.string.sign_in_success_toast,
                    Toast.LENGTH_LONG).show();
            displayChatMessages();
        } else {
            Toast.makeText(this,
                    R.string.sign_in_failed_toast,
                    Toast.LENGTH_LONG).show();

            // Close the app
            finish();
        }
    }
}
```

```xml
<string name="sign_in_success_toast">Successfully signed in. Welcome!</string>
<string name="sign_in_failed_toast">We could not sign you in. Please try again later.</string>
```

Build your project now and try the sign-in/sign-up process! :)

## 6. Post and display the messages

6.1 To post a message, add an onClickListener() to the FloatingActionButton (in the onCreate() method)

```java
FloatingActionButton fab = findViewById(R.id.fab);

fab.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        EditText input = findViewById(R.id.input);

        // Read the input field and push a new instance
        // of ChatMessage to the Firebase database
        FirebaseDatabase.getInstance()
                .getReference()
                .push()
                .setValue(new ChatMessage(input.getText().toString(),
                        FirebaseAuth.getInstance().getCurrentUser().getDisplayName())
                );

        // Clear the input
        input.setText("");
    }
});
```

6.2 To display the messages, we'll use a ListView and the FirebaseUI's class, the FirebaseListAdapter.

```java
private FirebaseListAdapter<ChatMessage> adapter;
```

6.3 Now paste this code snippet into the displayChatMessages() method

```java
ListView listOfMessages = findViewById(R.id.list_of_messages);

FirebaseListOptions<ChatMessage> options = new FirebaseListOptions.Builder<ChatMessage>()
        .setLayout(R.layout.message)
        .setQuery(FirebaseDatabase.getInstance().getReference(), ChatMessage.class)
        .setLifecycleOwner(this)
        .build();

adapter = new FirebaseListAdapter<ChatMessage>(options) {
    @Override
    protected void populateView(View view, ChatMessage model, int position) {
        TextView messageText = view.findViewById(R.id.message_text);
        TextView messageUser = view.findViewById(R.id.message_user);
        TextView messageTime = view.findViewById(R.id.message_time);

        messageText.setText(model.getMessageText());
        messageUser.setText(model.getMessageUser());

        messageTime.setText(DateFormat.format("dd-MM-yyyy (HH:mm:ss)",
                model.getMessageTime()));
    }
};

listOfMessages.setAdapter(adapter);
```
