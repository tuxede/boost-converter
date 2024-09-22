# boost-converter
Repo contains files decripting my unusual boost converter hobby project.

**1. WHY?**
As it was my hobby project, I didn't want to create something simple, that just works. There are ready made IC's with enought documentation that you don't need to invent anything. I wanted to try something hard. People on the internet say that a DCDC converter with a microcontroller to control logic is not a good idea. But why is it? You may never know if you don't try ;) 
Why a microntroller? Because I wanted the converter to be digitally controlled and to have additional functionality, that conventional converters don't posses. Like for example - Li-ion cell undervoltage protection with a sound/light indicator, with a programmed behavior. I didn't want to just cut power if power the source is below certain voltage, I wanted the converter to limit the current if voltage is sagging too much, or even lower the ouput voltage, too let other devices know that we are low on battery. Your own code on board gives you a lot of possibilities. 
For what purpose? I wanted it to serve as a 12V voltage source for various project, sometimes very current intensive ones, like RC vehicles, operating from single 18650 Li-ion cell. 

**2. GOALS/LIMITATIONS**
1. CHEAP! 
2. Physically small
3. Low input voltage - 3.0 to 4.2 V
4. A lot of ouput power - I simulated it to work outputting 6 A at 12 V (72 W from a single 18650!)
5. Would not break because of 1. and 2. - so needs thermal throttling
6. Flexible
7. Being overall a good product, low emissions etc, was not a goal at all


**3. Hardware**
1. Synchronous boost converter, with 2 low Rdson and low gate capacitance MOSFETs
2. Cheapest STM32 that I can find, that will still to the job
3. NE555s as MOSFETS gate drivers (because it's fun to use NE555, it's like a electronics meme, also cheap and available)
4. 4 layers PCB, to get any density and make routing easier, but remain cheap

**4. How it turned out**
OVERALL? NOT BAD
1. It worked for the most part 
2. It is reasonably small - 3.5 x 4 cm
3. It was in fact cheap, I ordered 10 PCBs and parts to assemble 3 boards and it was like 30$.


**5. For the most part??**
So what did work and what didn't?
1. It worked well with small loads, but didn't with high loads. I looks like I forgot that a load that on average consumes 2 A, can draw pulses of 15 A. So the controller wasn't fast enough to deal with 15 A spikes, the voltage sagged a lot during these, maybe by 30%. In the end, the voltage controll code looped ones every 25us (about 40kHz), but it should probably respond every MOSFET cycle to be very responsive (so 200kHz). I'm not sure whether the cheap STM32 will handle it. 
2. Voltage controll (just a digital PID) worked great with no load and with not complicated loads.
3. I didn't use synchronous rectification - I used the diode instead. I didn't have enought time to implement it. 
4. I did some design mistakes - for example, the converter has no way of cutting output voltage to 0. There's always a diode that can feed the current form input to output.

**6. Conclusions**
1. It is totally doable. I may be hard if you are 20 years behind and still micros like Atmega328. They have slow clocks, slow timers that can't easily produce fast PWM, very slow ADCs and so on. That means than fast responding control loop may be in fact impossible to implement. But even the STM32G030 had 64Mhz timers and ADC sample rate of about 1Mhz. 
2. My component choice and overall design was dictated by high current and small size. If you go bigger, slower and less power-dense, things should turn out a lot better. Because I used low inductance and capacitances, rate of change of the voltages were high and hard to controll. I actually later soldered on additional caps to make things a bit better.



