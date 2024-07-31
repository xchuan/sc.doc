sudo chmod u+x run.sh

ps -ef|grep minio

用户名大于3位，密码大于8位

groupadd minio-user
useradd -g minio-user minio-user
cat /etc/group
cat /etc/passwd
chown minio-user:minio-user /home/chuan/io/minio


vim  /etc/default/minio


MINIO_ROOT_USER="pixc"
MINIO_ROOT_PASSWORD="123568Qwe"
MINIO_VOLUMES="/home/chuan/io/minio"
MINIO_OPTS="--address 0.0.0.0:9002 --console-address '0.0.0.0:9001'"


新建Systemd服务启动文件minio.service，配置Systemd服务启动。
[Unit]
Description=MinIO
Documentation=https://docs.min.io
Wants=network-online.target
After=network-online.target
#minio文件具体位置
AssertFileIsExecutable=/opt/minio/minio
[Service]
WorkingDirectory=/opt/minio
# User and group Linux用户组,前文配置的Linux用户组和用户
User=minio-user
Group=minio-user
#创建的配置文件 minio.conf
EnvironmentFile=/minio.conf
ExecStartPre=
ExecStart=/minio server -C /minio/etc /minio/data
# Let systemd restart this service always
Restart=always
# Specifies the maximum file descriptor number that can be opened by this process
LimitNOFILE=65536
# Disable timeout logic and wait until process is stopped
TimeoutStopSec=infinity
SendSIGKILL=no
[Install]
WantedBy=multi-user.target

cp /opt/minio/minio.service /etc/systemd/system/


systemctl enable minio.service
systemctl daemon-reload
systemctl start minio
systemctl status minio.service


#重新执行systemd
systemctl daemon-reload
开机自动启动
systemctl enable minio.service
#启动服务
systemctl start minio.service
#停止服务
systemctl stop minio.service
#重启服务
systemctl restrat minio.service
#查看服务状态
systemctl status minio.service
#在引导时禁用服务
systemctl disable minio.service

使用S3挂载MiniIO到Linux文件系统
参考资料：https://blog.51cto.com/u_14967494/6033832

https://blog.csdn.net/qq_24950043/article/details/136068964