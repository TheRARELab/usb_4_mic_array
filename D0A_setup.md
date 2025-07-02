
# ReSpeaker USB 4 Mic Array
Available at [Seeed](https://www.seeedstudio.com/ReSpeaker-Mic-Array-v2.0-p-3053.html)


## Background Knowledge
[Manual](https://wiki.seeedstudio.com/ReSpeaker-USB-Mic-Array/#doa-direction-of-arrival)

[Seed Repository](https://github.com/respeaker/usb_4_mic_array.git)

## One-Time DOA (Direction of Arrival) Setup



**Step 1:** Plug the mic into your device

**Step 2:** Switch to this directory and update/download libraries
```
cd Directional_mic # change into the repository
sudo apt-get update 
pip install -r requirements.txt
```

**Step 3:** Run the DOA.py script
After you run DOA.py, the terminal will display the direction of the sound source in degrees by using the `pyusb` library and `Tuning` from `tuning.py` (for example when speaking from the USB micro port it will display 270° and then 0 from when going 90° to the right)
```
python3 DOA.py
```

## Troubleshooting
If you run into an error like `"usb.core.USBError: [Errno 13] Access denied (insufficient permissions)"` 

Follow these steps:

1. `lsusb` to identify the USB device you will see something like 
```
Bus 001 Device 004: ID 2886:0018 Seeed Technology Co., Ltd. ...
```
2. Create a udev rule to give permission permananetly 
```
sudo nano /etc/udev/rules.d/99-respeaker.rules
```
3. Paste this line into the udev file and replace the values with the device you want to use
```
SUBSYSTEM=="usb", ATTR{idVendor}=="2886", ATTR{idProduct}=="0018", MODE="0666", GROUP="plugdev"

```
4. Reload the rules
```
sudo udevadm control --reload
sudo udevadm trigger
```

5. Reboot the Pi and update/download libraries
```
sudo reboot
cd Directional_mic # change into the repository
sudo apt-get update 
pip install pyusb click # or do pip install -r requirements.txt
python dfu.py --download new_firmware.bin       #  with sudo if usb permission error
sudo python3 dfu.py --download 6_channels_firmware.bin # The 6 channels version
```

It should be able to work and rerun 
```python3 DOA.py```
