---
layout: post
title: "Skeletyl Build Log"
date: 2021-04-24
---

As I fall deeper in the DIY mechanical keyboard hole this quarantine, I decided to build a Skeletyl, a Dactyl Manuform-like keyboard. It is a sculpted, split ergonomic keyboard, with 4 rows and 5 columns per side, for a total of 36 keys. As my first 3d printed keyboard and handwired build, a lot of work went into it.

If the above paragraph didn't make any sense, this link may help:

<https://golem.hu/keyboard-lexicon/>

## Materials

Being a ""budget"" build, I tried to use as many materials that I already had on hand:
* Gateron yellow KS-3 linear switches
* Blank black DSA keycaps
* 24AWG black stranded hookup wire
* Translucent filament

Here's what I ended up getting to complete the build:
* 1N4148 diodes (36/100)
* 1' TRRS cable
* TRRS jacks (2/10)
* Pro Micros (2)
* Neopixel LED rings (2)
* M4 screws 8mm (10/50)
* M4 heated inserts 4mm (10/80)

Since hardware comes in bulk, I ended up with way more extra screws than I actually used. I may have been able to save more going with AliExpress instead of Amazon, but I didn't want to wait for shipping.

## 3D Printing the Case

I got the open-source case files from Bastardkb's GitHub repo:

<https://github.com/Bastardkb/Skeletyl>

After a couple test prints of just the thumb cluster and many, many stringing test pyramids, I settled on these Cura settings for my Qidi X-one2:

![Cura print settings](/images/skeletyl/print-settings.png)

The most important settings that affected the print were retraction in an attempt to minimize stringing, as well as tree supports. Using tree supports instead of the default ones helped reduce print time and material use. There was still some stringing between the switch holes, but nothing that an X-acto knife couldn't fix.

### Print Pics

![Thumb cluster test](/images/skeletyl/test-thumb-cluster.jpg)

Thumb cluster test print, hella stringy

![Tree supports](/images/skeletyl/tree-supports.jpg)

Tree supports. So much support material from the other side

![Post-cleaning](/images/skeletyl/post-cleaning.jpg)

Looking not too bad after cleaning and adding DSA blanks

## Handwiring the Keyboard

I used dereknheiley's wiring diagrams to handwire the keyboard:

<https://github.com/dereknheiley/Skeleton-Dactyl-Mini#wiring>

### Wiring Steps (with Pics!)

1) Columns

![Columns](/images/skeletyl/columns.jpg)

2) Rows

![Rows](/images/skeletyl/rows.jpg)

3) MCU

![MCU](/images/skeletyl/mcu.jpg)

4) TRRS jack + LED ring

![LEDs](/images/skeletyl/led.jpg)     

This part of the build took the longest, spending an hour or two each day on each step per side. For the columns, I used short lengths of wires with stripped ends. For the rows, I bent the diode legs then soldered them together. I decided to wire the rows and columns directly to the microcontroller rather than route them around the case to hide them.

## Coding the Firmware and Troubleshooting

I compiled and flashed the QMK keymap from the same repo that I got the wiring guide from, which mostly worked after a few changes. 

![VIA test](/images/skeletyl/test.jpg)

Felt good after testing the left side with VIA successfully, but once I plugged in the right side, the columns were reversed and the first two thumb keys didn't work. So pressing the "Y" key actually registered a "P" key. I thought it was a soldering issue, so I reflowed the solder points for the thumb row and even replaced the wire from the bottom row to the MCU. Turns out that the columns were actually reversed in the firmware, so reversing the columns in the layout code `skeletyl.h` file fixed the issue.

Another issue I ran into was that the RGB didn't work on both sides initially after enabling the leds in the firmware. It would only fully light up the side that the micro-USB plug was plugged in to. 

![RGB bug](/images/skeletyl/rgb-bug.jpg)

There are 16 leds on each ring on each side, for a total of 32 leds. After correcting the total number of leds defined in the `config.h` file from 16 to 32, both sides of the keyboard lit up properly.

Finally, I modified the keymap to use a Colemak-DH layout instead of QWERTY, and changed the layers to match a keymap that I am already familiar with.

![My layout](/images/skeletyl/layout.png)

## Final Thoughts

Coming from a reviung41 that I didn't use the outer columns on, the split 10u form factor doesn't feel too different. Since there are only 5 columns on each side, I have to rely on putting modifiers on the home row and layers for symbols and navigation. The split of the keyboard does feel nicer compared to non-split boards. Plus, there's enough space to fit a trackball mouse in between the sides.

![Trackball](/images/skeletyl/trackball.jpg)

The angle of the 'B' key in Colemak-DH feels more comfortable due to the sculpting of the keyboard. Maybe I'll try MT3 profile sculpted keycaps or printing tenting plates for additional comfort. 3d printing the case was pretty fun so I might try to find more split keyboards that fit on my tiny 150mm print bed. Hotswap would be nice too since I prefer tactile switches, but I can settle for the smoothness of Gateron yellows. Although this build took about a week and a half from 3d printing to handwiring, then programming and troubleshooting, the work was well worth it, and I ended up with a keyboard that I am super satisfied with.

## More Pictures

![RGB off](/images/skeletyl/no-rgb.jpg)

![RGB on](/images/skeletyl/rgb.jpg)

![Final wiring](/images/skeletyl/back.jpg)

![Bottom plate](/images/skeletyl/bottom-plate.jpg)
