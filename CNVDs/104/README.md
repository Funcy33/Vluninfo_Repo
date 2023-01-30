# Tenda Router W30E Vulnerability
This vulnerability lies in the `/goform/RouteStatic` page which influences the lastest version of Tenda Router W30E. (The latest version is [W30E_V1.0.1.25(633)](https://www.tenda.com.cn/download/detail-2218.html))
## Vulnerability Description
There is a stack-based buffer overflow vulnerability in function `fromRouteStatic`.

In function `fromRouteStatic` it reads 2 user provided parameters `entrys` and `mitInterface` into `v8` and `v7`, and these two variables are passed into function `sprintf` without any length check, which may overflow the stack-based buffer `s`.
![](./104/vuln.png)
