From the description of the challenge we can tell that we need to do something related to Bluetooth so i check the attached pcap file for Bluetooth devices and find 

![image](https://github.com/adwait3/forensics-ST/assets/148553626/a241b639-ae62-48fe-a662-4147d3db6f1a)

in these we see ear(2) which seems like some sort of hedaphones 

going to the end of the packet file we see a lot of exchanges between the local host and the earphones 

![image](https://github.com/adwait3/forensics-ST/assets/148553626/2725d627-5a49-47e4-b26c-a53bb4bf6ec0)

at this point, I started searching for more clues but all in vain so I looked at the challenge writeups and found a hint about our guy loving beep bops this instantly seemed like morse code and i found random volume changes which pointed towards this realisation 

![image](https://github.com/adwait3/forensics-ST/assets/148553626/46b14c35-3dee-481e-8182-4b3a3ae429cf)

we take this and take 
>1. volume up = -
>2. volume down = .
>3. play pause as space between different letters

this gives us the code 

![image](https://github.com/adwait3/forensics-ST/assets/148553626/5975d31e-9618-4e46-a543-ae956c8d9df8)

BLE#FTW 

= BLE_FTW

which is our answer and 
# flag = 0CTF{BLE_FTW}
