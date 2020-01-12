---
layout: post
title:      "Yield - Ruby vs. Python"
date:       2020-01-12 02:57:56 +0000
permalink:  yield_-_ruby_vs_python
---


(Note: This post represents my current understanding of the topic as a student, and should not be solely relied on for anything important.  If anything here is inaccurate, please let me know and I'll correct the content here.)

At first glance, it might seem that the `yield` keyword is basically the same between Ruby and Python, since they are both used in iteration.  However, there are some key differences that relate to how the two languages actually handle iteration and interaction between functions.  The way I think of it is that **Ruby yields *to,* Python yields *up.***

### Ruby `yield`s *to* a block

In Ruby, when you use the `yield` keyword in a method, what you're saying is that you are expecting there to be a block passed to the method - in other words, when someone calls your method, they should be following it with `do`.  You use `yield` just like a method call - for instance, you could place `new_value = yield(old_value)` in your method to indicate that `old_value` should be passed as an argument to a block, then the return value of that block stored as `new_value`.

For example, I could make something like this:
```
class Song
  attr_accessor :name, :artist, :genre
  @@all = []
  # other code here

  def self.print_review_scores
    all.each do |song|
      score = yield(song)
      puts "#{song.name} has a review score of #{score}."
    end
  end
end
```
What this means is that I am passing `self` as an argument to whatever block gets passed to #print_review_score.  Imagine, then that I had a method that assigned review scores like this:

```
def snobby_review(song)
  song.genre.name == 'classical' ? 10 : 1
end
```

I could then make this method: 

```
def print_snobby_reviews
  Song.print_review_scores do |song|
	  snobby_review(song)
  end
end
```

...and because `yield` hands control over to whatever is in the passed-in block, it would work exactly the same as if I had written my original method as:

```
def self.print_review_scores
  all.each do |song|
    score = snobby_review(song)
    puts "#{song.name} has a review score of #{score}."
  end
end
```


### Python `yield`s *up* a value

The same class listed above would be written in Python as follows:

```
class Song:
    all = []
    # __init__ and getters/setters go here
    @classmethod
		def print_review_scores(cls, revew_generator):
        for review in review_generator(cls.all):
            print(f"{review['songname']} has a review score of {review['score']}")
```

So where's `yield`?  Well, in Python, `yield` works a lot more like a `return` statement, except that rather than returning a single value, it returns the next item in whatever iteration it happens to be running.  Thus, the methods above would be implemented like this:

```
def snobby_review(song):
    if song.genre.name == "classical":
		    return 10
		else:
		    return 0
				
def generate_snobby_reviews(songs):
    for song in songs:
		    yield snobby_review(song)
```

Thus, each time `generate_snobby_reviews` is called in `print_review_scores`, it returns the next value in the array that was passed to it.
