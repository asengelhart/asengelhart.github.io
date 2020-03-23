---
layout: post
title:      "Engelhilf - A helpdesk ticketing app using Rails"
date:       2020-03-23 08:44:57 +0000
permalink:  engelhilf_-_a_helpdesk_ticketing_app_using_rails
---


For my Rails portfolio project, I decided to tackle a project I had wanted to do at my previous job: making a working helpdesk application.  The municipal government where I worked prior to starting my Flatiron course used the same web service for tracking both public works work orders and IT tickets, which made our lives difficult as there was no way to assign priority, filter out closed work orders or even create users without a lot of rigmarole with our service provider.  So in memory of those frustrations, I decided to see if I could do better.

Simply put, I was successfully (though somewhat belatedly) put together an app that records tickets, records comments on those tickets, allows ranking of tickets by urgency, and allows an administrator to filter based on urgency and whether a ticket is open or closed.  Users can create their own accounts or log in via Google, and administrators can both create user accounts and create tickets on behalf of users.  So yeah, I'd say I succeeded.

The one feature that I wasn't able to include was a way to automatically email users and/or administrators when a ticket or comment is created.  Rails features built-in mailing functionality but I ran out of time to figure it out.  Hopefully I'll get that up and running soon!
