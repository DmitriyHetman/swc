swc
===
swc is a small Wayland compositor implemented as a library.

It has been designed primary with tiling window managers in mind. Additionally,
notable features include:

* Easy to follow code base
* XWayland support
* Can place borders around windows

Dependencies
------------
* wayland
* libdrm
* libevdev
* libxkbcommon
* pixman
* [wld](http://github.com/michaelforney/wld)
* linux\[>=3.12\] (for EVIOCREVOKE)

For input hotplugging support, the following is also required:
* libudev

For XWayland support, the following are also required:
* libxcb
* xcb-util-wm

Implementing a window manager using swc
---------------------------------------
You must implement two callback functions, `new_window` and `new_screen`, which
get called when a new window or screen is created. In `new_window`, you should
allocate your own window window structure, and register a listener for the
window's event signal. More information can be found in `swc.h`.

```C
void new_window(struct swc_window * window)
{
    /* TODO: Implement */
}

void new_screen(struct swc_screen * screen)
{
    /* TODO: Implement */
}
```

Create a `struct swc_manager` containing pointers to these functions.

```C
const struct swc_manager manager = { &new_window, &new_screen };
```

In your startup code, you must create a Wayland display.

```C
display = wl_display_create();
```

Then call `swc_initialize`.

```C
swc_initialize(display, NULL, manager);
```

Finally, run the main event loop.

```C
wl_display_run(display);
```

Why not write a Weston shell plugin?
------------------------------------
In my opinion the goals of Weston and swc are rather orthogonal. Weston seeks to
demonstrate many of the features possible with the Wayland protocol, with
various types of backends and shells supported, while swc aims to provide only
what's necessary to get windows displayed on the screen.

I've seen several people look at Wayland, and ask "How can I implement a tiling
window manager using the Wayland protocol?", only to be turned off by the
response "Write a weston shell plugin". Hopefully it is less intimidating to
implement a window manager using swc.

TODO
----
* XWayland copy-paste integration
* Better multi-screen support, including mirroring and screen arrangement
* Mouse button bindings
* Resizing/moving windows by dragging
* Touchpad support

