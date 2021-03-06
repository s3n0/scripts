Switching on and off the LG TV via RS-232 serial interface
==========================================================

The shell script can be called from within the HDMI-CEC plugin or it can be called directly from the Enigma source code - always when the standby is de/activated. Therefore, choose the right shell scripts, depending on whether you want to control LG-TV via the HDMI-CEC plugin or by invoking standby mode in Enigma (`..../python/Screens/Standby.py` file).

- command to display basic serial interfaces under LINUX systems:
  - `setserial -g -a /dev/ttyS[0123]`

- command for listing of all available serial interfaces and especially access rights to them:
  - `ls -l /dev/ttyS*`

- there are two commands for LG-TV serial interface... so, as first, use the following commands to test the functionality of the RS-232 communication... after connecting the null-modem cable (without "hardware handshaking") between the set-top-box and the LG-TV ([OpenATV forum thread](https://www.opena.tv/english-section/32512-solution-standby-mode-lg-tv-hdmi-cec-simplink.html)):
  - turning off LG-TV:     `echo "ka 01 00" > /dev/ttyS0`
  - turning on LG-TV:      `echo "ka 01 01" > /dev/ttyS0`

- if your LG-TV has not turned off via the serial interface with the help of an echo command, then it is possible that you do not have access rights to the serial interface (write)... so try setting up serial interface access rights if necessary:
  - `chmod o+rw /dev/ttyS0`

- if the TV is still not communicating (TV turn off and on), it would have to try to configure the interface
  RS-232 to a slower speed, e.g. 9600 (for older LG TV)
  - display the current configuration of the "S0" interface:  `stty -a -F /dev/ttyS0`
  - change the speed of the "S0" interface to 9600 bps:   `stty -F /dev/ttyS0 9600`
  
- for some linux distributions it seems to have a separate setup for output and input, as follows:
  - `stty -F /dev/ttyS0 ispeed 9600 ospeed 9600`

- the `cat` command is normally used to display the contents of a file to standard output `stdout` but we can also display the serial interface input ... here's how to use it to extract content from input on the set-top-box:
  - `cat < /dev/ttyS0`
- another example of reading a serial port input and displaying results on stdout (in hexadecimal, i.e. byte - one by one):
  - `od -x < /dev/ttyS0`

Notes
=====

After rebooting the set-top box, immediately after the first standby activation, the user's shell-script (via the "Standby.py" algorithm) will not be run for the purpose of switching LG TV on / off. I haven't found out yet why (tested on OpenATV/Enigma only). So don't worry if the TV doesn't turn off after the set-top box is restarted (first standby activation, after rebooting the set top box). Try it again and then it will still work.

If you are using a USB <> RS232 adapter, you can also find the serial port here: `/dev/ttyUSB[0123]`

If the serial interface is built into a set-top box with an ARM chipset, then you can find the serial port here: `/dev/ttyAMA[0123]`
