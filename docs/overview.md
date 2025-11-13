# DesignWare I²S NGT – Overview

This document gives a high-level overview of the custom DesignWare I²S driver
used on Raspberry Pi Compute Module 5 (CM5) with the RP1 I/O subsystem.

The code in this repository is based on the upstream ALSA SoC Synopsys
DesignWare I²S and PCM drivers, with additional logic added for bring-up,
polling, and debug.

---

## Goals

- Use the DesignWare I²S IP block with the RP1 controller on CM5
- Bring up I²S even when the normal DMA / IRQ paths are not fully stable
- Allow simple debugging of TX/RX FIFOs and interrupt status
- Support a basic RX→TX loopback mode for hardware testing
- Keep the existing DesignWare PCM (PIO) code mostly unchanged

This is intended for **experiments and learning**, not production audio use.

---

## Main Ideas

At a high level, the customizations add:

- An optional **poller** based on `hrtimer`  
- **FIFO logging** for TX and RX samples  
- A simple **RX→TX loopback** test path  
- RP1-specific register / DMA address usage

The normal ALSA integration is still present (DAI / PCM), but this project
focuses more on low-level bring-up behaviour.

---

## Poller Mode (High-Level)

In poller mode, instead of relying only on interrupts, a timer wakes up at a
configurable interval and:

- Checks I²S interrupt/status registers
- Pushes data into the TX FIFO when space is available
- Reads data out of the RX FIFO when samples are present
- Optionally logs samples and status values for debugging

This is useful when:

- Interrupts are not firing as expected
- DMA is not yet configured or reliable
- You want to observe how the hardware behaves at a low level

---

## FIFO Logging and Loopback

The driver can be configured (via module parameters or build-time options) to:

- Log a limited number of TX/RX samples to the kernel log
- Record interrupt/status register values during operation
- Loop RX data back into TX, so that the I²S interface can be tested without
  an external audio generator

This kind of loopback mode is especially helpful during early hardware bring-up.

---

## RP1 / CM5 Specifics

Compared to older BCM-based platforms, CM5 with RP1 exposes the I²S block with
different register and DMA mappings.

The driver:

- Reads the component parameter registers to discover FIFO depth and data width
- Configures the DMA addresses and FIFO widths for RP1
- Uses the device tree node (with a custom `compatible` string) to bind to the
  correct I²S instance

The exact DTS node(s) used for this will be documented under `dts/` once the
final version is consolidated.

---

## What This Repo Does Not Cover

This repository does **not** try to:

- Provide a full CM5 kernel or OS image
- Explain complete cross-compilation / environment setup
- Implement a polished or upstream-ready audio driver

The focus is on:

- The modified I²S / PCM sources
- The DTS bindings needed to use them
- The logs and notes from real hardware experiments
