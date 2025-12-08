# 解决 chrome 无法更新，提示“检查更新时出错：9（错误代码为：0）

如果你重启电脑或重装 chrome 依然无法解决更新问题，可以尝试以下方法：

## 方法 1：终端命令修改权限

```bash
sudo chown -R root:wheel /Applications/Google\ Chrome.app

diskutil resetUserPermissions / `id -u`
```

## 方法 2：启动更新服务

找到 chrome 启动服务(开机启动项)并启动它：`com.google.GoogleUpdater.wake.system`
