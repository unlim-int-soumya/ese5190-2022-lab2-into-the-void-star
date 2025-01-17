University of Pennsylvania, ESE 5190: Intro to Embedded Systems, Lab 2A

SOUMYA RANJAN DASH<br />
https://www.linkedin.com/in/srd00/<br />
Tested on:DELL Inspiron 14 2-in-1 (14-inch, 2022), Windows 11 Home 21H2<br />


(TODO: Your README)

Include lab questions, screenshots, analysis, etc. (Remember, this is public, so don't put anything here you don't want to share with the world.)

Link to set up guide (Part 1 & 2): [Set Up Gide for Windows](https://github.com/satyajeetburla/-Lab-2-SETUP-GUIDE)

# 3.2
## Why is bit-banging impractical on your laptop, despite it having a much faster processor than the RP2040?
Bit-banging is the periodic polling of the GPIO pins by the CPU, which makes it challenging to finish other activities. The system's real-time performance would be impacted by this. Furthermore, because modern laptop CPUs are supposed to handle multiple difficult jobs, even a brief interruption could result in a "Bit-Banging" fatal fault.


## What are some cases where directly using the GPIO might be a better choice than using the PIO hardware? 
LEDs and push buttons.

## How do you get data into a PIO state machine?
The OSR serves as a holding place for data entering the state machine through the TX FIFO. Data is loaded into the OSR one 32-bit 
chunk at a time from the TX FIFO. When an out instruction is issued, the OSR can divide the data into smaller chunks by shifting to 
the left or right and sending the bits that fall off the end to one of a few potential destinations, such as the pins.


## How do you get data out of a PIO state machine? 
OUT instruction shift bit count bits out of the Output Shift Register (OSR), and write those bits to Destination. Additionally, increase the output shift count by Bit count, saturating at 32.

## In the example, which low-level C SDK function is directly responsible for telling the PIO to set the LED to a new color? 
put_pixel() helper method above to output a sequence of pixel values while setting LED to a new color value.

## How is this function accessed from the main “application” code?
pio_sm_put_blocking(), PIO is instructed to change the LED's color. It isn't accessed directly from the main code. Instead, the desired grb values are input using the put_pixel() function, and the presigned pio(either pio0 or pio1) is configured with the correct grb values using the pio_sm_put_blocking() function.

## What role does the pioasm “assembler” play in the example, and how does this interact with CMake?
pioasm is the program that handles the assembly program in the .pio file and translate it into the binary program to be loaded into the PIO state machine. In the example, the pioasm binary under build/tools/pioasm directory, which is built automatically. 






# 3.3 WS2812.pio's annotations
Annotations are made using the "//" - comment in "C" language. 

![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/PIO/Untitled_1.png)
<br>
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/PIO/Untitled_2.png)
<br>
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/PIO/Untitled_3.png)
<br>
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/PIO/Untitled_4.png)
<br>
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/PIO/Untitled_5.png)


# 3.3 .WS2812.c File anotation
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/C/Untitled_1.png)
<br>
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/C/Untitled_2.png)
<br>
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/C/Untitled_3.png)
<br>
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/C/Untitled_4.png)
<br>
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/C/Untitled_5.png)


# 3.4
<br>

[This](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/Part%203.4.xlsx) is the link of the spreadsheet that I have prepared for this portion of the answer.

## Which PIO instance is being used? Which state machine is being used with this PIO instance?
In this code, 0 instace of PIO and 0 instance of State machine were used.

## Which pin is this state machine configured to control? (you can either use settings from the example program, or for the Qt Py LED pin yours will be connected to)
Here WS2812_PIN was used by state machine to control.

## How long is the state machine's clock cycle? How much is this state machine’s clock scaled down relative to the system clock? 
State machines clock cycle is 48MHz. The clock divider can be configured to slow down the execution of the state machine: a clock divisor of n indicates that one 
instruction is performed every n system clock cycles. SDK's system clock frequency is set at 125MHz by default.

## In which direction will this state machine shift bits out of its “output shift register”?
Output Shift Register receives the input from the TX FIFO register bit-wise and shifts the output to the other output pins using OUT
command.

## What basic circuitry does a WS2812 LED need to operate?
Green, Red, and Blue are represented by three cascaded blocks on the WS2812. Each block has I/O ports such as (Power supply LED), Vdd, Gnd, Data output (DOUT), Data input (DIN), Power supply Control circuit (VCC), and Ground (VSS). Data bits indicating RGB brightness are serially sent into the Din pin, after which the chip strips out the first 24 bits (8 bits for R,G,B) and sends the remaining bits out the Dout pin. By connecting the LEDs in a string, with the Dout from one LED flowing to the Din of the next LED, each LED strips off the bits it requires from the front of the data stream and transmits the remainder of 
the data stream to the next LED. The number of LEDs you can drive with a single data line is theoretically unlimited; the only constraint is that the time it takes 
to update all the LEDs in the string rises linearly with the number of LEDs in the string. This results in a highly creative and 
effective method of addressing unique RGB data to any number of LEDs linked in a string.



# 3.5
## How do you connect a WS2812 to a microcontroller?
WS2812 contains pins for controlling data signal output(DOUT), Control Data Signal Input(DIN), Power supply control circuit(VCC), NC, Power Supply LED(VDD), and Ground(VSS). These pins helps WS2812 in estrablishing a good connectivity with RP2040 Microcontroller.

## How does a WS2812 translate bits to color values?
When serial data is supplied to the LED's input, it takes the first three bytes (red, green, blue) and passes the remainder to its 
serial data output. These LEDs are frequently connected in a single continuous chain, with each LED connected to a common power 
source and its data output connected to the next LED's input.

## How do you send a single 1 or 0 bit to the WS2812?
WS2812 line format. Here, Wide positive pulse is used for 1 and narrow positive pulse for 0.

## How many bits does it take to send a single color value?
In WS2812, each color is represented by 24 bits of data.

## What happens if you send more bits than this in a packet? When we try to send more than one bits than this in a packet, the packet is divided into multiple packets and sent. How do you tell a WS2812 you’re done sending data?
To fill the vacuum, the out instruction pushes data out of the OSR and zeroes in from the other end. Because the OSR is 32 bits wide, you will begin to see zeroes after shifting out a total of 32 bits. A pull instruction explicitly removes data from the TX FIFO and places it in the OSR (stalling the state machine if the FIFO is empty).

## How do you send data to more than one WS2812 in a chain?
We are using TX FIFO registers for sending data more than one WS2812 in a chain. The data that we are moving out of the OSR comes from the state machine's TX FIFO, which is more extensively documented in the RP2040 Datasheet. The TX FIFO is a data buffer between the state machine and the rest of the RP2040 that may be filled 
either directly from the CPU or through system DMA, which is substantially quicker.

## A photo or scan of your paper model
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/Part%203.5.1.jpg)
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/Part%203.5.2.jpg)

# 3.6
Your complete packet transmission spreadsheet.
[This](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/Part%203.6.xlsx) is the link of the spreadsheet that I have prepared for this portion of the answer. 

# 3.7
Timing Diagram
![ ](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%203/Part%203.7.jpg)


# 4 HELLO, BLINKENLIGHT

Kindly find the code in Part 4/USB + WS2812 code/ .
You can access by clicking the link as well [Code](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/tree/main/Part%204/USB%20%2B%20WS2812%20code/new_%20hello_usb_plus_color)

So Our task was to modify hello_usb.c to make your LED blink when you send serial commands, by incorporating the necessary C and PIO assembly code from the Pico WS2812 example. <br />

So remember from the lab 2A question document, it was said "to get the WS2812 example to work with your board, you will need to manually add code to enable power to your LED before you can control it. You can refer to GPIO documentation in the SDK and the “blink” example for how to do this. You will need to (a) find the right pin to set for NeoPixel power, referring to the board pinout from Adafruit, (b) initialize this pin via the SDK, (c) set the pin as an “output,” and (d) set the pin “high,” before you can send any LED commands."

Below photo shows the comparision between the hello world .c code and our new .c code for "HELLO, BLINKENLIGHT". <br /> 
The // commented section in the photos and code shows how we implemented it.

![](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%204/Photos%20Code/Main%20code%20comparision%20to%20hello%20world.png)<br />

You must be wondering what is set_neopixel_color in the main for loop! Dont worry we will discuss it next how we implemented it and why.<br /> 

Basically it it a functions to implement our API function that will set the LED color, which we will include to outr existing ws2812.c code.<br /> 

You can see the photo of the implementation below:<br /> 
![](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%204/Photos%20Code/neopixel.png)<br />
 We chose a specific RGB values you can choose any values.
 
 Now we need to create a new header file called “ws2812.h” that you will use to expose
code from “ws2812.c” to our main hello_usb code.<br /> 
You can see the photo of the implementation below:<br /> 
![](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%204/Photos%20Code/header%20file.png)

Finally we need to modify the CMakeList.txt of "hello_usb" code to make it workable for our new 
HELLO, BLINKENLIGHT or hello_usb.c code.
You can see the photo of the implementation below. I have highlighted the difference as well. For details on how to make CMakeList or CMake file you can always refer to SDK manual for RP2040<br /> 
![](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%204/Photos%20Code/shows%20changes%20made%20to%20cmakefile.png) <br /> 

Dont worry its just simple edit we made to make it workable. We just included the necessary libraries the CMake needs to use and the additional ws2812.c code that was required to be executed. Moreover, you can see your ws2812 cmake text file it will help you edit this. <br /> 

## HELLO, BLINKENLIGHT GIF <br /> 
Here is a GIF to show how our code works. <br /> 
![](https://github.com/satyajeetburla/ese5190-2022-lab2-into-the-void-star/blob/main/Part%204/Hello%20World%20%2B%20Blink%20GIF.gif)


# Brief discussion of decisions you made while working with your modeling documents.
We used spreadsheets, handwritten paper work as well as commenting on editors. 

o What were some strengths/weaknesses of
working with paper?<br /> 
Most handy tool ever used.<br /> 
But bad for visualisation or working with large datas.<br /> 
o What were some strengths/weaknesses of
working with spreadsheets?<br /> 
Spreadsheets helps in easy management of large data.<br /> 
weakness is it can be overwhelming when looked at.
o How might you approach this task using
other tools available to you?<br /> 
For timing digram use of CRO or any dedicated softare for timing diagram simulation, would have been helpful to measure the real-time values.<br /> 

# Thank You !!




