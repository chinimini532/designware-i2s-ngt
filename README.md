# designware-i2s-ngt

This repository contains experimental work on a customized Synopsys DesignWare I²S
driver for the Raspberry Pi Compute Module 5 (CM5) using the RP1 I/O subsystem.

The main goals of this project are:

- Use the DesignWare I²S IP block with RP1 on CM5
- Add an hrtimer-based poller path for bring-up and debug
- Log FIFO activity (TX/RX samples, ISR flags) during operation
- Support simple RX→TX loopback tests without a full ALSA userspace stack
- Keep the existing DesignWare PCM (PIO) glue mostly unchanged

This code is **experimental** and intended for learning, bring-up, and internal
testing, not for production audio use.

---

## Repository Layout

```text
src/    - driver sources (modified DesignWare I²S and PCM, plus Makefile)
dts/    - device-tree snippets / overlays for the I²S block (to be added)
docs/   - design notes, register info, and test documentation (to be added)
logs/   - dmesg traces, register dumps, and poller output (to be added)

---

## High-Level Overview

This driver is based on the upstream ALSA SoC Synopsys DesignWare I²S driver and
extended with:

An optional poller mode using hrtimer to periodically check and service
TX/RX FIFOs

FIFO logging to observe TX and RX samples and ISR flags for debugging

Optional RX→TX loopback to test the I²S path without external audio sources

RP1-specific configuration for DMA/register mappings on CM5

The intention is to make it easier to bring up and debug I²S on CM5 with RP1,
especially when normal DMA/interrupt-driven audio paths are not fully stable yet.



