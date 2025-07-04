#### elasticsearch 启动/关闭

```cmd
 /home/wfadmin/elasticsearch-7.6.2/elasticsearch-7.6.2/bin/elasticsearch -d -p /home/wfadmin/elasticsearch-7.6.2/elasticsearch.pid
kill -9 $(cat /home/wfadmin/elasticsearch-7.6.2/elasticsearch.pid)

/home/wfadmin/java_jar/elasticsearch/elasticsearch-6.8.6/bin/elasticsearch -d -p /home/wfadmin/java_jar/elasticsearch/elasticsearch-6.8.6/elasticsearch.pid
 
 kill -9 $(cat /home/wfadmin/java_jar/elasticsearch/elasticsearch-6.8.6/elasticsearch.pid)
```

/home/wfadmin/elasticsearch-7.6.2/elasticsearch-7.6.2 ：对应自己的安装elasticsearch目录





#### 安装nginx

```cmd
# 1. 安装依赖
sudo apt update
sudo apt install -y \
    g++ \
    libpcre3-dev \
    zlib1g-dev \
    libssl-dev

# 2. 下载 Nginx 源码
wget http://nginx.org/download/nginx-1.24.0.tar.gz
tar -zxvf nginx-1.24.0.tar.gz
cd nginx-1.24.0

# 3. 配置
./configure

# 4. 编译和安装
make
sudo make install
```

####  启动/关闭nginx服务（命令不可用时，前面加上 sudo）

```cmd
#启动脚本是在
# /usr/local/nginx/sbin/nginx
#启动,
# /usr/local/nginx/conf/nginx.conf ： 对应安装路劲 /home/wfadmin/nginx/nginx-1.28.0/conf/nginx.conf
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
#停止
/usr/local/nginx/sbin/nginx -s stop
#重载
/usr/local/nginx/sbin/nginx -s reload
#杀掉nginx
/usr/local/nginx/sbin/nginx -s quit
```

#### 配置自启

```cmd
sudo nano /etc/systemd/system/nginx.service
[Unit]
Description=nginx - high performance web server
Documentation=http://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/usr/local/nginx/logs/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /home/wfadmin/nginx/nginx-1.28.0/conf/nginx.conf
ExecStart=/usr/local/nginx/sbin/nginx -c /home/wfadmin/nginx/nginx-1.28.0/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target

# 重新加载 systemd 配置
sudo systemctl daemon-reload

# 启用 Nginx 服务
sudo systemctl enable nginx

# 启动 Nginx
sudo systemctl start nginx

# 检查状态
sudo systemctl status nginx
```





#### 开启**linux**防火墙(如果不行，在前面加上**sudo**)

```cmd
#查看已放行的端口
firewall-cmd --list-all
#将80端口加入到防火墙放行白名单中，并重载防火墙
firewall-cmd --add-port=80/tcp --permanent
# 关闭端口
sudo firewall-cmd --permanent --remove-port=80/tcp
firewall-cmd --reload

# 查看开放的端口号
firewall-cmd --list-all

# 查看防火墙状态
systemctl status firewalld
```



#### 服务启动与关闭

```cmd
nohup java -Xms128m -Xmx2048m -jar wfcrm-sendmsg-0.0.1-SNAPSHOT.jar> log.out  2>&1 &
ps -ef | grep wfcrm-sendmsg-0.0.1-SNAPSHOT.jar 
kill -9 端口
```



```cmd
[/home/wfadmin/anaconda3] >>> 
PREFIX=/home/wfadmin/anaconda3

# 添加环境环境变量
# >>> conda initialize >>>
# !! Contents within this block are manage by 'conda init' !!
__conda_setup="$('/home/wfadmin/anaconda3/bin/conda' 'shell.bash' 'hook' 2>/dev/null)"
if [$? -eq 0]; then
	eval "$__conda_setup"
else
	if [ -f "/home/wfadmin/anaconda3/etc/profile.d/conda.sh" ]; then 
	. "/home/wfadmin/anaconda3/etc/profile.d/conda.sh"
	else 
		export PATH="/home/wfadmin/anaconda3/bin:$PATH"
	fi
fi
unset __conda_setup
# <<<conda initialize <<<

查看所有可以安装python版本 conda search python
查看虚拟环境   conda env list  / conda info -e /  conda info --envs
激活虚拟环境   conda activate env_name
退出虚拟环境   conda deactivate env_name
```

