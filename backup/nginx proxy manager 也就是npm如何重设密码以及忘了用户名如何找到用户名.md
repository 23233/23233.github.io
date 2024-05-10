## 本教程仅适用于使用docker部署 

### 重设密码
这里面有两个镜像 一个是官方的 一个是 `jlesage`的 对比其中的话 `jlesage` 有快捷工具 

#### 官方镜像使用mysql
```shell
docker exec -it nginx-proxy-manager sh
mysql -u root -p
use nginxproxymanager;
UPDATE user SET is_deleted=1;
```

#### jlesage的镜像
```shell
 /opt/nginx-proxy-manager/bin/reset-password name@example.com newpassword
```

### 忘记邮箱/用户名

#### 使用mysql
```shell
docker exec -it nginx-proxy-manager sh
mysql -u root -p
use nginxproxymanager;
select * from user;
```

#### 使用sqlite
直接通过docker 把sqlite的db映射出去 然后随便拿一个软件 打开sqlite 就能看到 没加密 



