-   [Home](https://www.instructables.com/)
-   [Circuits](https://www.instructables.com/circuits/)
-   [Workshop](https://www.instructables.com/workshop/)
-   [Craft](https://www.instructables.com/craft/)
-   [Cooking](https://www.instructables.com/cooking/)
-   [Living](https://www.instructables.com/living/)
-   [Outside](https://www.instructables.com/outside/)
-   [Teachers](https://www.instructables.com/teachers/)

[Log In](https://www.instructables.com/account/login/) | [Sign Up](https://www.instructables.com/account/register/)

[![Instructables](https://www.instructables.com/assets/img/instructables-logo-v2.png)instructablescircuits](https://www.instructables.com/circuits/)[Projects](https://www.instructables.com/circuits/projects/)[Contests](https://www.instructables.com/contest/)

-   [PUBLISH](https://www.instructables.com/create/)

Enter search term

# Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial

By [TheElectromania](https://www.instructables.com/member/TheElectromania/) in [Circuits](https://www.instructables.com/circuits/)[Arduino](https://www.instructables.com/circuits/arduino/projects/)

778,556

475

125

Featured

![license](https://www.instructables.com/assets/img/license/by-nc-sa_small.png)

DownloadFavorite

## Introduction: Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial

[![Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial](https://content.instructables.com/ORIG/FK4/OVE9/IKJ8C6QZ/FK4OVE9IKJ8C6QZ.jpg?auto=webp&frame=1&width=1024&height=1024&fit=bounds&md=8afd1ad23a3d30f8f16058ecb3bc4588)](https://content.instructables.com/ORIG/FK4/OVE9/IKJ8C6QZ/FK4OVE9IKJ8C6QZ.jpg?auto=webp&frame=1&width=1024&height=1024&fit=bounds&md=8afd1ad23a3d30f8f16058ecb3bc4588)

[![TheElectromania](https://content.instructables.com/ORIG/FKY/1BDZ/II5VT6MZ/FKY1BDZII5VT6MZ.jpg?auto=webp&crop=1%3A1&frame=1&width=130)](https://www.instructables.com/member/TheElectromania/)By [TheElectromania](https://www.instructables.com/member/TheElectromania/)[Electromania](http://www.youtube.com/channel/UChjgqHTZ41qw2-dT_3MjdHg)Follow

More by the author:

[![Quick Digital thermometer using cheap USB to TTL converter and  DS18B20 - WITHOUT Arduino or Raspberry Pi](https://content.instructables.com/ORIG/FUX/6SRT/IMDZFZDW/FUX6SRTIMDZFZDW.jpg?auto=webp&crop=1%3A1&frame=1&width=130)](https://www.instructables.com/Quick-Digital-Thermometer-Using-Cheap-USB-to-TTL-C/)

[![Digital thermometer on OLED display using ESP8266 ESP-12E NodeMCU and DS18B20 temperature sensor](https://content.instructables.com/ORIG/F6S/RA20/ILV81KIU/F6SRA20ILV81KIU.jpg?auto=webp&crop=1%3A1&frame=1&width=130)](https://www.instructables.com/Digital-Thermometer-on-OLED-Display-Using-ESP8266-/)

[![PIR Motion Detector With Arduino: Operated at Lowest Power Consumption Mode](https://content.instructables.com/ORIG/FA6/ABPA/IL4FPJ96/FA6ABPAIL4FPJ96.jpg?auto=webp&crop=1%3A1&frame=1&width=130)](https://www.instructables.com/PIR-Motion-Detector-With-Arduino-Operated-at-Lowes/)

About: A Researcher, an Engineer and an electronics enthusiast [More About TheElectromania »](https://www.instructables.com/member/TheElectromania/)

NodeMCU Dev Board is based on widely explored esp8266 System on Chip from Expressif. It combined features of WIFI accesspoint and station + microcontroller and uses simple [LUA](http://www.lua.org/) based programming language. ESP8266 [NodeMCU](http://nodemcu.com/index_en.html#fr_54747361d775ef1a3600000f) offers-

--Arduino-like hardware IO

--Event-driven API for network applicaitons

--10 GPIOs D0-D10, PWM functionality, IIC and SPI communicaiton, 1-Wire and ADC A0 etc. all in one board

--Wifi networking (can be uses as access point and/or station, host a webserver), connect to internet to fetch or upload data.

--excellent few $ system on board for Internet of Things (IoT) projects.

Recently, there has been interest in programming ESP8266 systems using Arduino IDE. Programming, of ESP8266 using Arduino IDE is not very straight forward, until it is properly configured. Especially because, the Input and output pins have different mapping on NodeMCU than those on actual ESP8266 chip.

I had request about showing how to program ESP-12E NodeMCU using Arduino IDE. I struggled myself earlier in the beginning, so thought of making this Instructable for beginners. This is quick guide/tutorial for getting started with Arduino and ESP8266 NodeMCU V2 ESP-12Ewifi module. (I think, this method can be used for other NodeMCU boards too. (or only ESP8266 boards, but with necessary hardware modifications and using FTDI modules for programming- not covered in this tutorial because, this is only for NodeMCU dev boards).

This Instructable gives quick intro to-  
1) Installing Arduino core for ESP8266 WiFi chip in Arduino IDE and Getting started with sketches written using Latest stable Arduino IDE 1.6.7

2) Run/modify basic LED blink sketch to blink onboard LED and/or externally connected LED at pin D0 or GPIO-16 as per the pin configuration mentioned [here](https://github.com/esp8266/Arduino/issues/584) and [here](https://github.com/nodemcu/nodemcu-devkit).

**NOTE**- To use NodeMCU V1 or V2 or V3 dev boards using Arduino IDE, we do not need to flash it with firmware using nodemcu flasher. It is required only if we intend to program NodeMCU using Lua script with esplorer etc.

I have other video published on getting started with NodeMCU and flashing NodeMCU firmware on following [link](https://www.youtube.com/watch?v=x7GzK7zHKOk) https://www.youtube.com/watch?v=x7GzK7zHKOk

**Arduino logo and NodeMCU logo are their respective trademarks- logos shown in above image were taken from -https://github.com/nodemcu and [https://www.arduino.cc/en/Main/Software](https://www.arduino.cc/en/Main/Software)

Add TipAsk QuestionCommentDownload

## Step 1: NodeMCU ESP-12E Pin Mapping

[![NodeMCU ESP-12E Pin Mapping](https://content.instructables.com/ORIG/FPV/E4YC/IKLFP40J/FPVE4YCIKLFP40J.png?auto=webp&frame=1&width=549&height=1024&fit=bounds&md=c4a06d89ce48fe56110aed614a291fb9)](https://content.instructables.com/ORIG/FPV/E4YC/IKLFP40J/FPVE4YCIKLFP40J.png?auto=webp&frame=1&width=1024&height=1024&fit=bounds&md=c4a06d89ce48fe56110aed614a291fb9)

[![NodeMCU ESP-12E Pin Mapping](https://content.instructables.com/ORIG/FOG/POMF/IKLFPR1Q/FOGPOMFIKLFPR1Q.png?auto=webp&frame=1&width=651&height=1024&fit=bounds&md=dd938ff2d1188db9f84a71f55aee8dc2)](https://content.instructables.com/ORIG/FOG/POMF/IKLFPR1Q/FOGPOMFIKLFPR1Q.png?auto=webp&frame=1&width=1024&height=1024&fit=bounds&md=dd938ff2d1188db9f84a71f55aee8dc2)

First and foremost word of - CAUTION !

* The ESP8266 chip requires 3.3V power supply voltage. It should not be powered with 5 volts like other arduino boards.

* NodeMCU ESP-12E dev board can be connected to 5Vusing **micro USB** connector or **Vin** pin available on board.

* The I/O pins of ESP8266 communicate or input/output max 3.3V only. i.e. the pins are **NOT** 5V tolerant inputs.

In case you have to interface with 5V I/O pins, you need to use level conversion system (either built yourself using [resistor voltage divider](https://learn.sparkfun.com/tutorials/voltage-dividers/all?print=1) or using ready to use level converters (e.g. these ones [adafruit](https://www.adafruit.com/products/395) or [aliexpress](http://www.aliexpress.com/item/5V-3V-IIC-UART-SPI-Four-Channel-Level-Converter-Module-for-Arduino-Free-Shipping-via-China/32374128931.html?spm=2114.01010208.3.11.kC2PDM&ws_ab_test=searchweb201556_3,searchweb201644_5_505_506_503_504_502_10001_10002_10016_10017_10010_10005_10011_10006_10003_10004_10009_10008,searchweb201560_8,searchweb1451318400_-1,searchweb1451318411_6451&btsid=b78a1fbc-74e6-4451-9702-c852c7f7b8ae) etc.).

-------------------------------------------------------------------------------------------------------------------------------------------------

The pin mapping of NodeMCU dev board are different from those of ESP8266 GPIOs. Attached images gives mapping of pins, [source of images](https://github.com/nodemcu/nodemcu-devkit).

More information about pins is available on following links:

* Github - [NodeMCU](https://github.com/nodemcu/nodemcu-devkit-v1.0)

* Github-esp8266/[Arduino](https://github.com/esp8266/Arduino/issues/584)

Add TipAsk QuestionCommentDownload

## Step 2: Installing Arduino Core for NodeMCU ESP-12E Using Arduino Boards Manager

[![Installing Arduino Core for NodeMCU ESP-12E Using Arduino Boards Manager](https://www.instructables.com/assets/img/pixel.png)](https://content.instructables.com/ORIG/FMX/N4ZM/IKLFPSC4/FMXN4ZMIKLFPSC4.jpg?auto=webp&frame=1&fit=bounds&md=9af68ca8002503249bf6b26d3b0b5740)

As shown in the image, Copy the .json link with latest stable release of NodeMCU package from [Github page here](https://github.com/esp8266/Arduino#installing-with-boards-manager).

The link should look something like this-

http://arduino.esp8266.com/stable/package_esp8266com_index.json

Add TipAsk QuestionCommentDownload

## Step 3: Insert Link for .json NodeMCU Package Files Into Arduino IDE

[![Insert Link for .json NodeMCU Package Files Into Arduino IDE](https://www.instructables.com/assets/img/pixel.png)](https://content.instructables.com/ORIG/FC6/HCOM/IKJ8C6SU/FC6HCOMIKJ8C6SU.jpg?auto=webp&frame=1&width=1024&fit=bounds&md=3c25830ff3a19b8aa0df15e3a8b0b9ab)

Paste the copied link and insert it in Arduino IDE using following sequence-

_**File menu - Preferences-**_

Paste copied link into the area shown in black box in above image. Close and restart the Arduino IDE.

Add TipAsk QuestionCommentDownload

## Step 4: Tools - Boards Manager

[![Tools - Boards Manager](https://www.instructables.com/assets/img/pixel.png)](https://content.instructables.com/ORIG/FPQ/2JX4/IKJ8C6SI/FPQ2JX4IKJ8C6SI.jpg?auto=webp&frame=1&fit=bounds&md=b1cd0825dee76b7f7acfd1b0c27bb064)

[![Tools - Boards Manager](https://www.instructables.com/assets/img/pixel.png)](https://content.instructables.com/ORIG/FAG/LWJD/IKLFPSOW/FAGLWJDIKLFPSOW.jpg?auto=webp&frame=1&fit=bounds&md=4631d6518ca33c03f48029dd80797123)

_**Tools - Boards manager**_and search for ESP8266 and install the libraries/files given under heading _**ESP8266 by ESP community**_.

Restart the Arduino IDE once again.

Add TipAsk QuestionCommentDownload

## Step 5: Selecting NodeMCU Board in Arduino IDE

[![Selecting NodeMCU Board in Arduino IDE](https://www.instructables.com/assets/img/pixel.png)](https://content.instructables.com/ORIG/FPF/I7DM/IKJ8C6SP/FPFI7DMIKJ8C6SP.jpg?auto=webp&frame=1&fit=bounds&md=b0cc325322794ccce800dbf97be1f7bc)

[![Selecting NodeMCU Board in Arduino IDE](https://www.instructables.com/assets/img/pixel.png)](https://content.instructables.com/ORIG/FY4/SZTH/IKLFPU7S/FY4SZTHIKLFPU7S.jpg?auto=webp&frame=1&fit=bounds&md=793b389b4c3a07cc25c96dd599df24f2)

Go to **_Tools - Boards_** (scroll down the list of boards) - Select _**NodeMCU 1.0 ( ESP-12EModule).**_

Select the **_Port_** number at which you have connected nodeMCU. Rest of the settings can be left to default values.

Add TipAsk QuestionCommentDownload

## Step 6: LED Blink - Connections for External LED

[![LED Blink - Connections for External LED](https://www.instructables.com/assets/img/pixel.png)](https://content.instructables.com/ORIG/F78/ZUOX/IKJ8C6U1/F78ZUOXIKJ8C6U1.jpg?auto=webp&frame=1&fit=bounds&md=f410b7e7f3a74307dd4f88c31ff3f352)

[![LED Blink - Connections for External LED](https://www.instructables.com/assets/img/pixel.png)](https://content.instructables.com/ORIG/F3H/P9FT/IKJ8C6U3/F3HP9FTIKJ8C6U3.jpg?auto=webp&frame=1&fit=bounds&md=fc3d6b72024223f51e27258a8b8e9121)

[![LED Blink - Connections for External LED](https://www.instructables.com/assets/img/pixel.png)](https://content.instructables.com/ORIG/FQW/OECK/IKJ8C6S6/FQWOECKIKJ8C6S6.jpg?auto=webp&frame=1&width=1024&height=1024&fit=bounds&md=35c2a64f8bda06e6201a456ddca62c92)

We will be connecting external LED directly to GPIO16 or D0 pin of NodeMCU (no need of external current limiting resistor). This is the pin number for onboard LED or BUILTIN_LED (in my case it is blue LED - some boards might have green or red LED).

Add TipAsk QuestionCommentDownload

## Step 7: LED Blink - Example Sketch

[![LED Blink - Example Sketch](https://www.instructables.com/assets/img/pixel.png)](https://content.instructables.com/ORIG/FEW/7ADU/IKJ8C6SZ/FEW7ADUIKJ8C6SZ.jpg?auto=webp&frame=1&fit=bounds&md=cde46986e1045edb3f4fd9c445eaa17c)

Go to **_File - Examples - ESP8266 - Blink_**

In my video, I have modified the sketch to blink LED faster, but you can leave as it is and just upload the sketch to ESP and there you go... the On-board LED blue and external LED red starts blinking alternately at every second.

Congratulations for successful configuration of Arduino IDE for ESP8266 NodeMCU dev boards.

**Note-** In case, if Arduino IDE version 1.6.7 fails to work for you, try to go back to arduino 1.6.5 or backwards. (I have heard, some NodeMCU boards have issues with latest versions of Arduino IDEs and going to earlier versions of Arduino IDE solves the problems).

Good luck for fun with this amazing system on board.....

Helpful links-

[https://github.com/esp8266](https://github.com/esp8266)

[https://github.com/esp8266/esp8266-wiki/wiki](https://github.com/esp8266/esp8266-wiki/wiki)

[https://github.com/esp8266](https://github.com/nodemcu/nodemcu-firmware)

[http://nodemcu.com/index_en.html](http://nodemcu.com/index_en.html)

[https://nodemcu.readthedocs.org/en/dev/](https://nodemcu.readthedocs.org/en/dev/)

My Doit NodeMcu Lua ESP8266 ESP-12E WIFI Development Board was from [Banggood.com](http://eu.banggood.com/Wholesale-Warehouse-Geekcreit-Doit-NodeMcu-Lua-ESP8266-ESP-12E-WIFI-Development-Board-wp-Uk-985891.html)

Add TipAsk QuestionCommentDownload

## 17 People Made This Project!

-   ![Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial](https://www.instructables.com/assets/img/pixel.png)
    
    [arunkchandramohan12](https://www.instructables.com/member/arunkchandramohan12/) made it!
    
-   ![Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial](https://www.instructables.com/assets/img/pixel.png)
    
    [Wo1fMane](https://www.instructables.com/member/Wo1fMane/) made it!
    
-   ![Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial](https://www.instructables.com/assets/img/pixel.png)
    
    [dhendarno](https://www.instructables.com/member/dhendarno/) made it!
    
-   ![Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial](https://www.instructables.com/assets/img/pixel.png)
    
    [cwy5847](https://www.instructables.com/member/cwy5847/) made it!
    
-   ![Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial](https://www.instructables.com/assets/img/pixel.png)
    
    [HảoN28](https://www.instructables.com/member/H%25E1%25BA%25A3oN28/) made it!
    
-   ![Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial](https://www.instructables.com/assets/img/pixel.png)
    
    [HảoN28](https://www.instructables.com/member/H%25E1%25BA%25A3oN28/) made it!
    
-   ![Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial](https://www.instructables.com/assets/img/pixel.png)
    
    [AhmadN72](https://www.instructables.com/member/AhmadN72/) made it!
    
-   ![Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial](https://www.instructables.com/assets/img/pixel.png)
    
    [Osama Abdel-Rahman](https://www.instructables.com/member/Osama+Abdel-Rahman/) made it!
    
-   ![Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial](https://www.instructables.com/assets/img/pixel.png)
    
    [plasmazion](https://www.instructables.com/member/plasmazion/) made it!
    
-   See 8 More
    

Did you make this project? Share it with us!

I Made It!

## Recommendations

[![Mini Regulated Power Supply Unit](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/Mini-Regulated-Power-Supply-Unit/)

**[Mini Regulated Power Supply Unit](https://www.instructables.com/Mini-Regulated-Power-Supply-Unit/)** by [andrea biffi](https://www.instructables.com/member/andrea%2Bbiffi/) in [Electronics](https://www.instructables.com/circuits/electronics/projects/)

 77  8.3K

[![Arduino Cable Tracer](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/Arduino-Cable-Tracer/)

**[Arduino Cable Tracer](https://www.instructables.com/Arduino-Cable-Tracer/)** by [TechKiwiGadgets](https://www.instructables.com/member/TechKiwiGadgets/) in [Arduino](https://www.instructables.com/circuits/arduino/projects/)

 365  38K

[![Vegetables and Fruits Ripeness Detection by Color W/ TensorFlow](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/Vegetables-and-Fruits-Ripeness-Detection-by-Color-/)

**[Vegetables and Fruits Ripeness Detection by Color W/ TensorFlow](https://www.instructables.com/Vegetables-and-Fruits-Ripeness-Detection-by-Color-/)** by [Kutluhan Aktar](https://www.instructables.com/member/Kutluhan%2BAktar/) in [Electronics](https://www.instructables.com/circuits/electronics/projects/)

 24  2.8K

[![Digital Car Horn](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/Digital-Car-Horn/)

**[Digital Car Horn](https://www.instructables.com/Digital-Car-Horn/)** by [TomHammond](https://www.instructables.com/member/TomHammond/) in [Audio](https://www.instructables.com/circuits/audio/projects/)

 168  17K

-   ### Arduino Contest
    
    [![Arduino Contest](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/contest/arduino2021/)
-   ### Stone, Concrete, Cement Challenge
    
    [![Stone, Concrete, Cement Challenge](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/contest/concrete2021/)
-   ### Water Speed Challenge
    
    [![Water Speed Challenge](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/contest/h2o2021/)

![](https://www.instructables.com/assets/img/default/user.TINY.png)

Add TipAsk QuestionPost Comment

We have a **be nice** policy.  
Please be positive and constructive.

Add ImagesPost

## 125 Comments

[![firosekhan](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/member/firosekhan/)

[firosekhan](https://www.instructables.com/member/firosekhan/)

3 years ago

ReplyUpvote

Thanks for sharing this. I could test my new esp_12E with builtin LED. Success in first attempt. Bingo !!!

1 reply 

[![xhbte71039](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/member/xhbte71039/)

[xhbte71039](https://www.instructables.com/member/xhbte71039/)

2 years ago

ReplyUpvote

Thanks for detailed guide

[![bottamnhanhung](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/member/bottamnhanhung/)

[bottamnhanhung](https://www.instructables.com/member/bottamnhanhung/)

2 years ago

ReplyUpvote

Pursuing and learning technology always makes us smart and takes less time  

[![adams588](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/member/adams588/)

[adams588](https://www.instructables.com/member/adams588/)

2 years ago

ReplyUpvote

+1

1

[![joegellen20](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/member/joegellen20/)

[joegellen20](https://www.instructables.com/member/joegellen20/)

2 years ago

ReplyUpvote

i can't compiling every time i get this error: Arduino: 1.6.13 (Windows 10), Board: "NodeMCU 1.0 (ESP-12E Module), 80 MHz, 115200, 4M (3M SPIFFS)" Board nodemcuv2 (platform esp8266, package esp8266) is unknown Error compiling for board NodeMCU 1.0 (ESP-12E Module). This report would have more information with "Show verbose output during compilation" option enabled in File -> Preferences. can you help me  
______________________  
Regards, [mobdro](https://mobdrotvapk.com/)  

[![wizsorykid](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/member/wizsorykid/)

[wizsorykid](https://www.instructables.com/member/wizsorykid/)

4 years ago

ReplyUpvote

hello, please how can i do to generate a pwm signal with esp-12s using arduino IDE ?

1 reply 

[![PriyankaD19](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/member/PriyankaD19/)

[PriyankaD19](https://www.instructables.com/member/PriyankaD19/)

4 years ago

ReplyUpvote

Hey, can I use the PWM pin to dim a light bulb?

3 replies 

[![JavedH](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/member/JavedH/)

[JavedH](https://www.instructables.com/member/JavedH/)

Question 2 years ago

AnswerUpvote

I have installed Arduin0 1.8.7 and followed the instructions. But when I goto Step 6 to selct Port, it shows grey and not selectable. My board is NodMcu V3, LoLin with CH340G chip.  
Any help, please?  
  
Thanks.  

[![RenonE1](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/member/RenonE1/)

[RenonE1](https://www.instructables.com/member/RenonE1/)

2 years ago

ReplyUpvote

When Flashing NodeMCU development board, it is good. But when I unplug and plug the module again, it seem to go back to its empty state... why?????

[![pritz__](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/member/pritz__/)

[pritz__](https://www.instructables.com/member/pritz__/)

3 years ago

ReplyUpvote

What if we want to connect multiple sensors who needs 5V supply? and also want to ask how we are supposed to power the nodemcu without connecting it with computer with a data cable?

4 replies 

[![Gokuldas V R](https://www.instructables.com/assets/img/pixel.png)](https://www.instructables.com/member/Gokuldas+V+R/)

[Gokuldas V R](https://www.instructables.com/member/Gokuldas+V+R/)

3 years ago

ReplyUpvote

warning: espcomm_sync failed

error: espcomm_open failed

error: espcomm_upload_mem failed

showing these errors while uploading programs.

can somebody help me to reslove this?

I am using nodemcu esp8266 v3 module

More CommentsPost Comment

Open Menu

Programming ESP8266 ESP-12E NodeMCU Using Arduino IDE - a Tutorial by [TheElectromania](https://www.instructables.com/member/TheElectromania/)FollowDownload Favorite I Made It [View Comments](https://www.instructables.com/Programming-ESP8266-ESP-12E-NodeMCU-Using-Arduino-/#discuss) Share More Options

Categories

-   [
    
    Circuits](https://www.instructables.com/circuits/)
-   [
    
    Workshop](https://www.instructables.com/workshop/)
-   [
    
    Craft](https://www.instructables.com/craft/)
-   [
    
    Cooking](https://www.instructables.com/cooking/)
-   [
    
    Living](https://www.instructables.com/living/)
-   [
    
    Outside](https://www.instructables.com/outside/)
-   [
    
    Teachers](https://www.instructables.com/teachers/)

About Us

-   [Who We Are](https://www.instructables.com/about/)
-   [Why Publish?](https://www.instructables.com/create/)

Resources

-   [Sitemap](https://www.instructables.com/sitemap/)
-   [Help](https://www.instructables.com/how-to-write-a-great-instructable/)
-   [Contact](https://www.instructables.com/contact/)

Find Us

-   [](https://www.instagram.com/instructables/ "Instagram")
-   [](https://www.pinterest.com/instructables "Pinterest")
-   [](https://www.facebook.com/instructables "Facebook")
-   [](https://www.twitter.com/instructables "Twitter")

---

© 2021 Autodesk, Inc.

-   [Terms of Service](http://usa.autodesk.com/adsk/servlet/item?siteID=123112&id=21959721)|
-   [Privacy Statement](http://usa.autodesk.com/adsk/servlet/item?siteID=123112&id=21292079)|
-   Privacy settings|
-   [Legal Notices & Trademarks](http://usa.autodesk.com/legal-notices-trademarks/)

[![Autodesk](https://www.instructables.com/assets/img/footer/autodesk-logo-make-anything.png)](http://www.autodesk.com/)

{"mode":"full","isActive":false}