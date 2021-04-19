# Squids Documentation (JavaScript)

### Technical Overview

The JavaScript version uses SDL2 as same as Lua version, Using QuickJS as JavaScript engine!

The internal functions used by Squids functions are into object called `Kraken` which is codename of Squids 4 (Current one)

### Global Variables

```js
seqNum                                          // [number]     Number of sequences, Used by seq function as counter...
mx                                              // [number]     Mouse X position
my                                              // [number]     Mouse Y position
is_full_screen                                  // [number]     Is game in fullscreen mode?
mouse_down                                      // [bool]       Is mouse button down?
mouse_up                                        // [bool]       Is mouse button up?
quit                                            // [bool]       Quit the game?
paused                                          // [bool]       Is game paused?
next_font_id                                    // [number]     Next font id
fonts                                           // [array]      Array of fonts loaded/used
xpBlue                                          // [number]     Blue color in hex
xpGreen                                         // [number]     Green color in hex
xpBeige                                         // [number]     Beige color in hex
white                                           // [number]     white color in hex
hover_events                                    // [array]      Array of mouse hover events
mouse_down_events                               // [array]      Array of mouse down events
mouse_up_events                                 // [array]      Array of mouse up events
local_default_font                              // [font]       Default font used by Squids
local_active_font                               // [font]       Current font used by Squids
```

### Functions

```js
seq()                                           // [number]     Increases value of seqNum by 1 and returns it.
j2o(j)                                          // [object]     Convert and return JSON as JavaScript object or null if error, Same as JSON.parse.
o2j(o)                                          // [string]     Convert and return JavaScript object as JSON or null if exception, Same as JSON.stringify.
draw_start(color)                               // [void]       Starts drawing with specified hex color.
draw_end()                                      // [void]       Ends drawing.
toggle_full_screen()                            // [void]       Toggles/Exits fullscreen mode.
image_load(path)                                // [image]      Loads image from path and returns it.

deprecated_image_draw(img, x, y, sx, sy, rotation = 0, opacity = 255)       // [deprecated void]    Draw image loaded from path using image_load function.
image_draw(img, x, y, scl, opa = 1, rot = 0)                                // [void]               New version of draw image function.

music_load(path)                                // [music]      Loads music from path and returns it.
music_play(music, volume)                       // [void]       Plays music loaded with music_load and with specified volume [0.0 - 1.0].
music_volume(volume)                            // [void]       Sets music volume [0.0 - 1.0].
music_pause()                                   // [void]       Pauses music being played.
music_resume()                                  // [void]       Resumes music being played.

sound_load(path)                                // [sound]      Loads sound from path and returns it.
sound_play(sound, volume, loops = 0)            // [void]       Plays sound loaded with sound_load and with specified volume [0.0 - 1.0].
sound_volume(volume)                            // [void]       Sets sound volume [0.0 - 1.0].
sound_pause()                                   // [void]       Pauses sound being played.
sound_resume()                                  // [void]       Resumes sound being played.

rect_fill(color, x, y, w, h)                    // [void]       Draws filled rectangle with hex color.
rect_stroke(color, x, y, w, h)                  // [void]       Draws outlined rectangle with hex color.

sq_create(img = {w: 0, h: 0}, x = 0, y = 0)     // [void]       Returns object of squids with is the heart and soul of default squid.
roll(n)                                         // [number]     Returns number between 0 and n - 1 inclusive.
coin_flip()                                     // [bool]       Return true or false randomly.
display(w, h)                                   // [void]       Sets display width and height.
log(s)                                          // [void]       Logs string s.
sleep(ms)                                       // [void]       Sleeps for amount of milliseconds (ms).
event_next()                                    // [event]      Returns next event.
event_wait()                                    // [event]      Waits for event and return it.

image_draw_advanced(img, srcx, srcy, srcw, srch, destx, desty, destw, desth, pivot_x, pivot_y, rotation, opacity)   // [void]   Advanced version of draw image function.

toInt(n)                                                    // [number]     Returns floor of number (double/float) n.
hit_xy(pos = {x: 0, y: 0}, rect = {x:0, y:0, w:0, h:0})     // [bool]       Is position 1 inside the bounds of rectangle?

// [void] General function that loops through each element in the corresponding arrays, and sets their event state when conditions are met for that event
process_events()

set_default_font(font)                          // [void]   Sets default font used by Squids to another font.
set_active_font(font)                           // [void]   Sets current font used by Squids to another font.
create(label = "", x = 0, y = 0, w = 0, h = 0, handle_click = function() {}, font = local_default_font, activeFont = local_active_font) // [button]     Creates button with dimensions and font and click handle function, And return the button.

pseudo_width(charcode)                          // [number]     Returns pseudo width of char in font.
load(path, cw, ch)                              // [font]       Loads font from path with char width and char height, And returns font.
draw(fnt, txt, x, y, opa, opts = { })           // [void]       Draws text using font, With x and y and opacity and options (Options opts are optional).
free_all()                                      // [void]       Frees all loaded fonts.
```

### Internal Functions (Advanced)

These are the internal core functions used by JavaScript version of Squids, Under name `Kraken`, These document for who want to mod or rewrite/port Squids in near future.

```js
Kraken.draw_start(color)                                // [void]       Starts drawing with specified hex color.
Kraken.draw_end()                                       // [void]       Ends drawing.
Kraken.full_screen(fullscreen)                          // [number]     Toggles/Exits fullscreen mode using number param (1 for true, 0 for false).
Kraken.image_load(path)                                 // [image]      Loads image from path and returns it.

Kraken.image_draw(img_id, destx, desty, destw, desth, opacity, rotation, pivot_x, pivot_y, srcx, srcy, srcw, srch])     // [void]   Base function for all drawing images function, Draws image from id of loaded image.

Kraken.music_load(path)                                 // [music]      Loads music from path and returns it.
Kraken.music_play(music, volume)                        // [void]       Plays music from id which loaded with music_load and with specified volume [0.0 - 1.0].
Kraken.music_volume(volume)                             // [void]       Sets music volume [0.0 - 1.0].
Kraken.music_pause()                                    // [void]       Pauses music being played.
Kraken.music_resume()                                   // [void]       Resumes music being played.

Kraken.sound_load(path)                                 // [sound]      Loads sound from path and returns it.
Kraken.sound_play(sound, volume, loops = 0)             // [void]       Plays sound from id which loaded with sound_load and with specified volume [0.0 - 1.0].
Kraken.sound_volume(volume)                             // [void]       Sets sound volume [0.0 - 1.0].
Kraken.sound_pause()                                    // [void]       Pauses sound being played.
Kraken.sound_resume()                                   // [void]       Resumes sound being played.

Kraken.rect_fill(color, x, y, w, h)                     // [void]       Draws filled rectangle with hex color.
Kraken.rect_stroke(color, x, y, w, h)                   // [void]       Draws outlined rectangle with hex color.

Kraken.log(s)                                           // [void]       Logs string s.
Kraken.sleep(ms)                                        // [void]       Sleeps for amount of milliseconds (ms).
Kraken.event_next()                                     // [event]      Returns next event.
Kraken.event_wait()                                     // [event]      Waits for event and return it.
```
