# chal
```
Straightforward challenge, the flag is written on running notepad process. 
```

so we know we just need to extract the notepad file 

```
python3 vol.py -f '/home/kali/Desktop/memdump.mem'  windows.info
```

![image](https://github.com/adwait3/forensics-ST/assets/148553626/d694ca8a-b043-4ff3-aa12-37a90d91826a)

I used ps list

```
python3 vol.py -f '/home/kali/Desktop/memdump.mem'  windows.pslist
```

![image](https://github.com/adwait3/forensics-ST/assets/148553626/a520967c-a52b-4415-89ac-9928c4193c80)


`python3 vol.py -f '/home/kali/Desktop/memdump.mem'  -o "/home/kali/Desktop" windows.memmap --pid 6028 --dump`

then I used strings with grep but didn't get the full flag 
```
                                                                                                                                                                                                  
┌──(kali㉿kali)-[~/Desktop]
└─$ strings pid.6028.dmp | grep "BHflagY"
BHflagY{d22a 3  e e 
```

so I made it a text file to view the whole thing it had spaces which had to be removed

![flag](https://github.com/adwait3/forensics-ST/assets/148553626/acd3dfc9-02c9-4f05-ac64-caa4f277a294)

# flag = BHflagY{d22a3eed050c23c0880cc912368905c9d2527a41c328f81ef115b9464b800f7425333edb71d57b440b94dc766a2d49611d46968477b09dfa1f246585d87d7b5a}
