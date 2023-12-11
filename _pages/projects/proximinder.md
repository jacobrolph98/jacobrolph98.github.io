---
layout: post
title: "Proximinder"
description: Android app, remind user when in radius of location
---

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
  * Reminders require a longitude, latitude, radius and a note
  * Store Reminders locally 
  * Trigger notification containing reminder note when device location is within a radius

Some extra requirements I would like to add include;

* Let user sign into google account & save Reminders on cloud 
* Calculate path from given position, and inform user of all Reminders that would be triggered in advance