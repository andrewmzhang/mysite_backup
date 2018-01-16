---
layout: page
title:  MSJ International Trip App 
description: Documentation and log of how this app works
date:   2017-5-13 11:31:00 -0800
---

The primary purpose of this app is to provide an modern, easy to use interface
for students and staff to check up on and update itenerary events. 

Tools involved will mainly include the Firebase DB suite. 

An undedicated server will be hosted on Heroku for possible 3rd party API functionality. 
However, most server functionality will be provided by Firebase Cloud Functions including
database hooks, notifications, and possibly admin access. 

It is currently unknown if the client will be designed natively or in IonicJS. I will need
to check to see if IonicJS will properly support Firebase API.

As for the team, I intend to recruit a few Mission kids to help me expand the
project. Although I would like to first provide a proof of concept. 


## Primary Functionality

### Events

Events should include the following information:

 1. uid : (String) unique identification 
 2. timestamp: (Long) time at which the event was created
 3. title: (String) title of the event
 4. desc: (String) description of the event
 5. time: (Long) posix time in millis of when this event occurs
 6. duration: (Long) time in millis of how long this event lasts. 
 7. timezone: (Long?) timezone offset of this event
 8. type: (Long) integer designation of what kind of event this is. Default is 0. 
 9. groupRelevancy: integer designation of who this event should pertain to. 
 10. Some sort of weather data, I would like to add this. 
 11. Some sort of geolocation data, I would like to add this. 



