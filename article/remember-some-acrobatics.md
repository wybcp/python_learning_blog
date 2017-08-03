# 记一些杂技（持续更新）

- 命令行连接shadowsocks客户端

```bash
sslocal -s IP -p PORT -l LOCAL_PORT -k PASSWORD -m 加密方式
```

- 修改npm源

在`~/.npmrc`里添加下面这行

> 如果是用的 cnpm，需要添加到`~/.cnpmrc`
```test
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
```

```bash
# 安装包时指定源
npm install -gd package --registry=http://registry.npm.taobao.org
# 设置全局npm源码
npm config set registry http://registry.npm.taobao.org
```

- 查看sqlite表结构

```sql
.schema tab_name
```

- Ubuntu chromium安装Flash

```bash
sudo apt-get install chromium-browser
sudo apt-get install pepperflashplugin-nonfree
sudo update-pepperflashplugin-nonfree --install
```

- Ubuntu rpm包转deb

```bash
sudo apt-get install alien
sudo alien xxx.rpm
```

- Ubuntu任务栏点击最小化

```bash
gsettings set org.compiz.unityshell:/org/compiz/profiles/unity/plugins/unityshell/ launcher-minimize-window true
```

- Linux制作U盘镜像

```bash
dd if=ISOFILE of=/dev/sdX
```

- Docker中Python中文错误

> UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-8: ordinal not in range(128)

```bash
docker run -it --name python -e PYTHONIOENCODING=utf-8 -e LANG=en_US.utf8 -e LANGUAGE=en_US.utf8 -e LANG_ALL=en_US.utf8 -e LC_ALL=en_US.utf8 centos /bin/bash
```

或者进入容器执行以下指令设置字符集

```bash
export PYTHONIOENCODING=utf-8
export LANG=en_US.utf8
export LANGUAGE=en_US.utf8
export LANG_ALL=en_US.utf8
export LC_ALL=en_US.utf8
```

- 修改Linux PS1前缀

```bash
# .bashrc
PS1='\[\033[01;34m\]\w\[\033[00m\]\$ '
```

- 命令行设置代理

```bash
# .bashrc
export http_proxy=http://IP:PORT
export https_proxy=http://IP:PORT
```

- git使用Token

```bash
# .netrc 
machine IP
  login Username
  password Token
```

- pip更新所有包

```bash
pip install -U $(pip freeze | awk '{split($0, a, "=="); print a[1]}')
```
