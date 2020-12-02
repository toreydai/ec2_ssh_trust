1.将本地密钥对文件上传到EC2

```
scp -i zhy_linux.pem ./zhy_linux.pem ec2-user@<public-ip-instance1>:~/
scp -i zhy_linux.pem ./zhy_linux.pem ec2-user@<public-ip-instance2>:~/
```
<br>2. 在每个EC2上创建RSA密钥和公钥

```
EC2实例1上创建密钥和公钥
ssh -i zhy_linux.pem ec2-user@<public-ip-instance1>

mkdir ~/.ssh
chmod 700 ~/.ssh
cd ~/.ssh
ssh-keygen -t rsa

EC2实例2上创建密钥和公钥
ssh -i zhy_linux.pem ec2-user@<public-ip-instance2>

mkdir ~/.ssh
chmod 700 ~/.ssh
cd ~/.ssh
ssh-keygen -t rsa
```
<br>3. 整合公钥文件

```
在EC2实例1上执行以下命令：
ssh -i zhy_linux.pem <private-ip-instance1> cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
ssh -i zhy_linux.pem <private-ip-instance2> cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```
<br>4. 将整合后的公钥文件分发到每台EC2

```
在EC2实例1上执行如下命令：
scp -i zhy_linux.pem ~/.ssh/authorized_keys <private-ip-instance2>:~/.ssh/
```
<br>5. 测试SSH互信

```
ssh <private-ip-instance2> date
ssh <private-ip-instance2>
```
<br>参考链接：
https://www.jianshu.com/p/a1a6828d2d89
