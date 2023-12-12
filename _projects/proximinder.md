---
title: "Proximinder"
excerpt: Android app, remind user when in radius of location
---

[GitHub Repository](https://github.com/jacobrolph98/proximinder)

Rationale 
=========== 

My first experience with Android development was for my dissertation, which entailed eliciting requirements from a client, designing an app, with researching and learning how to use Android Studio. 
The app was relatively simple, displaying (local dummy) content data in list form for the user to select, save on their google account and checkout using PayPal API. 
I found learning Android development pleasant, as the documentation was informative and digestible, encouraging common or best conventions.
I undertook this project to expand on this experience and apply hindsight in a new use case. 
I decided on this feature set as it felt genuinely useful, expands on content display & Google sign in from my last project, and will use location API for a new experience. 
Although I found apps with similar functionality, this would be an apt learning experience nonetheless, and I included an extra requirement for pre-emptive alerts given a destination as a feature I couldn't find in other apps. 


Summary
============

The functional requirements of this app are as follows;

  * Let user create, edit & delete Reminders
  * Reminders require a longitude, latitude, radius, a note and an optional name of place
  * Store Reminders locally 
  * Trigger notification containing reminder note when device location is within a radius

Some extra requirements I would like to add include;

* Dark/Light mode toggle
* Let user sign into google account & save Reminders on cloud 
* Calculate path from given position, and inform user of all Reminders that would be triggered in advance

This application uses a single activity with a fragment for each different page. Creating reminders can use the same fragment as editing them, as the fragment can start with arguments of existing data.
I make use of an open-source API called Leku location picker, for settings long-latitude coordinates. It includes a google maps API which lets the user search for locations and place a pin. 

The opening page shows the list of existing reminders (if any), with a floating action button to create more and with a menu at the top right, for signing in and accessing settings.
Where a name is given, that is used instead of the long/lat coordinates.

![Proximinder list of Reminders](/assets/projects/proximinder/list.png)


![Proximinder new Reminder](/assets/projects/proximinder/new.png)

The user is taken to the prior screen for writing to reminders.
When the user chooses to set location, an API call is made to Leku for selecting a location. This returns a handful of data, though I only store longitude and latitude. Though in the future, some of this data could be used to auto-fill location names. 

![Proximinder pick location](/assets/projects/proximinder/location.png)

The user can tap on any of the bundles in the list to open the write menu, where fields can be filled and saved with the same workflow as creating, but there will be an option to delete. When pressed, a pop up will ask for confirmation. When deleted, the user is returned to the starting page. 


![Proximinder delete Reminder](/assets/projects/proximinder/delete.png)

Although the Google drive API had changed since my dissertation, I've completed the sign in process and the cloud save functionality is well underway. 


![Proximinder sign in](/assets/projects/proximinder/account.png)

The settings menu makes use of android preferences, where I can simply define settings in an XML document to be referenced by fragments. 

I am still yet to implement proximity detection, which will be done using GeoFence API.