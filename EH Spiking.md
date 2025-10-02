---
tags:
- EH
MOC: Technology
---
[[_0000 Home|Home]] | [[_0005 Cybersecurity MOC|Back to Cybersecurity MOC]] | [[EH Ethical Hacking index|Back to index]]
- Spiking can be done using the tool `generic_send_tcp`
	- syntax: `generic_send_tcp [ip] [port] [spike_script] optional:[SKIPVAR SKIPSTR]`
	- For the spike script
		- s_readline();
		- s_string("STATS "); you can replace stats with any of the vulnserver options
		- s_string_variable("0");