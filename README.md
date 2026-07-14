Serial Communication Using UART – LPC2148

Overview:-

1)This project demonstrates the implementation of UART (Universal Asynchronous Receiver Transmitter) serial communication using the LPC2148 ARM7TDMI-S microcontroller.

2)The program is developed in Embedded C and configures UART0 of the LPC2148 to continuously transmit the message **"HELLO WORLD"** through the serial communication interface.

3)The project focuses on register-level programming, UART peripheral configuration, baud rate setup, and serial data transmission in an ARM-based embedded system.


Objective:-
To configure UART0 of the LPC2148 microcontroller and transmit a string serially using Embedded C.


Hardware and Software Used

- LPC2148 ARM7TDMI-S Microcontroller
- LPC2148 Development Board
- Serial Communication Interface

Software:-

- Keil µVision
- Embedded C
- LPC214x Header Library



LPC2148 Registers Used

| Register | Function |
|----------|----------|
| PINSEL0  | Configures TXD0 and RXD0 pin functions |
| U0LCR    | Configures UART data format and DLAB |
| U0DLM    | Divisor Latch Most Significant Byte |
| U0DLL    | Divisor Latch Least Significant Byte |
| U0THR    | UART0 Transmit Holding Register |
| U0LSR    | UART0 Line Status Register |

---

## Working Principle

1)The UART0 TXD0 and RXD0 pins are configured using the `PINSEL0` register.

2)The UART0 Line Control Register (`U0LCR`) is configured for 8-bit data transmission, one stop bit, and no parity. The Divisor Latch Access Bit (DLAB) is enabled to configure the UART baud rate using the `U0DLL` and `U0DLM` registers.

3)The message **"HELLO WORLD"** is stored in a character array. A pointer is used to access each character of the string sequentially.

4)Each character is written into the UART0 Transmit Holding Register (`U0THR`). The Line Status Register (`U0LSR`) is checked to determine the transmitter status.

5)The process continues until the null character at the end of the string is detected. After the complete message is transmitted, the program introduces a software delay and repeats the transmission continuously.


Algorithm

1. Start the program.
2. Initialize UART0.
3. Configure TXD0 and RXD0 pins.
4. Configure UART data format.
5. Enable Divisor Latch Access.
6. Configure the UART baud rate divisor.
7. Disable Divisor Latch Access.
8. Assign the message array address to a pointer.
9. Read one character from the string.
10. Write the character to the UART Transmit Holding Register.
11. Check the UART transmitter status.
12. Increment the pointer.
13. Repeat until the null character is detected.
14. Introduce a software delay.
15. Repeat the transmission continuously.



Source Code

```c
#include<lpc214x.h> 
 
void uart_init(void); 
unsigned int delay; 
unsigned char *ptr; 
unsigned char arr[]="HELLO WORLD\r"; 
 
int main() 
{ 
 while(1) 
 { 
  uart_init(); 
  ptr = arr; 
  while(*ptr!='\0') 
  { 
   U0THR=*ptr++; 
   while(!(U0LSR & 0x20)== 0x20); 
   for(delay=0;delay<=600;delay++); 
  } 
  for(delay=0;delay<=60000;delay++); 
 } 
}  
void uart_init(void) 
{ 
  PINSEL0=0X0000005;      //select TXD0 and RXD0 lines      
 U0LCR = 0X00000083;     //enable baud rate divisor 
loading and 
 U0DLM = 0X00;              //select the data format 
  U0DLL = 0x13;           //select baud rate 9600 bps 
  U0LCR = 0X00000003; 
  
} 
 
