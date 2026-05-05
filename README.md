# NeoPixel Library for ESP32

A high-level NeoPixel (WS2812 / WS2812B) smart LED driver for **BlueScript** on ESP32.
This package is built on top of the standard `rmt` library, utilizing a custom RMT encoder to seamlessly transmit GRB color data and reset pulses to a strip of LEDs without blocking the CPU or triggering garbage collection during transmission.

## Installation

Install this package in your BlueScript project:

```bash
bscript project install https://github.com/bluescript-lang/pkg-neopixel-esp32.git
```

## Usage

### Basic: Control an LED Strip

Changes made to the LEDs (like `setPixelColor` or `clear`) only update the internal memory buffer. You must call `show()` to actually update the hardware LEDs.

```typescript
import { NeoPixel, Colors } from "neopixel";

// 1. Initialize a strip of 10 LEDs connected to GPIO 15
const strip = new NeoPixel(15, 10);

// 2. Set overall brightness (0 - 255)
strip.setBrightness(128); // 50% brightness

// 3. Clear all LEDs to black
strip.clear();
strip.show();
time.delay(500);

// 4. Set individual pixel colors
strip.setPixelColor(0, 255, 0, 0);       // Pixel 0: Red (RGB)
strip.setPixelColorHex(1, Colors.Green); // Pixel 1: Green (Hex/Enum)
strip.setPixelColorHex(2, 0x0000FF);     // Pixel 2: Blue (Hex literal)
strip.show();
time.delay(1000);

// 5. Fill the entire strip with one color
strip.fill(255, 255, 0); // Yellow
strip.show();
time.delay(1000);

// 6. Cleanup hardware resources
strip.close();
```

## API Reference

### Class: `NeoPixel`

The primary class to control a strip of NeoPixel LEDs.

#### `constructor(pin: integer, length: integer)`
Initializes the LED strip and claims the underlying RMT hardware channel.
- **pin**: The GPIO pin number connected to the Data In (DIN) of the LED strip.
- **length**: The total number of LEDs in the strip.

#### `setPixelColor(index: integer, r: integer, g: integer, b: integer): void`
Sets the color of a specific LED in the internal buffer using RGB values.
- **index**: The zero-based index of the LED.
- **r**: Red component (`0` to `255`).
- **g**: Green component (`0` to `255`).
- **b**: Blue component (`0` to `255`).

#### `setPixelColorHex(index: integer, hexColor: integer): void`
Sets the color of a specific LED using a 24-bit hexadecimal integer.
- **index**: The zero-based index of the LED.
- **hexColor**: The color value (e.g., `0xFF0000` for Red, or `Colors.Red`).

#### `fill(r: integer, g: integer, b: integer): void`
Fills all LEDs in the strip with the same RGB color.

#### `clear(): void`
Sets all LEDs to Black (`0, 0, 0`). *Equivalent to `fill(0, 0, 0)`.*

#### `setBrightness(brightness: integer): void`
Scales the brightness of all LEDs. The actual color output is calculated during `show()`.
- **brightness**: Brightness level from `0` (off) to `255` (maximum brightness).

#### `show(): void`
Transmits the current color buffer to the physical LED strip via the RMT channel. This applies the current brightness scaling.

#### `close(): void`
Frees the underlying RMT channel and encoder resources. Call this if you no longer need to control the LEDs to free up memory and hardware peripherals.


## Enums

### `Colors`
A set of pre-defined standard 24-bit color hex codes for convenience.

| Name | Hex Value |
| :--- | :--- |
| `Black` | `0x000000` |
| `White` | `0xFFFFFF` |
| `Red` | `0xFF0000` |
| `Green` | `0x00FF00` |
| `Blue` | `0x0000FF` |
| `Yellow` | `0xFFFF00` |
| `Cyan` | `0x00FFFF` |
| `Magenta`| `0xFF00FF` |
| `Orange` | `0xFFA500` |