# chal
```
 I recorded one's USB traffic on his computer, can you find the hidden secret?
```
first like the oter bletooth chals we see the devices 

![a](https://github.com/adwait3/forensics-ST/assets/148553626/4f266c9b-3edc-489b-9194-f05f4e96f9bb)

and find this xerox pixel buds 

then we again sort by length and see a lot of packets between host and these buds 

![audio](https://github.com/adwait3/forensics-ST/assets/148553626/a40adc9b-c90b-40fd-88e2-4c218bf988b0)

we see the protocol being used is MPEG -1

``designed to compress VHS-quality raw digital video and CD audio down to about 1.5 Mbit/s (26:1 and 6:1 compression ratios respectively) without excessive quality loss``

it is basically a protocol for pictures and audio and stands for Moving Picture Experts Group Phase 1 (MPEG-1)

i was clueless after this and after consulting writeups found that the audio is LATM stream but must be converted to LAOS hedder

after dumping i used 

```
import sys

import tqdm
import scapy.utils

LOAS_SYNC = 0x2b7
LATM_HEADER = b"\x47\xfc\x00\x00"


if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Usage: loas.py <pcap> <loas>")
        exit(1)

    pkts = scapy.utils.rdpcap(sys.argv[1])

    with open(sys.argv[2], "wb") as f:
        for i, pkt in tqdm.tqdm(enumerate(pkts), total=len(pkts)):
            data = bytes(pkt)
            payload = data[data.index(LATM_HEADER):]

            # append loas_header for each aac-latm frame
            loas_header = (LOAS_SYNC << 13) + (len(payload) & 0x1fff)
            f.write(loas_header.to_bytes(3, 'big'))
            f.write(payload)
```
then used 
`ffmpeg -i radio.loas radio.wav`

the audio file had the contents 

```
Welcome back to "Secret Flags Unveiled" on HITCON Radio! I'm John, your host for this intriguing journey into the world of secret flags.

Today, we'll explore the secret flag, where flag served as vital information for scoring in CTFs.

The secret flags are crucial to the success of HITCON CTF, and one of them is going to be revealed. Listen carefully, you get only one chance.

Flag start.
secret flags unveiled with bluetooth radio.
Flag end.

Just simply wrap the text you heard with the flag format. If you find some information missing, just dig deeper in the packet.

Stay tuned for more secret flags. This is John, signing off from "Secret Flags Unveiled" on HITCON Radio. Keep those flags flying high!
```

flag `hitcon{secret-flags-unveiled-with-bluetooth-radio}`
