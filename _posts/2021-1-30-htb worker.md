```
Created by fasto
```

![Desktop View](/img/wk/0.png)

# Sammmry :

 اثناء عملية جمع المعلومات نجد كلمة سر خاصة ب  
azure devops

نقوم  برفع الشال بواسطته ثم نجد   
كلمة سر اخرى نقوم بانشاء

pipline

administrator للحصول على صلاحيات 

## Sommaire

- 1 Recon
   - 1.1 Nmap
   - 1.2 svnserv
- 2 user
   - 2.1 Azure devops
      - 2.1.1 New branche
   - 2.2 Upload shell
- 3 Administrator
   - 3.1 New pipline


## 1 Recon

### 1.1 Nmap

```
nmap نبدا بفحص البورتات باستخدام
```
![Desktop View](/img/wk/1.png)

### 1.2 svnserv



  ‫نلاحظ بورت‬‬ 
  3690 مفتوح‫‬


svnserv وهو خاص ب 

على هذا البورت enemurate نعمل
```
svn checkout svn://10.10.10.203
```

![Desktop View](/img/wk/2.png)


```
svn diff -r1
```


```
svn diff -r2
```


![Desktop View](/img/wk/3.png)


واسم مستخدم وكلمة سر subdomain نجد




/etc/hostsنقوم باضافة الدومين في ملف





 ثم نقوم بفتح الدومين

 نكتب اسم المستخدم وكلمة السر التي تحصلنا عليها سابقا

## 2 user

### 2.1 Azure devops

![Desktop View](/img/wk/4.png)

azure devopsالان دخلنا الى

![Desktop View](/img/wk/5.png)


smartHotel 360 نضغط على

جديد work items نقوم بانشاء

نذهب الى

Boards->work items


ثم نضغط على


new work item ->feature


saveثم work items نكتب اسم


![Desktop View](/img/wk/6.png)


![Desktop View](/img/wk/7.png)


#### 2.1.1 New branche

‫
 نقوم الان بانشاء  new branche

وذلك بالذهاب الى

Repos->branches


new branchثم نضغط على

انا كتبت  branch نختر اسم


create branchثم نضغط test1  

![Desktop View](/img/wk/8.png)

![Desktop View](/img/wk/9.png)

![Desktop View](/img/wk/10.png)

![Desktop View](/img/wk/11.png)




الذي انشأناه سابقا work items نقوم باختيار


### 2.2 Upload shell


test 1 ثم نضغط على

smarthotel360 نغير

ونقوم بالضغط على spectral الى 

upload file

work itemsالخاص بنا وضع shell نرفع


![Desktop View](/img/wk/12.png)

create a pull requestبعد ذلك نضغط على





![Desktop View](/img/wk/13.png)

set auto-completeثم Approveنضغط على


![Desktop View](/img/wk/14.png)

![Desktop View](/img/wk/15.png)


complete margeنضغط الان على


![Desktop View](/img/wk/17.png)

في spectral.worker.htbبنجاح نضيف shell تم رفع

/etc/hosts


![Desktop View](/img/wk/19.png)


الأتي powershell بامر drive نقوم بعرض
```
get-psdrive -psprovider filesystem
```
w:\نجد

بالبحث داخله نجد كلمات سر
في المسار

W:\svnrepos\www\conf


robislنعلم ان اسم المستخدم هو c:\users ومن مجلد

![Desktop View](/img/wk/20.png)


![Desktop View](/img/wk/21.png)



![Desktop View](/img/wk/22.png)




## 3 Administrator

### 3.1 New pipline

 نستخدم كلمة السر و اليوزر لدخول 

azure devops


builds ثم peplines نذهب الى
```
new pipline - > azure repose->parsUnlimited-
>starterpipline->


```
 نكتب الامر التالي لتغير 
كلمة سر

administrator الخاصة ب

![Desktop View](/img/wk/23.png)

```
steps:

- script: net user administrator pasx00!
displayName: 'Run a one-line script'
```



![Desktop View](/img/wk/24.png)



![Desktop View](/img/wk/25.png)

![Desktop View](/img/wk/26.png)

![Desktop View](/img/wk/27.png)

![Desktop View](/img/wk/28.png)

![Desktop View](/img/wk/29.png)

![Desktop View](/img/wk/30.png)

![Desktop View](/img/wk/31.png)




الان نتصل ب


باستخدام كلمة السرالجديدة Evil-winrm

```
evil-winrm -i 10.10.10.203 -u Administrator -p 'pasx0 00 '!
```


![Desktop View](/img/wk/32.png)
