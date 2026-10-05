# How to make your own Starbie!

Hey there! Want to make your own Starbie but have no clue where to start? Right this way!

![Starbie render](GuidePics/Starbie%20Render.png)

In this guide, we'll go over how to make a simple 2-key Starbie with an MPU6050 and DHT11 Environment Sensor, step by step.

This process will be in 2 main parts:

1. PCB Design
    1. Drawing the schematic
    2. PCB Layout
    3. Export
2. Firmware!

If anything is unclear, 9 times out of 10 you can usually google it; though PLEASE send what you're stuck on into #half-life!

First we will start with:

## Designing your PCB

For this guide we're going to be using [KiCad](https://www.kicad.org/), which is an open source PCB design tool.

To start, we're going to have to import the necessary footprints. For this guide, I've created a guide for you called [the care package](https://github.com/SharKingStudios/Starbie/blob/main/Week%201%20Care%20Package.zip)!

Once that's downloaded, you should end up with a file called "Week 1 Care Package.zip" - unzip that and you'll end up with a bunch of files like this:

![Extracted files](GuidePics/extractedfiles.png)

The .sym files are symbol libraries, while the .pretty folder contains the footprint libraries. **You'll have to search up how to install them - I find YouTube works best!**

## Schematic Editor

The schematic of a PCB is what defines all the different connections of your PCB, so we're going to start with it first!

First, open KiCad up KiCad and create a new project, then click on the "Schematic Editor" button:

![Schematic editor](GuidePics/schematic%20editor.png)

This should open up the schematic editor. Once you're in, press the A key on your keyboard. This should open up a menu where you can add add components.

Here we will be adding all the components we need. In the end you will end up with something like this:

![Final Schematic](GuidePics/endschematic.png)

Search for and add the XIAO-ESP32-C3 (This will be our microcontroller!)

Press P to add in the +5V, +3.3V and GND symbols.
Create a net label by pressing L on your keyboard and naming the net.

Finally wire all of those together like this:

![ESP32](GuidePics/esp32.png)

*(The net labels tell the schematic that those parts need to be connected without having to draw wires all over.)*

Next, we need to add in the buttons to allow for some interaction with our Starbie. (Add a SW_Push symbol.)

Connect one side of the button to a net and the other side to GND like this:

![Buttons](GuidePics/buttons.png)

Next, we need to add the screen to display our character.

Search for a generic 01x04 pin and add that to your schematic, then wire it up like so:

![OLED](GuidePics/oled.png)

Next add in a generic 01x08 pin to represent the MPU6050 module. This will allow your Starbie to sense itself being moved around:

![MPU6050](GuidePics/mpu6050.png)

Finally, add in the DHT11 sensor. This will allow your Starbie to sense the temperature and moisture of the environment. (Add a DHT11 symbol.)

![Environment sensor](GuidePics/envsensor.png)

Once all the components are connected, we can start assigning footprints to the symbols we have here. Footprints are what gets physically drawn on the PCB. To do this, click the "run footprint assignment tool" in the top right.

![Footprint Assign Tool](GuidePics/footprintassigntool.png)

This should open up a window where you can assign different footprints to your components! Assign them based on the image below:

![Footprint Assign](GuidePics/footprintassign.png)

Once you're done, you can hit apply & save schematic. We're now officially done with the schematic! Onto making the physical PCB itself:

## Route the PCB

Go back to KiCad project page, and hit the "PCB editor" button. Once the PCB editor is open, hit the "Update PCB from schematic" button in the top right:

![PCB editor](GuidePics/updatepcbfromschematic.png)


It should have dumped all the components on the page like this:

![Imported parts](GuidePics/importedparts.png)

After that, create a custom boundry on the edge cuts layer (again Youtube is your friend here!) and arrange your components inside of it.

![Layout](GuidePics/layout.png)


Now it's time to route the PCB! Hit X on your keyboard and hit any golden pad that still has a thin blue line. It should dim the entire screen and show you where to go. Route the PCB like so:

![Routing](GuidePics/routing.png)

(to get the blue lines, change the layer on the right from F.cu to B.cu)

I've added a GND pour here, if you want more information about how you can use it you can check [this awesome tutorial](https://www.youtube.com/watch?v=DNTgrTukltw).

## Checking for errors:

DRC (or Design Rules Check) is a tool that automatically checks for PCB design mistakes, such as disconnected traces or other various errors.

You can access it via the button on the top right:

![Error to ignore](GuidePics/drc.png)

Try running it, you can ignore any warnings but you shouldnt have any errors except for these two:

![Error to ignore](GuidePics/errortoignore.png)

If the error is about a missing connection, go back and fix it!

Feel free to ask about any DRC errors you get in #half-life!

## Customization

Add some art or other cool things to your PCB by playing around with the Image Converter tool.

Google is your friend here, but you can probably figure it out by just messing around with it a bit!

![Image converter](GuidePics/imageconverter.png)

## Firmware

Now that you have a board designed, it needs a personality. Starbie's starter code is a single Arduino sketch: no VS Code extension, PlatformIO setup, or custom library required.

### Upload with Arduino IDE

The actual program is [Firmware/Starbie/Starbie.ino](Firmware/Starbie/Starbie.ino). Keep the Starbie folder and the Starbie.ino file together; Arduino IDE expects the sketch file to be inside a folder with the same name.

1. Install the current [Arduino IDE](https://www.arduino.cc/en/software).
2. Open **File → Preferences**. In **Additional Boards Manager URLs**, add:

   ~~~text
   https://espressif.github.io/arduino-esp32/package_esp32_index.json
   ~~~

3. Open **Tools → Board → Boards Manager**, search for esp32, and install **esp32 by Espressif Systems**.
4. Connect your XIAO ESP32-C3 with a USB data cable. Under **Tools → Board → esp32**, select **XIAO_ESP32C3**. Under **Tools → Port**, select the new port.
5. Open **Sketch → Include Library → Manage Libraries**. Install these libraries one at a time:

   - Adafruit GFX Library
   - Adafruit SSD1306
   - Adafruit MPU6050
   - DHT sensor library

   If Arduino asks to install dependencies, choose **Install All**.

6. Open Starbie.ino, then click Arduino's arrow-shaped **Upload** button.

The ESP32 package URL, ESP32 Boards Manager install, and XIAO_ESP32C3 board selection are documented by [Espressif](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html) and [Seeed Studio](https://wiki.seeedstudio.com/XIAO_ESP32C3_Getting_Started/).

### Controls

| Control | What it does |
| --- | --- |
| Button 1, first press | Opens the radial menu |
| Tilt while menu is open | Moves the selector ball; each general direction is one large sector |
| Button 1, second press | Chooses the highlighted action |
| Button 2 | Shows or hides the stats screen |
| Shake while seeing the pet | Gives Starbie its shake reaction |

The normal screen deliberately has no name or visible values—just the wandering pet. **NAP** stops it and emits drifting `z`s until it wakes after a short time or a shake. **PLAY** makes it sprint two quick left-to-right-to-left laps with hearts, while **PET** makes hearts float upward. Other actions do a short wiggle and jump.

### Make it yours

Nearly everything you need is together at the top of [Starbie.ino](Firmware/Starbie/Starbie.ino), inside the **BEGINNER SETTINGS** block. The one optional effect challenge below tells you exactly where to make its two edits.

#### 1. Match your own wiring

These are the pins for this guide's board:

| Part | XIAO label | ESP32-C3 GPIO |
| --- | --- | --- |
| OLED SDA and MPU6050 SDA | D4 | GPIO6 |
| OLED SCL and MPU6050 SCL | D5 | GPIO7 |
| DHT11 data | D1 | GPIO3 |
| Button 1 | D2 | GPIO4 |
| Button 2 | D3 | GPIO5 |

Change only the pin constants near the top if your submission uses a different layout:

~~~cpp
const int I2C_SDA_PIN = 6;
const int I2C_SCL_PIN = 7;
const int DHT_PIN = 3;
const int BUTTON_ONE_PIN = 4;
const int BUTTON_TWO_PIN = 5;
~~~

The OLED and MPU6050 share the same I2C SDA and SCL wires. If your version has no DHT11, use this setting instead of deleting code:

~~~cpp
const bool USE_DHT11 = false;
~~~

#### 2. Change the radial menu

The menu has four choices: top, right, bottom, and left. Each line has a label, changes to joy/energy/fullness, and one visual reaction:

~~~cpp
const MenuItem MENU_ITEMS[] = {
  {"NAP",   1,  18, -4, NAP_REACTION},   // top
  {"PLAY", 12, -9, -5, RUN_REACTION},    // right
  {"FEED",  3,  2, 18, JUMP_REACTION},   // bottom
  {"PET",   7,  0,  0, HEART_REACTION},  // left
};
~~~

`NAP_REACTION` makes the pet sleep, `HEART_REACTION` sends hearts up from it, `JUMP_REACTION` does the small wiggle and jump, and `RUN_REACTION` makes it run two fast laps with hearts. For example, you could turn the left choice into a dance move:

~~~cpp
{"DANCE", 15, -12, -3, JUMP_REACTION},  // left
~~~

Keep the menu at four items for now—the drawing code is set up for one item in each direction. Make the labels short so they fit in the OLED boxes.

#### 3. Add one special effect

Make your Starbie do one thing the starter code does not: give it a sparkle trail. This is only two small edits, and you can change the shape afterward to make it yours.

First, find `void drawHeart` lower in the file. Paste this **above** it:

~~~cpp
void drawSparkle(int x, int y) {
  display.drawPixel(x, y - 2, SSD1306_WHITE);
  display.drawPixel(x - 1, y - 1, SSD1306_WHITE);
  display.drawPixel(x + 1, y - 1, SSD1306_WHITE);
  display.drawPixel(x - 2, y, SSD1306_WHITE);
  display.drawPixel(x, y, SSD1306_WHITE);
  display.drawPixel(x + 2, y, SSD1306_WHITE);
  display.drawPixel(x - 1, y + 1, SSD1306_WHITE);
  display.drawPixel(x + 1, y + 1, SSD1306_WHITE);
  display.drawPixel(x, y + 2, SSD1306_WHITE);
}
~~~

Then find the `display.drawBitmap(...)` line inside `drawPet()`. Add this immediately below it:

~~~cpp
if (!isNapping() && (now / 250) % 2 == 0) {
  drawSparkle(petX - 4, petY + 18);
}
~~~

Upload after your board is built to see the little sparkle blink behind Starbie as it walks. Try changing the pixels in `drawSparkle` into a star, leaf, music note, or anything else that suits your pet.

#### 4. Draw your own bitmap pet

The starter pet is a 32 x 32 pixel, one-bit simplification of the supplied character. Its sprite is near the top of the sketch as `PET_SPRITE`. It wanders across the screen when no reaction is playing.

To use your own art, make a simple black-and-white image first. Keep the width divisible by 8; 32 x 32 is an easy first size. Upload it to [image2cpp](https://javl.github.io/image2cpp/), choose its **Horizontal 1 bit per pixel** output, and copy the Arduino byte array it creates. If your creature is the wrong color in the preview, use image2cpp's invert option.

Back in Starbie.ino, replace the full byte list inside one of the sprite definitions. Keep the name and update the size settings if your image is a different size:

~~~cpp
const int PET_SPRITE_WIDTH = 32;
const int PET_SPRITE_HEIGHT = 32;

const uint8_t PROGMEM PET_SPRITE[] = {
  // Paste image2cpp's bytes here.
};
~~~

## Next Steps

Now your done! Make sure all of your files are organized in your repository and that you have exported gerber files.
(Look up how to do this! Google is your friend)