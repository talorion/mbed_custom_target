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


## create folder for custom code

~~~
mkdir custom.mbed-os
mkdir -p custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/TARGET_WD_CORE_G1
mkdir -p custom.mbed-os/targets/TARGET_STM/TARGET_STM32F4/TARGET_STM32F437xV/device
~~~


