# Connecting a Harmony remote to a raspberry pi

1. TOC
{:toc}

## Introduction

This is a little stub post for me. I have a Logitech Harmony remote that I use to control a raspberry pi running Kodi for my media center. Occasionally I have to reinstall it and I always have to google a few things to remember how to do it, so this is just to consolidate that info.

## Get the Harmony Bluetooth ID

Since I don't have a GUI on my pi beyond Kodi, I have to do all the connecting over the command line. Which means it will be much easier if I have the device ID for the remote available. To find it I pair the remote with a Windows machine, and then from ```Control Panel\Hardware and Sound\Devices and Printers``` I can right click on the device, bring up its properties, and from the bluetooth tab get the unique identifier. For my remote that's ```00:04:20:f8:65:d1```.

## Create a device in Harmony

From the Harmony app go to create a new device. Under the manufacturer choose Microsoft and for the device choose Kodi. I always forget this part and just try and set it up as a generic PC, which doesn't work. When it gets to the pairing part of the setup we can connect from the pi

## Connect from the pi

SSH into the pi:

```bash
sudo bluetoothctl
agent on
default-agent
pair 00:04:20:f8:65:d1
connect 00:04:20:f8:65:d1
trust 00:04:20:f8:65:d1
```

That should do it. Harmony will say it's connected, you can also check with ```sudo bluetoothctl paired-devices```.
