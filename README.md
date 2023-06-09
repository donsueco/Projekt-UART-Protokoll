
UART Driver for STM32F411x Platform, Featuring an LED Application

This project comprises a UART (Universal Asynchronous Receiver-Transmitter) driver written in the C programming language for the STM32F411x microcontroller platform. The driver offers functions to initialize and configure the UART peripheral, transmit and receive data using the UART interface, and manage interrupts generated by the UART peripheral. Additionally, it includes an LED function for demonstration purposes.

Requirements

To utilize this driver, you will need the following components:
	STM32F411x development board
	STM32CubeIDE or another compatible IDE (Integrated Development Environment)
	STM32F411x HAL (Hardware Abstraction Layer) library
	UART cable or a USB-to-UART converter

Usage
	Clone the repository or download the source code as a ZIP file.
	Import the project into your IDE and integrate the required HAL libraries.
	Include the uart_driver.h header file in your application code.
	Initialize the UART peripheral using the USART2_Init() function, inputting the desired baud 	rate and other configuration parameters.

Functions

The UART driver provides the following functions:

	int USART2_write()
    	int USART2_read()

Example

The example below demonstrates how to employ the UART driver to transmit and receive data:

c
#include "uart.h"

void USART2_Init(void) {
    // 1. Enable clock access for UART2 by setting the 17th bit of APB1ENR register
    RCC->APB1ENR |= 0x20000;

    // 2. Enable clock access for Port A by setting the 1st bit of AHB1ENR register
    RCC->AHB1ENR |= 0x01;

    // 3. Enable alternate functions for pins PA2 and PA3
    // Clear bits 4, 5, 6, and 7 of MODER register for GPIOA
    GPIOA->MODER &= ~0x00F0;
    // Set bits 5 and 7 of MODER register for GPIOA to enable alternate function for PA2 and PA3
    GPIOA->MODER |= 0x00A0;

    // 4. Configure the type of alternate function for PA2 and PA3
    // Clear bits 8 to 15 of AFR[0] register for GPIOA
    GPIOA->AFR[0] &= ~0xFF00;
    // Set bits 8 to 11 and 12 to 15 of AFR[0] register for GPIOA to 0x7 for UART2 (PA2: TX, PA3: RX)
    GPIOA->AFR[0] |= 0x7700;


    // Configure UART settings
    // Set the Baud Rate Register value for USART2
    USART2->BRR = 0x0683;

    // Enable Transmit (TX) and Receive (RX) by setting bits 3 and 2, and configure data as 8-bit by
       clearing bit 12 of CR1 register
    USART2->CR1 = 0x000C;

    // Set the Control Register 2 value for USART2 (no additional configuration needed, hence set to
       0x000)
    USART2->CR2 = 0x000;

    // Set the Control Register 3 value for USART2 (no additional configuration needed, hence set to
       0x000)
    USART2->CR3 = 0x000;

    // Enable the UART by setting bit 13 of CR1 register
    USART2->CR1 |= 0x2000;
}

This code snippet provides a detailed explanation of the USART2_Init() function, which configures the UART peripheral. It enables clock access for UART2 and Port A, activates alternate functions for pins PA2 and PA3, and sets the appropriate UART settings, such as Baud Rate, data length, and enabling the UART.
