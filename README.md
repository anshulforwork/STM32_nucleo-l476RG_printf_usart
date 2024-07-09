
# STM32 Nucleo Board: Using `printf` with USART2

This project demonstrates how to use the `printf` function with USART2 on an STM32 Nucleo board. The code continuously prints "Hello World!" with an incrementing counter to a serial terminal via USART2.

## Table of Contents

- [Introduction](#introduction)
- [Hardware Requirements](#hardware-requirements)
- [Software Requirements](#software-requirements)
- [Project Setup](#project-setup)
  - [Hardware Setup](#hardware-setup)
  - [Software Setup](#software-setup)
- [Code Explanation](#code-explanation)
  - [UART Configuration](#uart-configuration)
  - [Data Transmission](#data-transmission)
  - [Main Function](#main-function)
- [Compilation and Upload](#compilation-and-upload)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [Acknowledgments](#acknowledgments)

## Introduction

This project provides a method to use the `printf` function for sending data over USART2 on an STM32 Nucleo board. This is useful for debugging and monitoring variables during development.

## Hardware Requirements

- STM32 Nucleo Board (Nucleo-L476RG)
- USB cable for connecting the Nucleo board to the PC

## Software Requirements

- STM32CubeIDE
- A terminal program (such as PuTTY or Tera Term)

## Project Setup

### Hardware Setup

1. Connect the STM32 Nucleo board to your PC using the USB cable.
2. Ensure that the board is powered on and recognized by your PC.

### Software Setup

1. Open STM32CubeIDE.
2. Create a new STM32 project.
3. Configure the project for the  Nucleo-L476RG board.
4. Initialize all configured peripherals (USART2).

## Code Explanation

### UART Configuration

Ensure USART2 is properly configured in your project. Here is a basic setup in the `main.c` file:

```c

#include <stdio.h>
```

### Data Transmission

Add the following function to handle data transmission over USART2:

```c
static void UWriteData(const char data);

void UWriteData(const char data)
{
    while(__HAL_UART_GET_FLAG(&huart2, UART_FLAG_TXE) == RESET);
    huart2.Instance->TDR = data;
}

int __io_putchar(int ch)
{
    UWriteData(ch);
    return ch;
}
```

### Main Function

Add the following code in the `main.c` file to print "Hello World!" repeatedly:

```c
#include "main.h"
#include "usart.h"

int main(void)
{
    HAL_Init();
    SystemClock_Config();
    MX_GPIO_Init();
    MX_USART2_UART_Init();

    setbuf(stdout, NULL); // Disable buffering of stdout

  while (1)
  {
    /* USER CODE END WHILE */
	  for(int i=0;i<100;i++)
	  {
	  	printf("Hello World ! %d\r\n",i);// for the usart printf
	  	HAL_Delay(1000);
	  }
    /* USER CODE BEGIN 3 */
  }
```

## Compilation and Upload

1. Build the project in STM32CubeIDE.
2. Connect your STM32 Nucleo board to your PC.
3. Upload the compiled binary to the board.

## Usage

1. Open a terminal program on your PC.
2. Connect to the virtual COM port associated with USART2.
3. Set the baud rate to 9600 (or as configured).
4. Monitor the output, which should display "Hello World!" followed by an incrementing number every second.

## Troubleshooting

- Ensure the correct COM port is selected in your terminal program.
- Verify the baud rate matches the configuration in your code.
- Check connections and power to the STM32 Nucleo board.

## Contributing

Contributions are welcome! Please fork this repository and submit a pull request for any enhancements or bug fixes.


## Acknowledgments

- Thanks to the STM32 community for their support and resources.
- Special thanks to STMicroelectronics for their development tools and documentation.
