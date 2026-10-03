# Kryptek

Kryptek is my custom full size (108 key) wireless mechanical keyboard, its built around an **Adafruit Feather nRF52840 Express**.

so basicaly i wanted a full size board that could work wireles over bluetooth but still be usable as a normal wired keyboard when i need it too, and i wanted to understand the entire thing instead of just putting together a kit.. the matrix, the diodes, the I2C gpio expanders, the battery, the screen, the leds ALL of it, so i designed the whole thing from scratch in kicad and fusion 360 which took a ridiculous amount of time lol

![Kryptek full 3D model](Image/Screenshot%202026-10-02%20192305.png)

## why i made this

im 17 and i spend a stupid amount of time on a keyboard for school, coding and basically everything else.. so i thought if im going to use one this much i might aswell build one that i actualy understand from the switch all the way down to the firmware

main goals i set for myself :

- design a full size PCB in kicad (108 keys, nav cluster and numpad included)
- make it **wireless** with bluetooth and a lipo battery, but keep usb-c for wired use
- scan the whole key matrix through I2C GPIO expanders
- add a 1.9" LCD for a custom animated status screen
- add separate caps lock, num lock, battery low and charging status indicators
- design a proper printable case, plate and bottom parts in fusion 360
- learn the full pipeline : schematic -> pcb -> 3d model -> case -> firmware

the hardest parts were fitting a full size matrix, the usb and display wiring, and getting all the little status features to coexist with the keyboard scan.. but seeing the complete layout and case come together made it worth it

## project status

this repo has the complete hardware design, cad files, firmware and manufacturing outputs. its basically a complete V1 build, although there are definitely things i would change if i make a V2

whats included :

- kicad project, schematic and pcb
- full fusion 360 design and `.step` export
- printable case parts (`.stl`) and plate
- production files (bom, positions, designators, ipc netlist and gerber zip)
- KMK / CircuitPython firmware for the nRF52840
- schematic and layout PDFs

current hardware summary :

| | |
|---|---|
| Layout | full size, 108 keys (main block + nav + numpad) |
| Controller | Adafruit Feather nRF52840 Express |
| Connectivity | bluetooth LE **and** USB-C wired mode (Fn+Esc toggles at runtime) |
| Matrix | 21 columns x 6 rows, scanned through 2x MCP23017 I2C expanders |
| Diodes | 1N4148W, one per switch (COL2ROW) |
| Battery | 500mAh 1S LiPo with battery voltage monitoring |
| Display | Waveshare 1.9" ST7789 IPS LCD |
| Indicators | Caps Lock, Num Lock, battery low and charging status LEDs |
| Stabilizers | MX compatible stabilizers for the large keys |
| Switches | MX compatible mechanical switches |
| Keycaps | Plum Blossom cherry profile shine-through PBT |
| Case | 3d printed custom case designed in Fusion 360 |
| PCB | custom PCB with production files included |

the case is mostly a clean white shell with the kryptek branding built into the design, and the blue display / indicator area gives it the little bit of personality i wanted without making the board look too busy

## images

### full 3d model
![Kryptek assembled 3D model](Image/Screenshot%202026-10-02%20192305.png)

### top case and layout
![Kryptek top case layout](Image/Screenshot%202026-10-02%20192357.png)

### pcb
![Kryptek PCB routing](Image/Screenshot%202026-10-02%20190416.png)

### pcb 3d view
![Kryptek PCB 3D view](Image/Screenshot%202026-10-03%20133606.png)

### schematic / wiring
the matrix, both gpio expanders, the mcu, usb-c, esd protection, stabilizers, leds and display connector are all documented in the schematic..

![Kryptek schematic](Image/Screenshot%202026-10-02%20190314.png)

### display and status indicators
![Kryptek display and status area](Image/Screenshot%202026-10-02%20192319.png)

### other case screenshots
![Kryptek case underside](Image/Screenshot%202026-10-02%20192327.png)

![Kryptek case bottom](Image/Screenshot%202026-10-02%20192312.png)

## repo file checklist

important files in this repo :

- `PCB/Kryptek.kicad_pro`
- `PCB/Kryptek.kicad_sch`
- `PCB/Kryptek.kicad_pcb`
- `CAD/Full/Kryptek.step`
- `Fusion/Kryptek.f3d`
- `CAD/Printing/`  -> `Plate.stl`, `Top Plate.stl`, `Bottom.stl`, `Bottom Lifter.stl`, `Bottom Lifter Screw.stl`
- `Firmware/Firmware.py`
- `Production/bom.csv`
- `Production/positions.csv`
- `Production/designators.csv`
- `Production/netlist.ipc`
- `Production/Kryptek.zip`  (gerbers)
- `bom.csv`
- `PDF/Kryptek.pdf`  (schematic print)
- `PDF/Kryptek-Layout.pdf`  (keyboard layout / drawing)

## cad

designed in **fusion 360**.. the editable design is in `Fusion/Kryptek.f3d` and theres a `.step` export in `CAD/Full/` if you just want the geometry. all the print ready parts are in `CAD/Printing/`

the case includes the top plate, bottom shell, bottom lifter and lifter screw parts, so the complete printable enclosure is kept together with the project instead of being only a render

## firmware note

firmware is **KMK** running on **CircuitPython**, and its all in one file :

- `Firmware/Firmware.py`

it has the nRF52840 Feather pin map, two MCP23017 expanders at `0x20` and `0x21`, the full 108 key matrix, the lock and battery led logic, the ST7789 display setup and the animated cherry blossom scene. the peripheral code is wrapped in try/except so if a display or led is missing it should still let you type instead of completely stopping

to flash it : double tap RESET on the feather, copy the CircuitPython `.uf2` if needed, then copy `Firmware.py` plus the `kmk/` folder and the required `lib/` drivers (`adafruit_mcp230xx`, `adafruit_bus_device`, `adafruit_ble`, `adafruit_st7789`, `adafruit_display_text`) onto the `CIRCUITPY` drive

by defalt it boots as **usb** so its easy to test during first bring up, and **Fn+Esc** switches over to bluetooth. if you want bluetooth first just swap `hid_type` and `secondary_hid_type` at the bottom of `Firmware.py`

## bom (summary)

full bom is in `bom.csv`. the original prices are in usd and the current INR fields are left blank for now

| # | Name | Purpose | Qty | Cost ($) | Distributor | Link |
|---|------|---------|-----|----------|-------------|------|
| 1 | 3D printed Case | keyboard case | 1 | 8.00 | Hack Club | [Buy](https://printlegion.hackclub.com) |
| 2 | PCB fabrication | the main board | 1 | 132.16 | LionCircuits | [Buy](https://www.lioncircuits.com/quote) |
| 3 | Plum Blossom Cherry Profile Top Shine-Through PBT Keycap Set V2 | keycaps to actualy type on | 1 | 28.11 | Curiosity Caps | [Buy](https://curiositycaps.in/products/plum-blossom-cherry-profile-shine-through-dye-sublimation-pbt-keycap-set) |
| 4 | Durock Clear Screw-In Stabilizers V2 | stabalizers for the big keys | 1 | 24.62 | Stackskb | [Buy](https://stackskb.com/store/durock-clear-screw-in-stabilizers-v2/?attribute_combination=7%2B1+Set&attribute_spacebar-size=6.25U) |
| 5 | Akko V3 Penguin Pro Switch (Pack of 45) | switches for the keys | 3 | 49.95 | Stackskb | [Buy](https://stackskb.com/store/hmx-volume-0-pack-of-35/) |
| 6 | Adafruit Feather nRF52840 Express | microcontroler / the brains | 1 | 63.69 | Robu | [Buy](https://robu.in/product/adafruit-feather-nrf52840-sense/) |
| 7 | WLY752530 3.7V 500mAh 1S LiPo Battery | battery | 1 | 1.71 | Robu | [Buy](https://robu.in/product/50mah-pcm-protected-micro-li-po-battery/) |
| 8 | 1N4148W SOD-123 Diode | diode (one per switch) | 216 | 1.59 | Robu | [Buy](https://robu.in/product/1n4148w-sod-123-1206-diodereel-of-3000/) |
| 9 | MCP23017-E/SO I/O expander | gpio expander | 4 | 8.78 | Robu | [Buy](https://robu.in/product/mcp23017-e-so-microchip-i-o-expander-16-bit-i2c-serial-1-8-v-5-5-v-soic-28-pins/) |
| 10 | Waveshare 1.9 inch LCD Display Module | the screen | 1 | 11.17 | Robu | [Buy](https://robu.in/product/waveshare-lcd-display-module-spi-interface-262k-colors/) |
| 11 | Lano Dark Blue Gx16 Aviator Connector 3.5m Cable | wired keyboard cable | 1 | 16.64 | Curiosity Caps | [Buy](https://curiositycaps.in/products/dark-blue-cable-black-connector-cable) |

the smaller capacitors, resistors, leds, usb connector, jst connector and protection parts are also listed in the full `bom.csv`

**total bom cost : $350.582**

---
