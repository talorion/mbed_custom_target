# SPIKE for crating a custom target in arm mbed-os

i want to try out to create a custom target WD_CORE_G1 for [mbed-os](https://github.com/ARMmbed/mbed-os)


## Prerequisites

* use a [STM32F437VI](https://www.st.com/content/st_com/en/products/microcontrollers-microprocessors/stm32-32-bit-arm-cortex-mcus/stm32-high-performance-mcus/stm32f4-series/stm32f427-437/stm32f437vi.html) MCU
* use a ST-Link with FW V2.J33.M25 as Programmer
* Use GCC_ARM toolchain
* utalize existing STM-targets to minimize code duplication
* stick to the Dcoumentation on [Adding and configuring targets](https://os.mbed.com/docs/mbed-os/v5.10/tools/adding-and-configuring-targets.html)

## Versions

* Windows 10
* vscode 1.37.0
* mbed --version: 1.10.0
* mbed 5.13.3 SHA:808c45062f940790e1287bd6b8420290c062775c

## add mbed-os

~~~
git submodule add https://github.com/ARMmbed/mbed-os.git
~~~

# init mbed os

create .mbed
~~~
touch .mbed
~~~

edit .mbed:
~~~
TOOLCHAIN=GCC_ARM
ROOT=.
TARGET=WD_CORE_G1
~~~


## create custom_targets.json

~~~
touch custom_targets.json
~~~

add target according to 
[Adding and configuring targets](https://os.mbed.com/docs/mbed-os/v5.10/tools/adding-and-configuring-targets.html)

to test if the newly createdtarget is valid i had to put the contents of custom_targets.json into mbed-os\targets\targets.json since the linting script in tools/targets/lint.py seems not to work with custom_targets.json

~~~
python mbed-os/tools/targets/lint.py targets BOARD_WD_CORE_G1
hierarchy: !!python/unicode 'Family (FAMILY_STM32) -> MCU (MCU_STM32F437) -> Board
  (BOARD_WD_CORE_G1)'
target errors:
  !!python/unicode 'BOARD_WD_CORE_G1':
  - _from_file found, and is not allowed
  !!python/unicode 'FAMILY_STM32':
  - !!python/unicode 'macros found, and is not allowed'
  - !!python/unicode 'overrides found, and is not allowed'
  - _from_file found, and is not allowed
  - !!python/unicode 'USTICKER is not allowed in device_has'
  - !!python/unicode 'STDIO_MESSAGES is not allowed in device_has'
  - !!python/unicode 'WATCHDOG is not allowed in device_has'
  - !!python/unicode 'RESET_REASON is not allowed in device_has'
  !!python/unicode 'MCU_STM32F437':
  - _from_file found, and is not allowed
  - !!python/unicode 'MPU is not allowed in device_has'
~~~


## create mbed_app.json

to utalize mbed  test configure platform.stdio and target.stdio 

~~~
touch mbed_app.json
~~~

to check how configuration is written use mbed compile --config

~~~
mbed compile -t gcc_arm -m BOARD_WD_CORE_G1 --config 
~~~

## setup compile toolchain for vscode

~~~
mkdir .vscode
touch .vscode/tasks.json
~~~

add tasks for compiling and running specific tests

## add minimal files needed according to other STM targets

create folders
~~~
mkdir custom.mbed-os
mkdir -p custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/TARGET_WD_CORE_G1
mkdir -p custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device
mkdir -p custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/TOOLCHAIN_GCC_ARM
~~~

create files
~~~
touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/objects.h

touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/cmsis_nvic.h
touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/flash_data.h
touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/stm32f437xx.h
touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/stm32f4xx.h
touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/system_init_pre.c
touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/system_stm32f4xx.h
touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/us_ticker_data.h

touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/TOOLCHAIN_GCC_ARM/startup_stm32f437xx.S
touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/TOOLCHAIN_GCC_ARM/STM32F437xV.ld

touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/TARGET_WD_CORE_G1/PeripheralNames.h
touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/TARGET_WD_CORE_G1/PeripheralPins.c
touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/TARGET_WD_CORE_G1/PinNames.h
touch custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/TARGET_WD_CORE_G1/system_clock.c
~~~

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/objects.h

using the same as in a prevoius implementaion (which works)
rewritten 

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/cmsis_nvic.h

using the same as in a prevoius implementaion (which works)

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/flash_data.h

rewritten 

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/stm32f437xx.h

using the same as in a prevoius implementaion (which works)

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/stm32f4xx.h

using the same as in a prevoius implementaion (which works)

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/system_init_pre.c

using the same as in a prevoius implementaion (which works)

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/system_stm32f4xx.h

using the same as in a prevoius implementaion (which works)

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/us_ticker_data.h

using the same as mbed-os\targets\TARGET_STM\TARGET_STM32F4\TARGET_STM32F429xI\device\us_ticker_data.h

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/TOOLCHAIN_GCC_ARM/startup_stm32f437xx.S

using a file generated by STM32CubeMX

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device/TOOLCHAIN_GCC_ARM/STM32F437xV.ld

using the same as mbed-os\targets\TARGET_STM\TARGET_STM32F4\TARGET_STM32F429xI\device\TOOLCHAIN_GCC_ARM\STM32F429xI.ld

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/TARGET_WD_CORE_G1/PeripheralNames.h

using the same as in a prevoius implementaion (which works)
rewritten 

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/TARGET_WD_CORE_G1/PeripheralPins.c

using the same as in a prevoius implementaion (which works)

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/TARGET_WD_CORE_G1/PinNames.h

rewritten 

### custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/TARGET_WD_CORE_G1/system_clock.c

using the same as in a prevoius implementaion (which works)


## setup mbedls so it recognises the new target

~~~
touch mbedls.json
~~~