<img src="https://squids.sleepless.com/images/image1.png">

# Squids II

The squiddening

Copyright 2012 - 2021 Sleepless Inc. All Rights Reserved  

## Table of contents

[Table of contents](#table-of-contents)

#### [What is Squids?](#what-is-squids-1)

* [Philosophy](#philosophy)

* [Very low barrier to entry](#very-low-barrier-to-entry)

#### [The Squids Dev App](#the-squids-dev-app-1)

* [Mobile version of dev app](#mobile-version-of-dev-app)  

#### [Squids SDK](#squids-sdk-1)

- [App Functions](#app-functions)
  - [`event_handler(type, id, x, y)`](#event_handler-type-id-x-y-)
    + [`SQUIDS_EVENT_REFRESH_DISPLAY`](#squids_event_refresh_display)
    + [`SQUIDS_EVENT_TICK`](#squids_event_tick)
    + [`SQUIDS_EVENT_TOUCH_DOWN`](#squids_event_touch_down)
    + [`SQUIDS_EVENT_TOUCH_UP`](#squids_event_touch_up)
    + [`SQUIDS_EVENT_ACTIVATING`](#squids_event_activating)
    + [`SQUIDS_EVENT_FOCUS_GAINED`](#squids_event_focus_gained)
    + [`SQUIDS_EVENT_FOCUS_LOST`](#squids_event_focus_lost)
    + [`SQUIDS_EVENT_DEACTIVATED`](#squids_event_deactivated)
- [API Functions](#api-functions)
  + [`squids_log(message)`](#squids_logmessage)
  + [`squids_set(key, val)`](#squids_setkey-val)
  + [`squids_get(key)`](#squids_getkey)
  + [`squids_draw_start()`](#squids_draw_start)
  + [`squids_draw_end()`](#squids_draw_end)
  + [`squids_image_load(uri)`](#squids_image_loaduri)
  + [`squids_image_draw(img, src_x, src_y, src_w, src_h, dest_x, dest_y, dest_w, dest_h, pivot_x, pivot_y, rotation, opacity)`](#squids_image_drawimg-src_x-src_y-src_w-src_h-dest_x-dest_y-dest_w-dest_h-pivot_x-pivot_y-rotation-opacity)
  + [`squids_sound_load(uri)`](#squids_sound_loaduri)
  + [`squids_sound_play(sound, volume)`](#squids_sound_playsound-volume)
  + [`squids_sound_stop(sound)`](#squids_sound_stopsound)
  + [`squids_text_load(key)`](#squids_text_loadkey)
  + [`squids_font_load(uri)`](#squids_font_loaduri)
  + [`squids_font_draw(text, x, y, font, opacity)`](#squids_font_drawtext-x-y-font-opacity)
- [Currently supported key values](#currently-supported-key-values)
   + [`"tick_rate"`](#tick_rate)
   + [`"show_keyboard"`](#show_keyboard)
   + [`"show_status_bar"`](#show_status_bat)
   + [`"sleepless"`](#sleepless)
- [Values for "key" currently supported](#values-for-key-currently-supported)
  + [`"dev_code"`](#dev_code)
  + [`"platform"`](#platform)
  + [`"squids_version"`](#squids_version)
  + [`"display_size_pixels"`](#display_size_pixels)

#### [*********** SLEEPLESS INTERNAL NOTES **************](#sleepless-internal-notes)
- [Notes about building the dev app](#notes-about-building-the-dev-app)
  - [SDL](#sdl)
  - [iOS](#ios)
  - [Android](#android)
    
------

## What is Squids?

Squids is a minimalist framework for developing games.  It was built to make apps that basically fall into the category of “2D casual games”, but it could be used for other things as well.

### Philosophy  
These are the primary goals of Squids:  
1. Very low barrier to entry
2. A generic, "virtual platform" device environment
  
### Very low barrier to entry  

This is accomplished by reducing the time it takes, as much as possible, to go from ground-zero, to writing code that actually implements the functions of your game/app.  It should be possible to create a new project and be to the point of writing game code in less than 10 minutes max; Ideally 1 minute or less.

------
## The Squids Dev App

The squids dev app is a pre-built, executable, program.  You can use it to develop your own Lua based Squids apps.

When the Dev App starts up, it prompts you for a "dev code" or a URL.  If you have a registered dev code, you can enter that and it will be translated into whatever root URL you have configured for the code.  Alternatively, you can enter a full URL which will be used directly as the root.   The root is the place where the app expects to find the data files that are included with it.  

The place where the files live is called the "dev root".  When you've entered a dev code and tap "OK", the dev app looks for the file `app.lua` in the dev root.  If found this is your program's starting point; It is loaded and executed.  Typically a simple game would consist of just the `app.lua` file and any graphics (.PNG) and sound (.WAV) files, all in this dev root directory.

### Mobile version of dev app  

The mobile version has 2 special functions:

1. **Force Reset** - To perform a reset, swipe from middle of the screen off the top in the leftmost ¼ of the screen width.  See the description of `squids_reset()`.

<br>

2.  **Log View** - To display the log, swipe from center of screen, off the top in the rightmost ¼ of the screen width.  This is just an alternate view that contains any text that was sent to `squids_log()`.  Tapping anywhere in this window returns you to the main view.

<img src="https://squids.sleepless.com/images/image3.png" style="text-align: center;">

------

## Squids SDK

The squids SDK defines the generic, virtual platform environment.  Your app or game is written to this API only.  The API hides virtually all the platform specific features of the native SDK.  What is gained is complete portability between supported platforms and greatly reduced development times.  What is lost is flexibility.  The API is, of necessity, a dramatically limited set of capabilities.  The virtual platform supports the drawing of graphics on a display, the playing of audio sound samples, and the receiving of events from user input, common app life cycle events, timer events, and other similar things.  The events are abstracted into those that are common to all platforms.

Squids is not intended to be flexible enough to make any app or game.  It is, however, meant to make 2D, casual games very easy to create.

### App Functions  
  
#### `event_handler( type, id, x, y )`  
  
```lua
event_handler( type, id, x, y ) {
    if( type == SQUIDS_EVENT_TICK ) {
        
    }
}
```  
  
The only function that needs to be defined by your game code.  Your code is driven entirely by events being fed into this function.  The following event types are defined:
  
#### `SQUIDS_EVENT_REFRESH_DISPLAY`  
  
Send when the system thinks you should redraw your display.  It should be honored, though for most games, a rapid tick rate makes doing so pretty pointless.

#### `SQUIDS_EVENT_TICK`  
  
This is the main event type for a typical game.  You tell Squids how often you'd like to receive these events, and then you can use those to drive your game.  You should set this to the rate at which you'd like your game "logic" to run, not necessarily your screen refresh rate.  Since squids apps are in control of screen rendering directly, they can (and probably should) choose to run the logic at full speed, but only draw the display as fast as they can, without causing the logic to slow down.

The id parameter is a tick count that will forever increase.  It will always start at 0 or higher, but don't assume that 0 will be the first number you see.  Every tick event will be a unique, increasing integer until a "reset".  
  
#### `SQUIDS_EVENT_RESIZE`

Sent when the window changes size.  The "id" parameter is currently always 0.  The x and y parameter are the new display size width and height.   
  
#### `SQUIDS_EVENT_TOUCH_DOWN`  
  
Sent when a finger is lifted from the screen.  The "id" parameter is currently not used.  The x and y parameters are the location where touch occurred.

#### `SQUIDS_EVENT_TOUCH_UP`

Sent when a finger is lifted from the screen. The "id" parameter is currently not used. The x and y parameters are the location where touch occurred.

#### `SQUIDS_EVENT_ACTIVATING`

Sent when the app is beginning to come back to life after having been sent to the background.

#### `SQUIDS_EVENT_FOCUS_GAINED`

Sent when the app has full focus.

#### `SQUIDS_EVENT_FOCUS_LOST`

Sent when the app has lost focus, but is still active (like if a system dialog overlays the app)

#### `SQUIDS_EVENT_DEACTIVATED`

Sent when the app has finished going into the background.

### API Functions

#### `squids_log(message)`

```lua
squids_log("Hello, world.")
```
Prints a message to some sort of log, somewhere.

#### `squids_set(key, val)`

```lua
squids_set("tick_rate", 30)
```
Controls something related to the underlying platform or the squids shell.

#### `squids_get(key)` 

```lua  
val = squids_get(key)
```
Returns information about the environment in which your program is running, such as the screen size, squids version, etc.  
  
> NOTE: Most/all of these may just turn into globals that Lua creates for you, like the EVENT numbers.

#### `squids_draw_start()`

```lua
function squids_app_event(e, id, x, y)
  if e == "tick" then
    squids_drawStart() -- prep for drawing
    squids_draw_image(...) -- draw stuff
    squids_drawEnd() -- make frame visible
  end
end
```
Tells the underlying platform to prepare the next frame for rendering.


#### `squids_draw_end()`

```lua
function squids_app_event(e, id, x, y)
  if e == "tick" then
    squids_drawStart() -- prep for drawing
    squids_draw_image(...) -- draw stuff
    squids_drawEnd() -- make frame visible
  end
end
```
Tells the underlying platform that you've finished drawing the next frame, and to now make it visible to the user.

#### `squids_image_load(uri)`

```lua
img, w, h = squids_image_load("http://foo.com/img.png")
```
Loads an image from a URI.

Returns an image reference, width, and height if successful. Returns nil, 0, 0 on failure.

#### `squids_image_draw(img, src_x, src_y, src_w, src_h, dest_x, dest_y, dest_w, dest_h, pivot_x, pivot_y, rotation, opacity)`

```lua
squids_draw_start() -- prepare next frame for drawing
squids_image_draw(bgImg, 0, 0, bgw, bgh, 0, 0, scrw, scrh, 0, 0, 0, 1.0, 0) -- draw some other stuff
squids_draw_end() -- make completed frame visible
```
Draws an image.

The rectangular potion to draw comes from the `src` image and is defined by `src_x`,`src_y` , `src_w` , and `src_h`. If `src_w` or `src_h` are 0, then the `img` width or height is used.

The rectangle is drawn to the display at `dest_x, dest_y` and scaled as needed to cover `dest_w, dest_h` on screen.

If rotation is non-0, it will be drawn rotated around a point relative to `dest_x, dest_y` defined by `pivot_x, pivot_y`. Note that the pivot coordinates are not scaled along with the image.

If opacity is less than 1.0, then it will be rendered semi-transparent. If opacity is 0.0, it will not be drawn at all.

> Note: Scaling is accomplished by specifying destination w,h, so a separate scaling x,y is no longer passed into this function.

#### `squids_sound_load(uri)`

```lua
beep = squids_load_sound( "beep.wav" )
if beep then
  squids_sound_play( beep, 0.5 )
end
```
Load sound from a URI.

Returns the sound object, or nil if the sound couldn't be loaded for some reason.

> Note: Only .WAV format files are supported.

#### `squids_sound_play(sound, volume)`

```lua
beep = squids_sound_load("beep.wav")
squids_sound_play(beep, 0.5) -- 50% of normal volume
```
Plays a sound sample. Optional volume is 0.0 thru 1.0 inclusive

> NOTE: the sound object must finish playing before it will play again. To have multiple overlapping sound effects running simultaneously, you have to call `p_makeSound()` on the same data to make multiple playable objects. Some lua lib code that lets you make N copies of a given sample and then rotates through them might be a good way mix together multiple instances of the same sample, and also limit the # of them playing at any given moment,In code below for example:

```lua
function makeSoundSet(file, num)
  local pf = function(self, v)
  local n = self.idx
  squids_sound_play(self.snds[n], v)
  n = n + 1
  if(n > #(self.snds)) then n = 1 end
  self.idx = n
end

local set = {idx = 1, play = pf, snds = {} }
for i = 1, num do set.snds[i] = squids_sound_load(file) end
return set
end

beepSet = makeSoundSet("beep.wav", 4) -- max 4 beeps at once
function a_event(x, y) beepSet:play(0.5) end
```

#### `squids_sound_stop(sound)`

```lua
squids_sound_stop(beep)
```
Makes a sound stop playing, or does nothing if it wasn't.

#### `squids_text_load(key)`

```lua
text = squids_text_load("foo.txt")
if text then
  squids_font_draw(text, 100, 100, font, 1.0)
end
```
Load text from a URI.

Returns the text object, or nil if it couldn't be loaded for some reason.
> Note: Only UTF8 encoded text is supported

#### `squids_font_load(uri)`

```lua
font = squids_font_load("font.png")
squids_font_draw("Hello world", 100, 100, font, 1.0)
```
Loads a font from a URI. This is not a real font. It's just a simplistic way of allowing you to draw text using just an image containing the character glyphs.

The image must contain 128 character glyphs. They must be arranged as 16 rows of 8 characters each. When text is drawn with this font, the character spacing will be "image width / 8" and the font height and line spacing will be "image height / 16".

Returns a font object, or nil if there was a problem.

> NOTE: Of course this is dumb, and monospace, but it's also easy.

#### `squids_font_draw(text, x, y, font, opacity)`

```lua
font = squids_font_load("font.png")
squids_font_draw("Hello world", 100, 100, font, 1.0)
```

Draw a string of text using a font.
Opacity value can be from 0.0 to 1.0

### Currently supported key values

#### `"tick_rate"`

```lua
squids_set("tick_rate", 30) -- send 30 "tick" events per sec
squids_set("tick_rate", 0.1) -- send a "tick" every 10 secs
squids_set("tick_rate", 1) -- send "tick" once per sec
```
Tells the shell how often you'd like to receive "tick" events.

Takes 1 argument "rate", which is the number of events per second. The fastest rate is limited to 60 per second (rate = 60). The slowest rate is limited to one every 10 seconds (rate = 0.1)

There is no way to prevent "tick" events from being sent at all. It is assumed that games will have a high tick rate, whereas non-game apps with little/no continuous animation, will use a very low rate and ignore the ticks, Instead doing most of their rendering in response to tap events and such.

Don't set a high rate unless you need to as it will likely drain the battery faster for no reason.

The default rate is 1 (one "tick" per second).

#### `"show_keyboard"`

```lua
squids_set("show_keyboard", "yes")
squids_set("show_keyboard", "no")
```
Causes the soft keyboard to appear and key events to be sent to app_event().

Does nothing on devices that don't have a soft keyboard.

#### `"show_status_bar"`

```lua
squids_set("show_status_bar", "yes")
squids_set("show_status_bar", "no")
```
On iOS, this controls the visibility of the status bar.

> NOTE: The shell hides the status bar by default,iOS only (?)

#### `"sleepless"`

```lua
squids_set("sleepless", "yes")
squids_set("sleepless", "no")
```
When set to "yes" the device is prevented from dimming it's screen and/or going into idle mode and falling asleep.

### Values for “key” currently supported

#### `"dev_code"`

```lua
dev_code = squids_get("dev_code")
```

Returns "" (empty string) when running in release mode.

If in dev mode, this returns the dev code entered into the developer app. This may be a fully formed URL, or it may be a simple string. If it is a URI, then it will be the same thing you get from "root". Dev codes that are simple strings are translated into URIs, so in that case, you'll get the simple string from "dev_code", and the associated URI from "root".

> PROBLEM: Currently just returns `"dev"`

#### `"platform"`

```lua
platform = squids_get("platform")
```

#### `"squids_version"`

```lua
ver = squids_get("squids_version") -- "1.0"
```
Returns the version number of the Squids shell.

#### `"display_size_pixels"`

```lua
sw, wh = squids_get("display_size_pixels") -- 640
```
Get the pixel resolution of the device's display.

> NOTE: This function returns 2 values

### Software Layers

<img src="https://squids.sleepless.com/images/image2.png">

------
## SLEEPLESS INTERNAL NOTES

### Notes about building the dev app
#### SDL
We're making an SDL version that should cover the desktops, Mac, Linux, Window.

Zach and Joe are working on this now (Mar 2020)

#### iOS

This is from 2013-ish when Joe first built this monster and it was intended just for mobile.

1. Create a new, "empty" iOS project
2. Change "Deployment Target" to "5.0" or greater
3. Delete and trash the `main.m` , `AppDelegate.m` , and `AppDelegate.h` files.
4. Drag and drop the `squids_ios.m` file into the project.
5. Add the AVFoundation, GLKit, OpenGLES, and QuartzCore frameworks
6. Drag/drop the lua dir (make sure you select "create groups" option for folders)
7. Drag/Drop the squids_ios.m file to project
8. Drag/drop your C source files to project (`game.c` is a shell you can start with)
9. Make data dir with lua/data files, and drag/drop to project (use "create groups")
10. Build.
11. Develop

#### Android
Yeah, never got round to this at all.
