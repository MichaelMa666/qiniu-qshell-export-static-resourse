@[TOC](七牛云对象存储OSS资源导出基本操作过程)

>七牛的对象存储测试域名被收回，[个站](maxiaobo.top)所有的图片全部GG了。找到新的图床后准备迁移，就用到了七牛自己的CLI工具qshell，
>注意windows工具是分32位/64位操作系统的。使用过程中，由于图片的外链域名已经被收回了，根本没法在配置文件里填写外链域名。
>给七牛客服小姐姐打了电话后，get骚操作如下。
>先把域名过期的bucket里的资源批量拷贝到一个新建的bucket，新建的bucket是有30天时限的外链域名的。然后再批量下载新bucket里的图片资源。

## 关键CMD命令
进入到qshell所在的目录。
`qshell account <AK> <SK>`通过验证AK/SK登录自己的七牛云账号。
将其他bucket里的文件都拷贝到新建的bucket中。比如我旧的bucket名字叫assets，新建的bucket叫export。先取出assets下的文件名字列表写到mxb.txt文件中。
`qshell listbucket assets mxb.txt`
()[]
只留下第一列文件名字。另存为叫cpp.txt
[]()
执行下面命令进行批量拷贝。
`qshell batchcopy assets export cpp.txt`
现在所有资源都在新的bucket下存在了，然后按照下面说明填好配置文件qshell.conf。
```
{
    "dest_dir"  :   "本地存放图片路径，比如./images",
    "bucket"    :   "七牛云空间名称",
    "domain"    :   "空间对应的域名或自定义的二级域名",
    "access_key":   "七牛账号对应的AccessKey",
    "secret_key":   "七牛账号对应的SecretKey",
    "is_private":   是否是私有空间，取值为true或false，默认false,
    "prefix"    :   "文件的前缀，默认为空",
    "suffix"    :   "文件的后缀，默认为空"
}
```
最后执行命令
`qshell qdownload 10 qshell.conf`
数字10是10个线程的意思，成功则如下图类似显示。
[]()
