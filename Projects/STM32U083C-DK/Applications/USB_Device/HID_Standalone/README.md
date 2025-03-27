## HID_Standalone Application Description

Use of the USB device application based on the Human Interface (HID).

This is a typical application on how to use the stm32u0xx USB Device peripheral, where the STM32 MCU is
enumerated as a HID device using the native PC Host HID driver to which the STM32U083C-DK
board is connected, in order to emulate the Mouse directions using User push-button mounted on the
STM32U083C-DK board.

At the beginning of the main program the HAL_Init() function is called to reset all the peripherals,
initialize the Flash interface and the systick. The user is provided with the SystemClock_Config() function
to configure the clock (SYSCLK).

#### <b>Expected success behavior</b>

When plugged to PC host, the STM32U083C-DK must be properly enumerated as a USB HID mouse device.
During the enumeration phase, device provides host with the requested descriptors (device, configuration, string).
Those descriptors are used by host driver to identify the device capabilities.
Once the STM32U083C-DK USB device successfully completed the enumeration phase, the device sends an HID report after a user button press.
Each report sent should move the mouse cursor by one step on host side.

#### <b>Error behaviors</b>

  - LED_RED is toggling to indicate a critical error has occured.

#### <b>Assumptions if any</b>

User is familiar with USB 2.0 "Universal Serial BUS" specification and HID class specification.

#### <b>Known limitations</b>

None

#### Notes

 1. Care must be taken when using HAL_Delay(), this function provides accurate delay (in milliseconds)
      based on variable incremented in SysTick ISR. This implies that if HAL_Delay() is called from
      a peripheral ISR process, then the SysTick interrupt must have higher priority (numerically lower)
      than the peripheral interrupt. Otherwise the caller ISR process will be blocked.
      To change the SysTick interrupt priority you have to use HAL_NVIC_SetPriority() function.

 2. The application needs to ensure that the SysTick time base is always set to 1 millisecond
      to have correct HAL operation.

For more details about the STM32Cube USB Device library, please refer to UM1734
"STM32Cube USB Device library".

### Keywords

Connectivity, USB_Device, USB, HID, Full Speed, Mouse, Remote Wakeup

### Directory contents

  - USB_Device/HID_Standalone/Core/Src/main.c                     Main program
  - USB_Device/HID_Standalone/Core/Src/stm32u0xx_hal_msp.c        MSP Initialization and de-Initialization codes
  - USB_Device/HID_Standalone/Core/Src/system_stm32u0xx.c         STM32U0xx system clock configuration file
  - USB_Device/HID_Standalone/Core/Src/stm32u0xx_it.c             Interrupt handlers
  - USB_Device/HID_Standalone/USB_Device/Target/usbd_conf.c       General low level driver configuration
  - USB_Device/HID_Standalone/USB_Device/App/usbd_desc.c          USB device HID descriptor
  - USB_Device/HID_Standalone/USB_Device/App/usbd_device.c        USB Device
  - USB_Device/HID_Standalone/Core/Inc/main.h                     Main program header file
  - USB_Device/HID_Standalone/Core/Inc/stm32u0xx_it.h             Interrupt handlers header file
  - USB_Device/HID_Standalone/Core/Inc/stm32u0xx_hal_conf.h       HAL configuration file
  - USB_Device/HID_Standalone/USB_Device/Target/usbd_conf.h       USB device driver Configuration file
  - USB_Device/HID_Standalone/USB_Device/App/usbd_desc.h          USB device HID descriptor header file
  - USB_Device/HID_Standalone/USB_Device/App/usbd_device.h        USB Device header

### Hardware and Software environment

  - This application runs on STM32U0xx devices.

  - This application has been tested with STMicroelectronics STM32U083C-DK board MB1933 Rev. B01
    and can be easily tailored to any other supported device and development board.

  - STM32U083C-DK board Set-up
    - Connect the STM32U083C-DK board CN14 to the PC through "TYPE-C" to "Standard A" cable.
    - Press the User push-button  to move the cursor.

### How to use it ?

In order to make the program work, you must do the following :
 - Open your preferred toolchain
 - Rebuild all files and load your image into target memory
 - Run the application


