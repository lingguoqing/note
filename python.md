# 镜像

- http://mirrors.aliyun.com/pypi/simple/
- https://pypi.tuna.tsinghua.edu.cn/simple
- https://pypi.mirrors.ustc.edu.cn/simple/
- http://pypi.hustunique.com/
- http://pypi.sdutlinux.org/
- http://pypi.douban.com/simple

#### 创建镜像

- C:\Users\guoqing.ling\AppData\Roaming\Python\Python310\Scripts
- virtualenv E:\venv\deepseek\Scripts
- 进入到 E:\venv\deepseek\Scripts
- activate 回车即可



#### 生成**requirements.txt**文件

```cmd
pip freeze > requirements.txt

pip install -r requirements.txt
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
安装python   conda create --name 环境名称 python=python版本
```
