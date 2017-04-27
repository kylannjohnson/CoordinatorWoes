# Keyboard inset not cleared on > API 25
This repo is only intended to showcase a potential bug regarding the combination of CoordinatorLayout, AppBarLayout, and a custom App ToolBar on Nougat devices (and above)

## The Potential Bug
When disabling the standard ActionBar in the app theme via `<item name="windowActionBar">false</item>`, the system will not honor the IME settings for
an Activity marked as `android:windowSoftInputMode="adjustResize|stateHidden"` when recalling and app from the background (e.g. after home button press)

### To Reproduce
1. After starting the app, bring the EditText to focus on the bottom of the screen and expose the keyboard
2. With the keyboard visible, press the Home Button
3. Relaunch the app
4. See the inset made by the System where the keyboard should be

## To work around the issue
1. Un-comment the lines in styles.xml to hide the windowActionBar and not show a windowTitle
2. Optional: uncomment out the Toolbar in activity_main.xml to show the "custom" toolbar

## Files of Interest
There are 2 files of note in this repo [styles.xml](app/src/main/res/values/styles.xml) and [activity_main.xml](app/src/main/res/layout/activity_main.xml)

### The Activity Layout
The layout for MainActivity is meant to replicate a "container based layout where an app would add its Views to a `<FrameLayout>` in order to make it the content of a `<DrawerLayout>`
This is represented by `MainActivity`.  This repo has a simple CoordinatorLayout to mimic a UI that should be resized when the keyboard is on-screen

### styles.xml
Here we find the toggle for `windowActionBar` and `windowNoTitle`. We set these to `false` and `true`, respectively, to turn off the standard ActionBar to replace it with our custom
ToolBar.  You can find a representation of that ToolBar in the activity layout as a standard Support ToolBar (which is what we extend).  

If the window ActionBar is used, then the bug shows. If the custom one is used (and the window ActionBar turned off), we see the expected behavior.
