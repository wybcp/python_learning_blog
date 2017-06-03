# 记一些杂技（持续更新）

- 命令行连接shadowsocks客户端

```bash
$ sslocal -s IP -p PORT -l LOCAL_PORT -k PASSWORD -m 加密方式
```

- 修改npm源

在`~/.npmrc`里添加下面这行

> 如果是用的 cnpm，需要添加到`~/.cnpmrc`
```test
sass_binary_site=https://npm.taobao.org/mirrors/node-sass/
```

```bash
# 安装包时指定源
$ npm install -gd package --registry=http://registry.npm.taobao.org
# 设置全局npm源码
$ npm config set registry http://registry.npm.taobao.org
```

- 查看sqlite表结构

```sql
.schema tab_name
```

- Ubuntu chromium安装Flash

```bash
$ sudo apt-get install pepperflashplugin-nonfree
$ sudo update-pepperflashplugin-nonfree --install
```

- Ubuntu rpm包转deb

```bash
$ sudo apt-get install alien
$ sudo alien xxx.rpm
```