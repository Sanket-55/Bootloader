# Bootloader
# STM32F102C8T6 Bootloader with Application

This project implements a simple bootloader on the STM32F102C8T6 microcontroller. The bootloader stays in a loop if a specific condition is met and jumps to the application otherwise. The onboard LED indicates the system state:
- **Bootloader Mode**: LED stays ON.
- **Application Mode**: LED blinks.

---

## What I Did
1. Created a bootloader that occupies the first 20 KB of flash (`0x08000000` to `0x08004FFF`).
2. Created an application that starts at `0x08006400` and runs in the remaining flash memory.
3. Configured the vector table offset for the application.
4. Programmed logic to jump from the bootloader to the application.

---

## How I Did It
1. **Bootloader**:
   - Wrote a function to check whether to stay in bootloader mode (GPIO pin-based).
   - Programmed a jump function to transfer execution to the application.

2. **Application**:
   - Configured the vector table relocation to `0x08006400`.
   - Added a simple LED blinking program to verify the application runs correctly.

3. Used **STM32CubeProgrammer** to flash both the bootloader and application to the respective memory regions.

---

## Changes to Make

### 1. Bootloader
- Linker script (`STM32F102C8T6.ld`):
  Set flash memory to `ORIGIN = 0x08000000` and `LENGTH = 20K`.
- Add logic in `main.c` to:
  - Check a GPIO pin state to decide whether to stay in bootloader mode.
  - Implement a jump to the application if the condition is met.

### 2. Application
- Linker script (`STM32F102C8T6.ld`):
  Set flash memory to `ORIGIN = 0x08006400` and `LENGTH = remaining flash size`.
- Add vector table relocation in `main.c`:
  ```c
  SCB->VTOR = FLASH_BASE | 0x00006400;
