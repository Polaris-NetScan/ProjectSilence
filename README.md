<h1 align="center">
  <br>
  <a href="https://github.com/mkdirlove/ProjectSilence"><img src="https://github.com/mkdirlove/ProjectSilence/blob/main/logo.png" alt="ProjectSilence
"></a>
  <br>
  Yet another simple tool for Bluetooth Security Assessment.
  <br>
</h1>

#### Installation

Copy-paste this into your terminal:

```sh
git clone https://github.com/Polaris-NetScan/ProjectSilence.git
```
```
cd ProjectSilence
```
```
python3 ProjectSilence.py
```
or
```
python3 ProjectSilence.py -h
```
#### Usage
``` 
╭──────── v1.0-dev ────────╮
│ ┏┓    •      ┏┓•┓        │
│ ┃┃┏┓┏┓┓┏┓┏╋  ┗┓┓┃┏┓┏┓┏┏┓ │
│ ┣┛┛ ┗┛┃┗ ┗┗  ┗┛┗┗┗ ┛┗┗┗  │
│       ┛    By @mkdirlove │
╰──────────────────────────╯
usage: No interaction recording [-h] -a ADDRESS [-t {BR_EDR,LE_PUBLIC,LE_RANDOM}] [-f OUTFILE] [-s SINK] [-v]

Try to pair to a device, connect to it and record sound without user interaction

options:
  -h, --help            show this help message and exit
  -a ADDRESS, --target-address ADDRESS
                        Target device MAC address
  -t {BR_EDR,LE_PUBLIC,LE_RANDOM}, --target-address-type {BR_EDR,LE_PUBLIC,LE_RANDOM}
                        Target device MAC address type
  -f OUTFILE, --file OUTFILE
                        File to store recorded audio
  -s SINK, --sink SINK  Sink to play the audio back
  -v, --verbose         Show verbose output
```


## Requirements

The code is written in Python and has been tested with Python 3.11.8, but it mainly uses widely available tools in Linux systems.

The PoC uses the following tools:
+ `bluetoothctl`
+ `btmgmt`
+ `pactl`
+ `parecord`
+ `paplay`

In Arch Linux distributions, `bluetoothctl` and `btmgmt` can be installed with the package `bluez-utils`, while `pactl`, `parecord` and `paplay` are available in the `libpulse` package.

For the PoC to work, it is necessary to have a working installation of the BlueZ Bluetooth stack, available in the `bluez`package for Arch Linux distributions. A working installation of an audio server compatible with PulseAudio, such as PipeWire, is also required to record and play audio.

## Setup

Ensure that your device is capable of functioning as an audio source, meaning it has a microphone, and that it is discoverable and connectable via Bluetooth.

For instance, to be discoverable and connectable, the earbuds used during the talk must be outside of their charging case. By default, they only activate the microphone when placed in the user's ears, although this setting can be adjusted in the configuration app.

Additionally, ensure that the device is not already connected, or alternatively, that it supports multiple connections.

## Execution

Firstly, the address of the device must be discovered using a tool such as `bluetoothctl`:

```
$ bluetoothctl
[bluetooth]# scan on
```

Once the address of the device is discovered, the script can handle the rest:

```
$ python BlueSpy.py -a <address>
```

Note: The script might prompt for superuser permissions to modify the configuration of your **BlueZ** instance and pair it with the remote device.

## Troubleshooting

`BlueSpy.py` is the main script that executes every step of the process. However, if you encounter issues with any of the phases, so it might be helpful to execute them individually:
+ `pair.py` utilizes the command-line tool `btmgmt` to modify the configuration of your **BlueZ** and initiate a pairing process with the remote device. The exact commands used are in the `pair` function inside `core.py`.
+ `connect.py` utilizes the command-line tool `bluetoothctl` to initiate a quick scan (necessary for BlueZ) and establish a connection to the device. The exact commands used are in the `connect` function inside `core.py`.
+ `just_record.py` utilizes the command-line tools `pactl` and `parecord` to search for the device in the system's audio sources (it must function as a microphone) and initiate a recording session. The exact commands used are in the `record` function inside `core.py`.
+ The `playback` function inside `core.py` executes `paplay` to play back the captured audio.

If you encounter issues with any of the phases, examine the commands in `core.py` and try to execute them in a shell. This will provide more information on what may be failing.
