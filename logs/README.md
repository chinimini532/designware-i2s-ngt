# NGT I²S Logs

This directory is intended to hold logs and traces captured while testing the
NGT DesignWare I²S driver on Raspberry Pi CM5.

Suggested files:

- `dmesg_poller_startup.txt`  
  Kernel log after loading the module and enabling the poller mode.

- `dmesg_loopback_test.txt`  
  Kernel log while running RX→TX loopback tests.

- `regs_debug_dump.txt`  
  Any manual register dumps or debug prints collected during bring-up.

These logs are useful to document real hardware behaviour and to revisit issues
or configurations later.
