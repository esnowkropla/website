+++
highlight = true
math = false
tags = ["rust", "webdev", "programming"]
date = "2017-08-04T10:43:49-04:00"
title = "Rust Web-Development, Part 1"

[header]
  image = ""
  caption = ""

+++
Now that [Hairy Little Buggers](http://store.steampowered.com/app/405240) is wrapped up, I figured I'd use my sabbatical to dive into web-development. This is also a good chance to play with [Rust](http://rust-lang.org) again; the last time I checked it out was before 1.0 when I was playing with the [Matasano crypto challenges](http://cryptopals.com/), so it will be interesting to see how much more mature the ecosystem is.

Now, I don't really know anything about web-development, so after a little research I picked up [Flask Web Development](https://www.amazon.ca/Flask-Web-Development-Developing-Applications/dp/1449372627/ref=sr_1_sc_1?ie=UTF8&qid=1501866858&sr=8-1-spell) based on the [Flask Mega-Tutorial](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world). I know Python fairly well so I can mentally translate the Python into Rust, and [Flask](http://flask.pocoo.org) seems like the framework I would pick were I to write a web app in Python. Unfortunately for Python, I've become more user-experience focused over the last few years so GC stutters and latency infuriate me. What I really want is to write code in languages without mandatory garbage collection, thus Rust.

## Framework Selection
Much as I love writing everything myself, sometimes it makes sense to stand on the shoulders of giants. A combination of browsing the rust sub-reddit and searching the web suggested five initial candidates for a web framework:
[Nickel](https://github.com/nickel-org/nickel.rs), 
[Iron](https://ironframework.io), 
[Rocket](https://rocket.rs), 
[Rouille](https://github.com/tomaka/rouille), and 
[Pencil](https://github.com/fengsp/pencil). My heartless judging is not meant to speak to the intrinsic qualities of each; probably even the most ignorant of their authors knows more about web stuff than I do. That said, what follows is the process I used to select a framework to play with:

### Rocket
Rocket requires the use of the `nightly` compiler, which annoys me.  A pretty decent fraction of common packages in the Rust ecosystem seemingly still rely on `nightly` rather than `stable`. Using Rust in production is already questionable given it's novelty, and using the `nightly` version of the compiler strikes me as a bridge too far. So Rocket's out.

### Rouille and Pencil
Rouille and Pencil are smaller frameworks, which aligns with my desire to do most of the programming and component selection myself rather than delegating to a monster framework like Rails or Django, so I tried them out next.  Both seem pretty immature though, so after getting my `hello-world` servers up and running it seemed like I would have to build a *lot* myself. At that point the impedance mismatch between my code and the framework makes me want to just write the framework part myself as well. Scratch these two off the list.

### Nickel and Iron
Finally Nickel and Iron seem pretty similar, with more of a batteries-included philosophy then Rouille or Pencil. Iron seems more mature and seems to have more development activity around it, so I picked Iron (sorry Nickel).

I got templating working with [handlebars-iron](https://github.com/sunng87), and was messing around with sessions and forms when I stumbled upon a bug I could not solve. Implementing [Post/Redirect/Get](https://en.wikipedia.org/wiki/Post/Redirect/Get) resulted in something in the stack (I'm not sure if it was [Hyper](https://github.com/hyperium/hyper) or Iron) parsing the incoming GET request as `Method::Extension(user=USERNAME&password=PASSWDGET)`, rather than what I would expect `Method::Get`. This is pretty simple code; after matching on the POST /upload route, issue a redirect to GET /. Iron includes handlers to redirect, and a `url_for` function to find the address of routes. After a couple of days of not being able to find a problem on my end, I gave up on Iron. The next step was to start debugging [Iron](https://ironframework.io) and [Hyper](https://github.com/hyperium/hyper) code, and pretty much the whole point of a framework is to avoid parsing headers yourself.
