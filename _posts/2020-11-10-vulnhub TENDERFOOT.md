# vulnhub tenderfoot

CTF السلام عليكم اليوم سنحل تحدي 

VULNHUB من تحديات  
يمكنك تحميله من هنا 

[tenderfoot](https://www.vulnhub.com/entry/tenderfoot-1,581/)
## Walkthrough

### scanning
 nmap نبدا بفحص السيرفر ب 

 ![Desktop View](/img/tnd/1.png)

     نلاحظ وجود 2 بورت

80 : الخاص بالويب سيرفر

22: ssh الخاص ب 

## Enumeration
 gobuster باستخدام  directory للبحث عن  brut force نقوم بعمل 

بالامر التالي 
``` bash
gobuster dir  -u http://192.168.1.33/. -w /usr/share/wordlist/dirbuster/directory-list-2.3-big.txt -t 50  -o big -b 403,404 --wildcard

```

 ![Desktop View](/img/tnd/2.png)


robots.txt نقوم بفتح 

 /hint نجد في الاسفل 

 ![Desktop View](/img/tnd/3.png)

   base32 نجد نص مشفر ب  /hint  عند التوجه الى 

![Desktop View](/img/tnd/4.png)

نقوم بفكه

![Desktop View](/img/tnd/5.png)

  entry.js نتوجه الان الى الملف  
![Desktop View](/img/tnd/9.png)

 monica نجد اسم 
 
   عند فحص سورس كود نجد نص مشفرfotocd نقوم بالتوجه الى 

   ![Desktop View](/img/tnd/6.png)

  brain fuck هذا النوع من التشفير يسمى 

   ![Desktop View](/img/tnd/7.png)

   نقوم بفكه 

   ![Desktop View](/img/tnd/8.png)


 داخل النص base64  نجد
 
 ssh نقوم بفكها لنتحصل على كلمة سر 

  ![Desktop View](/img/tnd/10.png)


monica نتصل باستخدام  كلمة السر $99990$ و الاسم السابق 

  ![Desktop View](/img/tnd/11.png)

  بعد البحث داخل السيرفرر نجد في المسار  التالي  
~/joey/have/a/gift/for/monica

note.txt

  ![Desktop View](/img/tnd/12.png)

 url يخبرنا ان نتصفح  

   ![Desktop View](/img/tnd/13.png)
   
   
 notes.txt و zip عند تصفحه نجد ملف 
 
 zip  نحمل ال 

![Desktop View](/img/tnd/14.png)


نجد كلمة سر note.txt عند فتح 

![Desktop View](/img/tnd/16.png)

zip file ل extract عندما نعمل 

اخر zip نجد ملف 
john نقوم بكسره ب 

![Desktop View](/img/tnd/18.png)

 h4ck3d نحصل على كلمة مرور 

 ![Desktop View](/img/tnd/19.png)
SUID نبحث عن ملفات  

باستخدم الامر التالي
```
find / -perm -u=s -type f 2>/dev/null
```

 ![Desktop View](/img/tnd/20.png)

SUID نجد كل البرامج التي لها صلاحيات

/opt/exec/chandler  نلاحظ  
مثير للاهتمام

execute عندما نعمل له  
 chandler نحصل على صلاحيات 

  ![Desktop View](/img/tnd/21.png)

  
  .cashe مجلد باسم  chandler الخاص ب home  نجد داخل مجلد 
  note.txt يحتوي على ملف 

 chandler بداخله كلمة سر خاصة ب 
chandler نقوم بالدخول الى 
```
su chandler
```
## Privilege Escalation 


 ان يشغل بصلاحيات رووت chandler ثم نرى ماذا  يستطيع  

 باستخدام الامر التالي
 ```
 sudo -l 
 ```

  ![Desktop View](/img/tnd/22.png)

gtfobiins نتوجه الى  ftp نجد ان يستطيع استخدام 

  ![Desktop View](/img/tnd/23.png)

نكتب
```
sudo ftp 
!/bin/sh
```

  ![Desktop View](/img/tnd/23.png)


ROOT نحصل على 
