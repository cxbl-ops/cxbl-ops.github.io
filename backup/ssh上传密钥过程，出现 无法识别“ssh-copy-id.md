# 一， 首先生成密钥对

```bash
ssh-keygen -t rsa 
```
# 二，上传公钥
```bash
ssh-copy-id admin@192.168.x.x
```
# 三，问题

:::info{title="错误信息"}
ssh-copy-id : 无法将“ssh-copy-id”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。
:::
### 执行
```bash
function ssh-copy-id([string]$userAtMachine, $args){   
    $publicKey = "$ENV:USERPROFILE" + "/.ssh/id_rsa.pub"
    if (!(Test-Path "$publicKey")){
        Write-Error "ERROR: failed to open ID file '$publicKey': No such file"            
    }
    else {
        & cat "$publicKey" | ssh $args $userAtMachine "umask 077; test -d .ssh || mkdir .ssh ; cat >> .ssh/authorized_keys || exit 1"      
    }
}
```
