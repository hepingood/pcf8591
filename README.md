<div align=center>
<img src="/doc/image/logo.png"/>
</div>

## LibDriver PCF8591

[English](/README.md) | [ 简体中文](/README_CN.md)

The PCF8591 is a single-chip, single-supply low-power 8-bit CMOS data acquisition device with four analog inputs, one analog output and a serial I2C-bus interface. Three address pins A0, A1 and A2 are used for programming the hardware address, allowing the use of up to eight devices connected to the I2C-bus without additional hardware. Address, control and data to and from the device are transferred serially via the two-line bidirectional I2C-bus. The functions of the device include analog input multiplexing, on-chip track and hold function, 8-bit analog-to-digital conversion and an 8-bit digital-to-analog conversion. The maximum conversion rate is given by the maximum speed of the I2C-bus.PCF8591 is used in closed-loop control system, low-power converter for remote data acquisition, battery powered equipment and analog acquisition in automotive audio TV applications.

LibDriver PCF8591 is a full function driver of PCF8591 launched by LibDriver.It provides AD reading, DA output, AD continuous reading and other functions.

### Table of Contents

  - [Instruction](#Instruction)
  - [Install](#Install)
  - [Usage](#Usage)
    - [example basic](#example-basic)
    - [example increment](#example-increment)
  - [Document](#Document)
  - [Contributing](#Contributing)
  - [License](#License)
  - [Contact Us](#Contact-Us)

### Instruction

/src includes LibDriver PCF8591 source files.

/interface includes LibDriver PCF8591 IIC platform independent template。

/test includes LibDriver PCF8591 driver test code and this code can test the chip necessary function simply。

/example includes LibDriver PCF8591 sample code.

/doc includes LibDriver PCF8591 offline document.

/datasheet includes PCF8591 datasheet。

/project includes the common Linux and MCU development board sample code. All projects use the shell script to debug the driver and the detail instruction can be found in each project's README.md.

### Install

Reference /interface IIC platform independent template and finish your platform IIC driver.

Add /src, /interface and /example to your project.

### Usage

#### example basic

```C
uint8_t res;
uint8_t i;
int16_t raw;
float adc;

res = pcf8591_basic_init(PCF8591_ADDRESS_A000, PCF8591_MODE_AIN0123_GND);
if (res)
{
    pcf8591_interface_debug_print("pcf8591: init failed.\n");

    return 1;
}

...

res = pcf8591_basic_write(1.2f);
if (res)
{
    pcf8591_interface_debug_print("pcf8591: write failed.\n");
    pcf8591_basic_deinit();

    return 1;
}

...

res = pcf8591_basic_set_channel(PCF8591_CHANNEL_0);
if (res)
{
    pcf8591_interface_debug_print("pcf8591: set channel failed.\n");
    pcf8591_basic_deinit();

    return 1;
}

...

/* read prev data */
res = pcf8591_basic_read((int16_t *)&raw, (float *)&adc);
if (res)
{
    pcf8591_interface_debug_print("pcf8591: read failed.\n");
    pcf8591_basic_deinit();

    return 1;
}
pcf8591_interface_delay_ms(1000);
for (i = 0; i < 3; i++)
{
    /* read data */
    res = pcf8591_basic_read((int16_t *)&raw, (float *)&adc);
    if (res)
    {
        pcf8591_interface_debug_print("pcf8591: read failed.\n");
        pcf8591_basic_deinit();

        return 1;
    }
    pcf8591_interface_debug_print("pcf8591: adc is %0.3f.\n", adc);
    pcf8591_interface_delay_ms(1000);
    
    ...
    
}

...

pcf8591_basic_deinit();

return 0;
```

#### example increment

```C
uint8_t res;
int16_t raws[4];
float adcs[4];
uint8_t len;
uint8_t i, j;

res = pcf8591_increment_init(PCF8591_ADDRESS_A000, PCF8591_MODE_AIN0123_GND);
if (res)
{
    pcf8591_interface_debug_print("pcf8591: init failed.\n");

    return 1;
}

...

res = pcf8591_increment_write(1.8f);
if (res)
{
    pcf8591_interface_debug_print("pcf8591: write failed.\n");
    pcf8591_basic_deinit();

    return 1;
}

...

/* read prev data */
len = 4;
res = pcf8591_increment_read((int16_t *)raws, (float *)adcs, (uint8_t *)&len);
if (res)
{
    pcf8591_interface_debug_print("pcf8591: increment read failed.\n");
    pcf8591_increment_deinit();

    return 1;
}
pcf8591_interface_delay_ms(1000);
for (i = 0; i < 3; i++)
{
    /* read data */
    len = 4;
    res = pcf8591_increment_read((int16_t *)raws, (float *)adcs, (uint8_t *)&len);
    if (res)
    {
        pcf8591_interface_debug_print("pcf8591: read failed.\n");
        pcf8591_increment_deinit();

        return 1;
    }
    for (j = 0; j < len; j++)
    {
        pcf8591_interface_debug_print("pcf8591: raw is %d.\n", raws[j]);
        pcf8591_interface_debug_print("pcf8591: adc is %0.3f.\n", adcs[j]);
        
        ...
        
    }
    pcf8591_interface_delay_ms(1000);
    
    ...
    
}

...

pcf8591_increment_deinit();

return 0;
```

### Document

Online documents: https://www.libdriver.com/docs/pcf8591/index.html

Offline documents: /doc/html/index.html

### Contributing

Please sent an e-mail to lishifenging@outlook.com

### License

Copyright (C) LibDriver 2015-2021 All rights reserved 



The MIT License (MIT) 



Permission is hereby granted, free of charge, to any person obtaining a copy

of this software and associated documentation files (the "Software"), to deal

in the Software without restriction, including without limitation the rights

to use, copy, modify, merge, publish, distribute, sublicense, and/or sell

copies of the Software, and to permit persons to whom the Software is

furnished to do so, subject to the following conditions: 



The above copyright notice and this permission notice shall be included in all

copies or substantial portions of the Software. 



THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR

IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,

FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE

AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER

LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,

OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE

SOFTWARE. 

### Contact Us

Please sent an e-mail to lishifenging@outlook.com