---
layout: post
title:      "Ithkuil Dictionary CLI"
date:       2020-01-20 03:33:11 +0000
permalink:  ithkuil_dictionary_cli
---


For the life of me, I cannot remember when or how I first heard of the Ithkuil language.  Languages - be they spoken, signed or programming - have long been a fascination of mine, and the idea of a language built to be as dense as Ithkuil is turned out to be a source of great delight.  I mean, just look at it!

http://ithkuil.net/00_intro.html

So rather than try and build something that would deal with the entire breadth of the grammar, I stuck with dealing with the basic roots and all of their associated stems.

## Root
Each root represents an overall concept and contains a total of six *patterns*, with each pattern containing three *stems*.  In my code, this is represented by having a Root object that contains a value (i.e. the actual sounds in the root), translation, and our six patterns.  The value and translation are accessible directly via attr_accessors, while the patterns and stems have custom readers and writers so I can get an individual item or the entire collection.

Now there's one issue with this: the actual lexicon page does not supply the patterns and stems for every single root.  Instead, most of the roots are derived from some other root, bearing their own values and translations but having the same patterns/stems as another root.  Solution?  Create a BasicRoot class, having its own patterns, and a DerivedRoot class, which references a BasicRoot and gets its patterns from there.

The Scraper class creates these objects by first loading the lexicon page (http://www.ithkuil.net/lexicon.htm), then iterating over it twice; once for <table> nodes (which contain basic roots), and once for <p> nodes that match a regex.  By doing this, I create all my BasicRoots at once, then find all the roots that need to associate with those BasicRoots.

## Patterns
The patterns within each root have some of the same issues as the roots themselves - many are listed as "same as above patterns but referring to (some aspect, context or some such)."  Thus, they have a similar solution - have a BasicPattern, containing grammatical information and an array of three stems (with custom readers for flexibility in use), and a DerivedPattern, associated with a BasicPattern but whose reader functions attach a suffix onto each stem to show the appropriate change (for example, a BasicPattern stem "conscious desire based on need/lack/goal" becomes a DerivedPattern stem "conscious desire based on need/lack/goal, referring to the feeling of desire").

Since I only create new Pattern objects when making BasicRoots, I can use the configuration of the table nodes to determine if a given pattern is a BasicPattern or a DerivedPattern.

## Searching
Most of the search logic is actually handled by Root class methods, rather than the CLI or Scraper.  Searching by value is done strictly by "==", since one would normally know exactly what to look for if you're looking up something by phonetic value.

Searching by translation, meanwhile, returns a hash containing roots whose translations are exactly equal to your search parameter, roots with a stem equal to your search parameter, roots with translations that *contain* the search parameter, and roots with stems that contain the parameter.
