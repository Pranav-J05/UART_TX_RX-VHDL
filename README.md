Overview:
This project implements a simple UART (Universal Asynchronous Receiver/Transmitter) system in VHDL
A baud rate generator (for 50 MHz clock, 9600 baud)
A UART transmitter (8N1: 8 data bits, no parity, 1 stop bit)
A UART receiver (8N1)
A top-level module integrating all components
A testbench for simulation and verification
Features:
Fully synthesizable VHDL code
Modular design: each function in its own file
Loopback testbench: TX output is fed to RX input for self-test
Easy to adapt for other baud rates or clock frequencies
Directory Structure:
├── baud_gen.vhd      # Baud rate generator
├── uart_tx.vhd       # UART transmitter
├── uart_rx.vhd       # UART receiver
├── uart_top.vhd      # Top-level integration
├── uart_top_tb.vhd   # Testbench

1. Baud Rate Generator
Designed a module to divide the 50 MHz system clock down to a 9600 Hz tick (baud rate).
Output: baud_tick signal pulses once per UART bit period.
2. UART Transmitter
Created a VHDL module that:
Waits for a tx_start signal
Serializes 8-bit parallel data with start and stop bits
Outputs on tx line, synchronized to baud_tick
Indicates busy status with tx_busy

3. UART Receiver
Developed a VHDL module that:
Monitors the rx line for a start bit
Samples incoming bits at each baud_tick
Assembles 8-bit data, checks stop bit
Outputs received byte on rx_data and pulses rx_ready when valid

4. Top-Level Integration
Wrote uart_top.vhd to instantiate and connect the baud generator, transmitter, and receiver.
Exposed all key signals as ports for easy connection to FPGA pins or testbench.

5. Testbench
Built uart_top_tb.vhd to:
Generate a 50 MHz clock and reset
Connect tx output to rx input (loopback)
Send test bytes (e.g., 0x55, 0xA3) via the transmitter
Wait for and verify reception on the receiver
Print received bytes to the simulator console (if supported)
Ensured simulation runs long enough for UART timing (at least a few ms)

How to Use:
Add all VHDL files to your Xilinx ISE project.
Set uart_top.vhd as the top module for synthesis/implementation.
Simulate with uart_top_tb.vhd to verify functionality.

