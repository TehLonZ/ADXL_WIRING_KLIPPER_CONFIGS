# ADXL_WIRING_KLIPPER_CONFIGS
Wiring Diagram and Klipper Config examples for Input Shaper

Repository for multiple ADXL configurations for Klipper FW Input Shaper. 

Materials described:
-----------------------------------------
XIAO SAMD21

https://www.seeedstudio.com/Seeeduino-XIAO-Arduino-Microcontroller-SAMD21-Cortex-M0+-p-4426.html

https://www.adafruit.com/product/4600

-----------------------------------------
XIAO RP2040

https://www.seeedstudio.com/XIAO-RP2040-v1-0-p-5026.html?queryID=63ae1d95dede3b94025ce6da26106e2b&objectID=5026&indexName=bazaar_retailer_products

https://www.adafruit.com/product/4900

-----------------------------------------------------
Raspberry Pi 3 - 4

https://www.raspberrypi.com/products/

-------------------------------------------------

Adafruit ADXL345 

* Other vendors may sell in similar format. 

https://www.adafruit.com/product/1231

-------------------------------------------

Configuration:
--------------
Follow secondary mcu instructions:

https://www.klipper3d.org/Measuring_Resonances.html

https://www.klipper3d.org/RPi_microcontroller.html



RPI to ADXL 

![image](https://user-images.githubusercontent.com/94410881/161445656-e775ffca-72fe-45c6-a7d3-df76b7fa07a1.png)

Connections:


![image](https://user-images.githubusercontent.com/94410881/161445704-dc11f1b1-6aa1-4155-9f02-131c4f77b3e0.png)

Klipper Config:

![image](https://user-images.githubusercontent.com/94410881/161445837-7c8a0a4f-a6e7-42c4-af99-2431b4007f0e.png)

```
#Using RPI GPIO 

[mcu rpi]
serial: /tmp/klipper_host_mcu

[adxl345]
cs_pin: rpi:None

[resonance_tester]
accel_chip: adxl345
probe_points:
    100, 100, 20  # an example
```

------------------------------------------

XIAO SAMD21

Follow flashing instructions via: 
Credit to : EsserPrototyping

https://github.com/TehLonZ/Seeed-XIAO-with-ADXL345-for-Klipper

https://github.com/EsserPrototyping/Seeed-XIAO-with-ADXL345-for-Klipper

To assist in finding Dev path for device use before flashing use:
```
#!/bin/bash

for sysdevpath in $(find /sys/bus/usb/devices/usb*/ -name dev); do
    (
        syspath="${sysdevpath%/dev}"
        devname="$(udevadm info -q name -p $syspath)"
        [[ "$devname" == "bus/"* ]] && exit
        eval "$(udevadm info -q property --export -p $syspath)"
        [[ -z "$ID_SERIAL" ]] && exit
        echo "/dev/$devname - $ID_SERIAL"
    )
done
```

Replace "dev/ttyACM1" below with path matching your device. 
```
sudo /usr/local/bin/bossac -i -d -p /dev/ttyACM1 -e -w -v -R --offset=0x2000 out/klipper.bin
```

![image](https://user-images.githubusercontent.com/94410881/161445991-78d8c25f-590b-4f82-9e22-6f0cd273ad15.png)


Connections:

![image](https://user-images.githubusercontent.com/94410881/161446057-180c67f9-82df-4b44-9871-63d36eef52f8.png)

Klipper Config:

![image](https://user-images.githubusercontent.com/94410881/161446095-f3f6bf27-a62e-4613-bafd-04c38d9bceff.png)

```
#Using XIAO SAMD21/25

[mcu xiao]
serial: /dev/serial/by-id/usb-Klipper_samd21g18a_

[samd_sercom my_sercom]
sercom: sercom0
tx_pin: xiao:PA6
rx_pin: xiao:PA5
clk_pin: xiao:PA7

[adxl345]
cs_pin: xiao:PB9

[resonance_tester]
accel_chip: adxl345
probe_points:
   55,55,20


#End ADXL Section.

```

-------------------------------------

XIAO RP2040

Flashing Instructions: 
https://github.com/TehLonZ/Xiao_rp2040_ADXL_Klipper

Adafruit QT PY used for Fritzing modeling. 

![image](https://user-images.githubusercontent.com/94410881/161446211-8b854d00-8524-45bb-bf31-4d5a66eb7517.png)

Connections:

![image](https://user-images.githubusercontent.com/94410881/161446242-f995a6a4-245a-4b65-8232-a7d077a7c436.png)

Klipper Config:

![image](https://user-images.githubusercontent.com/94410881/161446280-9d714d21-5a4c-4432-a2ee-6153e246fa00.png)

```
#USING XIAO RP2040
[mcu pico]
serial:  /dev/serial/by-id/usb-Klipper_rp2040_

[adxl345]
cs_pin: pico:gpio1
spi_bus: spi0a
axes_map: x,z,y

[resonance_tester]
accel_chip: adxl345
probe_points:
   100,100,20
   
```
