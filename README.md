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


