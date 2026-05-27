# Arduino-Communication-with-Pyfirmata

This project demonstrates a simple communication system between an **Arduino Uno** and an **Arduino Nano** using **Firmata** with Python.

The idea is simple:

- The Arduino Uno sends a digital signal through a chosen pin.
- A jumper wire connects this pin to another pin on the Arduino Nano.
- The Nano reads the signal and reacts based on whether the signal is HIGH or LOW.

In this example, both boards use **digital pin 8** for communication.

# Hardware Setup

## Connections

- Connect **Arduino Uno Pin 8** → **Arduino Nano Pin 8**
- Connect **GND** from Uno → **GND** from Nano
- (Optional) Use a push button for testing input signals.

![[Pasted image 20260524214942.png]]

# Python Side (Arduino Uno)

## Import Libraries and Configure the Board
```
from pyfirmata import Arduino, util  
import time  
  
# Connect to the Arduino Uno  
board = Arduino("COM3")  
  
# Start the iterator to avoid buffer overflow  
it = util.Iterator(board)  
it.start()  
  
# Configure the button pin  
button = board.digital[2]  
button.mode = 0 # INPUT  
button.enable_reporting()
```
# Important Disclaimer

I used a **push button** during testing.

When using Firmata, you must always verify whether the pin value is `None` before checking for `True` or `False`.

This happens because Firmata may not have received the pin data yet.
```
# Wait until Firmata receives valid data  
if state is None:  
time.sleep(0.2)  
continue
```

# Sending the Communication Signal

The Uno sends a digital HIGH (`1`) or LOW (`0`) signal through the communication pin.

This signal is then received by the Arduino Nano.
```
# Send communication signal  
if state is True:  
communication.write(1)  
print("Message sent")  
  
else:  
communication.write(0)  
print("No message sent")  
  
time.sleep(0.2)
```


# Arduino Nano Side

On the Nano side, the communication signal is stored using `.read()`.
```
commuSig = commu.read()  
  
led.write(1)  
  
# Check for None before using True or False  
if commuSig is None:  
time.sleep(0.2)  
continue
```
# Reacting to the Signal

You can build any logic around the received signal.

In this example, an LED is controlled depending on the communication state.

```
if commuSig is True:    led.write(0)    print("LED ON!")else:    led.write(1)    print("LED OFF")time.sleep(0.2)
```


# Notes

- Always connect the **GND** of both Arduinos together.
- Always check for `None` when using `pyFirmata`.
- This method can be expanded for:
    - Robot communication
    - Multi-board systems
    - Custom signal protocols
    - Sensor synchronization
# Technologies Used

- Python
- pyFirmata
- Arduino Uno
- Arduino Nano
- Firmata Protocol
