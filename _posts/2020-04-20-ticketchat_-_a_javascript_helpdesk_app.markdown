---
layout: post
title:      "TicketChat - a JavaScript helpdesk app"
date:       2020-04-20 04:07:15 +0000
permalink:  ticketchat_-_a_javascript_helpdesk_app
---


In continuing with the theme of my Rails project, my JavaScript project is a simple chatroom-style app that displays helpdesk tickets with their associated comments, associating them to a specific user.

I wound up using a grand total of six JS files in my application.  These are:

* index.js - Places a DOMContentLoaded event listener whose job is to actually initialize the DOM and get a login.  The DOM is initializd in...
* base_dom.js - Contains a static method which set up the nav bar and ticket container objects on the DOM.  Also contains a couple of helper methods, the most important of which is BaseDOM.htmlToElement, which converts a given template string to an Element object which can be inserted via Element.appendChild and similar.  This is important because editing the innerHTML of an object directly can disable any event listeners already placed on that element's children.
* users.js - The first of our model files, this contains the JavaScript constructor for an individual user, but also has static methods for rendering the login form and dealing with the backend's login/logout/current_user routes.  The most important static method here is check_login, which gets the current user from the Rails session: if present, it calls the appropriate function for rendering all ticket objects (see below); if absent, it clears the ticket container and displays a login form as a modal.
* tickets.js - As users.js, contains our constructor for individual tickets, as well as static methods for rendering all tickets, rendering the new ticket form and submitting the POST request to create a new ticket from that form.
* comments.js - As tickets.js for a ticket's comments.  This gets called from within tickets.js to render each comment and associate a comment submission form to the ticket's DOM object.
* api.js - Contains all helper methods to communicate with the backend, including wrappers for post and get requests.  These are written as async functions, which implicitly return a Promise - in other words, simply a different syntax from calling .then() multiple times on a fetch request.
