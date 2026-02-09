# Interrupt-Driven UART Command Line Interface (STM32)

**Status:** Completed and tested on hardware  
**Project Type:** Interrupt-driven serial command interface

---

## What It Does
A **UART-based command line interface** demonstrating how **byte-driven serial input** can be safely converted into **line-based commands** in an interrupt-driven embedded system.

- UART RX handled entirely via interrupts
- Incoming bytes buffered into complete lines
- Commands parsed and dispatched in the main loop
- Clean separation between ISR mechanics and application logic

This project focuses on **UART architecture, buffering, and parsing**, not protocol complexity.

---

## System Architecture
- **MCU:** STM32 (tested on Nucleo board)
- **Communication:** UART (USART2)
- **RX:** Interrupt-driven, byte-by-byte
- **TX:** Blocking (intentional, human-driven CLI)
- **Debug:** Serial terminal (picocom)

---

## Supported Commands
The CLI currently supports the following commands:

- `help` — List available commands  
- `6` — Prints a test response  
- `led on` — Turns the onboard LED on  
- `led off` — Turns the onboard LED off  
- Empty Enter — Prints a usage hint  

Commands are processed only after a full line (`\n` / `\r`) is received.

---

## System Behavior
- UART RX interrupt:
  - receives one byte at a time
  - stores bytes in a fixed buffer
  - detects newline to mark command completion
- RX interrupt never blocks and never performs parsing
- Main loop:
  - detects completed commands
  - parses input using string comparison
  - executes command actions
  - transmits responses

RX is explicitly re-armed after every byte to ensure continuous input.

---

## Core Concepts Demonstrated
- Interrupt-driven UART RX  
- Line buffering and message framing  
- ISR vs main-loop responsibility separation  
- Safe handling of user input  
- Deterministic command parsing  
- Non-polling, event-driven firmware design  

---

## Design Intent
The goal of this project was to:

- Understand how UART RX works at the byte level  
- Build a line-oriented interface on top of raw serial data  
- Enforce correct ISR discipline  
- Avoid parsing or blocking behavior inside interrupts  

The CLI was intentionally kept simple to isolate **UART architecture and control flow**.

---

## What I Learned
- Why UART RX must be explicitly re-armed  
- How to safely buffer serial input using interrupts  
- How to convert byte streams into meaningful commands  
- How to structure firmware around event-driven input  
- Why ISR minimalism is critical for scalable systems  

---

## Limitations
- Blocking TX (acceptable for human-driven CLI)
- Fixed RX buffer size (64 bytes)
- Simple string-based command parsing

These limitations were intentional to keep focus on **RX architecture and parsing logic**.

---

## Possible Extensions
- Non-blocking TX using interrupts or DMA  
- DMA-based UART RX with circular buffers  
- Argument-based commands  
- Bare-metal UART driver implementation  
- Integration with other peripherals (timers, PWM, I2C, SPI)

---

## Tools & Environment
- STM32CubeIDE  
- STM32 HAL  
- Linux development environment  
- picocom for serial communication  
- Git & GitHub for version control  

---

## Repository Structure

UART_Command_Line_Interface/  
├── Core/  
│   ├── Inc/  
│   └── Src/  
├── Drivers/  
│   └── STM32F4xx_HAL_Driver/  
├── .ioc  
├── README.md  
└── ...

---

## Status
Completed and tested on hardware.
