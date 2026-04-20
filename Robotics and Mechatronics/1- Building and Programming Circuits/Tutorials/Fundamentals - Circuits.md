# Electrical Foundations & High-Power Actuation

**Focus:** Ohm's Law, Power, and Relays 
**Goal:** Students understand _why_ microcontrollers need help driving heavy loads and learn to wire a relay safely.

> [!tip] Technical Terms!
> Don't forget to consult the [[Glossary of Terms - Robotics]] if there are words you're not sure of in this document! 

## Lesson 1: Ohm's Law & Microcontroller Limits
    

> [!info] Ohm's Law
> - Ohm's Law ($V = I \times R$) - Voltage is equal to Current times Resistance
> - Power ($P = V \times I$).

### The Problem with Power 

When you break down the [[Project Two - Smart Watering System]] assessment, you'll see that you're required to ensure that the power systems of your project are accuratly calculated in order to ensure the arduino doesn't..._blow up_. Microcontrollers have very specific power requirements to ensure that their onboard systems get exactly the current they need, but also they are designed to have a very low power draw to be as efficient as possible. Throwing a 5 - 12V DC motor into the mix, or devices that use _inductive load_ isn't good for our little microcontrollers, and this care for understanding current also applies to the inputs and outputs we use in our projects.

> [!note]- Examples of Current Needs
> - LEDs - Rated for a max of 20mA _(that's milliamps)_
> - DHT11's - 0.3mA to 2.5mA
> - The Arduino itself generally allows for up to 40mA from a single I/O and a max of 200mA across the entire circuit!

**Enter, Ohm's Law! **

There are a few fundamental laws about circuit’s that we can use to analyse circuits for troubleshooting and during the design process. Ohm’s Law is one of these! It states that _“the electric current through a conductor between two points is directly proportional to the voltage across the two points.”_  

![[ohms_triangle.png]] 
_(image as stolen from:_ [Electronic's Tutorials](https://www.electronics-tutorials.ws/dccircuits/dcp_2.html))

A couple of words to explain there. **Proportional** is a fancy way of saying "equals to".  What this means is, your voltage should be equal to the current in your circuit, times any resistance in the circuit. _Remember that energy cannot be created or destroyed? Well, here it is._ Want to make sure your LED is only getting the current it needs? Use Ohm’s Law to get the right level of resistance in your circuit!

To test your understanding of Ohm’s Law, try to calculate the current of a 12V circuit with two 330Ω resistors manually, and then use a [calculator](https://www.allaboutcircuits.com/tools/ohms-law-calculator/) to confirm your findings.

##### Challenge 
Calculate the current draw of a 5V DC pump vs. the maximum output of an Arduino digital pin (~40mA). Prove mathematically that wiring a pump directly to the board will fry the microcontroller.



## Lesson 2: Isolation & Relays
    
**Learning Intention:** Understand inductive loads and mechanical/optical isolation. Explain how a relay uses a tiny electromagnet (Arduino side) to pull a high-power physical switch (Pump side).
        
**Practical Outcome:** Wire a 5V Relay module. Write a basic script to make the relay "click" on and off every 5 seconds.

### Relay Circuit Design

Relays are the answer to the question: _"so, how do I power this giant 12v pump from a 5V microcontroller?"_

And it's not just pumps, it's any system you need to power which exceeds the limits of the Arduino.

> [!NOTE]- See here for how relays work!
> A relay is essentially an electrically operated switch that allows a low-power signal to safely control a much higher-power circuit. At its core, it consists of an electromagnet (a coil of wire wrapped around a metal core) and a set of mechanical contacts. When a small electrical current—such as a 5-volt signal sent from a microcontroller like an Arduino—passes through the coil, it energises the electromagnet and generates a temporary magnetic field. This magnetic field is the driving force behind the relay's operation, acting as the invisible "hand" that will flip the switch.
> 
> Once this magnetic field is generated, it attracts a small, movable metal plate called an armature. As the armature is pulled toward the electromagnet, it physically pushes or pulls the electrical contacts of the secondary, high-power circuit, forcing them to snap together (turning the heavy-duty device on) or spring apart (turning it off). Because the low-power control side and the high-power load side only interact via this magnetic attraction and are physically separated by space, the relay provides complete electrical isolation. This crucial separation is what allows a delicate electronic board to safely control high-voltage devices, like a water pump or a heater, without the risk of electrical feedback destroying the sensitive control circuitry.

### Relay Circuit

![[How-To-Use-A-Relay-With-Arduino-728x410.jpg]]

As you can see from the diagram above, relays are connected to both the Arduino and the external power supply, but the target of the relay never touches the Arduino.

The Arduino still needs its own power to operate and the relay _isolates_ the Arduino's power from the external and often larger power source needed for the external output.

- Connect the Arduino's 5V and GND to the relay, along with a single digital pin. (this does not need to be a PWM pin)
- Connect the Positive pin from the Power Source to the other side of the relay, and then the Ground of the Power source to the Ground of the output.
- Connect the voltage of the external output to the Relay.

The Positive of both the Output and the Power Source are running through the Relay because this is the point of the relay. When the pin from the Arduino runs **high** it will make a connection between both those positive leads and thus complete the circuit, powering the output.

### Relay Code

```arduino
const int relayPin = 4; // Pin connected to the relay IN pin

void setup() {
  pinMode(relayPin, OUTPUT);
  // Keep relay OFF initially (assuming active-low module)
  digitalWrite(relayPin, LOW); 
}

void loop() {
  digitalWrite(relayPin, HIGH);  // Turn relay ON
  delay(2000);                 // Wait 2 seconds
  digitalWrite(relayPin, LOW); // Turn relay OFF
  delay(2000);                 // Wait 2 seconds
}
```

The code is also fairly straight forward.

- Setup the Relay Pin as an **Output**
- Run the Relay pin **LOW** in setup to ensure that it begins off when the Arduino powers up.
- In the Loop, run the relay **HIGH** whenever you wish to activate it. The control structures you setup to determine when it runs are entirely up to you.

##### Challenge

- Build the circuit above, and attach an LED to demonstrate the functionality of the relay.
- Use a 6V battery pack to power your output but remember, you'll need to combine your work of Ohm's Law to ensure you've got enough resistance in the circuit.
- Show your working / calculations in your notes!

## Lesson 3: BOUNUS ROUND! Kirchhoff's Voltage Law

Kirchhoff’s Voltage Law is another of these fundamental laws. It states that “for a closed loop series path the algebraic sum of all the voltages around any closed loop in a circuit is equal to zero.” This is due to the fact that in a closed circuit, no voltage should be lost.

...should be. As you’ll learn from time to time while this should be the case, as you stare at the servo motor that isn’t moving or the LED that isn’t lit and wonder why it’s likely because there’s an issue with your circuit that will require both a multimeter and an understanding of Kirchhoff’s 2nd Law (yes, there is another one) to fix.

It’s time for some maths. Go through the information present on [this website](https://www.electronics-tutorials.ws/dccircuits/kirchhoffs-voltage-law.html), take some notes and then answer the following discussion questions below.

These are discussion questions, which means you can 100% have a conversation with the class members around you about your answers before writing them down.

### Discussion Questions

1. Conservation of Energy. What is that about and why does it matter for circuit design?
    
2. Applications for Kirchhoff’s Law. When do you need to consider it? Why is it beneficial?
    
3. What are the implications for circuits running in parallel? All the examples are in series, what do you think you’d need to consider?
    
##### Tasks:

1. Design and setup a basic circuit using a breadboard, jumper cables and a variety of resistors. Set the voltage to 5V-12V and then use a multimeter to test the connections and see if Kirchhoff’s Law applies. Record your circuit diagram, testing and equations for your experiment.

## Bonus BONUS ROUND - Voltage Dividers!

Now it’s time to talk about Voltage Dividers.

Occasionally (really, more often than not), you need to turn a large voltage into a smaller one. One of the fundamental ways to do this is with a series of resistors in a circuit running off an input voltage to create the required output voltage. This is called a Voltage Divider.

Here are some examples of these in circuit diagrams.

![[voltage_divider_example.png]]

These examples are [100% Stolen](https://learn.sparkfun.com/tutorials/voltage-dividers/all)

The resistor closest to the input voltage is R1, and the resistor closest to the ground is R2. The voltage drop across R2 is called Vout and that’s the divided voltage which this circuit is designed to make.

### Voltage Divider Equation

Memorise this!!

![[voltage_divider_equation.png]]

Let’s discuss:

1. What does the equation state?
    
2. How can you derive / prove the equation?
    
##### Then, let’s put this into practice:

1. Try and create a voltage divider to turn a 6V input into a lower input. Design your circuit, do some maths and hook up an LED to the Vout. Use a multimeter to measure the LED voltage to see if you’re correct!
2. Potentiometers...let’s talk about those. They’re pretty much variable voltage dividers. Have a look at the following diagram of a potentiometer and discuss how this works. (Pin 1 is the Vin, Pin 3 is the GND, Pin 2 is the wiper/Vout.

<br>

![[potentiometer_voltagediv.png]]