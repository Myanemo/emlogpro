# emlog pro introduce
emlog is a lightweight blog and CMS system based on PHP and MYSQL.

# Vulnerability description
In the emlog pro project, there is a zip file that can be uploaded at admin/views/plugin.php, and after decompression, there is no filtering or analysis of the content, so that you can upload a compressed file with php, and upload it to the server through decompression, thus getshell

Project address：
https://github.com/emlog/emlog

The code audit found no filtering at the upload and decompression points
![image.png](https://cdn.nlark.com/yuque/0/2024/png/32816347/1713938621102-b1728996-e9dc-446e-bb3c-a08c6e8e5f5a.png#averageHue=%232c2c2b&clientId=ufc81e642-144e-4&from=paste&height=919&id=u658c90b4&originHeight=919&originWidth=1333&originalType=binary&ratio=1&rotation=0&showTitle=false&size=102009&status=done&style=none&taskId=u90478fda-9536-47e6-9092-3bb207ac529&title=&width=1333)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/32816347/1713938661011-6d3409fe-7bfc-4386-a9ab-8510942e5843.png#averageHue=%232d2c2b&clientId=ufc81e642-144e-4&from=paste&height=867&id=u2bdc3fc1&originHeight=867&originWidth=1287&originalType=binary&ratio=1&rotation=0&showTitle=false&size=99297&status=done&style=none&taskId=uc043ff44-1ee2-4bad-873b-ea9e627144e&title=&width=1287)
Through analysis, it is found that as long as the folder in the compressed package and the file name in the folder are the same, the upload can be successful.

# Vulnerablility reproduction
Download emlog pro2.3.2(latest version) and use phpstudy to set up and create a database
![image.png](https://cdn.nlark.com/yuque/0/2024/png/32816347/1713939807119-b2a92bec-eec2-4db7-b766-042d8bb1da06.png#averageHue=%23ebd3af&clientId=ufc81e642-144e-4&from=paste&height=630&id=u8ab75e6a&originHeight=630&originWidth=800&originalType=binary&ratio=1&rotation=0&showTitle=false&size=45616&status=done&style=none&taskId=u0994928a-9fb7-46ae-9bf8-de9afd41ce2&title=&width=800)
Place the downloaded project in the root directory of phpstudy.
![image.png](https://cdn.nlark.com/yuque/0/2024/png/32816347/1713940487670-70fd96ed-1449-4c50-bfb1-b4d9412cb20a.png#averageHue=%23fcfbfa&clientId=ufc81e642-144e-4&from=paste&height=542&id=u780683c6&originHeight=542&originWidth=893&originalType=binary&ratio=1&rotation=0&showTitle=false&size=44331&status=done&style=none&taskId=u6531a163-21af-465d-bf65-2bec5bc7f17&title=&width=893)
Then go to [http://localhsot/emlogpro2.3.2/install.php](http://localhsot/emlogpro2.3.2/install.php) for installation, can be configured
After the installation is complete, log in using the account and password set
![image.png](https://cdn.nlark.com/yuque/0/2024/png/32816347/1713939868419-b3c68902-af4a-4a29-af21-49863542d89d.png#averageHue=%2373cead&clientId=ufc81e642-144e-4&from=paste&height=853&id=uf184b412&originHeight=853&originWidth=1916&originalType=binary&ratio=1&rotation=0&showTitle=false&size=84040&status=done&style=none&taskId=uf2bcd57b-13ea-49dd-8a5a-f55530ff9c0&title=&width=1916)
漏洞点位于安装插件处
![image.png](https://cdn.nlark.com/yuque/0/2024/png/32816347/1713939907499-69bc8077-1259-4ce5-9f06-42943fca7ace.png#averageHue=%2372cdb0&clientId=ufc81e642-144e-4&from=paste&height=874&id=u1d5bda77&originHeight=874&originWidth=1920&originalType=binary&ratio=1&rotation=0&showTitle=false&size=85717&status=done&style=none&taskId=u2d4f4d4e-2009-43db-8f68-3e8fab5a9d4&title=&width=1920)
Construct a zip package as follows:
![image.png](https://cdn.nlark.com/yuque/0/2024/png/32816347/1713939966124-c4db9acf-a91e-4f0d-9bca-ddab29306bd2.png#averageHue=%23f3f1f0&clientId=ufc81e642-144e-4&from=paste&height=239&id=u7105ba5a&originHeight=239&originWidth=462&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14622&status=done&style=none&taskId=u8365683a-c6e6-41da-b882-e11cd7f263f&title=&width=462)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/32816347/1713940270883-5583b130-c961-4335-abc1-97c618c397f5.png#averageHue=%23685f43&clientId=ufc81e642-144e-4&from=paste&height=207&id=ua6ef690e&originHeight=207&originWidth=566&originalType=binary&ratio=1&rotation=0&showTitle=false&size=4076&status=done&style=none&taskId=uab9da6b8-efb1-4764-b696-7554f4a44dc&title=&width=566)
Note: The zip package must have a folder, and the folder and file name must be the same, as above 123/123.php

Click Install Plugin to upload 123.zip
![image.png](https://cdn.nlark.com/yuque/0/2024/png/32816347/1713940144176-da5ecb58-5a9b-4dee-8644-d3373ce58c97.png#averageHue=%2391845d&clientId=ufc81e642-144e-4&from=paste&height=519&id=u8ae9946f&originHeight=519&originWidth=1387&originalType=binary&ratio=1&rotation=0&showTitle=false&size=51153&status=done&style=none&taskId=u6168402b-df43-4189-aa95-c2d62f7880a&title=&width=1387)
After successful installation
![image.png](https://cdn.nlark.com/yuque/0/2024/png/32816347/1713940162956-1493d8aa-d800-4b8d-8ad7-123af9464250.png#averageHue=%23c8f0e0&clientId=ufc81e642-144e-4&from=paste&height=673&id=u68898d94&originHeight=673&originWidth=1710&originalType=binary&ratio=1&rotation=0&showTitle=false&size=67375&status=done&style=none&taskId=ud74ef001-907d-4e6f-86ec-c677deb0904&title=&width=1710)
Go to [http://localhost/emlog2.3.2/content/plugins/123/123.php](http://localhost/emlog2.3.2/content/plugins/123/123.php)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/32816347/1713940228915-a49d364c-ffeb-41eb-8591-ce2e6e39cea6.png#averageHue=%23b8976a&clientId=ufc81e642-144e-4&from=paste&height=924&id=ua0108849&originHeight=924&originWidth=1522&originalType=binary&ratio=1&rotation=0&showTitle=false&size=187006&status=done&style=none&taskId=u5413570c-456d-4835-ac2a-a37baf32010&title=&width=1522)
The php file was successfully uploaded and parsed

# The affected version
emlog pro 2.3.x

