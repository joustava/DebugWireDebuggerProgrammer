# DebugWireDebuggerProgrammer

> A sketch enabling you to program AND debug a certain range of AVR microprocessors.
> It is highly recommended to read the following articles first:
> https://sites.google.com/site/wayneholder/debugwire
> https://sites.google.com/site/wayneholder/debugwire2
> https://sites.google.com/site/wayneholder/debugwire3


## Quick start

1. Make sure your Arduino IDE has support for [programming ATtiny controllers](https://github.com/SpenceKonde/ATTinyCore)

2. Program the Arduino used as programmer with the `DebugWireDebuggerProgrammer` sketch.

3. Assemble this programmer [circuit](https://sites.google.com/site/wayneholder/debugwire3)

4. Put the switch on the circuit in PROGRAMMING state.
Flash (Upload using programmer) the following sketch onto the ATtiny (or any other supported chip)
```
//
//  Toggle ATTiny85 (Toggle PORTB.PB4, pin 3)
//
//                +-\/-+
//  RESET / PB5 1 |    | 8 Vcc
//   CLKI / PB3 2 |    | 7 PB2 / SCK
//   CLK) / PB4 3 |    | 6 PB1 / MISO
//          Gnd 4 |    | 5 PB0 / MOSI
//                +----+

#define LEDBIT    (1 << 4)            // PB4 (connected to LED to ground via 330 ohm resistor
#define INBIT     (1 << 3)            // PB3 (connected to pushbutton switch to gnd)

void setup() {
  DDRB = LEDBIT;                      // Set PB4 as output, all others as inputs
  PORTB = INBIT;                      // Set pullups on input pin PB3
}

void loop() {
  while (true) {
    PORTB |= LEDBIT;                  // Turn the LED On
    PORTB &= ~LEDBIT;                 // Turn the LED Off
    if ((PINB & INBIT) == 0) {        // Check if switch pressed
      __asm__ ("break\n\t");          // If so, return control to debugger
    }
  }
}
```

5. Switch to DEBUG mode, reset the programmer and open the Arduino IDE serial monitor. You'll be greeted with a little menu.

6. In serial monitor, press '+' to enable on chip debugging.
7. In serial monitor, press 'B' to engage the debugger.



## Debugging
