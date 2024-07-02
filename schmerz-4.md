chal 
`What were the contents of the secret file?`

we see a password protected zip file 

used volatility to dump the notepad.exe file

``` python3 vol.py -f memdump.mem windows.memmap --pid 7536 --dump```

using this we find the password using `fcarckzip`

`83KvvO60Zf69Yyq8`

now we extract the zip and get a tv.jpg
from which we get our flag 

`ajeet-mestry-UBhpOIHnazM-unsplash`
