# Starbie

![Starbie front render](Renders/Starbie%20Render.png)

Starbie is a tiny motion-controlled digital pet: basically a desktop Tamagotchi with minimal gameplay buttons. Instead of pressing controls, you interact with it by physically moving the board.

*Looking for the guide! You can find it [here](Week%201%20Guide.md)!*

## What it is

Starbie is designed to sit on your desk, show its personality on a small display, and react to how you handle it. Tilting, moving, and otherwise checking in on your pet are meant to be part of the experience.

The board includes a microcontroller, display, motion sensor, and environmental sensor in a compact PCB design. It is USB powered and built around beginner-friendly through-hole soldering for the add-on components.

## Why?

This project was created to be the week 1 beginner tutorial for [halflife](https://halflife.hackclub.com/)!

## Project status

The PCB design, renders, and an Arduino starter firmware are included here. The default program is a two-eye pet with a tilt-controlled radial menu, hidden stats, and a shake reaction.

## Software

[`Firmware/`](Firmware/) contains one self-contained Arduino IDE sketch for the XIAO ESP32-C3. Builders customize pins, menu actions, starting values, and motion settings together at the top of the sketch. The folder also includes a draggable [browser simulator](Firmware/simulator.html). See the [firmware README](Firmware/README.md) for setup notes.

## Repository contents

- `PCB/` - KiCad source files and fabrication exports
- `Renders/` - front and back PCB renders
- `Week 1 Guide.md` - project guide material
- `Firmware/` - Arduino IDE starter sketch and interactive browser simulator
