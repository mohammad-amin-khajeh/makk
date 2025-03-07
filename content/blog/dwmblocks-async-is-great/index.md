+++
title = "dwmblocks-async is just great"
description = "simple, suckless, and efficient yet feature-rich."
date = "2025-03-07"
[extra]
toc = true
[taxonomies]
tags = ['suckless','software','workflow']
categories = ['tutorial', 'educational']
+++


## shortcomings of the default dwm bar

I love dwm, it's fast, simple, efficient and can be configured to do anything you'd like through patches.[^1]\
It's been my home for the past 9~ months(as of writing) and frankly, I don't plan on migrating to something else.\
One thing that I am not particularly a fan of though is the default status bar:

  1. You have to set the status bar through an infinite loop, like so:

      ```sh
      while :; do
        xsetroot -name "$(date)"
        sleep 30
      done
      ```

      This gets inefficient and wasteful when running multiple commands that need to be updated.
  2. [dwmblocks](https://github.com/torrinfail/dwmblocks) is not asynchronous.\
      While the regular dwmblocks addresses the previous issue, it still has issues of its own.\
      The biggest one being the fact that it executes each block sequentially.\
      "Who cares?" you're probably saying.\
      It will lead to annoying freezes if a block doesn't fire immediately, among other things.

## Enter dwmblocks-async

dwmblocks-async, as the name suggests, blocks are executed asynchronously.\
Meaning we don't have to wait for a block to return(which can take seconds, depending on the block) and then move on to the next one.

It also has all the features of the default dwmblocks plus some more.

## How does it actually work though?

It's simple.
You specify a script or just any code in your language of choice in the `config.h` file. The `print` statement in which will be shown in the bar.

Take the following simple shell script as an example:

```sh, name=current_date.sh
now=$(date '+%Y %b %d (%a) %I:%M%p')
echo "$now"
```

And in `config.h`

```c, name=config.h
// Define blocks for the status feed as X(icon, cmd, interval, signal).
#define BLOCKS(X)                                                        \
    X("", "~/.local/src/dwmblocks-async/blocks/current_date.sh", 10,  0) \
```

{{admonition(type='note' text='full path must be specified.')}}

\
This will show the current time, as outputted by the `echo` statement[^2]:
{{admonition(type='tip' text='click on the image the open it in a new tab for better viewing')}}
[{{full_width_image(src='./bar_example.png', alt="bar example")}}](./bar_example.png)

<br/>

## leveraging signals

Signals are a great way to update blocks only upon certain events and thus not wasting any resources.

Suppose you have a volume block that shows the current volume percentage.\
Does it make more sense to update the block every second or only when pressing the lower/raise volume keys?

Take the following block that does just that:[^3]

```sh,name=volume.sh
#!/usr/bin/env bash
echo "$(pamixer --get-volume)%"
```

And in `config.h`:

```c, name=config.h

// Define blocks for the status feed as X(icon, cmd, interval, signal).
#define BLOCKS(X)                                                    \
    X("",    "~/.local/src/dwmblocks-async/blocks/volume.sh", 0, 10) \
```

Note how interval is set to `0` (Does not get executed on its own).\
And signal is `10`.

Now in order to trigger the script we just need to execute: `kill -num $(pidof dwmblocks)` where `num` equals 34 + the signal of the block.\
So in our case it would be 44 (34+10): `kill -44 $(pidof dwmblocks)`

As an example in my [sxhkd](https://github.com/baskerville/sxhkd) config I have the following binding:

```sxhkdrc, name=~/.config/sxhkd/sxhkdrc
XF86Audio{Raise,Lower}Volume
  pactl set-sink-volume @DEFAULT_SINK@ {+,-}2% && kill -44 $(pidof dwmblocks) # update dwmblocks-async's volume block
```

Now, only when I press the lower/raise volume keys will the block update!

## Adding click functionality

There are 3 requirements to have clickable blocks.

  1. patch dwm with the `statuscmd` patch[^4]:

      ```c, config.h
      { ClkStatusText,        0,                   Button1,        sigstatusbar,   {.i = 1 } },
      { ClkStatusText,        0,                   Button2,        sigstatusbar,   {.i = 2 } },
      { ClkStatusText,        0,                   Button3,        sigstatusbar,   {.i = 3 } },
      { ClkStatusText,        0,                   Button4,        sigstatusbar,   {.i = 4 } },
      { ClkStatusText,        0,                   Button5,        sigstatusbar,   {.i = 5 } },
      ```

      The above snippet must be available in your dwm build.

  2. Enable the following in your dwmblocks-async's `config.h` :

      ```c, name=config.h
      // Control whether blocks are clickable.
      #define CLICKABLE_BLOCKS 1
      ```

  3. Lastly, add the following snippet to your scripts:

      ```sh,name=script_name.sh
      case "$BLOCK_BUTTON" in
      1) leftClick ;;
      2) middleClick ;;
      3) rightClick ;;
      4) scrollUp ;;
      5) scrollDown ;;
      esac
      ```

      Each number corresponds to the button you have to press to execute a command(with the given button).

## Practical example

Remember the volume percentage block from before?
Let's add the following snippet to it:

```sh
case "$BLOCK_BUTTON" in
1) pactl set-sink-mute @DEFAULT_SINK@ toggle;;
3) st -e "$EDITOR" "$0" ;;
4) pactl set-sink-volume @DEFAULT_SINK@ +1%;;
5) pactl set-sink-volume @DEFAULT_SINK@ -1%;;
esac
```

Now if we `left click` on the block we mute the output(or unmute it if it's muted already).

Now if we `right click` we open the script's file in st.

If we `scroll up` we raise the volume by 1%.

And if we `scroll down` we lower the volume by 1%.

## Summary

dwmblocks-async is a simple and efficient bar for dwm that adheres to the suckless philosophy and gets the job done.

Unlike the regular dwmblocks, it runs asynchronously so no annoying flickers and freezes.

It's everything that I could ever want in a bar and if you're using dwm then you should definitely give it a try!

[^1]: if you do not wish to go through the hassle(or the joy) of patching dwm manually you can just use [dwm-flexipatch](https://github.com/bakkeby/dwm-flexipatch) like me.
[^2]: I know we could just do `date '+%Y %b %d (%a) %I:%M%p'` and achieved the same result. But I wanted to be more explicit for the sake of example.
[^3]: `pamixer` must be installed.
[^4]: In the case of `dwm-flexipatch`. The `BAR_DWMBLOCKS` patch must also be present.
