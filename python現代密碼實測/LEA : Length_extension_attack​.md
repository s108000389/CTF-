-[題目](#題目)  
-[解題](#解題)

### 題目
```python
#!/usr/bin/env python3
import hashlib
from base64 import b64encode, b64decode

with open("salt", 'rb') as data:
    salt = data.read().strip()
    assert(len(salt) == 20)

with open("flag", 'r') as data:
    flag = data.read().strip()

normal = '\033[0m'
red = '\033[91m'
yellow = '\033[93m'

money = 0
coupons = {b"Anr9Ecttjm": 100,
           b"NjOwRzhBO1": 500,
           b"8X3kU8nKpP": 2000}

def encode_coupon(text):
    return b64encode(hashlib.sha256(salt + text).digest() + text).decode('ascii')

def decode_coupon(text):
    text = b64decode(text)
    return text[:32], text[32:]

def menu():
    print("----- menu -----")
    print("your current money : {}".format(money))
    print("1) use coupon")
    print("2) $100  : apple")
    print("3) $300  : golden apple")
    print("4) $1000 : flag")
    print("5) walk out of the store")

print("===== Welcome to Legend East Apple Shop =====")
print("We are celebrating our 20th anniversary")
print("free coupon for you : {}".format(encode_coupon(b"Anr9Ecttjm")))

while True:
    try:
        menu()
        choice = input('> ').strip()
        if choice == '1':
            auth, coupon = decode_coupon(input("coupon : ").strip())
            if auth == hashlib.sha256(salt + coupon).digest():
                for code, cash in coupons.items():
                    if code in coupon:
                        money += cash
                        coupons[code] = 0
        elif choice == '2':
            if money >= 100:
                money -= 100
                print("Here is your {}APPLE{}".format(red, normal))
            else:
                print("You don't have enough money ...")
        elif choice == '3':
            if money >= 300:
                money -= 300
                print("Here is your {}APPLE{}".format(yellow, normal))
            else:
                print("You don't have enough money ...")
        elif choice == '4':
            if money >= 1000:
                money -= 1000
                print("Here is your {}".format(flag))
            else:
                print("You don't have enough money ...")
        elif choice == '5':
            print("Have a nice day")
            break
        else:
            print("No such option")
            break
    except:
        exit(0)
```

### 解題
```
from pwn import * 
from base64 import b64encode as b64e
from base64 import b64decode as b64d
from hashpumpy import hashpump

def encode_coupon(text):
    return b64e(hashlib.sha256(salt + text).digest() + text).decode('ascii')

def decode_coupon(text):
    text = b64d(text)
    return text[:32], text[32:]

conn = remote('120.114.62.209',4118)
conn.recvuntil('free coupon for you : ')
auth , content = decode_coupon(conn.recvline().strip())
print(auth , content)


add_content = "NjOwRzhBO18X3kU8nKpP"
key_length = 20


new_hash , new_content = hashpump(auth.hex(), content, add_content, key_length)
new_hash = bytes.fromhex(new_hash)
payload = b64e(new_hash+new_content)

conn.sendline('1')
conn.sendline(payload)

conn.interactive()

```
