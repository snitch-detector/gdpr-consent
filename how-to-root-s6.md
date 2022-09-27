# How to root the Samsung Galaxy S6

See [here](https://www.naldotech.com/root-7-0-nougat-firmware-galaxy-s6-s6-edge-supersu/) and [here](https://www.naldotech.com/install-twrp-7-0-nougat-samsung-galaxy-s6-s6-edge/)
Assumes that you have `adb` installed on your Linux host.

## Preparation

We also install the CA cert here because it's easier to do directly as we cannot remount `/system` later.

- download [SuperSU root file](https://forum.xda-developers.com/attachment.php?attachmentid=4069354&d=1489158227)
- place the zip on the device e.g. `/sdcard/` (you can use `adb push`)
- place the modified CA cert at `/sdcard/` (in our case `c8750f0d.0`)
   - fetched from http://mitm.it behind proxy (I think the linux pem works)
   - modified as described [here](https://stackoverflow.com/questions/13981011/cacerts-bks-does-not-exist/18390177#18390177)

## Install heimdall

`sudo apt install heimdall-flash`

## Install TWRP

- Download TWRP image [here](https://dl.twrp.me/zeroflte/) as `recovery.img`
- Make sure that device is loaded
- Power On and connect device to host
- `adb reboot download`
  - reboots device into download mode
- `heimdall detect`
  - to see if heimdall finds device
- heimdall flash --RECOVERY recovery.img --no-reboot
- power off using `VOL-DOWN + HOME + OFF`
- boot into recovery using `VOL-UP + HOME + OFF`
  - you should see the _teamwin recovery project_ screen

## Install the CA cert

- click `Mount`
- check `System` and go back
- click `Advanced` then `Terminal`
- `cp /sdcard/c8750f0d.0 /system/etc/security/cacerts/c8750f0d.0`
- `chmod 644 /system/etc/security/cacerts/c8750f0d.0`
- go back

## Install SuperSU

- click `Install`
- choose the SuperSU zip
- swipe
- you may clear cache and dalvik code
- reboot

## Configure SuperSU

- open SuperSU App
- go to Settings
- enable `Enable Superuser`
- disable `Re-authentication`
- set `Default access` to `[Grant]`
- check `Trust System User`

## Test
- `adb shell`
- `su` should grant super user, if not check phone for permission pop-up

## Install Frida Server

- `adb push frida-server-15.2.2-android-arm64 /sdcard/frida-server`
- `adb shell`
    - `su`
    - `mv /sdcard/frida-server /data/local/tmp/frida-server`
    - `chmod +x /data/local/tmp/frida-server`
- start: `adb shell su 0 "/data/local/tmp/frida-server &"`
