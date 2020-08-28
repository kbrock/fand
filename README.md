
Son and I saw a cool [raspberry pi case] that we could print. It needed a fan (which we both liked) and it was time to get to work. Since it was going to have minimal work load most of the time, I did not want the fan running all the time. We got a fan with 5V and a PWM controll. We got an expensive [fan] which it turns out had a wonderful design and [documentation]. I ebay'd a cheap heat sinc for the processor ($3) to help make the fan more effective.

The nice thing about these fans is it does not need a diod across the fan voltage. It also has no need for a protection diagram.

You see, I spent way too much time searching for the correct circuitry.

There is a nice youtube and blog post from sensorsiot:

- https://www.sensorsiot.org/variable-speed-cooling-fan-for-raspberry-pi-using-pwm-video138/
- https://www.sensorsiot.org/pimp-my-raspberry-pi-3/

The docs say not to use a NPN driving circuite (p8) and prefer a CMOS driver. Which makes sense. It looks like a better square wave.

Such conflicting messages on the forumns:

- https://www.raspberrypi.org/forums/viewtopic.php?t=244194
- https://www.raspberrypi.org/forums/viewtopic.php?t=216884


But the phrase that got my attention the most is:

> This is the very circuit that is used for GPIO pins inside most micro-controllers.

That sold me that I do not need to create my own CMOS driver and can rely upon the one built into the raspberry pi.

I would guess that cheaper fans do not have the protections in place, and would want to have a circuit to isolate the drive. Please research to find a way that makes you more comfortable. 


I sliced up the cable (and felt that I had screwed up way too many times)
For our robot project, we did get a crimper, so we put females onto the ends and hooked it up.

| wire|  def  |  pin|comments|
| ----|-------|  ---|--------|
|  Red| +5V   |Pin 4|our female connector is lopsided. so it's obvious which pin is gnd/5V|
|Black| GND   |Pin 6||
|Blue |GPIO 23|Pin 16| PWM Signal to the fan - have a |
|Green|GPIO 24|Pin 18| RPM Signal from fan - broke so ignored for now|


I put a 10k resister between the blue wire and pin 16. I just soldered it inline.


The instructions on running the script felt a little off, so I researched how to properly set this up and went with an `init.d` script. got some fun [initd info] to give me too many options. Since I'm familiar with init.d over systemd, I used that. The system automatically adds the systemd support files anyway

# Installation

```
mkdir /usr/local/sbin
cp fand-initd /etc/init.d
cp fand-sbin  /usr/local/sbin

sudo update-rc.d fand defaults
sudo systemctl start fand
sudo systemctl status fand
```

[initd info]: https://www.dexterindustries.com/howto/run-a-program-on-your-raspberry-pi-at-startup/
[raspberry pi case]: https://www.youmagine.com/designs/raspberry-pi-3-model-b-casing-with-40mm-fan
[fan]: http://amzn.com/B07DXS86G7
[documentation]: https://noctua.at/pub/media/wysiwyg/Noctua_PWM_specifications_white_paper.pdf
