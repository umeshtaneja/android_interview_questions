# Mostly Asked Android Interview Questions.

## The list contains latest and trending android interview questions.

![image_android](https://github.com/umeshtaneja/android_interview_questions/assets/22681420/416f169f-78e2-43c1-bda6-484b04b2d627)

---
> Let's get started !! 

# Android Activity and its Life Cycle

An **Activity** in Android represents a single screen with a user interface. It's a crucial component of the Android application architecture, and understanding its lifecycle is essential for proper development and management of Android applications.

## Activity Lifecycle:

1. **onCreate():** This is the first method called when the Activity is created. Here, you initialize essential components of the Activity, such as layout, views, and data. It's followed by `onStart()`.

2. **onStart():** This method is called when the Activity becomes visible to the user but isn't yet in the foreground. At this point, the Activity is visible but may not be interactable by the user. It's followed by `onResume()`.

3. **onResume():** This is called when the Activity is ready to interact with the user. It's the point where the Activity is in the foreground and actively running. This is also where you should start animations, play sounds, and acquire resources that are needed only when the Activity is visible. After `onResume()`, the Activity is in the Resumed state.

4. **onPause():** This method is called when the Activity loses focus but is still visible to the user. This happens when another Activity comes into the foreground or when the device goes to the background. You should perform operations here that are lightweight and can be paused or resumed, such as animations or updates to data. After `onPause()`, the Activity is in the Paused state.

5. **onStop():** This is called when the Activity is no longer visible to the user. It typically happens when another Activity completely covers the screen, or when the user navigates away from the app. You should release resources here that are not needed while the Activity is not visible. After `onStop()`, the Activity is in the Stopped state.

6. **onDestroy():** This is the final method called before the Activity is destroyed. It's called either because the Activity is finishing or the system is temporarily destroying it to save space. You should clean up resources, unregister listeners, and save any data that should persist beyond the lifetime of the Activity.

## Handling Configuration Changes:

During the lifecycle, configuration changes such as screen rotations can occur, leading to the destruction and recreation of the Activity. To handle this properly, you can override `onSaveInstanceState()` to save the Activity's state and restore it in `onCreate()` or `onRestoreInstanceState()`.

# Lifecycle of Fragments in Android

Fragments are reusable portions of UI in an Activity, allowing for modular and flexible UI designs. Understanding the lifecycle of Fragments is crucial for managing resources efficiently, maintaining a responsive user experience, and ensuring proper handling of UI state.

## Fragment Lifecycle:

1. **onAttach():** The Fragment is attached to an Activity. This is where the Fragment establishes a connection to its hosting Activity.

2. **onCreate():** The Fragment is created. Here, you initialize essential components of the Fragment, such as layout, views, and data.

3. **onCreateView():** This method is specifically for inflating the layout of the Fragment's UI. You should inflate and configure your layout here.

4. **onViewCreated():** Called after onCreateView() returns, providing a convenient way to access and interact with the Fragment's views.

5. **onActivityCreated():** This is called after the hosting Activity's onCreate() method has returned. It's a good place to perform any final initialization once the Activity is ready.

6. **onStart():** The Fragment becomes visible to the user.

7. **onResume():** The Fragment is in the foreground and actively running.

8. **onPause():** The Fragment is paused. This typically happens when another Fragment comes into the foreground or when the user navigates away from the Activity.

9. **onStop():** The Fragment is no longer visible to the user.

10. **onDestroyView():** This is called when the view hierarchy associated with the Fragment is being destroyed.

11. **onDestroy():** The Fragment is being destroyed. You should clean up resources, unregister listeners, and save any data that should persist beyond the lifetime of the Fragment.

12. **onDetach():** The Fragment is detached from its hosting Activity.

# Understanding Launch Modes in Android Activities

Launch mode is an instruction for the Android OS which specifies how an activity should be launched. It instructs how any new activity should be associated with the current task. Before delving further, it's essential to understand two crucial topics:

## Tasks

A task is a collection of activities that users interact with when performing a certain job. In general, an application contains a number of activities. Normally, when a user launches an application, a new task will be created, and the first activity instance is called the root of the task.

When a user launches an app from the home icon, it navigates through different screens, with different activities placed on top of one another. This collection of activities is known as tasks.

## Back Stack

Activities are arranged in the order in which each activity is opened. This maintained stack is called the Back Stack. When you start a new activity using `startActivity()`, it "pushes" a new activity onto your task and puts the previous Activity in the back stack.

Once you press the back button, it "pops" the topmost activity, removes it from the back stack, and takes you back to the previous activity.

There are four launch modes for activity in Android, each serving different purposes:

1. **Standard**: 
   - This is the default launch mode for activities. 
   - Each time an activity is started, a new instance of the activity is created and added to the top of the stack.

2. **SingleTop**: 
   - If an instance of the activity already exists at the top of the stack, a new instance will not be created.
   - Instead, the existing instance will receive the new intent through its `onNewIntent()` method.

3. **SingleTask**: 
   - A new task will always be created and a new instance of the activity will be added to the task.
   - If an instance of the activity already exists in a different task, the existing task will be brought to the foreground, and the existing instance of the activity will receive the new intent through its `onNewIntent()` method.

4. **SingleInstance**: 
   - Similar to singleTask, but the system creates a new task for the activity, and no other activities can be part of this task.
   - If another activity starts this activity, the system creates a new task and instance for it, regardless of whether there's an existing instance of the activity elsewhere.

To specify the launch mode for an activity in the AndroidManifest.xml file, you can use the `android:launchMode` attribute inside the `<activity>` element, like so:

```xml
<activity android:name=".YourActivity"
          android:launchMode="[standard | singleTop | singleTask | singleInstance]" />
```

# 1. Launch Mode: Standard

Standard is the default launch mode of an activity if not specified otherwise. It creates a new instance of an activity in the task from which it was started. Multiple instances of the activity can be created and multiple instances can be added to the same or different tasks. In other words, you can create the same activity multiple times in the same task as well as in different tasks.

To declare the standard launch mode for an activity in the AndroidManifest.xml file:

```xml
<activity android:name=".YourActivity"
          android:launchMode="standard" />
```

# Example

Suppose you have activities A, B, C, and D, and your activity B has launch mode set to "standard". Now, if you launch activity B again:

**State of Activity Stack before launching B:**

A -> B -> C -> D

**State of Activity Stack after launching B:**

A -> B -> C -> D -> B

## 2.SingleTop

In this launch mode, if an instance of the activity already exists at the top of the current task, a new instance will not be created, and the Android system will route the intent information through `onNewIntent()`. If an instance is not present on top of the task, then a new instance will be created.

Using this launch mode, you can create multiple instances of the same activity in the same task or in different tasks, only if the same instance does not already exist at the top of the stack.

To declare the singleTop launch mode for an activity in the AndroidManifest.xml file:

```xml
<activity android:name=".YourActivity"
          android:launchMode="singleTop" />
```

# Example

**Case 1:**

Suppose you have activities A, B, and C, and your activity D has launch mode set to "singleTop". Now, if you launch activity D:

**State of Activity Stack before launching D:**

A -> B -> C

**State of Activity Stack after launch D activity:**

A -> B -> C -> D (Here D launch as usual)

**Case 2:**

Suppose you have A, B, C and D activities and your activity D has “launch mode = singleTop”. Now you again launching activity D -

**State of Activity Stack before launch D:**

A -> B -> C -> D

**State of Activity Stack after launch D activity:**

A -> B -> C -> D (Here old instance gets called and intent data route through onNewIntent() callback)

## 3. singleTask

In this launch mode a new task will always be created and a new instance will be pushed to the task as the root one. If an instance of activity exists on the separate task, a new instance will not be created and Android system routes the intent information through onNewIntent() method. At a time only one instance of activity will exist.

```xml
<activity android:launchMode=”singleTask” />
```
# Example

**Case 1:**

Suppose you have A, B and C activities and your activity D has “launch mode = singleTask”. Now you launching activity D -

**State of Activity Stack before launch D:**

A -> B -> C

**State of Activity Stack before launch D:**

A -> B -> C -> D (Here D launch as usual)

**Case 2:**

Suppose you have A, B, C and D activities and your activity B has “launch mode = singleTask”. Now you again launching activity B-

**State of Activity Stack before launch D:**

A -> B -> C -> D

**State of Activity Stack after launch B activity:**

A -> B (Here old instance gets called and intent data route through onNewIntent() callback)

Also notice that C and D activities get destroyed here.

## 4. singleInstance

This is very special launch mode and only used in the applications that has only one activity. It is similar to singleTask except that no other activities will be created in the same task. Any other activity started from here will create in a new task.

```xml
<activity android:launchMode=”singleInstance” />
```
# Example

**Case 1:**

Suppose you have A, B and C activities and your activity D has “launch mode = singleInstance”. Now you launching activity D -

**State of Activity Stack before launch D:**

A -> B -> C

**State of Activity Stack after launch D activity**

Task1 — A -> B -> C

Task2 — D (here D will be in different task)

Now if you continue this and start E and D then Stack will look like-

Task1 — A -> B -> C -> E

Task2 — D

**Case 2:**

Suppose you have A, B, C activities in one task and activity D is in another task with “launch mode = singleInstance”. Now you again launching activity D-

**State of Activity Stack before launch D**

Task1 — A -> B -> C

Task2 — D

**State of Activity Stack after launch B activity**

Task1 — A -> B -> C

Task2 — D (Here old instance gets called and intent data route through onNewIntent() callback)

# Intent Flags

Android also provides Activity flags by which you can change the default behavior of Activity association with Tasks while starting it via `startActivity()` method. These flags values can be passed through Intent extra data.

### FLAG_ACTIVITY_NEW_TASK

This flag works similar to `launchMode = singleTask`.

### FLAG_ACTIVITY_CLEAR_TASK

This flag will cause any existing task that would be associated with the activity to be cleared before the activity is started. The activity becomes the new root of an otherwise empty task, and any old activities are finished.

### FLAG_ACTIVITY_SINGLE_TOP

This flag works similar to `launchMode = singleTop`.

### FLAG_ACTIVITY_CLEAR_TOP

If set, and the activity being launched is already running in the current task, then instead of launching a new instance of that activity, all of the other activities on top of it will be closed, and this Intent will be delivered to the (now on top) old activity as a new Intent.




