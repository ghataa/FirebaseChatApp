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

2.2 Also define the following dimens (in res/values/dimen.xml):

```xml
<dimen name="activity_vertical_margin">16dp</dimen>
<dimen name="activity_horizontal_margin">16dp</dimen>
```

2.3 And string resource (res/values/strings.xml):
```xml
<string name="input_text_hint">Input</string>
```

2.4 Finally add this gradle dependency (to app/build.gradle file) and sync project:

```groovy
dependencies {
    implementation 'com.android.support:design:27.1.1'
}
```

2.5 TODO send icon
