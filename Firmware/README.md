# Starbie firmware

Open [`Starbie/Starbie.ino`](Starbie/Starbie.ino) in Arduino IDE. It is the entire firmware in one beginner-editable file—there is no custom library or PlatformIO setup to manage.

The first part of the file is **BEGINNER SETTINGS**. It contains the pins, optional DHT11 switch, starting stats, radial-menu actions and reactions, motion settings, and the pet bitmap. The menu automatically treats the position where Button 1 opens it as centered; `SWAP_MPU_AXES`, `MENU_X_DIRECTION`, and `MENU_Y_DIRECTION` handle any physical MPU6050 orientation. Everything below that works automatically.

## Arduino IDE setup

1. In Arduino IDE, open **File → Preferences** and add this to **Additional Boards Manager URLs**:

   ```text
   https://espressif.github.io/arduino-esp32/package_esp32_index.json
   ```

2. Open **Tools → Board → Boards Manager**, search for `esp32`, and install **esp32 by Espressif Systems**.
3. Select **Tools → Board → esp32 → XIAO_ESP32C3** and select the board's port under **Tools → Port**.
4. Open **Sketch → Include Library → Manage Libraries** and install:

   - Adafruit GFX Library
   - Adafruit SSD1306
   - Adafruit MPU6050
   - DHT sensor library

   If Arduino asks to install dependencies, choose **Install All**.
5. Open `Starbie.ino`, press the arrow-shaped **Upload** button, and wait for the upload to finish.

Seeed's XIAO ESP32-C3 setup guide also confirms selecting `XIAO_ESP32C3` from the ESP32 Arduino board list. [Seeed Studio guide](https://wiki.seeedstudio.com/XIAO_ESP32C3_Getting_Started/)

## Controls

- **Button 1:** open the radial menu. Tilt in an action's general direction to select its full sector, then press Button 1 again to select it.
- **Button 2:** show or hide the stats screen. It never changes the pet.
- **Shake:** the only action that works naturally outside the menu.

The normal screen is a wandering 32 x 32, one-bit version of the supplied character. **NAP** freezes it in place with drifting `z`s, **PLAY** runs two fast left-to-right-to-left laps with hearts, **PET** sends hearts upward, and other actions do a tiny side-to-side shake followed by a jump. A shake wakes a sleeping pet early. Values only appear on the stats screen.

The final word in each menu line selects the visual reaction: `NAP_REACTION`, `JUMP_REACTION`, `HEART_REACTION`, or `RUN_REACTION`.

## Change the bitmap pet

The supplied art is a 32 x 32 black-and-white sprite named `PET_SPRITE` near the top of `Starbie.ino`.

To make your own, first create a high-contrast black-and-white image at the size you want. Keep its width divisible by 8; `32 x 32` is a friendly starting size. Then open [image2cpp](https://javl.github.io/image2cpp/), a browser tool that turns an image into an Arduino byte array. Use its **Horizontal 1 bit per pixel** output for an Adafruit GFX `drawBitmap` image. If the preview looks inverted, use image2cpp's invert option.

Replace an entire sprite definition, including its `{ ... }` byte list, and keep its name:

```cpp
const int PET_SPRITE_WIDTH = 32;
const int PET_SPRITE_HEIGHT = 32;

const uint8_t PROGMEM PET_SPRITE[] = {
  // Paste the bytes image2cpp gave you here.
};
```

Update `PET_SPRITE_WIDTH` and `PET_SPRITE_HEIGHT` whenever your image size changes. The browser simulator uses the matching starter sprite, so it is useful for seeing the reactions before uploading.
