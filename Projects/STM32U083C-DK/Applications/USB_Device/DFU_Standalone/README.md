## DFU_Standalone Application Description

Compliant implementation of the Device Firmware Upgrade (DFU)
capability to program the embedded Flash memory through the USB peripheral.

At the beginning of the main program the HAL_Init() function is called to reset all the peripherals,
initialize the Flash interface and the systick. The user is provided with the SystemClock_Config() function
to configure the clock (SYSCLK).
The DRD IP in full speed mode (FS) uses internally a 48-MHz clock, which is generated from HSI48.
The DFU transactions are based on Endpoint 0 (control endpoint) transfer. All requests and status
control are sent/received through this endpoint.

In this application, two operating modes are available:
 1. DFU operating mode:
    This mode is entered after an MCU reset in case:
     - The DFU mode is forced by the user: the user presses the USER push-button.
     - No valid code found in the application area: a code is considered valid if the MSB of the initial
       Main Stack Pointer (MSP) value located in the first address of the application area is equal to
       0x20000

 2. Run-time application mode:
    This is the normal run-time activities. A binary which toggles all LEDs on the STM32U083C-DK board is
    provided in Binary directory.

#### <b>Expected success behavior</b>

When plugged to PC host, the STM32U083C-DK must be properly enumerated as an USB DFU device.
During the enumeration phase, device provides host with the requested descriptors (device, configuration, string).
Those descriptors are used by host driver to identify the device capabilities.
Once the STM32U083C-DK USB device successfully completed the enumeration phase, open STM32CubeProgrammer you can now download and upload to/from media .

#### <b>Error behaviors</b>

  - LED_RED is toggling to indicate a critical error has occured.
  - Host PC shows that USB device does not appear as DFU device.

#### <b>Assumptions if any</b>

User is familiar with USB 2.0 "Universal Serial BUS" specification and DFU class specification.

#### <b>Known limitations</b>

None

#### Notes

 1. After each device reset (unplug the STM32U083C-DK board from PC), Plug the STM32U083C-DK board with Key USER push-button button
pressed to enter the DFU mode.

 2. A binary which toggles LEDs on the STM32U083C-DK board is provided in Binary directory.

 3. The application needs to ensure that the SysTick time base is set to 1 millisecond
      to have correct HAL configuration.

 4. Care must be taken when using HAL_Delay(), this function provides accurate delay (in milliseconds)
      based on variable incremented in SysTick ISR. This implies that if HAL_Delay() is called from
      a peripheral ISR process, then the SysTick interrupt must have higher priority (numerically lower)
      than the peripheral interrupt. Otherwise the caller ISR process will be blocked.
      To change the SysTick interrupt priority you have to use HAL_NVIC_SetPriority() function.

 5. The built-in USB peripheral of the stm32u0xx does not provide a specific interrupt for USB cable plug/unplug
      detection. The correct way to detect the USB cable plug/unplug is to detect the availability of the VBUS line
      using a normal GPIO pin (external interrupt line).

### Keywords

Connectivity, USB_Device, USB, DFU, Firmware upgrade

### Directory contents

  - USB_Device/DFU_Standalone/Core/Src/main.c                           Main program
  - USB_Device/DFU_Standalone/Core/Src/system_stm32u0xx.c               STM32U0xx system clock configuration file
  - USB_Device/DFU_Standalone/Core/Src/stm32u0xx_it.c                   Interrupt handlers
  - USB_Device/DFU_Standalone/Core/Src/stm32u0xx_hal_msp.c              MSP Initialization and de-Initialization codes
  - USB_Device/DFU_Standalone/USB_Device/App/usb_device.c               USB Device application code
  - USB_Device/DFU_Standalone/USB_Device/App/usb_desc.c                 USB device descriptor
  - USB_Device/DFU_Standalone/USB_Device/App/usbd_dfu_flash.c           Internal flash memory management
  - USB_Device/DFU_Standalone/USB_Device/Target/usbd_conf.c             General low level driver configuration
  - USB_Device/DFU_Standalone/Core/Inc/main.h                           Main program header file
  - USB_Device/DFU_Standalone/Core/Inc/stm32u0xx_it.h                   Interrupt handlers header file
  - USB_Device/DFU_Standalone/Core/Inc/stm32u0xx_hal_conf.h             HAL configuration file
  - USB_Device/DFU_Standalone/USB_Device/App/usb_device.h               USB Device application header file
  - USB_Device/DFU_Standalone/USB_Device/App/usbd_desc.h                USB device descriptor header file
  - USB_Device/DFU_Standalone/USB_Device/App/usbd_dfu_flash.h           Internal flash memory management header file
  - USB_Device/DFU_Standalone/USB_Device/Target/usbd_conf.h             USB device driver Configuration file

### Hardware and Software environment

  - This application runs on STM32U0xx devices.

  - This application has been tested with STMicroelectronics STM32U083C-DK board MB1933 Rev. B01
    and can be easily tailored to any other supported device and development board.

  - STM32U083C-DK board Set-up
    - Connect the STM32U083C-DK board CN14 to the PC through "TYPE-C" to "Standard A" cable.

### How to use it ?

In order to make the program work, you must do the following :
 - Open your preferred toolchain
 - Rebuild all files and load your image into target memory
 - To run the application, proceed as follows:

    - Download the appropriate STM32CubeProgrammer from ST web site and install it.
    - Once you've updated the STM32 DFU device driver you must verify from Windows Device Manager
      that DFU driver is selected with the STM32CubeProgrammer driver.
    - Open "STM32CubeProgrammer", select the usb interface and connect to DFU device.
    - Download the "STM32U083C-DK_LED_Toggle@0x08020000.bin" provided in Binary directory and set the start address.
    - To run the downloaded application, reset the board.
