# chal
```
(DBlog-bin.000007) There was a major heist at the local bank.
 Initial findings suggest that an intruder from within the bank, specifically someone from the bank’s database maintenance team, aided in the robbery.
 This traitor granted access to an outsider, who orchestrated the generation of fake transactions and the depletion of our valuable customers’ accounts.
 We have the phone number, ‘789-012-3456’, from which the login was detected, which manipulated the bank’s employee data.
Additionally, it’s noteworthy that this intruder attempted to add gibberish to the binlog and ultimately dropped the entire database at the end of the heist.
 Your task is to identify the first name of the traitor, the last name of the outsider, and the time at which the outsider was added to the database.
 Flag format: VishwaCTF{TraitorFirstName_OutsiderLastName_HH:MM:SS}
```
________________________________________________________________________________

using file we first figure what file we have 

![image](https://github.com/adwait3/forensics-ST/assets/148553626/f912a8c8-2dd0-411c-a912-c71ebbdbebea)

after a lot of searching and going through the net I found a utility `mysqlbinlog` which we can use

