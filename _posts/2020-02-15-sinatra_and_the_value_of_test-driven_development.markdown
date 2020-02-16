---
layout: post
title:      "Sinatra and the value of test-driven development"
date:       2020-02-16 01:20:25 +0000
permalink:  sinatra_and_the_value_of_test-driven_development
---


So for my Sinatra portfolio project, I opted to do something a bit simpler and more of a minimum-viable-project than my CLI project; namely, I built out the first half of a commerce website that allows producers to list goods with prices and inventory, and would eventually allow consumers to browse and purchase those goods.  So far, my site allows for user authentication, associates items to producers and allows full support for CRUD operations.

Now I could have spent my time building out my project to include the consumer side, or perhaps include a keyword-based search system (which I still might, time permitting).  But I opted for a different approach, which technically goes outside of the project requirements but I feel is going to be vital going forward as a developer.

I chose to write all my own tests for my code using Rspec and Capybara.

I did this because I have found myself making extensive use of the tests provided to us in the labs, in order to quickly check that my code is working and give me explicit goals to shoot for.  By checking my code against the tests frequently, I know that everything is working as intended, and - presuming the tests are structured well - I can quickly isolate problems with my code, knowing that if all the tests up to the one that failed passed, the problem must specifically reside in the point that the test purports to check.

Of course, that's the catch - the tests themselves have to be set up correctly.  You need to have confidence that the tests themselves are not broken, and that they cover all of the different things that the code can be made to do.  Tests need to be able to access the various aspects of the code and correctly read and write values as appropriate - easier said than done when dealing with an MVC framework and you need to check both underlying code and user experience.  Just as importantly, tests need to account for every possible scenario so that you know you are handling them correctly.

To help in that process, I began using a pattern called "red, green, refactor."  As I understand it, this consists of a cycle of three steps:

1. Red: Write your tests and ensure that they are working correctly.  In other words, the errors they through should be because your code legitimately isn't passing the tests, rather than being broken by themselves. In this step, you think about the things you want your code to do, and think of various things that might pop up that your code needs to gracefully handle.
2. Green: Write your code so that it passes the tests.  Don't worry about it being pretty at this point; just get it working.
3. Refactor: Now that you know, roughly, how to make your tests pass, refactor your code to adhere to DRY, OOP and all the other best-practices that you may have ignored in step 2, making sure that you still pass your tests.  At this point, go back to step 1 with other functionality you may have realized you need to include.

By doing this, I made sure that the only things that are likely to go wrong when actually using the code are either because there's some difference between test and production environments that I missed, or because I didn't think of a thing that my tests needed to cover.  I *shouldn't* need to be concerned that something that I *know* should work one way won't work that way, because I have already confirmed that it does.
