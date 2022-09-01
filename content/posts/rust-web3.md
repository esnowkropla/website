+++
date = "2018-09-01T13:32:47-04:00"
title = "A Rust Webserver"
math = false
tags = ["rust", "python", "webdev", "programming"]
highlight = true
draft = true

[header]
  caption = ""
  image = ""

+++

The plan here is to examine a simple webserver that echos back the contents of requests as HTML. The purpose is to compare how you would do this in Rust vs Python, and what static typing and lifetimes and all the things Rust gives you feel from a programmer ergonomics standpoint compared to the equivalent in Python.  This is just some example toy code, and obviously not spec compliant*, but hopefully it illustrates some of the things that I think about programming languages.

*You should read [the spec](https://tools.ietf.org/html/rfc2616#section-5.1) though! It's actually pretty straightforward, and learning things properly is a good thing.
## Server Structure

The basic binary code is just [this](https://github.com/esnowkropla/rusty-web/blob/master/src/bin/main.rs):
```rust
extern crate rusty_web;

use rusty_web::server;

fn main() {
    server();
}
```

which loads the function `server` from our library and kicks it off.  The `server` function looks like [this](https://github.com/esnowkropla/rusty-web/blob/master/src/lib.rs):

```rust
fn handle_connection(mut stream: TcpStream) {
    let mut buff = String::new();
    stream.read_to_string(&mut buff).unwrap();
    let request = Request::new(&buff);
    println!("{:?}", buff);
    println!("{:?}", request);
}

pub fn server() {
    let listener = TcpListener::bind("127.0.0.1:8000").unwrap();

    for stream in listener.incoming() {
        handle_connection(stream.unwrap());
    }
}
```
to make this example a little less trivial, let's give the server two behaviours rather than one: If a GET request comes in, we'll respond with an HTML version of the request, and if a POST request comes in, we'll respond with a JSON object containing the form data.
