# Combination-Lock-Circuit

As my final design project for ECE3872 (Electronics Lab), I designed and built a mostly analog circuit to simulate a combination lock in which the user can create a code, store it in memory, and if someone later tries to enter a combination that does not match the user set code, an alarm system is triggered.

### User Input
The circuit utilizes seven switches that are all controlled by the user. The first switch shown on the schematic acts as a reset switch, and when it is closed it allows the current code to be cleared to 0000. However, the code is only cleared if the user knows the current code.The next four switches (B0-B3)
correspond to the bits of the code. Since there are four switches and each can be in the off state or on state, this allows there to be 16 possible codes. The SetCode switch allows the user to set a new code based on the current states of B0-B3. The last switch, CheckCode, allows the current states of B0-B3 to be compared against the stored code.

### Storing the Code
The schematic to the right shows how one bit is stored with a 555-timer. The states of one bit of the code and SetCode are fed into a NMOS NAND gate and the output of the NAND gate is fed to the trigger pin of the 555-timer. If the bit and SetCode are equal to 1, the output of the NAND gate will be 0, and because the trigger pin is active low this will result in the output of the 555-timer being 1. Otherwise, the output of the 555-timer will be 0.

### Checking the Code
If the bits match, meaning they are both 0 or they are both 1, then the output of the XOR gate will
be 0. Otherwise, the output of the XOR gate will be 1. The switching diode acts as an OR gate when multiple XOR gate outputs are tied together.

### Code is Correct
If all the bits match the corresponding stored bits and the CheckCode switch is closed, then a green LED will turn on indicating to the user that the code they
entered is correct. To perform this logic, an NMOS NOR gate is used with the output of the diode OR gate and the state of the CheckCode switch being used as inputs. If all the individual bits match the stored bits, then the diode OR gate will feed the NOR gate a 0. The CheckCode switch has an active low configuration, so if it is closed it will feed the NOR gate a 0. Both those 0s NORed together will output a 1, turning on the green LED.

### Code is Incorrect
If all the bits do not match the corresponding stored bits and the CheckCode switch is closed, then a red LED will flash on and off and a speaker will turn on, together creating an alarm indicating to the user that the code they entered is incorrect. Once the alarm turns on, the 555-timer in bistable mode does not allow it to turn off until the code is correct. To perform this logic, an NMOS inverter and NMOS NAND gate are used. The inverse state of the CheckCode switch acts as one of the inputs to the NAND gate. The other input to the NAND gate is the output of the diode OR gate. The output of the NAND gate is then fed to the trigger pin of the 555-timer. For example, if the CheckCode switch is closed, the inverter will output a 1. If all the bits do not match the stored bits, the OR gate will output a 1. This in turn will result in the NAND gate outputting a 0 and triggering the 555-timer.

### Alarm System
To create the alarm, two 555-timers in astable mode are used. The 555-timer connected to the red LED outputs a signal with a frequency of about 0.8 Hz and a duty cycle of about 75%, meaning that the LED will be on for about 0.96 seconds and off for about 0.3 seconds each
cycle. The 555-timer connected to the speaker outputs a signal with a frequency of about 440 Hz and a duty cycle of about 55%, meaning the speaker outputs a tone relatively close to a concert A.

### Resetting the Code
To reset the code to 0000 the user must first know the current code. To achieve this logic, an NMOS NOR gate and NMOS inverter is used. The two inputs to the NOR gate are the state of the reset switch and the output of the diode OR gate. The output of the NOR gate is then inverted and is sent to the reset pins of the four 555-timers storing the code. For example, if the reset switch is closed, the active low configuration will result in a 0 and if the user is currently inputting the correct code, then the diode OR gate will output a 0. This will result in the NOR gate outputting a 1, which will then get inverted before being sent LED and Speaker to the 555-timers. The reset pins on the 555-timers are active low, thus the code will be cleared to 0000.
