# Buffer overflow steps

### General steps

- Spiking
	- Figure out what is vulnerable
- Fuzzing
	- Determine at what bytes/where in memory program crashes
	- `/usr/share/metasploit-framework/tools/exploit/pattern_create.rb`
- Finding offset
	- Find offset of crash
	- `/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb`
- Overwriting the EIP
- Finding bad characters
	- \x00 is always a bad character
	- When using immunity, "Click ESP and follow in dump"
	- Find missing characters in dump
- Finding the correct module
- Exploitation

### Ret2libc

- If the machine has ASLR, addresses aren't randomized and libc's address for the binary remains static.
- To determine libc's address:
	- `$ ldd binary | grep libc`
- Find system, exit and /bin/sh's address:
	- `$ gdb -q binary`
	- `(gdb) b main 	# this will make gdb stop at main(), so you can read the addresses.`
	- `(gdb) p system`
	- `(gdb) p exit`
	- `(gdb) find &system,+9999999,"/bin/sh"`
	- To verify the addresses you get from the above,
	- `(gdb) x/s address	# This should say /bin/sh or sh`
- If the machine does not have ASLR, just run the binary with the python exploit.
- If the machine has ASLR, you may have to run the binary with the python exploit in a loop. You're bound to get root sometime.
- If the machine does not have gdb, use readelf/ldd.
- The exploit will be **system + exit + /bin/sh**
- Use struct.pack
- If the binary is hosted on a public port, use pwntools. Way easier.

### Tools:

- pattern_create
- pattern_offset
- pwntools (python)
- struct (python)
- readelf
- ldd
- gdb
