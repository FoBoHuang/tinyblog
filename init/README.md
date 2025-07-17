# Systemd 配置、安装和启动

- [Systemd 配置、安装和启动](#systemd-配置安装和启动)
	- [1. 前置操作](#前置操作)
	- [2. 创建 tinyblog systemd unit 模板文件](#创建-tinyblog-systemd-unit-模板文件)
	- [3. 复制 systemd unit 模板文件到 sysmted 配置目录](#复制-systemd-unit-模板文件到-sysmted-配置目录)
	- [4. 启动 systemd 服务](#启动-systemd-服务)

## 1. 前置操作

1. 创建需要的目录

```bash
sudo mkdir -p /data/tinyblog /opt/tinyblog/bin /etc/tinyblog /var/log/tinyblog
```

2. 编译构建 `tinyblog` 二进制文件

```bash
make build # 编译源码生成 tinyblog 二进制文件
```

3. 将 `tinyblog` 可执行文件安装在 `bin` 目录下

```bash
sudo cp _output/platforms/linux/amd64/tinyblog /opt/tinyblog/bin # 安装二进制文件
```

4. 安装 `tinyblog` 配置文件

```bash
sed 's/.\/_output/\/etc\/tinyblog/g' configs/tinyblog.yaml > tinyblog.sed.yaml # 替换 CA 文件路径
sudo cp tinyblog.sed.yaml /etc/tinyblog/ # 安装配置文件
```

5. 安装 CA 文件

```bash
make ca # 创建 CA 文件
sudo cp -a _output/cert/ /etc/tinyblog/ # 将 CA 文件复制到 tinyblog 配置文件目录
```

## 2. 创建 tinyblog systemd unit 模板文件

执行如下 shell 脚本生成 `tinyblog.service.template`

```bash
cat > tinyblog.service.template <<EOF
[Unit]
Description=APIServer for blog platform.
Documentation=https://github.com/FoBoHuang/tinyblog/blob/master/init/README.md

[Service]
WorkingDirectory=/data/tinyblog
ExecStartPre=/usr/bin/mkdir -p /data/tinyblog
ExecStartPre=/usr/bin/mkdir -p /var/log/tinyblog
ExecStart=/opt/tinyblog/bin/tinyblog --config=/etc/tinyblog/tinyblog.yaml
Restart=always
RestartSec=5
StartLimitInterval=0

[Install]
WantedBy=multi-user.target
EOF
```

## 3. 复制 systemd unit 模板文件到 sysmted 配置目录

```bash
sudo cp tinyblog.service.template /etc/systemd/system/tinyblog.service
```

## 4. 启动 systemd 服务

```bash
sudo systemctl daemon-reload && systemctl enable tinyblog && systemctl restart tinyblog
```
