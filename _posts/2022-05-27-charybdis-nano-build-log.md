---
layout: post
title: "Charybdis Nano Build Log"
date: 2022-05-27
---

[TLDR want to see the finished board](#final-assembly)

I decided to build a [Charybdis Nano](https://github.com/Bastardkb/Charybdis) since I liked the Skeletyl so much and wanted this build to be different from it. This time, I had the opportunity to use the flex PCBs instead of handwiring it, which allowed for per-key RGB lighting. The many mods for this keyboard already made by the community also made it appealing.

There were a few features I wanted to implement, some of them experimental:

- EC11 rotary encoder PCB on the left
- PMW3389 sensor from Tindie as I didn't want to source electronics for or have extra trackball PCBS
- KB2040 Kee Boar Driver powered since it's available, cheap, and powerful

## Let's get creative with bodge wires

I had to bodge a few wires on the shield PCB for KB2040 compatibility and to enable some other features, such as split hand detection and enabling the halves to communicate with each other faster. 

Here's what the bodge wires looked like on the right side shield:

![Right shield bodge wires](/images/cnano/right-bodges.jpg)

```
GP29/R1 -> serial_tx (tip of the TRRS jack) with `SERIAL_USART_PIN_SWAP` defined
GP3/SCL -> split_hand (5.1k resistor going to 5v or GND depending on side)
GP2/SDA -> CS (for the trackball)
```

On the left shield, `CS` was rerouted to `GP9` (the thumb cluster row pin) so that the encoder press would work.

![Left shield bodge wire](/images/cnano/left-bodge.jpg)

Additional bodge wires on the left hand thumb cluster were also needed to extend the matrix and RGB to the encoder PCB. I utilized the unused pins since the 3rd thumb key was cut off. I connected the last `DOUT` pad to a 90° header pin on `C3` and bridged `C6` to a 90° header pin on `C5`, as shown:

![Left thumb bodge wires](/images/cnano/thumb-wiring.jpg)

I used the 90° header pins with Dupont cables to keep the encoder PCB swappable. Likewise, I used JST-XH cables+connectors with Mill-max sockets on the shield PCB to also keep it modular. Thankfully, it all worked as intended:

![Left side test](/images/cnano/left-test.jpg)

## Fitting the PMW3389 sensor

The bottom BTU assembly was modified in Tinkercad to make space for the rectangular PMW3389 board. I ended up filing the two lens holder legs as well to get it to fit.

![PMW3389 holder](/images/cnano/pmw3389-holder.jpg)

Even though the sensor fit, the trackball wasn't very responsive because it was sitting too close to the lens. To fix this, I used standoffs and foam tape to get the correct liftoff distance for the ball.

![Ball holder assembled](/images/cnano/standoff-abuse1.jpg)

## More modules

Because the KB2040 doesn't have onboard EEPROM, I used an I2C EEPROM breakout board so I can save mouse and RGB settings between reboots. The Qwiic connector JST was perfect for this since the rest of the pins were being used by the matrix and trackball/encoder peripherals. The 1"x1" board size allowed it to fit under the trackball and encoder, too.

![EEPROM breakout under encoder](/images/cnano/standoff-abuse2.jpg)

I experimented with switching relays for a bit, but they were ditched once I found out haptic feedback on each side of a split isn't supported in QMK (_yet_ 🤞). Plus, they were falling off since foam tape couldn't hold them up.

![Relays](/images/cnano/relays.jpg)

Closeups of the different breakout boards' wiring.

![EEPROM breakout and encoder bottom](/images/cnano/eeprom-encoder-modules.jpg)

![PMW3389 and encoder top](/images/cnano/pmw3389-encoder.jpg)

## Final assembly

The white 30° alien tents give it a Portal gun vibe, apparently. It's more stable than the 30° Skeletyl tent I previously tried, and having the encoder/trackball level with the desk is also nice.

![Tents](/images/cnano/tents.jpg)

![RGB](/images/cnano/rgb1.jpg)

![RGB dark](/images/cnano/rgb2.jpg)

Thanks for scrolling! If you're curious, the QMK configuration for the KB2040 plus mods is available [here](https://github.com/somepin/qmk_firmware/tree/dbl-cnano-kb2040/keyboards/bastardkb/charybdis/3x5/v2/kb2040), and the files to print the knob are [here](https://www.tinkercad.com/things/0CBprOJqlCN).