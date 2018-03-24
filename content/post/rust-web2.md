+++
date = "2017-10-28T18:32:47-04:00"
title = "Thoughts on Rust Web-Development"
math = false
tags = ["rust", "webdev", "programming"]
highlight = true
draft = false

[header]
  caption = ""
  image = ""

+++

This entry will be a little scattershot, just some miscellaneous thoughts and impressions.

## Interacting with APIs
It's a little inconvenient to be making a lot of little structs for each feature of an API; where in JavaScript or Python you would have a dictionary, if you want type safety you need to make a struct with the proper fields. Strictly speaking you don't *need* to; operating on [untyped JSON](https://docs.serde.rs/serde_json/#operating-on-untyped-json-values) is actually fairly straightforward, but when you know that the rest of your codebase isn't going to crash it's hard to .  The [serde-rs](https://github.com/serde-rs/json) library is everything I want in a JSON serialization library. There's not much in particular to call out, which is good in an infrastructure library; although I can say it's ... much more ergonomic than the serialization I wrote for [Hairy Little Buggers](http://www.hairylittlebuggers.com).

I wrote about half a dozen little programs to scrape data from YouTube and Twitch as part of [secret project](http://www.this link doesn't go anywhere), as well as the main app; I would probably do one-off scraping type scripts in Python in future, with app code written in Rust.

## Type Safety!
Rust's type safety gives is extremely hard to give up; working with Python I constantly have
