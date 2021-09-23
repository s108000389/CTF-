# python現代密碼實測
```
! pip install pycrypto
```
- [Module number](https://pythonhosted.org/pycrypto/Crypto.Util.number-module.html)
![image](https://user-images.githubusercontent.com/79491888/134443776-aa5f44d2-873e-4b7a-9be1-2889140c9597.png)
```
https://base64.guru/converter/encode/hex
https://cryptopals.com/
```
```
from Crypto.Util.number import GCD

GCD(12,18)
```
![image](https://user-images.githubusercontent.com/79491888/134444369-1fd6ece0-40ca-423b-9100-a718d2e440fe.png)

## AES對稱式加解密
### AES加密
```
from Crypto.Cipher import AES

obj = AES.new('This is a key123', AES.MODE_CBC, 'This is an IV456')

message = "flag(HappyCrypt)"

ciphertext = obj.encrypt(message)
ciphertext
```
### AES解密
```
obj2 = AES.new('This is a key123', AES.MODE_CBC, 'This is an IV456')

obj2.decrypt(ciphertext)
```
![image](https://user-images.githubusercontent.com/79491888/134444033-68229ed4-5dbe-47fb-b85b-1ffcc8e5d68f.png)
