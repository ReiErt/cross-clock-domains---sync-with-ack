# cross-clock-domains---sync-with-ack
frequency agnostic CDC valid_in Synchronizer. valid_in pulse is passed between clock domains, regardless of frequency. Upstream clock_domain receives ack once downstream clock_domain is finished.

![Alt text](/sync_with_ack_img.png)

## Project Motivation
How do we ensure a vld_in flag is always seen and not lost to metastability? This project is my first experience syncing clock domains of different frequencies. In embedded projects, it is common for signals to enter or leave the FPGA, crossing clock domains of different frequencies.
To ensure any two clock domains of any frequency can safely receive a valid_in flag, without loosing the flag to metastability, a synchroniser is needed. In addition, this synchroniser sends an acknowledgement to the upstream clock domain upon sending the vld_in to the downstream module. 

## Project Task
1) A single module that safetly passes vld_in signals between two clock domains, where
	a) freq(clk_a) > freq(clk_b)  
	b) freq(clk_a) = freq(clk_b)
	c) freq(clk_a) < freq(clk_b)
2) The rdy_out / rdy_in signal informs the upstream clock domain to not execute until downstream clock domain is finished. With this addition, no fifo is necessary. Performance suffers from this decision, as no pipelining is implemented. However, this project is my introduction to CDC, so I want to keep the design simple. 
3) Reset Synchronizer is helpful to ensure reset can occur asynchronously while desertion never causes metastability.

#Description
This module contains two clock domains, where the upstream clock domain is faster. A valid_in pulse is passed between clock domains. Busy signal is used for further flow control.
A reset synchronizer is used to asynchronisly assert the reset and de-assert the reset to any clock domain synchronously two clock cycles laters.

# Schematic Signal Flow
1. tb drives clock and reset and valid_in
2. reset_synchroniser asychronously asserts and synchronously de-asserts reset 
3. clock_domain_a receives vld_in, does computation and outputs vld_out
4. vld signal is taken from clock_domain_a and placed in clock_domain_b
5. clock_domain_b receives vld_in, does computation and outputs vld_out
6. when clock_domain_b is finished, clock_domain receives next vld_in to begin again

![Alt text](/schematic.png)

