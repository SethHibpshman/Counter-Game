# Counter Game

> A two-player button-mashing contest built on the ESP32-S3, featuring a live countdown timer, real-time score tracking, a sparkle winner animation, and automatic game reset: all rendered on a 128x64 OLED display.

## Table of Contents

- [Overview](#overview)
- [Functionality](#functionality)
  - [Gameplay](#gameplay)
  - [Countdown Sequence](#countdown-sequence)
  - [Winner Animation](#winner-animation)
  - [Reset Behavior](#reset-behavior)
- [System Design](#system-design)
  - [Hardware](#hardware)
  - [Software](#software)
  - [Pin Mapping](#pin-mapping)
- [Bill of Materials](#bill-of-materials)
- [Author](#author)
- [License](#license)


## Overview

Counter Game is a two-player competitive button-mashing game built on the ESP32-S3 and programmed in MicroPython. Each player presses their button as fast as possible during a 10-second round, while live scores and a countdown timer update in real time on a shared OLED display. When time runs out, the winner is announced with a sparkle particle animation.

The project was a first demonstration of my knowledge after reading a book on MicroPython and the ESP32 Module. This demo covers hardware input handling, OLED display rendering, timing and debounce logic, and real-time game state management.


## Functionality

### Gameplay

Two players compete side by side on a single device. Each button press increments that player's score, which updates live on the OLED. The game runs for exactly **10 seconds**, after which the higher score wins.

| Player | Button | Display Side |
|---|---|---|
| Player #1 | Button 1 (GPIO 7) | Left |
| Player #2 | Button 2 (GPIO 16) | Right |

The live countdown timer is displayed at the top-center of the screen throughout the round.

### Countdown Sequence

Before each round, a 3-second countdown is displayed centered on the screen:

```
3 → 2 → 1 → Start!
```

Each number holds for 1 second. "Start!" is displayed briefly before the game loop begins.

### Winner Animation

After the 10-second round, the winner screen displays:

- The winning player's name (e.g. `Player #1 won!`)
- Their total press count (e.g. `(42 presses)`)
- A randomized **sparkle particle effect** — dots scattered across the screen, each fading out over 10 frames

In the case of equal scores, a `Tie!` message is shown instead. The animation loops until both buttons are held for 2 seconds to return to the main game.

### Reset Behavior

At any point, during the game or on the winner screen, **holding both buttons simultaneously for 2 seconds** resets the device back to the countdown sequence.


## System Design

### Hardware

| Component | Details |
|---|---|
| Microcontroller | ESP32-S3 |
| Display | SSD1306 128x64 OLED (SoftI2C) |
| Player 1 Button | Momentary pushbutton, active-low with internal pull-up |
| Player 2 Button | Momentary pushbutton, active-low with internal pull-up |

### Software

Written entirely in **MicroPython** as a single-file application.

**Dependencies:**
- `ssd1306` — OLED driver
- `urandom` — Sparkle animation randomization
- `machine` — Pin and I2C control
- `time` — Millisecond timing via `ticks_ms` and `ticks_diff`

```
main.py    # Full application: hardware init, display helpers, game loop, winner animation
```

**Key functions:**

| Function | Description |
|---|---|
| `draw_centered(text, center_x, y)` | Renders text horizontally centered on a given x-axis |
| `draw_players(score1, score2, countdown)` | Draws the main game screen with scores or countdown overlay |
| `draw_timer(seconds_left)` | Draws the countdown timer at the top-center of the screen |
| `start_countdown()` | Runs the 3-2-1-Start pre-game sequence |
| `game_loop()` | Main 10-second game: handles scoring, timer, and reset detection |
| `winner_animation(score1, score2)` | Displays winner text and sparkle particle effect |

### Pin Mapping

| Pin | Function |
|---|---|
| 7 | Player 1 button |
| 16 | Player 2 button |
| 46 | OLED SCL |
| 21 | OLED SDA |


## Bill of Materials

| Item | Qty | Unit | Total |
|---|---|---|---|
| [ESP32-S3 Dev Board](https://a.co/d/08OKpdB2) | 1 | $6.40 | $6.40 |
| SSD1306 OLED 128x64 | 1 | $3.00 | $3.00 |
| Momentary Pushbuttons | 2 | $0.25 | $0.50 |
| Resistors | — | — | $0.05 |
| Breadboard / Wiring | 1 | — | ~$2.00 |
| **Grand Total** | | | **~$11.95** |

> **Notes:**
> - Unit costs reflect bulk or kit purchasing; individual retail prices may be higher.
> - BOM assumes breadboard prototyping. A custom PCB would change the wiring cost.


## Author

**Seth Hibpshman**
Student of Electrical Engineering, Eastern Washington University


## License

[GNU General Public License v3.0](LICENSE)
