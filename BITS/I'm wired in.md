## chal 
`MogamBro got scared after knowing that his PC has been hacked and tried to type a SOS message to his friend through his 'keyboard'. Can you find the contents of that message, obviously the attacker was logging him!`

## sol

first i went to the ad1 file through fkt imager and extracted the keylog pcap 

![Screenshot 2024-06-13 155751](https://github.com/adwait3/forensics-ST/assets/148553626/07a16b6d-d903-4ef4-bbf6-779027ced004)


then i filtered the usb packets using the command 

`usb.transfer_type==0x01 and frame.len==35 and !(usb.capdata == 00:00:00:00:00:00:00:00)
`

then I used this [repo](https://github.com/syminical/PUK) to parse my keyboard input 

i used 
`
tshark -r usb.pcapng -T fields -e usb.capdata | grep -E "." | grep -v '00000000.txt 
python PUK.py capdata.txt
`

![image](https://github.com/adwait3/forensics-ST/assets/148553626/2b0b9980-3878-4e8e-84f5-9eb6930f21d8)

## flag = BITSCTF{I_-7h1nk_th3y_4Re_k3yl0991ng_ME!}
