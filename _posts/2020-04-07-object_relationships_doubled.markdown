---
layout: post
title:      "Object Relationships, Doubled"
date:       2020-04-07 23:09:02 +0000
permalink:  object_relationships_doubled
---

## Or, How Can A Model Have Many of Something It Already Belongs To?

Consider the following common scenario:  You are making a blogging app that allows users to create posts and to comment on others' posts.  Your app needs to be able to track a user's posts and comments, and needs to be able to keep track of which posts a user has commented on.  Similarly, a post has an author and many comments, but you would also like to be able to keep track of who has commented on a post.

Naturally, this suggests a many-to-one relationship between posts and users, respectively.  It also suggests a many-to one relationship between comments and posts, as well as between comments and users.  So far so obvious.  But your specifications also suggest a many-to-many relationship between posts and users through their comments.  The trouble is, you've already defined the relationship between posts and users as many-to-one.  So how do we establish a second relationship on top of the one you've already established?

One option would be to simply create methods in your Post and User models.  Something like:
```
class Post < ApplicationRecord
  # other code here
	def comment_users
	  self.comments.map { |comment| comment.user }
  end
```

But that doesn't seem very Rails-like, does it?  And indeed, by using a method like this, you are depriving yourself of the chance to use all the magic that Rails puts to use on associations, not the least of which is being able to use nested attributes to work with related objects via `fields_for`, using nested routes, etc.

To get around this, one might add an additional association with User by calling `has_many :users, through: :comments`.  But even assuming that Rails lets you do this after already having called `belongs_to: user`, this would be ripe for confusion down the line - how would I necessarily know in a week's time what the difference between `post.user` and `post.users` is?

What Rails does let us do is define a relationship with a custom name, by using the `source` argument to state which model we are creating a custom-named relationship with.  Thus, we could define our many-to-many relationship by calling, for example, `has_many :comment_authors, through: :comments, source: :user`.  Later, when we call, say, `post.comment_authors`, we will get back a full-fledged association between our post and all the users who have commented on the post, exactly as if we had called a simple `has_many` macro but without the risk of confusion once we start using the method throughout our app.
