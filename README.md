# dht22 Kernel Driver

A DHT22 kernel driver for Raspberry Pi.
See the specification on http://www.electrodragon.com/w/AM2302

History:

 - Original code from https://github.com/KermsGit/dht22
 - Added fix in https://github.com/chrisice/dht22 for compiling when using kernel 5.10.52 v7+
 - Changed pin from 4 to 12
 - Better driver messages
 
Add the "bin/dht22.dtbo" to the "/boot/overlays/" directory and add a lines with

```
# Enable DHT22 sensor
dtoverlay=dht22,gpiopin=12
```
to the /boot/config.txt

Do a "make" and a "modprobe industrialio; insmod dht22.ko" in the dmesg you should see now:

```
[  273.097098] chksum_ok = 1
[  273.097107] humidity = 42.0% RH
[  273.097116] temperature = 21.0° C
```

A cat "/proc/dht22/gpio04" shows:

```
root@pixel:~/Source/kernel/dht22# cat /proc/dht22/gpio04

DHT22 on gpio pin 12:
  temperature = 21.9° C
  humidity = 38.0% RH
  timestamp = 259
  no checksum error
```

To get the raw data you can read the values in the sys file system:

```
 /sys/bus/iio/devices/iio\:device0/in_temp_input
 /sys/bus/iio/devices/iio\:device0/in_humidityrelative_input
```

It shows the sensor values as integers.

You can copy the dht22.ko to the /lib/modules/`uname -r`/kernel/drivers/iio/humidity/ than the system  should load the modul automaticaly.
Do not forget to make a "depmod -a".

At the moment the driver is in a early beta stage.
But you should not get read errors.


## Install using make
Clone repo
cd to repo directory
run ```make```
run ```make install``` to place the necessary files in the kernel modules directory and /boot/overlays
reboot

You can verify that it is working correctly by running this command, which should show similar output
```
# cat /sys/devices/platform/dht22\@0/iio\:device0/in_humidityrelative_input 
59.400000000
```
