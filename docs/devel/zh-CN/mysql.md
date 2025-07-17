# 安装和配置 MariaDB 数据库

本文介绍在 Debian 上快速安装 MariaDB，并初始化 `tinyblog` 数据库。

## Debian MariaDB 快速安装

```bash
$ sudo apt install -y mariadb-server mariadb-client
$ sudo systemctl enable mariadb
$ sudo systemctl start mariadb
$ sudo systemctl enable mariadb
$ sudo mysqladmin -uroot password 'tinyblog1234' # 设置root初始密码
```

## 创建 MariaDB 用户

```bash
$ mysql -h127.0.0.1 -P3306 -uroot -p'tinyblog1234'
> grant all on tinyblog.* TO tinyblog@127.0.0.1 identified by 'tinyblog1234';
> flush privileges;
> exit;
```

## 初始化 `tinyblog` 数据库

用 `tinyblog` 用户登录 MariaDB，创建 `tinyblog` 数据库。创建命令如下。

```bash
$ cd $HOME/golang/src/github.com/FoBoHuang/tinyblog
$ mysql -h127.0.0.1 -P3306 -u tinyblog -p'tinyblog1234'
> source configs/tinyblog.sql;
```
