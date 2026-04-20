# Assessment Task 2: Automated Smart Planter (Data & Actuation)

**Course:** Robotics & Mechatronics (Combined T & A) 
**Unit:** Unit 1 (Building & Programming Circuits) 
**Task Type:** Practical Project (Product & Data Focus) 

**Note:** Your design processes, circuit calculations, and testing logs must be submitted in your separate **Evidence Guide**.

> [!CAUTION]- How to Use This Document
> 
> _Read this document before you start prototyping._ 
> * Use **Section 2** to understand the specific learning goals for your enrolled course level (T or A).
> 
> - Use **Section 3** as a checklist while you build to ensure your system meets the technical expectations.
>     
> - Use **Section 5** to ensure you hand in the correct evidence of your practical application.

    

## 1. Context: How this fits into our Course

In Task 1, you learned how to process immediate digital and analog inputs. In Task 2, we are exploring long-term data collection and high-power actuation.

**The Scenario: "Project Oasis."** A local botanical research team needs a way to maintain delicate plant life while reducing water waste. Your task is to build a smart, automated watering system using an Arduino Uno. The system must not only keep a plant alive using a water pump, but it must also log environmental data to a MicroSD card. This data will be used to track the plant's health and optimise water usage over time.

## 2. The Learning Being Assessed (Big Ideas)

**For Accredited (A) Students:**

- **Safe Construction:** Your ability to safely wire a circuit that includes water, pumps, and electronics without causing short circuits.
    
- **Component Integration:** Your ability to wire and program new hardware modules (SD Card Readers, Soil Moisture Sensors, Relays).
    
- **Data Collection:** Your ability to write code that successfully creates a text file on an SD card and records basic sensor readings.
    

**For Tertiary (T) Students:**

- **Power & Actuation (Theory):** Your ability to understand electrical / circuit theory and document the physics of relays and inductive loads (why we can't power a DC pump directly from an Arduino pin).
    
- **Advanced Protocols:** Your ability to configure and troubleshoot the SPI communication protocol required for the MicroSD module.
    
- **Algorithmic Optimisation / Modularity:** Your ability to write logic that uses multiple data points to make "smart" decisions (e.g., _only_ watering when soil is dry AND the temperature/light levels indicate it isn't the middle of the day, to prevent evaporation).
    

## 3. Technical Requirements & Variation

You must build a functional prototype. You may test this on a real potted plant, or a simulated environment (a cup of dry dirt and a cup of water).

### A. The Actuator (The Relay & Pump)

Your system must physically move water.

- **Base Requirement:** You must use a 5V Relay Module to switch on a submersible DC Water Pump.
    
- **Safety Constraint:** You must **only** use low-voltage DC pumps (5V to 12V). You are strictly forbidden from cutting or stripping any 240V mains power cables. Keep your breadboard and Arduino physically elevated away from water sources.
    

### B. The Sensors (The Inputs)

Your system needs to know _when_ to water.

- **Base Requirement:** You must include a Soil Moisture Sensor. You must also include at least one other environmental sensor (e.g., a DHT11 Temperature/Humidity sensor or an LDR Light Sensor).
    
- **(T-Course) Expectation:** Analog sensors must be calibrated. You must map the raw analog data (0-1023) into meaningful percentages (0% to 100% moisture) before making logic decisions.
    

### C. The Data Logger (The SD Card)

Your system must keep a record of what it does.

- **Base Requirement:** You must wire a MicroSD Card adapter using the SPI pins on your Arduino Uno.
    
- **(A-Course) Expectation:** Your code must open a `.txt` or `.csv` file and write the current sensor readings to it at regular intervals.
    
- **(T-Course) Expectation:** Your code must write formatted `.csv` data (Comma Separated Values) that tracks the sensors AND the state of the pump (e.g., `Time, Moisture%, Temp, Pump_Triggered`). Your code should use `millis()` for logging intervals to avoid halting the pump logic with `delay()`.
    

## 4. Task Conditions

- **Time:** You will have [Insert Number] weeks of in-class time to prototype, solder, and finalize your system.
    
- **Resources:** Full access to the Robotics Lab. You will be provided with Arduino Unos, SD Modules, Relays, 5V DC Pumps, and silicone tubing.
    
- **Collaboration:** While you may test your pumps in shared "wet zones" in the lab, your circuit, code, and Evidence Guide must be entirely your own independent work.
    

## 5. What Counts As Evidence (Submission)

**Due:** End of class on [Insert Date].

To prove your learning in practical application, you must submit:

1. **The Physical Prototype:** The Arduino, sensors, and pump setup demonstrated to your teacher (Evidence of your construction and WHS safety).
    
2. **The Source Code:** Your `.ino` file (or Arduino Online Link) submitted to Google Classroom (Evidence of your control structures, SPI setup, and relay logic).
    
3. **The Data Log:** The actual `.csv` or `.txt` file pulled directly from your MicroSD card and uploaded to Google Classroom (Evidence that your data logger successfully recorded over time).
    
4. **Project Description:** Some documentation about your build, operating instructions and a breakdown of it's functionality.

> [!Warning]- Evidence Guide Specifics for this Task:
> - **Proof of Logic (Schematics):** You must include a clear circuit diagram showing how the Relay isolates the high-current pump from the low-current Arduino.
>     
> - **Impact & Ethics (WHS):** Your Risk Assessment **must** explicitly address the hazards of mixing conductive liquids (water) with electronics, and detail the control measures you took to prevent damage or injury.