# Crypto I Writeup

Name: Robert Nash
Section: 0201

I pledge on my honor that I have not given or received any unauthorized
assistance on this assignment or examination.

Digital acknowledgement: Robert Nash

## Assignment Writeup

### Part 1 (70 Pts)

```
#!/usr/bin/env python3

import hashlib
import string

HASHFILE = "/Users/robertnash/Comp_Sci/389Rspring19/assignments/9_Crypto_I/hashes.txt"
PASSFILE = "/Users/robertnash/Comp_Sci/389Rspring19/assignments/9_Crypto_I/passwords.txt"


def crack():
    hashes = open(HASHFILE, 'r').read().splitlines()
    passwords = open(PASSFILE, 'r').read().splitlines()
    characters = string.ascii_lowercase

    for c in characters:
        for p in passwords:

            salted_pass = c + p
            digest = hashlib.sha256(salted_pass).hexdigest()

            for h in hashes:
               if h == digest:
                    print(p + ":" + h)


if __name__ == "__main__":
    crack()
```

### Part 2 (30 Pts)

```
#!/usr/bin/env python3

import hashlib
import string
import socket
import time


PASSFILE = "/Users/robertnash/Comp_Sci/389Rspring19/assignments/9_Crypto_I/passwords.txt"


def server_crack():
    passwords = open(PASSFILE, 'r').read().splitlines()
    characters = string.ascii_lowercase
    server_ip = '134.209.128.58'
    server_port = 1337

    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect((server_ip, server_port))

    for i in range(0, 3):
        data = s.recv(1024)
        print(data)
        data = data.splitlines()
        server_hash = data[2]

        for c in characters:
            for p in passwords:

                salted_pass = c + p
                digest = hashlib.sha256(salted_pass).hexdigest()

                if server_hash == digest:
                    print("Sent: " + salted_pass)
                    s.send(salted_pass + "\n")
                    time.sleep(1)
                    continue

    data = s.recv(1024)
    print(data)

if __name__ == "__main__":
    server_crack()
```

The flag is `CMSC389R-{h@sh1ng_@nd_sl@sh1ng}`
