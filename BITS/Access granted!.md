we have to work with a mem dump file in this 

`mwmdump.mem`

first i use 

```
python3 vol.py -f '/home/kali/Desktop/forensics kaam/summer/memdump.mem' windows.pslist 
```

![processlist](https://github.com/adwait3/forensics-ST/assets/148553626/31bee1cc-ffa2-43f3-bf75-7598ef4a94fa)


this seemed a bit useless so i research on how to get passwords fro mmemdumpss and figured out i need to get some kind of hash so i use 

```
python3 vol.py -f '/home/kali/Desktop/forensics kaam/summer/memdump.mem' windows.hashdump
```

![hash](https://github.com/adwait3/forensics-ST/assets/148553626/6bcc0cc8-06b6-4501-8985-5d2a605178fe)

from this we usee admin and momgambros password are same and the nthash is used for passwords primarily

`8a320467c7c22e321c3173e757194bb3`

from this we just need to crack the password we can use hashcat first we craeet a text file with our hash in it and use 

```
hashcat -m 1000 hash.txt /usr/share/wordlists/rockyou.txt.gz 
```

![ans](https://github.com/adwait3/forensics-ST/assets/148553626/2bf1376a-903c-44d2-88f2-a281deab9bc7)


from this we get our flag 

# flag ; BITSCTF{adolfhitlerrulesallthepeople}

