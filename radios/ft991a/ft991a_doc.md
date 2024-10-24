
# Equalizer SSB
## Original Mic MH-31:
* rearward switch onto position 2

Button | Label | Value
---|---|---
MENU 110   | SSB TX BPF   | 100-2900
MENU 119   | PRMTRC EQ 1 FREQ   | 100
MENU 120   | RMTRC EQ 1 LEVEL   | -8
MENU 121   | PRMTRC EQ 1 BTWH   | 1
MENU 122   | PRMTRC EQ 2 FREQ   | 700
MENU 123   | PRMTRC EQ 2 LEVEL   | -8
MENU 124   | PRMTRC EQ 2 BTWH   | 2
MENU 125   | PRMTRC EQ 3 FREQ   | 2300
MENU 126   | PRMTRC EQ 3 LEVEL   | 8
MENU 127   | PRMTRC EQ 3 BTWH   | 1
F   | MIC-EQ   | ON
F   | PROC   | OFF
F   | MIC-GAIN  | 55
F   | WIDTH   | 2200
F   | AGC   | SLOW

Good starting point. A lot of pileups won with poor equipment.


# CW MODE

Button | Label | Value
---|---|---
MENU 012   | KEYER TYPE   | ELEKEY-B
MENU 013   | KEYER DOT/DASH | REV
MENU 014   | CW WEIGHT   | 3.0
MENU 050   | CW LCUT FREQ   | 800Hz
MENU 051   | CW LCUT SLOPE   | 18dB/oct
MENU 052   | CW HCUT FREQ  | 900Hz
MENU 053   | CW HCUT SLOPE   | 18dB/oct
MENU 054   | CW OUT LEVEL   | 50
MENU 055   | CW AUTO MODE   | OFF
MENU 056   | CW BK-IN TYPE   | FULL
MENU 057   | CW BK-IN DELAY   | 200msec
MENU 058   | CW WAVE SHAPE   | 4msec
MENU 059   | CW FREQ DISPLAY   | DIRECT FREQ 
F   | AGC   | FAST


# Remote Control
You can access the device via /dev/ttyUSBx, but that may change. That is why you should address it via /dev/serial/by-id/... .

## Power

Power On: (Does not work with Archlinux hamlib from the extra repository. The hamlib-git from the AUR works.)

> **rigctl -m 1035 -r /dev/serial/by-id/usb-Silicon_Labs_CP2105_Dual_USB_to_UART_Bridge_Controller_00F8F9FA-if00-port0 set_powerstat 1**

Power Off:

> **rigctl -m 1035 -r /dev/serial/by-id/usb-Silicon_Labs_CP2105_Dual_USB_to_UART_Bridge_Controller_00F8F9FA-if00-port0 set_powerstat 0**

Power Status:

> **rigctl -m 1035 -r /dev/serial/by-id/usb-Silicon_Labs_CP2105_Dual_USB_to_UART_Bridge_Controller_00F8F9FA-if00-port0 get_powerstat**

## Set Channel

### 70cm CW
e.g. 70cm CW call frequency, 300 Hz bandwidth, 5 Watt PEP, no squelch, 700 Hz sidetone, LOCK on:

> **rigctl -m 1035 -r /dev/serial/by-id/usb-Silicon_Labs_CP2105_Dual_USB_to_UART_Bridge_Controller_00F8F9FA-if00-port0 F 432050000 M CW 300 L RFPOWER 0.050000 L SQL 0.000000 L AGC 2 L CWPITCH 700 U LOCK 1**


### LW AM
e.g. Listen to BBC Radio 4 on Longwave, 198 kHz, AM Narrow, IPO = AMP2, Lock on:

> **rigctl -m 1035 -r /dev/serial/by-id/usb-Silicon_Labs_CP2105_Dual_USB_to_UART_Bridge_Controller_00F8F9FA-if00-port0 F 198000 M AMN 6000 L RFPOWER 0.050000 L PREAMP 20 U LOCK 1**


### CB DATA-USB
e.g. Listen to JS8Call on CB Radio Channel 25 DATA-USB, 27.245 MHz, IPO = IPO, AGC off, Attenuator off, Lock on:

> **rigctl -m 1035 -r /dev/serial/by-id/usb-Silicon_Labs_CP2105_Dual_USB_to_UART_Bridge_Controller_00F8F9FA-if00-port0 F 27245000 M PKTUSB 3000 L RFPOWER 0.050000 L PREAMP 0 L AGC 0 L ATT 0 U LOCK 1**

### 70cm POCSAG
e.g. Decode POCSAG on 439.98750 MHz, 25000 Hz bandwidth (only 16 kHz possible), 5 Watt PEP, no squelch, Lock on:

> **rigctl -m 1035 -r /dev/serial/by-id/usb-Silicon_Labs_CP2105_Dual_USB_to_UART_Bridge_Controller_00F8F9FA-if00-port0 F 439987500 M PKTFM 25000 L SQL 0.000000 L RFPOWER 0.050000 U LOCK 1**

**check squelch!**

**make sure that the computer volume is loud enough! (e.g. alsamixer, pactl, pacmd)**

Record with parec and pipe into multimon-ng:
> **parec -r -d alsa_input.usb-Burr-Brown_from_TI_USB_Audio_CODEC-00.analog-stereo --format=s16le --rate=22050 --channels=1 --raw | multimon-ng -t raw -c -a POCSAG1200 --timestamp -**

# Firmareupdate

There is a new MAIN Firmware Ver. 02-07 (06/18/24) for the FT991A available.

**THIS FIRMWARE IS NOT COMPATIBLE WITH THE FT991 TRANSCEIVER!!!**

[Product Website](https://www.yaesu.com/indexVS.cfm?cmd=DisplayProducts&ProdCatID=102&encProdID=490C4A71118AD0F4E825E89D821B73BB)

