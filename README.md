# cmshub - A Collection Of Open Source CMS Vulnerability
  cmshub是一个收集各类开源CMS的漏洞库，以及自己的一些感悟总结。

# 如何进行代码审计
  基础：php基础，常规漏洞基础，深入后的框架基础<br/>
  以下想到什么写什么，不涉及具体漏洞原理。<br/>
  后面补充具体案例

# 常规流程
  1. 阅读项目的执行流程，差不多到初始化结束即可<br/>
     了解什么位置做了安全处理，怎么做的安全处理<br/>
     了解common函数的位置<br/>
     这一步要做到熟悉整个运行流程，目录结构，以便后期快速定位
  2. 如果可以的话，在网上查找待审计项目的历史漏洞。看看表哥们是怎么审计的，以及厂商是怎么修补的。
     了解表哥们的审计过程，以及该漏洞的出现原因，可以在接下来的审计中多注意一下。
  3. 变量跟踪+危险函数查找<br/>
     从危险函数的角度，要先明确要审计的漏洞类型，根据这个漏洞的原理，去归纳整理规则，不过是用grep还是其他的方式，找到危险函数的位置，并向上回溯，观察是否可控
  4. 如果可以，制定一下审计整个项目的计划（好吧，其实我自己也没做到）

# 常见漏洞

## 重装漏洞
  1. 查看项目是如何判断是否已经安装（注意判断完了有没有die、exit）
  2. 如果存在该漏洞，注意配置文件写入的地方，写入一句话

无敏感函数，看逻辑

## SQL注入漏洞

#### 起因：SQL语句拼接
  1. 查找SQL语句 或 mysql_query mysqli_query pdo (针对做预处理的情况，只查找预处理前拼接的情况)，一般项目里会有专门做数据库查询的类，可直接从这部分开始回溯
  2. 回溯可疑点拼接前是否经过安全处理，查看是否可绕过<br/>
    a. addslash 对数字型不起作用<br/>
    b. 转义单引号 看编码 宽字节注入<br/>
    c. 看处理逻辑是否可绕过，如果是软waf类似360wenscan，这种先找找网上绕过的方法。如果是自己写的处理，那就好好看代码吧<br/>
关注SQL语句，看看是否是拼接(这边回溯可能要很多，但是有可能回溯到的位置会很有意思，比如解析xml出来的注入)
mysql_query,mysqli_query,pdo<br/>
#### 案例
- [yxcms v1.2.1 cookie注入](https://github.com/t0w4r/cmshub/blob/master/yxcms/v1.2.1.md#cookie-%E6%B3%A8%E5%85%A5)
- [yxcms v1.2.1 前台注入](https://github.com/t0w4r/cmshub/blob/master/yxcms/v1.2.1.md#%E5%89%8D%E5%8F%B0sql%E6%B3%A8%E5%85%A5)
- [zzcms v8.2 SQL注入](https://github.com/t0w4r/cmshub/blob/master/zzcms/v8.2.md#sql%E6%B3%A8%E5%85%A5)
- [zzcms v8.2 登陆处盲注](https://github.com/t0w4r/cmshub/blob/master/zzcms/v8.2.md#%E7%99%BB%E9%99%86%E5%A4%84%E7%9B%B2%E6%B3%A8%E5%A4%8D%E7%8E%B0%E6%97%B6%E5%8F%91%E7%8E%B0%E7%9A%84%E5%8F%A6%E4%B8%80%E4%B8%AA%E6%BC%8F%E6%B4%9E%E5%90%8E%E6%9D%A5%E5%8F%91%E7%8E%B0%E5%B7%B2%E7%BB%8F%E6%9C%89%E5%89%8D%E8%BE%88%E5%8F%91%E7%8E%B0%E4%BA%86)

## 文件包含

#### 起因：包含文件字段可控
  1. 查找include|require|include_once|require_once，查看包含文件字段是否可控
  2. 利用方式可分为lfi和rfi，配合php的一些伪协议可达到读取文件内容，执行代码等

include|require|include_once|require_once <br/>
file:// php:// phar:// http:// zip:// <br/>
上传个图片再包含也行<br/>

## 远程命令执行

#### 起因：调用系统命令处，未做处理
system|passthru|pcntl_exec|shell_exec|escapeshellcmd|exec|popen<br/>
回溯参数是否可控即可<br/>
; && || | > ${IPS} 命令执行<br/>

## 文件上传漏洞
#### 起因：文件上传对文件名没有做限制，导致上传了脚本类型的文件

  1. 该漏洞重点关注 move_uploaded_file 或 上传功能multipart/form-data ，观察是否没有校验文件类型就生成了文件
  2. 过滤通常分为白名单和黑名单，关于黑名单，直接找黑名单以外的后缀名，或着根据windows的特性::$DATA绕过，或%00
  如果是白名单，看是否重命名，利用容器解析漏洞<br/>

move_uploaded_file

## 文件任意操作漏洞
#### 起因：未对待操作文件路径做校验
分为删除、修改、新建等操作<br>
`unlink|copy|fwrite|file_put_contents|bzopen`<br>
#### 案例
- [yxcms v1.2.1 后台任意文件删除](https://github.com/t0w4r/cmshub/blob/master/yxcms/v1.2.1.md#%E5%90%8E%E5%8F%B0%E4%BB%BB%E6%84%8F%E6%96%87%E4%BB%B6%E5%88%A0%E9%99%A4)
- [zzcms v8.2 install配置文件任意内容写入](https://github.com/t0w4r/cmshub/blob/master/zzcms/v8.2.md#install-%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E4%BB%BB%E6%84%8F%E5%86%85%E5%AE%B9%E5%86%99%E5%85%A5)
- [zzcms v8.2 任意文件删除](https://github.com/t0w4r/cmshub/blob/master/zzcms/v8.2.md#%E4%BB%BB%E6%84%8F%E6%96%87%E4%BB%B6%E5%88%A0%E9%99%A4)

## 逻辑漏洞
  1. 垂直越权，普通用户可执行管理员的操作，此类关注功能模块，相对应的去找代码
  2. 水平越权，普通用户可获取其他用户的信息或修改其他用户的信息等，同样黑盒结合白盒
  3. 逻辑错误，程序员编程不严谨导致的意料之外的漏洞。可以多关注权限验证这块，如加密算法的缺陷

这块更多的关注黑盒的功能点测试，辅以白盒<br/>
用户越权，忘记密码（通常分阶段，如果阶段间没有联系，可以任意重置），权限修改（传入可控的权限id）<br/>
#### 案例
- [yxcms v1.2.1 session伪造](https://github.com/t0w4r/cmshub/blob/master/yxcms/v1.2.1.md#session-%E4%BC%AA%E9%80%A0)

## 代码执行
#### 起因：对eval的参数没有做处理
重点匹配eval，对参数回溯<br/>
也会出现在模版解析中<br/>
很多框架会实现或调用一个模版引擎，通常会产生cache文件，如果产生cache文件前，有可控点，那么在接下来的include执行时，就会造成任意代码执行（关注assign()，display(),fetch()）<br/>

## XXE
#### 起因：xml的外部实体
重点匹配 `new DOMDocument() loadXML() simplexml_load_string`<br/>
观察是否xml可控

## SSRF
#### 起因：对提供的url未做限制
重点匹配 `curl_setopt file_get_contents fsockopen`<br/>
观察url是否可控，协议头是否可控，通过功能定位代码

## CSRF
#### 起因：未对提交的表单做tokens等处理
无敏感函数，黑盒关注敏感表单是否提交token等操作

## 反序列化
`unserialize __construct __destruct __toString __wakeup __sleep()`

## 框架挖掘
待补充



# tricks
php的一些特性

## parse_str 会对传入的字串做一次urldecode


## 文件包含突破限制
  %00 gpc=off,php<5.3.4 <br/>
./././[]././ 路径长度截断 php<5.2.8<br/>
..... 点号路径截断 php<5.2.8 windows<br/>



## empty
  empty(0)==true

## xml+注入
  xml 实体编码后 用 simplexml_load_string()后会转换实体<br/>
特殊字符 特殊含义 实体编码 <br/>
```
> => &gt;<br/>
< => &lt;<br/>
" => &quot; <br/>
' => &apos; <br/>
& => &amp;<br/>
```
会再转成'"<br/>
关注HTTP_RAW_POST_DATA





