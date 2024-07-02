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

we found 

``````
### INSERT INTO `bank`.`maintainers`
### SET
###   @1=7 /* INT meta=0 nullable=0 is_null=0 */
###   @2='Matthew' /* VARSTRING(200) meta=200 nullable=1 is_null=0 */
###   @3='Miller' /* VARSTRING(200) meta=200 nullable=1 is_null=0 */
###   @4='matthew.miller@example.com' /* VARSTRING(400) meta=400 nullable=1 is_null=0 */
###   @5='789-012-3456' /* VARSTRING(60) meta=60 nullable=1 is_null=0 */
###   @6='Database Administrator' /* VARSTRING(400) meta=400 nullable=1 is_null=0 */
###   @7='DBA' /* VARSTRING(200) meta=200 nullable=1 is_null=0 */
###   @8='12:00:00' /* TIME(0) meta=0 nullable=1 is_null=0 */
###   @9='14:00:00' /* TIME(0) meta=0 nullable=1 is_null=0 */

``````

so mathew miller is the traitor

```SET
###   @1=1 /* INT meta=0 nullable=0 is_null=0 */
###   @2='John' /* VARSTRING(200) meta=200 nullable=1 is_null=0 */
###   @3='Darwin' /* VARSTRING(200) meta=200 nullable=1 is_null=0 */
###   @4='1990:01:01' /* DATE meta=0 nullable=1 is_null=0 */
###   @5='johndoe@example.com' /* VARSTRING(400) meta=400 nullable=1 is_null=0 */
###   @6='+1234567890' /* VARSTRING(60) meta=60 nullable=1 is_null=0 */
###   @7='123 Main St' /* VARSTRING(1020) meta=1020 nullable=1 is_null=0 */
###   @8='Anytown' /* VARSTRING(400) meta=400 nullable=1 is_null=0 */
###   @9='Anystate' /* VARSTRING(400) meta=400 nullable=1 is_null=0 */
###   @10='12345' /* VARSTRING(80) meta=80 nullable=1 is_null=0 */
###   @11=1 /* INT meta=0 nullable=1 is_null=0 */
# at 80520
```

john darwin is the outsider

and timestamp 


```#240227 15:31:29 server id 1  end_log_pos 80551 CRC32 0xebf8ef83 	Xid = 2928```


so the flag is 

VishwaCTF{Mattew_Darwin_15:31:29}

