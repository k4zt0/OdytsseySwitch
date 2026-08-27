# Odytssey Switch

Five-key wireless macropad built around an ESP32-S3, with a 0.96" OLED and a
thumbstick. Reaches a host over USB HID, BLE HID, or Wi-Fi, and can drive other
gear through its I2C, UART and addressable-LED outputs.

## Hardware

| Block   | Part |
| ------- | ---- |
| MCU     | ESP32-S3-WROOM-1 — BLE 5, Wi-Fi 4, native USB, pre-certified module |
| Keys    | 5 x Kailh MX hotswap sockets on GPIO 4, 5, 6, 7, 15 |
| Stick   | 2-axis thumbstick, X on IO1 and Y on IO2 (ADC1 only), press on IO16 |
| Display | SSD1306 0.96" 128x64 over I2C0 (SDA IO8, SCL IO9) |
| Outputs | Qwiic I2C port, WS2812 strip header, UART0 + 4 spare GPIO |
| Power   | USB-C bus powered, AP2112K-3.3 LDO, USBLC6-2SC6 ESD array |

Both analogue axes sit on ADC1 because ADC2 stops working once the radio is
transmitting. The thumbstick is fed 3V3 rather than 5V so its wiper range
matches the ADC full scale.

## Board

- 112 x 102 mm, 2 layer FR-4, 1.6 mm, 3 mm corner radius
- Front carries the keys and the two module sockets; every other part is on the
  back, so the panel reflows from one side only
- No through-hole parts — the module connectors are SMD sockets, so assembly
  needs no soldering at all
- Routed on both layers, 345 tracks and 207 vias, ground pours stitched
  together and bonded solid to pads rather than through thermal spokes
- The ESP32 module's antenna overhangs the left edge; keep metal away from it
- Ground pours on both layers, stitched with vias

## Assembly

Order with SMT assembly. Nothing needs soldering afterwards:

1. Plug the OLED module into J2
2. Plug the thumbstick module into J7
3. Drop five MX switches into the hotswap sockets
4. Fit keycaps

Buy modules that already have their pin headers soldered on.

## Fabrication

`fab/` holds Gerbers, Excellon drill files, the drill map, the `.gbrjob`,
pick-and-place data and the BOM.

| Setting | Value |
| ------- | ----- |
| Base material | FR-4, TG130-140 |
| Layers | 2 |
| Thickness | 1.6 mm |
| Panel | single PCB |
| Surface finish | ENIG |
| Min hole | 0.2 mm |

The 1.6 mm thickness is not optional: MX hotswap sockets are cut for it.

## Layout

Third-party footprints come from [kiswitch](https://github.com/kiswitch/kiswitch);
only `Switch_Keyboard_Hotswap_Kailh.pretty` is vendored here, under `libs/`.
`fp-lib-table` points at it with `${KIPRJMOD}`.

## Status

Complete and ready to order.

| Check | Result |
| ----- | ------ |
| ERC | 0 errors |
| DRC | 0 errors |
| Unconnected | 0 |
| Schematic parity | 0 issues |
| Netlist vs. design intent | checked pin by pin, no mismatch |

Two DRC warnings remain and are both expected: the ESP32 module's silkscreen
crosses the left board edge by 0.15 mm, which is the deliberate antenna
overhang, and sixteen footprints differ from their library copies, which has no
bearing on fabrication.

The autorouter did not length-match the USB differential pair. At USB 2.0 Full
Speed that is not a problem in practice, but tidy those two traces by hand if
you want it right.
