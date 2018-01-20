# Widgets

### _What is an App Widget?_
- They are main apps that live on the Home screen
- They are usually an extension of an existing app already installed on your device
- They display uptodate information from the app they belong to.

### _Creating your first App Widget_

To add a Widget to your application, you need to add 3 things:
- First, a MyWidgetProvider class that extends AppWidgetProvider
- WidgetProviderInfo.xml which represents the Widget's metadata 
- WidgetLayout.xml that describes the Widget's views

### _Creating your first App Widget_

- Widget communicates with the App using broadcast messages
- The MyWidgetProvider, WidgetProviderInfo.xml and WidgetLayout.xml make the Widget a BroadcastReceiver
- We will need to register it in the Manifest using the <receiver/> type
- Android Studio will help us to create all these files with a few clicks

### _Creating your first App Widget_

- Right click on your **app** module > New > Widget > App Widget
- Give it a name: MyWidgetProvider and keep the other defaults
- And these files are created: MyWidgetProvider class and my_widget_provider_info.xml
- The receiver type is automatically added to the Manifest
- The Layout is profiled-in with a TextView
- Run your app and you'll see the Widget

### _Customizing your App Widget_
 
- Widget Layouts are based on [RemoteViews](https://developer.android.com/reference/android/widget/RemoteViews.html) because the Widget is treated as a second app that runs on the Home screen
- Not every view type can be added in a [Widget Layout](https://developer.android.com/guide/topics/appwidgets/index.html#CreatingLayout)
- RemoteViews have special methods other then **findViewById**. It has methods like setTextViewText(viewId, text) etc.

### _Customizing your App Widget_

- Open up my_widget.xml layout file
- Replace the default TextView by an ImageView and add a drawable to its src
- Open up my_widget_info.xml in the xml folder

``` 
<appwidget-provider xmlns:android="http://schemas.android.com/apk/res/android"
	android:initialLayout="@layout/my_widget"
	android:minHeight="40dp"
	android:minWidth="40dp"
	android:previewImage="drawable/launcher_icon"
	android:resizeMode="horizontal|vertical"
	android:updatePeriodMillis="1800000"
	android:widgetCategory="home_screen" />
``` 
- The smallest update is 30 min = 1800000 Milliseconds

### _Customizing your App Widget_

- Open up MyWidgetProvider class
- The onUpdate method is called every time a new Widget is created, as well as every update involved which is reset to 30 min
- The user can have a multiple instances of the same widget on the Home Screen
- AppWidgetManager gives access to information about all existing widgets on the Home screen
- If you run this app, you can see that the Widget option is now available
- You can drag it over to your Home screen 

### _Click events_

- The way RemoteView handle Click events is different from Normal Views
- RemoteViews can be linked to a PendingIntent and launch once that view is clicked

``` 
views.setOnClickPendingIntent(R.id.image.pendingIntent)
``` 
### _What is a pending Intent_

- The pending intent is just a wrapper on the intent that allows other applications to have access and run that intent in your application

- To launch our app, we need to create an intent for the MainActivity class and then wrap it with a Pending Intent and then call setOnClickPendingIntent

```java
static void updateAppWidget(Context context, AppWidgetManager appWidgetManager, int appWidgetId) {

        // Construct the RemoteViews object
        RemoteViews views = new RemoteViews(context.getPackageName(), R.layout.recipe_widget_provider);

        // Create an Intent to launch the MainActivity when clicked
        Intent intent = new Intent(context, MainActivity.class);
        PendingIntent pendingIntent = PendingIntent.getActivity(context, 0, intent, 0);

        views.setOnClickPendingIntent(R.id.widget_cake_image, pendingIntent);

        // Instruct the widget manager to update the widget
        appWidgetManager.updateAppWidget(appWidgetId, views);
    }
``` 


