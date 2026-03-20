# UART Protocol Implementation in Verilog

## 📌 Overview

This project implements the **UART (Universal Asynchronous Receiver/Transmitter)** protocol using Verilog HDL. UART is a widely used asynchronous serial communication protocol for communication between processors and peripherals.

The design includes:

* UART Transmitter (TX)
* UART Receiver (RX)
* Baud Rate Generator
* Top module integrating TX and RX
* Testbench for functional verification

---


## 🧠 UART Protocol – Concept Explanation

### What is UART?

UART is a **serial communication protocol** where data is transmitted **one bit at a time** over TX and RX lines.

It is **asynchronous**, meaning:

* No clock signal is shared
* Communication depends on **baud rate**

---

## ⏱ Baud Rate

Baud rate defines the **number of bits transmitted per second**.

Example:

* 9600 baud → 9600 bits/sec
* Bit duration = 1 / 9600 ≈ 104 µs

### Baud Rate Generation

For system clock = **50 MHz**:

Baud Divider = 50,000,000 / 9600 ≈ 5208

So each bit lasts **5208 clock cycles**.

---

## 📡 Default UART Frame (8N1)

### Configuration

* **1 Start Bit**
* **8 Data Bits**
* **No Parity**
* **1 Stop Bit**

```text
8N1 → 8 data bits, No parity, 1 stop bit
```

---

### Frame Format

```text
Idle | Start | D0 D1 D2 D3 D4 D5 D6 D7 | Stop
  1      0        8 Data Bits (LSB first)   1
```

---

## 🔁 How Transmission Starts

UART line remains in **idle state (logic 1)**.

Transmission begins when:

```text
Idle (1) → Start Bit (0)
```

This **falling edge** tells the receiver:

* A new frame has started
* Begin timing and sampling

---

## 🔄 Transmission Process

1. Line is **idle (1)**
2. Transmitter sends **start bit (0)**
3. Sends **data bits (LSB first)**
4. Sends **stop bit (1)**
5. Returns to **idle state**

---

## 📥 Receiver Operation

1. Detects **start bit (falling edge)**
2. Waits for **mid-bit timing**
3. Samples each data bit
4. Collects 8 bits into a byte
5. Checks stop bit
6. Raises **ready signal (rdy)**

---

## 🏗️ Design Architecture

The design consists of:

* **Baud Rate Generator**

  * Generates timing ticks
* **UART TX**

  * Parallel → Serial conversion
* **UART RX**

  * Serial → Parallel conversion
* **Top Module**

  * Integrates TX and RX

---



---

## 🧪 Simulation

The design is verified using a testbench.

### Test Cases:

* Send `0x41`
* Send `0x55`

### Expected Output:

```text
received data is 41
received data is 55
```

---

## ⚙️ How to Run

1. Open in Vivado 
2. Compile RTL + Testbench
3. Run simulation
4. Observe waveform and console output

---

## 📸 Waveform

<img width="1920" height="1020" alt="Screenshot 2026-03-18 230139" src="https://github.com/user-attachments/assets/b8762d13-f690-415c-af57-038516329186" />


---

## 📚 Applications

* Serial communication
* Debug interfaces (UART console)
* Embedded systems
* FPGA communication

---

## 🔮 Future Improvements

* Add parity support
* Add FIFO buffering
* Implement SystemVerilog assertions
* Develop UVM testbench

---

## 👩‍💻 Author

**Anusha Sanapathi**

---

## ⭐ Summary

UART enables reliable communication by defining:

* Frame structure (start, data, stop)
* Timing using baud rate
* Sampling without a clock signal

These rules ensure correct data transfer between devices.
