# chal

`The exploit not only manipulated MogamBro's secret but also tried to establish an external TCP connection to gain further access to the machine. But I don't really think he was able to do so. Can you figure out where the exploit was trying to reach to?`

## sol 

extracted this keys file 

![image](https://github.com/adwait3/forensics-ST/assets/148553626/52f8c577-4b9b-44e2-a5ab-924954d43915)

to import the keys into the pcap i used 

`Edit → Preferences → Protocols → TLS → Browse Key file`

then i exported all http protocols and used strings and found the flag

## flag = BITSCTF{5te4l1ng_pr1v47e_key5_ez:)}
