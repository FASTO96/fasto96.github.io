Created by fasto

# sammmry

![Desktop View](/img/ok/0.png)

 openbsdشغالة على machine هي Openkeys



 في البداية نجد يوزر نقوم باستعماله للحصول  على  
  Authentication Bypass وذلك بعمل ssh key  
  
 بعد ‫ذلك‬ ‫نجد ان‫ ‫اصدار‬‬  كيرنل مصاب ب
 cve 2019- 19520 
نستغلها للحصول على root
    
cve 2019-19520 

- 1 Recon
   - 1.1 Nmap
   - 1.2 ffuf
   - 1.3 Auth.php.swap
- 2 Authentication Bypass
   - 2.1 Ssh key
- 3 privilege escalation
   - 3.1 cve 2019-

  

## Walkthrough

### scanning
 nmap نبدا بفحص السيرفر ب 

 ![Desktop View](/img/ok/1.png)

نلاحظ وجود 2 بورت

80 : الخاص بالويب سيرفر

22: ssh خاص ب 

مفتوح شغال على port 80 نجد

 
  نجد انه مصاب ب  openBSD httpd عند البحث في جوجل عن 
  
  Authentication Bypass
  
  
  https://www.secpod.com/blog/openbsd-authentication-bypass-and-local-privilege-escalation-vulnerabilities/

### 1.2 ffuf

  80 ‫على‬ directory brute force نعمل 

![Desktop View](/img/ok/2.png)

نقوم بالنظر داخلها   directory  نجد العديد من  




1.3 Auth.php.swap

 ![Desktop View](/img/ok/3.png)

نتصفح

http://10.10.10.199/includes/Auth.php.swp
 
 


 
 
 نجد داخله اسم مستخدم

jennifer

 ![Desktop View](/img/ok/4.png)

 ## 2 Authentication Bypass

bypassنقوم باستخدام هذا الاسم في

ثم نكتب في burp و proxy نشغل

User : - schallenge

Pass : jennifer

 ![Desktop View](/img/ok/5.png)


burp ب request ونلتقط login نضغط


cookieنهاية ال في ;username=jennifer ثم نضيف

 ![Desktop View](/img/ok/6.png)

forwardبعد ذلك نضغط

 ![Desktop View](/img/ok/7.png)

### 2.1 Ssh key
jenniferخاص ب ssh key نحصل على

نضعه في ملف ونقوم بتغير الصلاحيات يمكنك تسميته ما تشاء

chmod 600 key ssh –i key jennifer@10.10.10.199

 ![Desktop View](/img/ok/8.png)

  ![Desktop View](/img/ok/9.png)



## 3 privilege escalation

### 3.1 cve 2019-19520
 لترى اصدار الكيرنل uname-a  نقوم بكتابة 

  عند ذلك نجد انها مصابة نحمل الاستغلال من هنا
  https://raw.githubusercontent.com/bcoles/local-exploits/master/CVE-2019-19520/openbsd-authroot

   ![Desktop View](/img/ok/10.png)



 ![Desktop View](/img/ok/11.png)

  نضعه في ملف ونغير الصلاحية


  ```
Chmod +x exploit
```
 نقوم بتشغيل الاستغلال  وننتظر قليلا يطبع لنا كلمة سر
 
 ‫‪EGG‬‬ ‫‪LAR‬‬
‫‪GROW‬‬ ‫‪HOG‬‬ ‫‪DRAG‬‬ ‫‪LAIN‬‬

 ![Desktop View](/img/ok/12.png)

 root نكتبها ونحصل على  

 ![Desktop View](/img/ok/13.png)

