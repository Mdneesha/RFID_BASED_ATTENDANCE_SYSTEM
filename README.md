# RFID-Based Attendance System with Database Integration

## Project Overview
This project automates attendance tracking using RFID technology, interfaced with an LPC2148 microcontroller. RFID cards are used to log user attendance, and all data is integrated with a Linux-based system using UART communication and stored in a `.csv` database.

## Key Features
- Contactless attendance via RFID cards
- Admin card support for managing users
- Real-time clock for accurate IN/OUT timestamps
- EEPROM storage for persistent data (admin)
- LCD and 4x4 Keypad interface for interaction
- UART0/UART1 communication with PC
- Linux-side C application for database management

## Hardware Requirements
- LPC2148 ARM7 Microcontroller
- RFID Reader & RFID Cards
- 16x2 LCD Display
- 4x4 Matrix Keypad
- AT25LC512 EEPROM (SPI based)
- Push-button switch
- MAX232 IC for UART level conversion
- USB-to-UART converter

## Software Requirements
- Keil uVision (Embedded C Development)
- Flash Magic (Firmware flashing)
- Linux OS (for PC-side code execution)
- GCC compiler for Linux C programs

## Code Structure (Embedded C)

| File                 | Description |
|----------------------|-------------|
| `rfidmajor_main.c`   | Main program logic, includes `main()`, `admin_menu()`,'serial_open()' |
| `rfidmajor_fun.c`    | Handles RFID processing and time calculations; includes `get_current_time()`, `calculate_duration()` |
| `lcd.c / lcd.h`      | LCD display routines |
| `spi.c / spi.h`      | SPI communication setup |
| `spi_eeprom.c / .h`  | EEPROM read/write logic |
| `uart.c / uart.h`    | UART0 and UART1 communication |
| `rtc.c / rtc.h`      | RTC time/date functions |
| `kpm.c / kpm.h`      | Keypad input handling |
| `delay.c / delay.h`  | Delay implementation |
| `rfidread.c`         | Contains card reading logic interrupt raising for admin change sync system time to rtc check the card pacled admin or user send data to pc through uart0 based on admin or user  |
| `pin_connect_block.c/h` | Pin setup utilities |
| `types.h`            | Type definitions |
| `*_defines.h`        | Macro and hardware-specific definitions |

## Code Structure (Linux C Program)

- `rfidmajor_main.c`: Entry point and user interface logic
- `rfidmajor_fun.c`: Parses incoming data, updates `.csv`, manages timing and formatting

## Workflow

1. **Startup**: LCD displays project name, loads ADMIN card from EEPROM.
2. **Card Detection**:
   - If ADMIN: Sends `A<CARDNUM>$` to PC; Linux app shows menu (Add/Edit/Delete user).
   - If USER: Sends `U<CARDNUM>$` to PC; Linux app updates attendance logs.
3. **Admin Menu via Switch Interrupt**:
   - Option 1: Change ADMIN card
   - Option 2: Exit

## CSV File Format

| S.No | User ID | Name | Date | Working Hours | IN/OUT | IN Time | OUT Time |
|------|---------|------|------|----------------|--------|----------|-----------|
| 1    | 1234    | John | 08-08-2025 | 5 hours 30 minutes | IN     | 09:00   | 14:30     |

## How to Run

### Microcontroller Side
1. Compile code using Keil (main file: `rfidread.c`)
2. Flash `.hex` using Flash Magic
3. Connect RFID, EEPROM, LCD, Keypad, and UART interfaces

### Linux Side
1. Compile `rfidmajor_main.c` and `rfidmajor_fun.c` using GCC
2. Execute app: `./rfid_attendance
3. Interact via admin menu when ADMIN card is scanned
4. If user card is placed check is it valid user or not.If not display message invalid user, If present upadate entry log into .csv file.

