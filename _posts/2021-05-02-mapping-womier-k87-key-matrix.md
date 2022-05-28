---
layout: post
title: "Mapping Womier K87 Key Matrix"
date: 2021-05-02
---

After handwiring my own mechanical keyboard switch matrix, natrually the next step was to reverse engineer an existing one. Why do this after handwiring a keyboard from scratch? Another project, obviously. But actually, I wanted to port over QMK to my Womier K87 tenkeyless keyboard. QMK would make switching keys and layouts much easier than using the stock firmware, which only has mediocre software to change keymappings. Similar work has been done before on the Redragon K552, another tenkeyless mechanical keyboard which uses the same microcontroller.

As always, starting a project like this required some research, which meant lurking on Discord servers and GitHub repositories. Oddly enough, a lot of knowledge in the mechanical keyboard scene is stored on Discord. This makes information harder to look for since Discord servers aren't as public as websites. ~~(But I guess I'm one to speak as I'm putting this on Gemini ¯\\\_(ツ)\_/¯)~~ Anyways, the most useful Discord I found was Sonix Keyboard Hacking Community, which consists of other keyboard enthusiasts who port QMK to their prebuilt keyboards. Wormier Gang is another server that has successfully ported QMK to the Womier K66, the K87's smaller sibling.

I found this GitHub repository after reading up on and asking around in the Sonix Keyboard server:

<https://github.com/IslamAlam/blitzwolf-bw-kb-1#evision-vs11k09a-1-debug-recovery-mode--swd>

The repo's readme essentially outlines the steps needed to reverse engineer a keyboard so QMK can work on it. A crucial step is mapping the rows and columns of the keyboard's PCB to the microcontroller. This is so the firmware can know which microcontroller pins are being used by each row and column.

To be able to map, I needed to use a multimeter set to continuity mode. Continuity mode checks if two points are connected electrically. Usually, multimeters beep when the check passes, and are silent and display "1" otherwise. But since I could only find a cheap Harbor Freight multimeter, mine didn't beep. Instead of listening for a beep, I just looked at the multimeter display for a number that wasn't "1". 

I started off by checking the columns. I probed the cathode side of each switch diode with the black lead, and the microcontroller pin with the red lead. For the rows, I put the black lead on the right pin of each switch, and kept the red lead on the microcontroller pins. As I went, I checked each pin with the microcontroller's datasheet. The datasheet had each pin's name, location, and function, and can be found here: 

<https://github.com/SonixQMK/Mechanical-Keyboard-Database/blob/main/docs/MCU chip/SN32F240B_V1.9_EN.pdf>

I also drew a map to record each row and column to it's corresponding pin, and ended up with this:

![Key Matrix](/images/k87-key-matrix.png) 

It turned out the Womier K87 uses the same pins as the Redragon K552. So in theory, the Redragon QMK firmware might be able to be flashed and the keys would work. I didn't try since I don't have a copy of the K87's stock firmware to flash back to.

The next step to porting QMK to the Womier would be to acquire a ST-Link V2 clone to dump the stock firmware. Jumper cables from the ST-Link would be soldered to the PCB on these points: 

![SWD pads](/images/k87-mcu-anno.jpg)

However, it's unlikely that I'll continue with the ST-Link as I'd only be using it once for this project. Not sure if I'll pursue further reverse engineering projects either, but this was still a good learning experience nonetheless.

**Update:** Despite getting a clone ST-Link programmer for an unrelated project, I ended up selling this keyboard about a month after this was posted. Then, someone way smarter than me finished the Sonix QMK port with working RGB 😄

## Further Reading

<http://pcbheaven.com/wikipages/How_Key_Matrices_Works/>

<http://blog.komar.be/how-to-make-a-keyboard-the-matrix/>



