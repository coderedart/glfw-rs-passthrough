## WARNING
README might be wrong because i left it as is for now. I eventually plan to abandon this library and revert back to original piston's `glfw-rs`, once glfw releases stable version `3.4` with the mouse passthrough patch.


<!--
    Copyright 2013-2014 The GLFW-RS Developers. For a full listing of the authors,
    refer to the AUTHORS file at the top-level directory of this distribution.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
-->

# glfw-rs

[![Crates.io](https://img.shields.io/crates/v/glfw)](https://crates.io/crates/glfw)
[![Docs.rs](https://docs.rs/glfw/badge.svg)](https://docs.rs/glfw)
[![Build Status](https://travis-ci.org/PistonDevelopers/glfw-rs.svg?branch=master)](https://travis-ci.org/PistonDevelopers/glfw-rs)

GLFW bindings and wrapper for The Rust Programming Language.

## Example

~~~rust
use glfw_passthrough as glfw;

use glfw::{Action, Context, Key};

fn main() {
    let mut glfw = glfw::init(glfw::FAIL_ON_ERRORS).unwrap();

    let (mut window, events) = glfw.create_window(300, 300, "Hello this is window", glfw::WindowMode::Windowed)
        .expect("Failed to create GLFW window.");

    window.set_key_polling(true);
    window.make_current();

    while !window.should_close() {
        glfw.poll_events();
        for (_, event) in glfw::flush_messages(&events) {
            handle_window_event(&mut window, event);
        }
    }
}

fn handle_window_event(window: &mut glfw::Window, event: glfw::WindowEvent) {
    match event {
        glfw::WindowEvent::Key(Key::Escape, _, Action::Press, _) => {
            window.set_should_close(true)
        }
        _ => {}
    }
}
~~~

## Using glfw-rs

### Prerequisites

Make sure you have [compiled and installed GLFW 3.x](http://www.glfw.org/docs/latest/compile.html).
You might be able to find it on your package manager, for example on OS X:
`brew install glfw3` (you may need to run `brew tap homebrew/versions`).
If not you can download and build the library
[from the source](http://www.glfw.org/docs/latest/compile.html) supplied on the
GLFW website. Note that if you compile GLFW with CMake on Linux, you will have
to supply the `-DCMAKE_C_FLAGS=-fPIC` argument. You may install GLFW to your
`PATH`, otherwise you will have to specify the directory containing the library
binaries when you call `make` or `make lib`:

~~~
GLFW_LIB_DIR=path/to/glfw/lib/directory make
~~~

### Including glfw-rs-passthorugh in your project

Add this to your `Cargo.toml`:

~~~toml
[dependencies.glfw-passthrough]
version = "*"
~~~

#### On Windows

By default, `glfw-rs-passthrough` will try to compile the `glfw` library. If you want to link to your custom
build of `glfw` or if the build doesn't work (which is probably the case on Windows), you can
disable this:

~~~toml
[dependencies.glfw-passthrough]
git = "https://github.com/coderedart/glfw-rs-passthrough.git"
default-features = false
~~~

