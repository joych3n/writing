# GitHub 的代码仓库 SSH 连接问题及解决方案

## 问题描述

仓库用 SSH 协议 clone 成功，但之后执行 `git pull` / `git push` 报错：

`remote: Repository not found. fatal: repository 'https://github.com/xxx/yyy.git/' not found`

同时运行 `ssh -T git@github.com` 返回：

`Connection closed by 198.18.0.12 port 22`

## 原因

常见原因是网络代理（机场）或防火墙阻断了 22 端口，导致 SSH 连接被中断或被劫持。

## 解决方案

1. 临时/根本的网络调整，将 Clash（或其它代理）切到直连模式，或允许通过 22 端口的直连。切换后 SSH 恢复正常。

2. 改用 HTTPS 远程地址（无需依赖 22 端口），查看当前远程：`git remote -v`，修改为 HTTPS：`git remote set-url origin https://github.com/你的用户名/仓库名.git`

3. 如果改为 HTTPS 后仍有认证问题，清除本地保存的 Git 凭证（例如系统凭证管理器、credential helper 或 Keychain 中的条目），然后重新推送时重新输入凭证或使用 Personal Access Token。

## 排查建议

- 测试 SSH 连接：`ssh -T git@github.com`
- 切换代理后再次尝试 `git pull` / `git push` 以确认是否与代理有关。

简洁总结：问题多因 22 端口被拦截，临时可切直连；若不想改网络，直接把远程改成 HTTPS 是更方便的长期方案。
